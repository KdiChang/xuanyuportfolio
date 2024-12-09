<template><div><p>阅读论文 <a href="https://link.jianshu.com/?t=http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf" target="_blank" rel="noopener noreferrer">Dynamo: Amazon’s Highly Available Key-value Store</a> 后的一些总结。</p>
<!-- more -->
<h2 id="_1-background" tabindex="-1"><a class="header-anchor" href="#_1-background" aria-hidden="true">#</a> 1. Background</h2>
<p>Dynamo 是 Amazon 开发的分布式存储系统，它在 Amazon 所处的应用环境中面对的问题和场景：</p>
<ul>
<li>大多数场景并不需要复杂的查询功能；</li>
<li>由于使用商用机作为服务器，机器或网络失败将成为相对常见的场景，需要妥善处理；</li>
<li>为了提供良好的用户体验，服务需要极高的可用性和较好的性能；</li>
<li>随着业务量逐步增大，服务的处理能力也需要平滑提升</li>
</ul>
<p>在其他很多互联网业务中也适用。</p>
<p>针对上述背景，Dynamo 的基本定位：</p>
<ul>
<li>仅提供简单的 key-value 查询</li>
<li>高可用性</li>
<li>易扩展性</li>
</ul>
<p>因此 Dynamo 在设计中会舍弃掉许多负担，如关系型数据库复杂的查询模型、对 ACID 的支持，对强一致性的追求，以及复杂的存储结构等。同时，Dynamo 认为其面对的是相对安全的内部网络环境，所以并没有处理安全或权限问题。</p>
<h2 id="_2-introduction" tabindex="-1"><a class="header-anchor" href="#_2-introduction" aria-hidden="true">#</a> 2. Introduction</h2>
<ul>
<li>客户端使用 put、get 接口读写指定 key 所对应的数据；</li>
<li>将整个数据空间划分为不同分片后存储在不同节点上；</li>
<li>通过分片复制和一系列故障发生时的应对方案来保证整个服务的高可用性；</li>
<li>为了可用性损失了一些一致性，可能发生的数据冲突有可能需要应用程序处理；</li>
<li>去中心化的维护整个集群的成员及故障信息，并用 gossip 同步。</li>
</ul>
<h2 id="_3-system-architecture" tabindex="-1"><a class="header-anchor" href="#_3-system-architecture" aria-hidden="true">#</a> 3. System Architecture</h2>
<h3 id="_3-1-system-interface" tabindex="-1"><a class="header-anchor" href="#_3-1-system-interface" aria-hidden="true">#</a> 3.1 System Interface</h3>
<p>将对象与 key 相关联：</p>
<ul>
<li>
<p><code v-pre>get(key)</code>：定位与 <code v-pre>key</code> 关联的对象副本，并返回一个对象或一个包含冲突版本和对应上下文的对象列表。上下文信息与对象一起存储，以便系统可以验证请求中提供的上下文的有效性。</p>
</li>
<li>
<p><code v-pre>put(key, context, object)</code>：基于对象（<code v-pre>object</code>）的 <code v-pre>key</code> 决定将对象的副本放在哪，并将副本写入到磁盘。<code v-pre>context</code> 包含对象的系统元数据且对调用者不透明，包括对象的版本信息等。</p>
</li>
</ul>
<h3 id="_3-2-数据分片-partitioning" tabindex="-1"><a class="header-anchor" href="#_3-2-数据分片-partitioning" aria-hidden="true">#</a> 3.2 数据分片（Partitioning）</h3>
<p>为了更好更灵活的操作管理存储的数据，考虑对数据空间进行分片并将分片分配到不同存储机器。Dynamo 采用类似一致性哈希的方式进行分片的划分和分配。关于数据分片的算法，Dynamo 经历了几个阶段的选择和进化：</p>
<ol>
<li>
<p>传统的一致性哈希：将机器节点随机对应在哈希环（hash ring）上，数据 key 所对应的哈希值所在位置沿一致性哈希环顺时针遇到的第一个节点负责自己所在的 range。</p>
<ul>
<li>优点：新节点加入，或旧节点退出时，只影响紧相邻的下一个节点</li>
<li>缺点：负载不均匀，且没有考虑到不同的机器性能的不同</li>
</ul>
<p> </p>
<p>考虑节点的变动：
<img src="/img/in-post/2018-11-30/cache-change.png" alt=""></p>
</li>
<li>
<p>增加虚拟节点（virtrual nodes）的一致性哈希：每个机器节点对应哈希环上的多个虚拟节点，可以根据机器性能方便的调节所负责的虚拟节点数。数据 key 所对应的哈希值所在位置沿一致性哈希环顺时针移动遇到的前 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.10903em;">N</span></span></span></span> 个节点负责自己所在的 range（论文讨论的就是这种策略）。</p>
<ul>
<li>优点：充分考虑机器性能的不同且可以做到负载均衡</li>
<li>缺点：分片转移时，实现上需要对整个 range 进行遍历；加入或删除节点时 Merkle tree 需要重新计算</li>
</ul>
 <img src="/img/in-post/2018-11-30/strategy1.png" width="300px" alt="strategy1" />
