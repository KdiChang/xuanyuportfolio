<template><div><p>对我认为的本科最难的课（2333）第一次实验的流水账式记录。</p>
<p>希望早日脱离大型机的苦海，阿门。</p>
<!-- more -->
<h2 id="_1-创建组" tabindex="-1"><a class="header-anchor" href="#_1-创建组" aria-hidden="true">#</a> 1. 创建组</h2>
<h3 id="_1-1-组的结构" tabindex="-1"><a class="header-anchor" href="#_1-1-组的结构" aria-hidden="true">#</a> 1.1 组的结构</h3>
<p><img src="/img/in-post/2019-04-03/racf-2.1.png" alt=""></p>
<h3 id="_1-2-登陆-tso" tabindex="-1"><a class="header-anchor" href="#_1-2-登陆-tso" aria-hidden="true">#</a> 1.2 登陆 TSO</h3>
<p>以 RACFLAB 组管理员身份登陆TSO：</p>
<ol>
<li>
<p>以 ST016 用户身份登录 TSO，进 ISPF；</p>
</li>
<li>
<p>&quot;P&quot; <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.3669em;"></span><span class="mrel">→</span></span></span></span> &quot;6&quot;（或直接&quot;P.6&quot;）</p>
</li>
<li>
<p>使用 <code v-pre>LU</code> RACF 命令查看 ST016 的属性</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>LU ST016
</code></pre></div></li>
</ol>
<h3 id="_1-3-在-racflab-下定义子组" tabindex="-1"><a class="header-anchor" href="#_1-3-在-racflab-下定义子组" aria-hidden="true">#</a> 1.3 在 RACFLAB 下定义子组</h3>
<p>利用 RACF 命令定义以下子组：</p>
<ul>
<li>
<p>定义 DIV16ADM 用户管理组（相当于公司人事部门），RACF 命令：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDGROUP DIV16ADM OWNER(RACFLAB) SUPGROUP(RACFLAB)
</code></pre></div></li>
<li>
<p>定义 DIV16FUN 功能组（相当于公司各职能部门），后继实验将在该组下定义各个子功能组，RACF 命令：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDGROUP DIV16FUN OWNER(RACFLAB) SUPGROUP(RACFLAB)
</code></pre></div></li>
<li>
<p>定义 DIV16RES 资源组（为有机组织和保护系统资源—包括数据集 / CICS 交易 / 系统和用户程序等资源—而设立的组），后继实验将在该组下定义各个子资源组，RACF 命令：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDGROUP DIV16RES OWNER(RACFLAB) SUPGROUP(RACFLAB)
</code></pre></div></li>
</ul>
<p>利用 RACF 命令查看新建的组进行验证：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>LG DIV16ADM
LG DIV16FUN
LG DIV16RES
</code></pre></div><h3 id="_1-4-在-div16fun-下定义子组-功能组" tabindex="-1"><a class="header-anchor" href="#_1-4-在-div16fun-下定义子组-功能组" aria-hidden="true">#</a> 1.4 在 DIV16FUN 下定义子组（功能组）</h3>
<p>利用 RACF 命令定义以下子组：</p>
<ul>
<li>
<p>定义 FUN16PRD 功能组，该组将用于对生产系统数据集（Production Data Sets）的访问进行集中授权（即如果该组对生产系统数据集有访问权限，该组的成员将自动继承这一权限），RACF 命令：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDGROUP FUN16PRD OWNER(DIV16FUN) SUPGROUP(DIV16FUN)
</code></pre></div></li>
<li>
<p>定义 FUN16TST 功能组，该组将用于对测试系统数据集（Test Data Sets）的访问进行集中授权（即如果该组对测试系统数据集有访问权限，该组的成员将自动继承这一权限），RACF 命令：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDGROUP FUN16TST OWNER(DIV16FUN) SUPGROUP(DIV16FUN)
</code></pre></div></li>
</ul>
<h3 id="_1-5-在-div16res-下定义子组-资源组" tabindex="-1"><a class="header-anchor" href="#_1-5-在-div16res-下定义子组-资源组" aria-hidden="true">#</a> 1.5 在 DIV16RES 下定义子组（资源组）</h3>
<p>利用 RACF 命令定义以下子组（RACF 中数据集 Profile 的 HLQ 必须是 RACF 系统中的一个用户或者组，这里为即将要保护的数据集 RES16PRD.* 和 RES16TST 定义 2 个子组）：</p>
<ul>
<li>
<p>定义 RES16PRD 资源组，该组将用于保护生产系统的数据集。RACF 命令：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDGROUP RES16PRD OWNER(DIV16RES) SUPGROUP(DIV16RES)
</code></pre></div></li>
<li>
<p>定义 RES16TST 资源组，该组将用于保护测试系统的数据集。RACF 命令：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDGROUP RES16TST OWNER(DIV16RES) SUPGROUP(DIV16RES)
</code></pre></div></li>
</ul>
<h3 id="_1-6-查找组-profile" tabindex="-1"><a class="header-anchor" href="#_1-6-查找组-profile" aria-hidden="true">#</a> 1.6 查找组 Profile</h3>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>SEARCH CLASS(GROUP) MASK(DIV16)
</code></pre></div><p> </p>
<h2 id="_2-用户管理" tabindex="-1"><a class="header-anchor" href="#_2-用户管理" aria-hidden="true">#</a> 2. 用户管理</h2>
<p><img src="/img/in-post/2019-04-03/racf-3.1.png" alt=""></p>
<h3 id="_2-1-新建用户" tabindex="-1"><a class="header-anchor" href="#_2-1-新建用户" aria-hidden="true">#</a> 2.1 新建用户</h3>
<ol>
<li>
<p>在不太了解用户需要什么权限的情况下，一般只给出最低权限，利用 RACF 命令完成以下设置：</p>
<ol>
<li>指定用户的默认组为 DIV16ADM</li>
<li>为用户指定初始密码</li>
<li>考虑为用户指定 OWNER</li>
<li>新增以下用户:
<ol>
<li>TSO1601  for user Janet Smith</li>
<li>TSO1602  for user Robert Anderson</li>
<li>TSO1603  for user Leslie Brown</li>
<li>TSO1604  for user Arthur Fielding</li>
<li>TSO1605  for user Susan Johnson</li>
</ol>
</li>
</ol>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDUSER TSO1601 OWNER(DIV16ADM) DFLTGRP(DIV16ADM) PASSWORD(PASS)
# 以此类推
</code></pre></div></li>
<li>
<p>查看用户是否建立成功</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>LU TSO1601
</code></pre></div><p>查看用户的 TSO 段是否有内容</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>LU TSO1601 TSO
</code></pre></div></li>
<li>
<p>如果用户 Profile 没有指定 TSO 段的内容，用户将无法顺利登陆 TSO 系统，需要为用户指定 TSO 段信息</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ALU TSO1601 TSO(PROC(IKJDB2) ACCTNUM(#ACCT) SIZE(4096))
</code></pre></div></li>
<li>
<p>用户登陆 TSO 之后，如果要进入 ISPF 面板系统，首次进入 ISPF 系统时系统会自动为用户建立几个编目的数据集，通常普通用户的文件无法在主机系统主目录（Master Catalog）中进行编目，所以，要实现为新用户创建好用户目录（User Catalog）和别名（Alias）。</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>DEFINE ALIAS(NAME(TSO1601) RELATE('CATALOG.UCAT.STGRP'))
</code></pre></div><p>在 OPTION3.4 中 DSN Level 输入 TSO1601 回车，看是否显示其 &quot;ALIAS&quot;，如果出现类似下面的结果则表明 ALIAS 创建成功：</p>
<div class="language-bash ext-sh"><pre v-pre class="language-bash"><code>DSLIST - Data Sets Matching TSO1601                       Row <span class="token number">1</span> of <span class="token number">3</span>

Command - Enter <span class="token string">"/"</span> to <span class="token keyword">select</span> action           Message        Volume
---------------------------------------------------------------------
TSO1601                                                 *ALIAS
************************ End of Data Set list ***********************
</code></pre></div></li>
</ol>
<h3 id="_2-2-重置用户密码" tabindex="-1"><a class="header-anchor" href="#_2-2-重置用户密码" aria-hidden="true">#</a> 2.2 重置用户密码</h3>
<p>当用户忘记密码的时后需要管理员 ST016 为该用户重新指定一个初始密码。</p>
<p>场景：Janet Smith（TSO1601）遗忘了密码：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ALU TSO1601 PASSWORD(PASS)
</code></pre></div><h3 id="_2-3-revoke-用户" tabindex="-1"><a class="header-anchor" href="#_2-3-revoke-用户" aria-hidden="true">#</a> 2.3 Revoke 用户</h3>
<p>当用户帐号暂时不用的时候，安全起见应该将该帐号挂起（Revoke）。
场景：Arthur Fielding（TSO1604）将会出差一段时间，在这段时间应将该用户的帐号挂起：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ALU TSO1601 REVOKE
</code></pre></div><h3 id="_2-4-resume-用户" tabindex="-1"><a class="header-anchor" href="#_2-4-resume-用户" aria-hidden="true">#</a> 2.4 Resume 用户</h3>
<p>当挂起的用户帐号需要重新启用的时候，应该及时地将帐号 Resume。
场景：Arthur Fielding（TSO1604）出差回来，希望能够继续使用以前的帐号：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ALU TSO1601 RESUME
</code></pre></div><h3 id="_2-5-search-查找" tabindex="-1"><a class="header-anchor" href="#_2-5-search-查找" aria-hidden="true">#</a> 2.5 Search 查找</h3>
<p>使用 Search 命令查找以上新建的用户 Profile</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>SEARCH CLASS(USER) MASK(TSO1601)
</code></pre></div><h3 id="_2-6-将用户关联到组" tabindex="-1"><a class="header-anchor" href="#_2-6-将用户关联到组" aria-hidden="true">#</a> 2.6 将用户关联到组</h3>
<p>RACF 中给用户访问资源权限的最佳方法是将用户关联到可以访问这些资源的组中，这些组称为功能组（Functional Group）。</p>
<ul>
<li>
<p>将用户 Arthur Fielding（TSO1604）连接到组 FUN16PRD，实现其对生产数据集的访问：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>CONNECT TSO1604 GROUP(FUN16PRD)
</code></pre></div></li>
<li>
<p>将用户 Susan Johnson（TSOxx05）连接到组 FUN16TST，实现其对测试数据集的访问：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>CONNECT TSO1605 GROUP(FUN16TST)
</code></pre></div></li>
</ul>
<h3 id="_2-7-验证用户是否关联到组" tabindex="-1"><a class="header-anchor" href="#_2-7-验证用户是否关联到组" aria-hidden="true">#</a> 2.7 验证用户是否关联到组</h3>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>LU TSO1604
LU TSO1605
LG FUN16PRD
LG FUN16TST
</code></pre></div><p> </p>
<h2 id="_3-分散式-racf-安全管理" tabindex="-1"><a class="header-anchor" href="#_3-分散式-racf-安全管理" aria-hidden="true">#</a> 3. 分散式 RACF 安全管理</h2>
<p><strong>目的</strong>：实现 RACF 中的管理权限下放（Delegation）</p>
<p><strong>内容</strong>：新建几个管理员用户，其中一个管理员负责用户安全的管理，一个管理员负 责将用户连接到功能组，另外一个管理员管控制对数据集资源的访问：</p>
<table>
<thead>
<tr>
<th>USER</th>
<th>AUTHORITY</th>
</tr>
</thead>
<tbody>
<tr>
<td>TSO1601</td>
<td>group special for DIV16ADM</td>
</tr>
<tr>
<td>TSO1602</td>
<td>connect authority for FUN16PRD &amp; FUN16TST</td>
</tr>
<tr>
<td>TSO1603</td>
<td>create authority for RES16PRD &amp; RES16TST</td>
</tr>
</tbody>
</table>
<p><img src="/img/in-post/2019-04-03/racf-4.1.png" alt=""></p>
<h3 id="_3-1-用户身份定位" tabindex="-1"><a class="header-anchor" href="#_3-1-用户身份定位" aria-hidden="true">#</a> 3.1 用户身份定位</h3>
<ul>
<li>
<p>TSO1601（Janet Smith）：该管理员将对 DIV16ADM 组用户的安全进行管理，包括为用户重置密码，挂起和启用用户：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>CONNECT TSO1601 GROUP(DIV16ADM) SPECIAL
</code></pre></div></li>
<li>
<p>TSO1602（Robert Anderson）：该管理员可以将用户关联到 DIV16FUN 组下的子功能组中，以实现用户对特定数据的访问权限：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>CONNECT TSO1602 GROUP(FUN16PRD) AUTHORITY(CONNECT)
CONNECT TSO1602 GROUP(FUN16TST) AUTHORITY(CONNECT)
</code></pre></div></li>
<li>
<p>TSO1603（Leslie Brown）：该管理员可以为 RES16PRD 和 RES16TST 组数据集创建数据集 PROFILE，以控制用户对组数据集的访问：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>CONNECT TSO1603 GROUP(RES16PRD) AUTHORITY(CREATE)
CONNECT TSO1603 GROUP(RES16TST) AUTHORITY(CREATE)
</code></pre></div></li>
</ul>
<h3 id="_3-2-测试" tabindex="-1"><a class="header-anchor" href="#_3-2-测试" aria-hidden="true">#</a> 3.2 测试</h3>
<p>测试步骤1的功能是否实现：</p>
<ol>
<li>
<p>以 TSO1601 身份登陆 TSO，尝试修改用户密码等</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ALU TSO1602 PASSWORD(PASS)
</code></pre></div></li>
<li>
<p>以 TSO1602 身份登陆 TSO，将 TSO1601 用户关联到 FUN16PRD 和 FUN16TST：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>CONNECT TSO1601 GROUP(FUN16PRD) 
CONNECT TSO1601 GROUP(FUN16TST)
</code></pre></div></li>
<li>
<p>以 TSOxx02 身份登陆 TSO，将 TSO1601 从 FUN16PRD 和 FUN16TST 组中移走：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>REMOVE TSO1601 GROUP(FUN16PRD)
REMOVE TSO1601 GROUP(FUN16TST)
</code></pre></div></li>
</ol>
<p> </p>
<h2 id="_4-数据集保护-i" tabindex="-1"><a class="header-anchor" href="#_4-数据集保护-i" aria-hidden="true">#</a> 4. 数据集保护 I</h2>
<p><strong>目的</strong>：实现对用户数据集和组数据集的保护。</p>
<p><strong>内容</strong>：首先保护用户数据集，然后对生产数据集和测试数据集进行保护，然后进行授权后的验证。</p>
<p>为了简化实验，RES16PRD 和 RES16TST 组既是 Data Control Group，也是 Resource Onwership Group。</p>
<h3 id="_4-1-保护数据集" tabindex="-1"><a class="header-anchor" href="#_4-1-保护数据集" aria-hidden="true">#</a> 4.1 保护数据集</h3>
<p>保护以下用户的数据集，保护准则：只有用户本身可以访问自己的数据集，其他人都不能访问。（用户的数据集是指以用户名为 HLQ 的所有数据集）</p>
<ul>
<li>
<p>TSO1601  for user Janet Smith</p>
</li>
<li>
<p>TSO1602  for user Robert Anderson</p>
</li>
<li>
<p>TSO1603  for user Leslie Brown</p>
</li>
<li>
<p>TSO1604  for user Arthur Fielding</p>
</li>
<li>
<p>TSO1605  for user Susan Johnson</p>
</li>
</ul>
<p>以 TSO1601 身份登陆 TSO 然后执行 RACF 命令：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDSD 'TSO1601.**' UACC(NONE) 
ADDSD 'TSO1602.**' UACC(NONE) 
ADDSD 'TSO1603.**' UACC(NONE) 
ADDSD 'TSO1604.**' UACC(NONE) 
ADDSD 'TSO1605.**' UACC(NONE) 
</code></pre></div><h3 id="_4-2-查看-profile" tabindex="-1"><a class="header-anchor" href="#_4-2-查看-profile" aria-hidden="true">#</a> 4.2 查看 PROFILE</h3>
<p>查看步骤1创建的用户数据集PROFILE：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>LISTDSD DA('TSO1601.**') ALL
LISTDSD DA('TSO1602.**') ALL
LISTDSD DA('TSO1603.**') ALL
LISTDSD DA('TSO1604.**') ALL
LISTDSD DA('TSO1605.**') ALL
</code></pre></div><h3 id="_4-3-定义-rpofile-赋-alter-权" tabindex="-1"><a class="header-anchor" href="#_4-3-定义-rpofile-赋-alter-权" aria-hidden="true">#</a> 4.3  定义 RPOFILE + 赋 ALTER 权</h3>
<p>定义RES16TST组数据集的RPOFILE。在前面的实验中，TSO1603 被指定为 RES16PRD 和 RES16TST 的 Create 用户，以拥有对这 2 个组的数据集的保护权限。</p>
<ol>
<li>
<p>以 TSO1603 登陆 TSO，对 RES16TST 数据集进行以下保护：</p>
<ul>
<li>
<p>Audit all unsuccessful accesses (Hint: AUDIT)</p>
</li>
<li>
<p>Make the owner TSO1603 (Hint: OWNER)</p>
</li>
<li>
<p>No other users or groups should have access (Hint: UACC)</p>
</li>
</ul>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDSD 'RES16TST.**' AUDIT(FAILURES) OWNER(TSO1603) UACC(NONE)
</code></pre></div><p>修改上面定义的 <code v-pre>RES16TST.**</code> PORFILE的访问列表，给FUN16TST组赋予ALTER访问权限：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>PERMIT 'RES16TST.**' ID(FUN16TST) ACCESS(ALTER)
</code></pre></div></li>
<li>
<p>对 RES16PRD 数据集进行以下保护：</p>
<ul>
<li>
<p>Audit all unsuccessful accesses (AUDIT)</p>
</li>
<li>
<p>Audit successful accesses at UPDATE and higher (AUDIT)</p>
</li>
<li>
<p>Make the owner TSO1603 (OWNER)</p>
</li>
<li>
<p>No other users or groups should have access (UACC)</p>
</li>
</ul>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDSD 'RES16PRD.**' AUDIT(FAILURES SUCCESS(UPDATE)) OWNER(TSO1603) UACC(NONE)
</code></pre></div><p>修改上面定义的 <code v-pre>RESxxPRD.**</code> PORFILE的访问列表，给FUN16PRD组赋予ALTER访问权限：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>PERMIT 'RES16PRD.**' ID(FUN16PRD) ACCESS(ALTER)
</code></pre></div></li>
</ol>
<h3 id="_4-4-验证" tabindex="-1"><a class="header-anchor" href="#_4-4-验证" aria-hidden="true">#</a> 4.4 验证</h3>
<p>确定组数据集 PROFIEL 是否创建并按照预定的要求保护成功</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>LISTDSD DATASET('RES16TST.**') ALL
LISTDSD DATASET('RES16PRD.**') ALL
</code></pre></div><h3 id="_4-5-创建组数据集" tabindex="-1"><a class="header-anchor" href="#_4-5-创建组数据集" aria-hidden="true">#</a> 4.5 创建组数据集</h3>
<p>以 ST016 用户登陆 TSO，创建 RES16TST 和 RES16PRD 组数据集：</p>
<ol>
<li>
<p>创建 ALIAS：RES16TST 和 RES16PRD</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>DEFINE ALIAS(NAME(RES16TST) RELATE(CATALOG.UCAT.STGRP))
DEFINE ALIAS(NAME(RES16PRD) RELATE(CATALOG.UCAT.STGRP))
</code></pre></div></li>
<li>
<p>测试是否成功</p>
<p>在 OPTION 3.4 中 DSN Level 分别输入 RES16TST**、**RES16PRD，回车，看是否显示其 Alias。</p>
</li>
</ol>
<p>以 TSO1603 登陆 TSO 并创建组数据集：</p>
<p>创建一个顺序数据集 RES16PRD.DATA (RECFM=FB, LRECL=80) 和 RES16TST.DATA (RECFM=FB, LRECL=80)</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>                        Allocate New Data Set
Data Set Name  . . . : RESxxPRD.DATA
Management class . . .       (Blank for default management class)
Storage class  . . . .       (Blank for default storage class)
 Volume serial . . . .       (Blank for system default volume) **
 Device type . . . . .       (Generic unit or device address) **
Data class . . . . . .       (Blank for default data class)
 Space units . . . . . TRKS  (BLKS, TRKS, CYLS, KB, MB, BYTES or RECORDS)
 Average record unit         (M, K, or U)
 Primary quantity  . . 1     (In above units)
 Secondary quantity    1     (In above units)
 Directory blocks  . .       (Zero for sequential data set) *
 Record format . . . . FB
 Record length . . . . 80 
 Blocksize   . . . . . 
 Data set name type :        (LIBRARY, HFS, PDS, or blank)  *
                             (YY/MM/DD, YYYY/MM/DD
 Expiration date . . .        YY.DDD, YYYY.DDD in Julian form
Enter "/" to select option    DDDD for retention period in days
   Allocate Multiple Volumes  or blank)

( * Specifying LIBRARY may override zero directory block)

( ** Only one of these fields may be specified)
</code></pre></div><h3 id="_4-6-验证" tabindex="-1"><a class="header-anchor" href="#_4-6-验证" aria-hidden="true">#</a> 4.6 验证</h3>
<ul>
<li>
<p>验证 TSO1604 访问 RES16PRD 组数据集：成功访问；</p>
</li>
<li>
<p>验证 TSO1604 访问 RES16TST 组数据集：拒绝访问；</p>
</li>
<li>
<p>验证 TSO1605 访问 RES16TST 组数据集的保护：成功访问。</p>
</li>
<li>
<p>以 TSO1601 登陆，删除 RES16PRD 打头的数据集（如 <code v-pre>RES16PRD.DATA</code>）</p>
<p>保留 TSO1601 登陆的 Session，再打开一个新的 Session，以 TSO1603 登陆 TSO，修改 <code v-pre>RESxxPRD.**</code> Profile，给 TSO1601 赋 ALTER 权：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>PERMIT 'RES16PRD.**' ID(TSO1601) ACCESS(ALTER) 
</code></pre></div><p>测试：</p>
<ul>
<li>再尝试用 TSO1601 用户删除 <code v-pre>RES16PRD.DATA</code></li>
<li>TSO1601 重新登陆后再尝试删除 <code v-pre>RES16PRD.DATA</code></li>
</ul>
</li>
</ul>
<p> </p>
<h2 id="_5-数据集保护-ii" tabindex="-1"><a class="header-anchor" href="#_5-数据集保护-ii" aria-hidden="true">#</a> 5. 数据集保护 II</h2>
<p>目的：实现对用户数据集和组数据集的保护</p>
<p>内容：创建数据集，确定创建数据集需要的权限，然后建立 Generic PROFILE 对数据集进行保护，最后对 PROFILE 的 Warning 状态进行理解和配置，并使用 LISTDSD 命令确定最佳匹配 PROFILE 和确定 Generic PROFILE 所保护的数据集范围。</p>
<h3 id="_5-1-创建全匹配-profile" tabindex="-1"><a class="header-anchor" href="#_5-1-创建全匹配-profile" aria-hidden="true">#</a> 5.1 创建全匹配 PROFILE</h3>
<p>为 RES16PRD.DATA 创建全匹配 PROFILE。以 TSO1603 登陆（RES16PRD 组 CREATE 特权人员，即数据管理人员），为 <code v-pre>RES16PRD.DATA</code> 创建一个全匹配的 PROFILE 进行保护：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDSD 'RES16PRD.DATA' UACC(READ)
</code></pre></div><h3 id="_5-2-打开-warning-状态" tabindex="-1"><a class="header-anchor" href="#_5-2-打开-warning-状态" aria-hidden="true">#</a> 5.2 打开 Warning 状态</h3>
<p>以 TSO1603 登陆，把 <code v-pre>RES16TST.**</code> PROFILE 的 Warning 状态打开：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ALTDSD 'RES16TST.**' WARNING
</code></pre></div><p>测试：以 TSO1604 登陆，浏览 <code v-pre>RES16TST.DATA</code> 数据集</p>
<h3 id="_5-3-关闭-warning-状态" tabindex="-1"><a class="header-anchor" href="#_5-3-关闭-warning-状态" aria-hidden="true">#</a> 5.3 关闭 Warning 状态</h3>
<p>以 TSO1603 登陆，把 <code v-pre>RES16TST.**</code> PROFILE 的 Warning 状态关闭：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ALTDSD 'RES16TST.**' NOWARNING
</code></pre></div><p>测试：以 TSO1604 登陆，浏览 <code v-pre>RES16TST.DATA</code> 数据集</p>
<h3 id="_5-4-update-权限" tabindex="-1"><a class="header-anchor" href="#_5-4-update-权限" aria-hidden="true">#</a> 5.4 UPDATE 权限</h3>
<p>TSO1603 登陆。假设 <code v-pre>RES16PRD.NEWAPPL.FINANCE.DATA</code> 和 <code v-pre>RES16PRD.NEWAPPL.HR.DATA</code> 是一个新应用系统的 2 个数据集，FUN16TST 组需要对这 2 个数据集有 UPDATE 权限，而不能对其他应用系统的数据集有操作权限。注意，FUN16PRD 组仍然需要对所有的 RES16PRD 数据集保留原有的操作权限。</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDSD 'RES16PRD.NEWAPPL.**' UACC(NONE) FROM('RES16PRD.**')
PERMIT 'RES16PRD.NEWAPPL.**' ID(FUN16TST) ACC(UPDATE)
</code></pre></div><p>检测哪一个 PROFILE 在保护 <code v-pre>RES16PRD.NEWAPPL.FINANCE.DATA</code> 和 <code v-pre>RES16PRD.NEWAPPL.HR.DATA</code>：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>LISTDSD DATASET('RES16PRD.NEWAPPL.FINANCE.DATA') GEN
LISTDSD DATASET('RES16PRD.NEWAPPL.HR.DATA') GEN
</code></pre></div><p>检测一个 Generic PROFILE <code v-pre>RES16PRD.**</code> 保护了那些数据集：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>LISTDSD DATASET('RES16PRD.**') DSNS
</code></pre></div><p> </p>
<h2 id="_6-保护-tso-资源" tabindex="-1"><a class="header-anchor" href="#_6-保护-tso-资源" aria-hidden="true">#</a> 6. 保护 TSO 资源</h2>
<p>目的：授权用户登录 TSO</p>
<p>内容：该实验将首先为 AP（Application Programmer）和 SP（System Programmer）用户建立组结构，然后为 TSO 的登陆过程（TSOPROC）和用户帐号（ACCTNUM建立一些通用资源 PROFILE 进行保护，接着新增 AP/SP 用户 PROFILE，对 TSO 段进行赋值，并对他们授权访问 TSO。</p>
<p>组结构：</p>
<p><img src="/img/in-post/2019-04-03/racf-7.1.png" alt=""></p>
<h3 id="_6-1-创建组结构" tabindex="-1"><a class="header-anchor" href="#_6-1-创建组结构" aria-hidden="true">#</a> 6.1 创建组结构</h3>
<ol>
<li>
<p>在 DIV16FUN 下创建子组 FUN16AP</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDGROUP FUN16AP OWNER(DIV16FUN) SUPGROUP(DIV16FUN)
</code></pre></div></li>
<li>
<p>在 DIV16FUN 下创建子组 FUN16SP</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDGROUP FUN16SP OWNER(DIV16FUN) SUPGROUP(DIV16FUN)
</code></pre></div></li>
<li>
<p>在 DIV16RES 下创建子组 RES16TSO，用以管理 TSO 资源授权</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDGROUP RES16TSO OWNER(DIV16RES) SUPGROUP(DIV16RES)
</code></pre></div></li>
</ol>
<h3 id="_6-2-新增用户" tabindex="-1"><a class="header-anchor" href="#_6-2-新增用户" aria-hidden="true">#</a> 6.2 新增用户</h3>
<p>新增 AP 和 SP 用户，这些用户需要访问TSO</p>
<ol>
<li>
<p>新增 SP 用户 TSO1607，要求如下:</p>
<ul>
<li>
<p>OWNER和默认组应该是DIV16ADM</p>
</li>
<li>
<p>可以登陆TSO</p>
<ol>
<li>账户使用 ACCT#Sxx</li>
<li>登陆过程使用 PROC#Sxx</li>
<li>Region 大小为 4096</li>
</ol>
</li>
</ul>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDUSER TSO1607 OWNER(DIV16ADM) NAME('SYSTEM PROGRAMMER') DFLTGRP(DIV16ADM) PASSWORD(123456)

ALU TSO1607 TSO(PROC(IKJDB2) ACCTNUM(#ACCT) SIZE(4096) COMMAN(ISPF))

CONNECT TSO1607 GROUP(FUN16SP)
</code></pre></div></li>
<li>
<p>新增 AP 用户 TSO1608，要求如下:</p>
<ul>
<li>
<p>OWNER和默认组应该是DIV16ADM</p>
</li>
<li>
<p>可以登陆TSO</p>
<ol>
<li>账户使用 ACCT#Axx</li>
<li>登陆过程使用 PROC#Axx</li>
<li>Region 大小为 4096</li>
</ol>
</li>
</ul>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDUSER TSO1608 OWNER(DIV16ADM) NAME('SYSTEM PROGRAMMER') DFLTGRP(DIV16ADM) PASSWORD(123456)

ALU TSO1608 TSO(PROC(IKJDB2) ACCTNUM(#ACCT) SIZE(4096) COMMAN(ISPF))

CONNECT TSO1608 GROUP(FUN16AP)
</code></pre></div></li>
</ol>
<h3 id="_6-3-创建登陆过程" tabindex="-1"><a class="header-anchor" href="#_6-3-创建登陆过程" aria-hidden="true">#</a> 6.3 创建登陆过程</h3>
<p>为 TSO 用户创建一个新的登陆过程 PROC#Sxx 和 PROC#Axx。打开文件 VENDOR.PROCLIB ，在 Command 中输入：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>S PROC#S16;COPY IKJDB2
S PROC#A16;COPY IKJDB2
</code></pre></div><h3 id="_6-4-保护登录过程" tabindex="-1"><a class="header-anchor" href="#_6-4-保护登录过程" aria-hidden="true">#</a> 6.4 保护登录过程</h3>
<p>保护 PROC#Sxx 登陆过程（TSOPROC类）。</p>
<ol>
<li>
<p>创建通用资源 TSOPROC 的 RPOFILE，保护 AP 和 SP 的 TSO 登陆过程。</p>
<p>授权规则：PROC#Sxx 只有 SP 才能使用（READ 权限），其他人不可以使用；PROC#Axx 只有 AP 才能使用（READ 权限），其他人不可以使用。</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>RDEFINE TSOPROC PROC#S16 OWNER(DIV16FUN) UACC(NONE)
PE PROC#S16 CLASS(TSOPROC) ID(FUN16SP) AC(READ)

RDEFINE TSOPROC PROC#A16 OWNER(DIV16FUN) UACC(NONE)
PE PROC#A16 CLASS(TSOPROC) ID(FUN16AP) AC(READ)
</code></pre></div></li>
<li>
<p>浏览 PROC#Sxx 和 PROC#Axx PROFILE，它们用于保护不同的 TSO 登陆服务：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>RLIST TSOPROC PROC#S16 AUTHUSER
RLIST TSOPROC PROC#A16 AUTHUSER
</code></pre></div></li>
<li>
<p>刷新 TSOPROC 类在内存中的 PROFILE</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>SETROPTS RACLIST(TSOPROC) REFRESH
</code></pre></div><p>可能会提示不需要刷新。</p>
</li>
</ol>
<h3 id="_6-5-保护-acctnum" tabindex="-1"><a class="header-anchor" href="#_6-5-保护-acctnum" aria-hidden="true">#</a> 6.5 保护 ACCTNUM</h3>
<p>创建两个 TSO 账户（ACCTNUM），并创建一个通用资源 RPOFILE 保护该 ACCTNUM。</p>
<ol>
<li>
<p>创建 RPOFILE：ACCT#Sxx 该 ACCTNUM 为 SP 提供 TSO 登陆服务。</p>
<p>授权规则：ACCT#Sxx 只有 SP 才能使用(READ 权限)，其他人不可以使用。</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>RDEFINE ACCTNUM ACCT#S16 OWNER(DIV16FUN) UACC(NONE)
PE ACCT#S16 CLASS(ACCTNUM) ID(FUN16SP) AC(READ)
</code></pre></div></li>
<li>
<p>创建 PROFILE：ACCT#Axx 该 ACCTNUM 为 AP 提供 TSO 登陆服务。</p>
<p>授权规则：ACCT#Axx 只有 AP 才能使用(READ 权限)，其他人不可以使用。</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>RDEFINE ACCTNUM ACCT#A16 OWNER(DIV16FUN) UACC(NONE)
PE ACCT#A16 CLASS(ACCTNUM) ID(FUN16AP) AC(READ)
</code></pre></div></li>
<li>
<p>浏览 PROFILE：ACCT#Sxx 和 ACCT#Axx</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>RLIST ACCTNUM ACCT#S16 AUTHUSER
RLIST ACCTNUM ACCT#A16 AUTHUSER
</code></pre></div></li>
</ol>
<h3 id="_6-6-保护-tsoauth" tabindex="-1"><a class="header-anchor" href="#_6-6-保护-tsoauth" aria-hidden="true">#</a> 6.6 保护 TSOAUTH</h3>
<p>TSOAUTH 通用资源类提供保护 TSO 权限的功能，TSO 权限主要包括：ACCT，JCL，MOUNT， OPER，RECOVER 等。系统已经定义了一个 JCL PROFILE 用于保护 TSO 的 JCL 权限，该权限允许通过 TSO 向 JES 提交 JCL 批量作业。</p>
<ol>
<li>
<p>（不做）为 SP 和 AP 用户赋权访问 JCL 权限：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>PE JCL CLASS(TSOAUTH) ID(FUN16AP FUN16SP) ACCESS(READ)
</code></pre></div></li>
<li>
<p>查看 SP 和 AP 用户是否拥有提交 JCL 作业的权利：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>RLIST TSOAUTH JCL
</code></pre></div></li>
</ol>
<h3 id="_6-7-保护用户数据集" tabindex="-1"><a class="header-anchor" href="#_6-7-保护用户数据集" aria-hidden="true">#</a> 6.7 保护用户数据集</h3>
<ol>
<li>
<p>保护 TSO1607 的用户数据集</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDSD 'TSO1607.**' UACC(NONE)
</code></pre></div></li>
<li>
<p>保护 TSO1608 的用户数据集</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>ADDSD 'TSO1608.**' UACC(NONE)
</code></pre></div></li>
</ol>
<h3 id="_6-8-创建-alias" tabindex="-1"><a class="header-anchor" href="#_6-8-创建-alias" aria-hidden="true">#</a> 6.8 创建 ALIAS</h3>
<p>为 TSOxx07 和 TSOxx08 创建 ALIAS（普通用户不能修改 Master Catalog，所以为了让用户可以创建自己的编目数据集，必须为用户创建 ALIAS，ALIAS 指向 User Catalog）。</p>
<ol>
<li>
<p>为 TSOxx07 创建别名</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>DEFINE ALIAS(NAME(TSO1607) RELATE('CATALOG.UCAT.STGRP'))
</code></pre></div></li>
<li>
<p>为 TSOxx08 创建别名</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>DEFINE ALIAS(NAME(TSO1608) RELATE('CATALOG.UCAT.STGRP'))
</code></pre></div></li>
</ol>
<p> </p>
<h2 id="_7-使用-jcl-执行-racf-命令" tabindex="-1"><a class="header-anchor" href="#_7-使用-jcl-执行-racf-命令" aria-hidden="true">#</a> 7. 使用 JCL 执行 RACF 命令</h2>
<p>编写 JCL 作业，然后 SUBMIT：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>//ST016R1 JOB CLASS=A,MSGLEVEL=(1,1),MSGCLASS=H,
// NOTIFY=ST016
//SEND EXEC PGM=IKJEFT01
//SYSPRINT DD DUMMY
//SYSTSPRT DD SYSOUT=*
//SYSTSIN DD *
SEARCH CLASS(GROUP) MASK(DIV16)
SEARCH CLASS(GROUP) MASK(FUN16)
SEARCH CLASS(GROUP) MASK(RES16)
SEARCH CLASS(USER) MASK(TSO16)
LG FUN16PRD
LG FUN16TST
LG FUN16SP
LG FUN16AP
SEARCH CLASS(DATASET) MASK(TSO16)
SEARCH CLASS(DATASET) MASK(RES16)
LISTDSD DA(TSO1601.**) AU
SEARCH CLASS(TSOPROC) FILTER(PROC#%16)
RLIST TSOPROC PROC#S16 AU
RLIST TSOPROC PROC#A16 AU
SEARCH CLASS(ACCTNUM) FILTER(ACCT#%16)
RLIST ACCTNUM ACCT#S16 AU
RLIST ACCTNUM ACCT#A16 AU
</code></pre></div></div></template>