<p><span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span>，<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.05017em;">B</span></span></span></span>，<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.07153em;">C</span></span></span></span> 为三个独立的节点，是 key <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.8444em;vertical-align:-0.15em;"></span><span class="mord"><span class="mord mathnormal" style="margin-right:0.03148em;">k</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height:0.3011em;"><span style="top:-2.55em;margin-left:-0.0315em;margin-right:0.05em;"><span class="pstrut" style="height:2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight">1</span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height:0.15em;"><span></span></span></span></span></span></span></span></span></span> 在一致性哈希环上的首选列表。阴影部分表示 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span>，<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.05017em;">B</span></span></span></span>，<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.07153em;">C</span></span></span></span> 负责的 range。</p>
<p> </p>
<p>考虑虚拟节点：
<img src="/img/in-post/2018-11-30/virtural-node.png" alt=""></p>
</li>
<li>
<p>数据空间等分 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.8778em;vertical-align:-0.1944em;"></span><span class="mord mathnormal">Q</span></span></span></span> 份，<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.13889em;">T</span></span></span></span> 个机器节点时，每个机器分得 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.05764em;">S</span></span></span></span> 个分片，其中 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.8778em;vertical-align:-0.1944em;"></span><span class="mord mathnormal">Q</span><span class="mspace" style="margin-right:0.2778em;"></span><span class="mrel">=</span><span class="mspace" style="margin-right:0.2778em;"></span></span><span class="base"><span class="strut" style="height:0.7667em;vertical-align:-0.0833em;"></span><span class="mord mathnormal" style="margin-right:0.13889em;">T</span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">×</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.05764em;">S</span></span></span></span></p>
<ul>
<li>优点：分片固定大小，可对应单个文件，因此容易加入或删除节点，且容易备份。</li>
</ul>
 <img src="/img/in-post/2018-11-30/strategy2.png" width="300px" alt="strategy2" />
</li>
</ol>
<h3 id="_3-3-分片备份-replication" tabindex="-1"><a class="header-anchor" href="#_3-3-分片备份-replication" aria-hidden="true">#</a> 3.3 分片备份（Replication）</h3>
<p>为了系统的高可用性，Dynamo 的每个分片都有 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.10903em;">N</span></span></span></span> 个副本，存储在哈希环上顺时针方向的 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.10903em;">N</span></span></span></span> 个节点上。这 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.10903em;">N</span></span></span></span> 个节点称为该数据的 <strong>preference list</strong>。其中的每一个节点都可以对接受针对该数据的操作请求。</p>
<img src="/img/in-post/2018-11-30/replication.png" width="450px" alt="replication" />
<p class="desc">对于key K : preference list = B, C, D</p>
<p><strong>Coordinator 协调器</strong>：每个 key 被分配到一个 coordinator 节点，coordinator 节点复制这些 key 到环上顺时针方向的 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.7667em;vertical-align:-0.0833em;"></span><span class="mord mathnormal" style="margin-right:0.10903em;">N</span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">−</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:0.6444em;"></span><span class="mord">1</span></span></span></span> 个后继节点。这样的结果是，系统中每个节点负责环上的从其自己到第 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.10903em;">N</span></span></span></span> 个前继节点间的一段区域。</p>
<p>考虑节点故障：preference list 可以包含超过 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.10903em;">N</span></span></span></span> 个节点。其他的节点可以是这个环上的，也可以时其他数据中心的，可以根据需要配置。</p>
<p>考虑虚拟节点：由于使用了虚拟节点，key 的前 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.10903em;">N</span></span></span></span> 个后继位置可能属于少于 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.10903em;">N</span></span></span></span> 个物理节点，因此 preference list 将跳过一些虚拟节点位置，以保证 list 中只包含不同的物理节点。</p>
<h3 id="_3-4-数据版本-data-versioning" tabindex="-1"><a class="header-anchor" href="#_3-4-数据版本-data-versioning" aria-hidden="true">#</a> 3.4 数据版本（Data Versioning）</h3>
<p><strong>强一致性</strong>：假如 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span> 先写入了一个值到存储系统，存储系统保证后续 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span>、<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.05017em;">B</span></span></span></span>、<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.07153em;">C</span></span></span></span> 的读取操作都将返回最新值。如果不能保证，写操作就会失败。</p>
<p>Dynamo 根据 CAP 原理（在一个分布式系统中，Consistency（一致性）、 Availability（可用性）、Partition tolerance（分区容错性），三者不可兼得），在一致性上做出让步，在任何时刻用户都可以进行写操作，提供<strong>最终一致性</strong>：过程松，结果紧，最终结果必须保持一致性。</p>
<p>最终一致性允许更新操作可以异步地传播到所有副本。</p>
<p>由于 preference list 中的每个节点都可以对同一个数据进行处理，可能存在更新的数据还没来得及传播到所有的副本就返回数据给调用者的情况，导致 get() 操作返回的对象不是最新的。当有大量并行访问或故障发生时，集群中的不同机器看到的同一个数据状态可能不同，这时就发生了冲突。</p>
<p><strong>例子：</strong></p>
<p>当客户希望增加一个项目到购物车（或从购物车删除）但最新的版本不可用时，该项目将被添加到旧版本（或从旧版本中删除）。不可用的新版本和变化后的旧版本中的信息都需要保留，因此不同版本需要进行协调。</p>
<p>为了保留所有版本的信息，Dynamo 将每次数据修改的结果当作一个新的且不可改变的数据版本。这造成一份数据会存在多个版本，分布在不同的节点上。多数情况下，系统会自动合并这些版本，一旦合并尝试失败，冲突的解决就交给应用端来解决。</p>
<p>Dynamo 引入 <strong>vector clock</strong> 来缓解冲突的发生，这时 get(key) 操作返回的不是单一的数据，而是一个多版本的数据列表，最终由应用端对冲突进行合并。</p>
<div class="language-python ext-py"><pre v-pre class="language-python"><code>vector clock <span class="token operator">:=</span> <span class="token builtin">list</span><span class="token punctuation">{</span> <span class="token punctuation">(</span>node<span class="token punctuation">,</span> counter<span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">.</span><span class="token punctuation">}</span>
node <span class="token operator">:=</span> 节点编号
counter <span class="token operator">:=</span> 该数据在节点上的处理序序号
</code></pre></div><ul>
<li>vector clock 通过列出在数据在每个节点上的处理序列来发现不同 vector clock 之间的因果关系，其中每个 (node, counter) 可以看做是一个分量；</li>
<li>当某个 vector clock 的所有分量都小于另一个时，该 vector clock 就是另一个的因，可以被覆盖；</li>
<li>没有因果关系的所有 vector clock 需要全部返回客户端，在应用端处理。</li>
</ul>
<img src="/img/in-post/2018-11-30/vector-clock.png" width="450px" alt="vector-clock" />
<p><strong>例子：</strong></p>
<ol>
<li>
<p>假设该商城有 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span>、<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.05017em;">B</span></span></span></span>、<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.07153em;">C</span></span></span></span> 三个 node，则 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.10903em;">N</span><span class="mspace" style="margin-right:0.2778em;"></span><span class="mrel">=</span><span class="mspace" style="margin-right:0.2778em;"></span></span><span class="base"><span class="strut" style="height:0.6444em;"></span><span class="mord">3</span></span></span></span>。令 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.13889em;">W</span><span class="mspace" style="margin-right:0.2778em;"></span><span class="mrel">=</span><span class="mspace" style="margin-right:0.2778em;"></span></span><span class="base"><span class="strut" style="height:0.6444em;"></span><span class="mord">1</span></span></span></span>，由 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.7667em;vertical-align:-0.0833em;"></span><span class="mord mathnormal" style="margin-right:0.13889em;">W</span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">+</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:0.7224em;vertical-align:-0.0391em;"></span><span class="mord mathnormal" style="margin-right:0.00773em;">R</span><span class="mspace" style="margin-right:0.2778em;"></span><span class="mrel">&gt;</span><span class="mspace" style="margin-right:0.2778em;"></span></span><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.10903em;">N</span></span></span></span> 得 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.00773em;">R</span><span class="mspace" style="margin-right:0.2778em;"></span><span class="mrel">=</span><span class="mspace" style="margin-right:0.2778em;"></span></span><span class="base"><span class="strut" style="height:0.6444em;"></span><span class="mord">3</span></span></span></span>，则：</p>
<p><img src="/img/in-post/2018-11-30/iphone-example1.png" alt="iphone-example1"></p>
<p>最终：<strong>A: 4500[A:2]  B：5000[A:2,B:1]  D:  3000[A:2,C:1]</strong></p>
<p>这时有人询问 iPhone 的价格，<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.00773em;">R</span><span class="mspace" style="margin-right:0.2778em;"></span><span class="mrel">=</span><span class="mspace" style="margin-right:0.2778em;"></span></span><span class="base"><span class="strut" style="height:0.6444em;"></span><span class="mord">3</span></span></span></span>，读到所有的数据。显而易见，<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span> 上的版本最低，应被舍弃。而对 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.05017em;">B</span></span></span></span> 和 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.07153em;">C</span></span></span></span> 被送到客户端去处理，我们可以让它有个判断依据——比如时间戳——现在客户端看到 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.07153em;">C</span></span></span></span> 上的数据是最新的，那么结论就是 3000。</p>
<p> </p>
</li>
<li>
<p><span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.13889em;">W</span><span class="mspace" style="margin-right:0.2778em;"></span><span class="mrel">=</span><span class="mspace" style="margin-right:0.2778em;"></span></span><span class="base"><span class="strut" style="height:0.6444em;"></span><span class="mord">2</span></span></span></span>，<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.00773em;">R</span><span class="mspace" style="margin-right:0.2778em;"></span><span class="mrel">=</span><span class="mspace" style="margin-right:0.2778em;"></span></span><span class="base"><span class="strut" style="height:0.6444em;"></span><span class="mord">2</span></span></span></span>：</p>
<p><img src="/img/in-post/2018-11-30/iphone-example2.png" alt="iphone-example2"></p>
<p>最终：<strong>A: 3000[A:2,B:1,C:1]  B: 5000[A:2,B:1]  C:  3000[A:2,B:1,C:1]</strong></p>
<p>由于 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.05017em;">B</span></span></span></span> 的版本低，所以无论读哪两个，都会返回 3000，无需客户端做出协调。</p>
<p> </p>
<p>由此也可以看出提高 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.13889em;">W</span></span></span></span> 可以降低冲突，提高一致性。但代价是：写两份比写一份要慢，并且同时写成功的概率低于写成功一份的概率——也就是 Availability 降低。这符合 CAP 理论。</p>
</li>
</ol>
<p>可能的问题：如果许多服务器协调对一个对象的写，vector clock 的大小可能会增长。因为写入通常是Coordinator（也就是 Preference list 的第一个节点）执行，所以这个问题一般不会发生。但在多个服务器故障时，写请求可能会被不是 coordinator 的节点执行，导致 vector clock 的大小增长。</p>
<p>因此需要限制 vector clock 的大小，Dynamo 采用了以下方案：对每个 (node, counter) 对，Dynamo 存储一个时间戳表示最后一次更新的时间。当 vector clock 中 (node, counter) 对的数目达到一个阈值(如 10)，最早的一对将从时钟中删除。</p>
<p>显然，这个方案会导至在协调时效率低下，因为后代关系不能准确得到。但这个问题还没有出现在生产环境，因此这个问题没有得到彻底研究。</p>
<h3 id="_3-5-读写过程" tabindex="-1"><a class="header-anchor" href="#_3-5-读写过程" aria-hidden="true">#</a> 3.5 读写过程</h3>
<p>Dynamo 采用类似 Quorum 的方式保证数据正确，即 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.7667em;vertical-align:-0.0833em;"></span><span class="mord mathnormal" style="margin-right:0.13889em;">W</span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">+</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:0.7224em;vertical-align:-0.0391em;"></span><span class="mord mathnormal" style="margin-right:0.00773em;">R</span><span class="mspace" style="margin-right:0.2778em;"></span><span class="mrel">&gt;</span><span class="mspace" style="margin-right:0.2778em;"></span></span><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.10903em;">N</span></span></span></span>。</p>
<p>Put 流程：</p>
<ul>
<li>coodinator 生成新的数据版本，及 vector clock 分量</li>
<li>本地保存新数据</li>
<li>向 preference list 中的所有节点发送写入请求</li>
<li>收到 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.7667em;vertical-align:-0.0833em;"></span><span class="mord mathnormal" style="margin-right:0.13889em;">W</span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">−</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:0.6444em;"></span><span class="mord">1</span></span></span></span> 个确认后向用户返回成功</li>
</ul>
<p>Get 流程：</p>
<ul>
<li>coodinator 向 preference list 中所有节点请求数据版本</li>
<li>得到 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.7667em;vertical-align:-0.0833em;"></span><span class="mord mathnormal" style="margin-right:0.00773em;">R</span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">−</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:0.6444em;"></span><span class="mord">1</span></span></span></span> 个答复</li>
<li>coodinator 通过 vector clock 处理有因果关系的数据版本</li>
<li>将没有因果关系的所有数据版本返回给用户</li>
</ul>
<h3 id="_3-6-故障处理-handling-failures-hinted-handoff" tabindex="-1"><a class="header-anchor" href="#_3-6-故障处理-handling-failures-hinted-handoff" aria-hidden="true">#</a> 3.6 故障处理（Handling Failures: Hinted Handoff）</h3>
<p>Dynamo 中用 <strong>hinted handoff</strong> 的方式保证在出现暂时的节点或网络故障时，集群依然可以正常提供服务。</p>
<p>流程：</p>
<img src="/img/in-post/2018-11-30/replication.png" width="400px" alt="replication" />
<ul>
<li><span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span> 失败，然后本来在 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span> 上的一个分片将被发送到下一个本来没有该分片的节点 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.02778em;">D</span></span></span></span>。发送到 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.02778em;">D</span></span></span></span> 的分片在其原数据中将有一个暗示，表明哪个节点才是在分片的预期接收者（节点 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span>）；</li>
<li><span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.02778em;">D</span></span></span></span> 将数据保存在一个单独的本地存储中，并成为该分片的处理节点，同时不断检测原节点；</li>
<li>检测到 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span> 可用时，<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.02778em;">D</span></span></span></span> 会尝试发送分片到 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span>，发送成功后 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.02778em;">D</span></span></span></span> 将删除本地分片。</li>
</ul>
<p>优点：确保读取和写入操作不会因为节点临时或网络故障而失败</p>
<p>由于一个 key 的 preference list 的构造可以是基于跨多个数据中心的节点的，这种跨多个数据中心的复制方案甚至能够处理整个数据中心都故障的情况。</p>
<p>某些情况下仍然会导致数据丢失：如果 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.02778em;">D</span></span></span></span> 发现 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span> 重新上线了，会将本应该属于 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal">A</span></span></span></span> 的分片传送回去，如果这期间 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.02778em;">D</span></span></span></span> 发生故障，该分片就会丢失。</p>
<h3 id="_3-7-副本同步-handling-permanent-failures-replica-synchronization" tabindex="-1"><a class="header-anchor" href="#_3-7-副本同步-handling-permanent-failures-replica-synchronization" aria-hidden="true">#</a> 3.7 副本同步（Handling permanent failures: Replica synchronization）</h3>
<p>当故障发生或者有新节点加入、离开集群时，都涉及分片的拷贝和传输。因此希望能够快速检查分片中内容是否相同，并通过仅发送不同的部分来减少数据传输量。Dynamo 采用 <strong>Merkle Tree</strong> 来解决这个问题。</p>
<p>Merkle tree 是一个哈希树（hash tree）：</p>
<ul>
<li>每个叶子节点对应一个数据项，并记录其哈希值</li>
<li>每个非叶子节点记录其所有子节点的哈希值</li>
</ul>
<p>使用：</p>
<img src="/img/in-post/2018-11-30/replication.png" width="400px" alt="replication" />
<ul>
<li>
<p>每个节点为它维护的每一个分片维护一个 Merkle Tree</p>
<p>例如：<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.02778em;">D</span></span></span></span> 维护 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.7667em;vertical-align:-0.0833em;"></span><span class="mord mathnormal">A</span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">−</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.05017em;">B</span></span></span></span>、<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.7667em;vertical-align:-0.0833em;"></span><span class="mord mathnormal" style="margin-right:0.05017em;">B</span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">−</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.07153em;">C</span></span></span></span>、<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.7667em;vertical-align:-0.0833em;"></span><span class="mord mathnormal" style="margin-right:0.07153em;">C</span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">−</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:0.6833em;"></span><span class="mord mathnormal" style="margin-right:0.02778em;">D</span></span></span></span> 上的分片，所以要维护 3 个 Merkle Tree。</p>
<p>当两个节点的 Merkle Tree 进行比较时，只针对共同承载的分片的 Merkle Tree 进行比较，也就是对副本进行比较。</p>
</li>
<li>
<p>需要比较分片是否相同时，自根向下的比较两个 Merkle Tree 的对应节点，可以快速发现并定位差异所在。</p>
<ol>
<li>从根节点开始；</li>
<li>源节点将 Merkle Tree 当前层的哈希列表传递给其他有该副本的节点（目标节点）；</li>
<li>目标节点接受到这个列表以后，就把这个列表与本身 Merkle Tree 的同一层的哈希列表做比较。如果都相同，说明不用同步。如果不同，找出哈希值不同的子树，并向原节点发出请求，告诉原节点哪些子树是不同的；</li>
<li>重复 2、3 步骤，直到找到叶子节点。这样就可以找出哪些分片是不同的；</li>
<li>源节点将不同的 key 的值传给目标节点。</li>
</ol>
</li>
</ul>
<p>优点：</p>
<ul>
<li>时间：Merkle Tree 利用树形结构迅速定位到不同的分片，时间复杂度为 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:1em;vertical-align:-0.25em;"></span><span class="mord mathnormal" style="margin-right:0.02778em;">O</span><span class="mopen">(</span><span class="mop">l<span style="margin-right:0.01389em;">g</span></span><span class="mspace" style="margin-right:0.1667em;"></span><span class="mord mathnormal">n</span><span class="mclose">)</span></span></span></span></li>
<li>空间：在分布式情况下，空间可以理解为相应的网络传输数据量。Merkle Tree 不必传输所有的节点，只需传输不同的节点。</li>
</ul>
<h3 id="_3-8-成员信息及故障检测-membership-and-failure-detection" tabindex="-1"><a class="header-anchor" href="#_3-8-成员信息及故障检测-membership-and-failure-detection" aria-hidden="true">#</a> 3.8 成员信息及故障检测（Membership and Failure Detection）</h3>
<p>考虑到节点失败无法恢复的情况并不常见，Dynamo 加入或离开集群都需要手动通过命令完成。</p>
<ul>
<li>当有用户请求时，coordinator 会发现不可达的节点，并用其他节点代替之，之后开始周期性探测其是否恢复；</li>
<li>Dynamo 集群中的每台机器都会维护当前集群的成员及节点不可达等信息，这些信息通过 gossip 协议广播到整个集群；</li>
<li>客户端可以通过任意一个节点获得并维护这种成员信息，从而精确的找到自己要访问的数据所在。</li>
</ul>
</div></template>
