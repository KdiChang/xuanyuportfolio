<template><div><p>“虽然研究进度堪忧，但鱼还是要摸的”，在这样的理念的驱动下，菜鸡最终折腾出了一个目前看上去还算可以的方案。</p>
<!-- more -->
<p>我的博客最初 fork 自 <a href="https://github.com/Huxpro/huxpro.github.io" target="_blank" rel="noopener noreferrer">Huxpro/huxpro.github.io</a>，用了一段时间之后开始瞎改，把别人干净的代码改得乱七八糟。博客用的是 Jekyll 框架，而 Jekyll 就是 Github Pages 的默认引擎，所以在部署的时候 Github Pages 连 build 这一步都帮你省了。于是在很长一段时间内，作为一只懒惰的菜鸡，我并没有什么动力来折腾这些东西。而现在之所以要折腾，是因为不折腾的确不行了。</p>
<div class="custom-container warning"><svg viewBox="0 0 25 25" xmlns="http://www.w3.org/2000/svg" xml:space="preserve" style="fill-rule:evenodd;clip-rule:evenodd;stroke-linejoin:round;stroke-miterlimit:2"><path d="M297.65 123.32C133.27 123.32 0 256.58 0 421c0 164.42 133.27 297.6 297.65 297.6 164.38 0 297.64-133.26 297.64-297.6 0-164.34-133.29-297.68-297.64-297.68Zm-.6 483.32c-22.591 0-41.18-18.589-41.18-41.18s18.589-41.18 41.18-41.18 41.18 18.589 41.18 41.18-18.589 41.18-41.18 41.18Zm42.38-164.27h-.23c-.139 22.489-18.681 40.916-41.17 40.916-22.586 0-41.171-18.585-41.171-41.171l.001-.255c0-1 .05-1.93.11-2.88V276.73h.03v-.27a45.8 45.8 0 0 1-.005-.642c0-22.59 18.589-41.18 41.18-41.18s41.18 18.59 41.18 41.18c0 .214-.002.428-.005.642v.27h.06l.02 165.64Z" style="fill:#f3cc2e;fill-rule:nonzero" transform="matrix(.042 0 0 .042 0 -5.18)"/></svg><p class="custom-container-title">WARNING</p>
<p>不过这版博客已经是用 VuePress 重写过后的版本了，所以这篇文章中的某些部分已经并不适用了...</p>
</div>
<h2 id="奇怪的起因" tabindex="-1"><a class="header-anchor" href="#奇怪的起因" aria-hidden="true">#</a> 奇怪的起因</h2>
<p>说起来这还是一个漫长的故事。首先我的博客的公式渲染引擎用的是 <a href="https://katex.org/" target="_blank" rel="noopener noreferrer">Katex</a>，因为它快，比 Mathjax 快多了（可以参考<a href="https://katex.org/" target="_blank" rel="noopener noreferrer">这里</a>）。</p>
<p>kramdown 的默认数学公式渲染引擎是 Mathjax，而从 <code v-pre>v2.0.0</code> 开始 kramdown 似乎引入了一些插件来支持 Katex，比如 <a href="https://github.com/kramdown/math-katex" target="_blank" rel="noopener noreferrer">kramdown-math-katex</a>。那么问题来了，Github Pages 的 Jekyll 是不支持除了<a href="https://pages.github.com/versions/" target="_blank" rel="noopener noreferrer">这些插件</a>以外的插件的。那么唯一的办法就只有在本地 build 网站，然后把 build 好的文件 push 到 <code v-pre>gh-pages</code> 分支上去。</p>
<p>不行，这样多麻烦，Jekyll 的天生优势不就没了吗，不然我为啥不用 Hexo 或者 Hugo，它们还比 Jekyll 快，不行不行。</p>
<p>那个时候还没有 Github Actions 这种东西，我又完全不知道别的持续集成方案，就算知道了估计也不想去折腾。于是我在<a href="https://xuc.me/blog/katex-and-jekyll/" target="_blank" rel="noopener noreferrer">这里</a>看到了一个比较 hack 的方法，首先让 kramdown 以为我们要用 Mathjax：</p>
<div class="language-yaml ext-yml"><pre v-pre class="language-yaml"><code><span class="token key atrule">markdown</span><span class="token punctuation">:</span> kramdown    
<span class="token key atrule">kramdown</span><span class="token punctuation">:</span>
  <span class="token key atrule">math_engine</span><span class="token punctuation">:</span> mathjax
</code></pre><div class="highlight-lines"><br><br><div class="highlight-line">&nbsp;</div></div></div><p>这样 kramdown 就会把像 <code v-pre>$$a + b$$</code> 这样的公式转换成 Mathjax 能识别的 HTML 形式：</p>
<div class="language-javascript ext-js"><pre v-pre class="language-javascript"><code><span class="token operator">&lt;</span>script type<span class="token operator">=</span><span class="token string">"math/tex"</span><span class="token operator">></span>a <span class="token operator">+</span> b<span class="token operator">&lt;</span><span class="token operator">/</span>script<span class="token operator">></span>
</code></pre></div><p>然后通过 JavaScript 把 <code v-pre>&lt;script type=&quot;math/tex&quot;&gt;</code> 标签里的东西用 Katex 渲染出来：</p>
<div class="language-javascript ext-js"><pre v-pre class="language-javascript"><code><span class="token function">$</span><span class="token punctuation">(</span><span class="token string">"script[type='math/tex']"</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">replaceWith</span><span class="token punctuation">(</span><span class="token keyword">function</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">var</span> tex <span class="token operator">=</span> <span class="token function">$</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">text</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">return</span> katex<span class="token punctuation">.</span><span class="token function">renderToString</span><span class="token punctuation">(</span>tex<span class="token punctuation">,</span> <span class="token punctuation">{</span>
    <span class="token literal-property property">displayMode</span><span class="token operator">:</span> <span class="token boolean">false</span>
  <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token function">$</span><span class="token punctuation">(</span><span class="token string">"script[type='math/tex; mode=display']"</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">replaceWith</span><span class="token punctuation">(</span><span class="token keyword">function</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token keyword">var</span> tex <span class="token operator">=</span> <span class="token function">$</span><span class="token punctuation">(</span><span class="token keyword">this</span><span class="token punctuation">)</span><span class="token punctuation">.</span><span class="token function">html</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">return</span> katex<span class="token punctuation">.</span><span class="token function">renderToString</span><span class="token punctuation">(</span>tex<span class="token punctuation">.</span><span class="token function">replace</span><span class="token punctuation">(</span><span class="token regex"><span class="token regex-delimiter">/</span><span class="token regex-source language-regex">%.*</span><span class="token regex-delimiter">/</span><span class="token regex-flags">g</span></span><span class="token punctuation">,</span> <span class="token string">''</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token punctuation">{</span>
    <span class="token literal-property property">displayMode</span><span class="token operator">:</span> <span class="token boolean">true</span>
  <span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre></div><p>虽然看上去不太优雅，但好歹能用，本菜鸡的理念是能用就行，于是就这样过了一段时间。直到几个月前，我发现我博客的公式不对劲了，它们突然都变成了这样：<code v-pre>\(a + b\)</code>。</p>
<p>我一脸懵逼，第一反应是我是不是又把博客哪儿的代码搞崩了，但想了想我似乎也没有动过公式那部分的代码？而且当时我部署在 Coding Pages 上的博客公式还是正常的，那么大概率就是 Github Pages 哪里不对劲了。</p>
<p>搜了一下发现 Github Pages 把 kramdown 的版本更新到了 <code v-pre>v2.2.0</code>，从这一版开始，为了兼容 Mathjax <code v-pre>v3.x</code>，kramdown 会把 inline math 转换成 <code v-pre>\(a + b\)</code> 的形式，把 block math 转换成 <code v-pre>\[a + b\]</code> 的形式（参考<a href="https://github.com/gettalong/kramdown/commit/c3acf8df1db49d2456050f4456f3f542294e2e8f" target="_blank" rel="noopener noreferrer">这个 commit</a>），于是上面那段脚本就坏掉了。</p>
<p>于是一切都回到了最初的起点，摆在面前的办法只剩下把在本地用 kramdown <code v-pre>v2.1.0</code> build 好的网站扔 <code v-pre>gh-pages</code> 上去。这样干了几个月后懒惰的我实在忍不了了，并决定让 Github Actions 来帮我干这件事。</p>
<p>所以我当初为啥不选 Hexo...</p>
<h2 id="github-actions" tabindex="-1"><a class="header-anchor" href="#github-actions" aria-hidden="true">#</a> Github Actions</h2>
<p>在 repo 下建一个目录 <code v-pre>.github/workflow</code>，在这个目录下放一个 <code v-pre>.yaml</code> 格式的 workflow 文件。GitHub 只要发现 <code v-pre>.github/workflows</code> 下有 <code v-pre>.yaml</code> 文件，就会自动运行它们。<a href="https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions" target="_blank" rel="noopener noreferrer">这里</a>是 workflow 文件的详细文档。</p>
<h3 id="jekyll" tabindex="-1"><a class="header-anchor" href="#jekyll" aria-hidden="true">#</a> Jekyll</h3>
<p><a href="https://jekyllrb.com/docs/continuous-integration/github-actions/" target="_blank" rel="noopener noreferrer">Jekyll 官方</a>已经给了一个现成的 <a href="https://github.com/helaili/jekyll-action" target="_blank" rel="noopener noreferrer">action</a>，直接引用它就好：</p>
<div class="language-yaml ext-yml"><pre v-pre class="language-yaml"><code><span class="token key atrule">name</span><span class="token punctuation">:</span> build Jekyll site and deploy it to GitHub Pages

<span class="token comment"># 检测 master 分支上的推送和 pr</span>
<span class="token key atrule">on</span><span class="token punctuation">:</span>
  <span class="token key atrule">push</span><span class="token punctuation">:</span>
    <span class="token key atrule">branches</span><span class="token punctuation">:</span> <span class="token punctuation">[</span> master <span class="token punctuation">]</span>
  <span class="token key atrule">pull_request</span><span class="token punctuation">:</span>
    <span class="token key atrule">branches</span><span class="token punctuation">:</span> <span class="token punctuation">[</span> master <span class="token punctuation">]</span>

<span class="token key atrule">jobs</span><span class="token punctuation">:</span>
  <span class="token key atrule">jekyll-build-and-deploy</span><span class="token punctuation">:</span>
    <span class="token key atrule">runs-on</span><span class="token punctuation">:</span> ubuntu<span class="token punctuation">-</span>latest
    <span class="token key atrule">steps</span><span class="token punctuation">:</span>
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> checkout
        <span class="token key atrule">uses</span><span class="token punctuation">:</span> actions/checkout@v2
      
      <span class="token comment"># 检测 vendor/bundle 下有没有已经安装好的包</span>
      <span class="token comment"># 如果有的话就不用再 bundle install 了，节省时间和资源</span>
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> check cache
        <span class="token key atrule">uses</span><span class="token punctuation">:</span> actions/cache@v1
        <span class="token key atrule">with</span><span class="token punctuation">:</span>
          <span class="token key atrule">path</span><span class="token punctuation">:</span> vendor/bundle
          <span class="token key atrule">key</span><span class="token punctuation">:</span> runner.os−gems−<span class="token punctuation">{</span>%raw%<span class="token punctuation">}</span><span class="token punctuation">{</span><span class="token punctuation">{</span> hashFiles('<span class="token important">**/Gemfile.lock')</span> <span class="token punctuation">}</span><span class="token punctuation">}</span><span class="token punctuation">{</span>% endraw %<span class="token punctuation">}</span>
          <span class="token key atrule">restore-keys</span><span class="token punctuation">:</span> <span class="token punctuation">|</span><span class="token scalar string">
            ${{ runner.os }}-gems-</span>
      
      <span class="token comment"># 引用 helaili/jekyll-action 来打包 Jekyll 网站</span>
      <span class="token comment"># 并把打包好的文件推到同一个 repo 的 gh-pages 分支</span>
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> build and deploy
        <span class="token key atrule">uses</span><span class="token punctuation">:</span> helaili/jekyll<span class="token punctuation">-</span>action@2.0.4
        <span class="token key atrule">env</span><span class="token punctuation">:</span>
          <span class="token key atrule">JEKYLL_PAT</span><span class="token punctuation">:</span> $<span class="token punctuation">{</span><span class="token punctuation">{</span> secrets.GITHUB_TOKEN <span class="token punctuation">}</span><span class="token punctuation">}</span>
        <span class="token key atrule">with</span><span class="token punctuation">:</span>
          <span class="token key atrule">target_branch</span><span class="token punctuation">:</span> <span class="token string">'gh-pages'</span>
</code></pre></div><p>相当于这个 workflow 会自动检测 <code v-pre>master</code> 分支上的 push 和 pull_request，一旦检测就到准备环境，然后运行 <code v-pre>bundle exec jekyll build</code> 打包网站，并把打包产物扔 <code v-pre>gh-pages</code> 分支上去。</p>
<p>需要注意的是必须得有一个 <code v-pre>Gemfile</code> 和 <code v-pre>Gemfile.lock</code> 文件，<code v-pre>Gemfile</code> 里面写上要装的包，比如本博客的 <code v-pre>Gemfile</code> 长这样：</p>
<div class="language-ruby ext-rb"><pre v-pre class="language-ruby"><code>source <span class="token string-literal"><span class="token string">'https://rubygems.org'</span></span>

gem <span class="token string-literal"><span class="token string">'jekyll'</span></span><span class="token punctuation">,</span> <span class="token string-literal"><span class="token string">'~> 4.0'</span></span>
gem <span class="token string-literal"><span class="token string">'kramdown'</span></span><span class="token punctuation">,</span> <span class="token string-literal"><span class="token string">'= 2.1.0'</span></span>  <span class="token comment"># 这里为了 Katex 把 kramdown 版本固定在了 2.1.0</span>
gem <span class="token string-literal"><span class="token string">'jemoji'</span></span><span class="token punctuation">,</span> <span class="token string-literal"><span class="token string">'~> 0.11.1'</span></span>
gem <span class="token string-literal"><span class="token string">'jekyll-paginate'</span></span><span class="token punctuation">,</span> <span class="token string-literal"><span class="token string">'~> 1.1.0'</span></span>
</code></pre></div><p>如果用了自定义域名，那就还需要在 <code v-pre>master</code> 分支放一个 <code v-pre>CNAME</code>，这样 <a href="https://github.com/helaili/jekyll-action" target="_blank" rel="noopener noreferrer">helaili/jekyll-action</a> 就会把 <code v-pre>CNAME</code> 也推到 <code v-pre>gh-pages</code> 分支去。直接在 <code v-pre>gh-pages</code> 分支加 <code v-pre>CNAME</code> 是没有什么用的，因为下次自动推送时它就会被清掉...</p>
<h3 id="node-js" tabindex="-1"><a class="header-anchor" href="#node-js" aria-hidden="true">#</a> Node.js</h3>
<p>所有能用 Node.js 搞定的东西（比如这个基于 VuePress 的博客）都能用以下工作流处理：</p>
<h4 id="npm" tabindex="-1"><a class="header-anchor" href="#npm" aria-hidden="true">#</a> npm</h4>
<div class="language-yaml ext-yml"><pre v-pre class="language-yaml"><code><span class="token key atrule">name</span><span class="token punctuation">:</span> build and deploy

<span class="token comment"># 检测 master 分支上的推送和 pr</span>
<span class="token key atrule">on</span><span class="token punctuation">:</span>
  <span class="token key atrule">push</span><span class="token punctuation">:</span>
    <span class="token key atrule">branches</span><span class="token punctuation">:</span> <span class="token punctuation">[</span> master <span class="token punctuation">]</span>
  <span class="token key atrule">pull_request</span><span class="token punctuation">:</span>
    <span class="token key atrule">branches</span><span class="token punctuation">:</span> <span class="token punctuation">[</span> master <span class="token punctuation">]</span>

<span class="token key atrule">jobs</span><span class="token punctuation">:</span>
  <span class="token key atrule">build-and-deploy-vuepress</span><span class="token punctuation">:</span>
    <span class="token key atrule">runs-on</span><span class="token punctuation">:</span> ubuntu<span class="token punctuation">-</span>latest

    <span class="token key atrule">steps</span><span class="token punctuation">:</span>
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> Checkout
        <span class="token key atrule">uses</span><span class="token punctuation">:</span> actions/checkout@v2

      <span class="token comment"># Node.js 环境</span>
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> Setup Node
        <span class="token key atrule">uses</span><span class="token punctuation">:</span> actions/setup<span class="token punctuation">-</span>node@v2.1.2
        <span class="token key atrule">with</span><span class="token punctuation">:</span>
          <span class="token key atrule">node-version</span><span class="token punctuation">:</span> <span class="token string">'12.x'</span>

      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> Cache dependencies
        <span class="token key atrule">uses</span><span class="token punctuation">:</span> actions/cache@v2
        <span class="token key atrule">with</span><span class="token punctuation">:</span>
          <span class="token key atrule">path</span><span class="token punctuation">:</span> ~/.npm
          <span class="token key atrule">key</span><span class="token punctuation">:</span> $<span class="token punctuation">{</span><span class="token punctuation">{</span> runner.os <span class="token punctuation">}</span><span class="token punctuation">}</span><span class="token punctuation">-</span>node<span class="token punctuation">-</span>$<span class="token punctuation">{</span><span class="token punctuation">{</span> hashFiles('<span class="token important">**/package-lock.json')</span> <span class="token punctuation">}</span><span class="token punctuation">}</span>
          <span class="token key atrule">restore-keys</span><span class="token punctuation">:</span> <span class="token punctuation">|</span><span class="token scalar string">
            ${{ runner.os }}-node-</span>
      
      <span class="token comment"># npm run build</span>
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> Build
        <span class="token key atrule">run</span><span class="token punctuation">:</span> <span class="token punctuation">|</span><span class="token scalar string">
          npm ci
          npm run build</span>
      
      <span class="token comment"># 推送到同一个 repo 的 gh-pages 分支</span>
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> Deploy
        <span class="token key atrule">uses</span><span class="token punctuation">:</span> peaceiris/actions<span class="token punctuation">-</span>gh<span class="token punctuation">-</span>pages@v3
        <span class="token key atrule">with</span><span class="token punctuation">:</span>
          <span class="token key atrule">github_token</span><span class="token punctuation">:</span> $<span class="token punctuation">{</span><span class="token punctuation">{</span> secrets.GITHUB_TOKEN <span class="token punctuation">}</span><span class="token punctuation">}</span>
          <span class="token key atrule">publish_dir</span><span class="token punctuation">:</span> dist <span class="token comment"># build 输出文件夹</span>
          <span class="token key atrule">cname</span><span class="token punctuation">:</span> zxh.io  <span class="token comment"># 如果用了自定义域名，在这里设置</span>
</code></pre></div><p>其中 <a href="https://github.com/peaceiris/actions-gh-pages" target="_blank" rel="noopener noreferrer">peaceiris/actions-gh-pages</a> 也是一个别人写好的 action，能把指定路径的文件推到 <code v-pre>gh-pages</code> 分支。</p>
<h4 id="yarn" tabindex="-1"><a class="header-anchor" href="#yarn" aria-hidden="true">#</a> yarn</h4>
<p>如果包管理工具用的 <code v-pre>yarn</code>：</p>
<div class="language-yaml ext-yml"><pre v-pre class="language-yaml"><code><span class="token key atrule">name</span><span class="token punctuation">:</span> build and deploy

<span class="token key atrule">on</span><span class="token punctuation">:</span>
  <span class="token key atrule">push</span><span class="token punctuation">:</span>
    <span class="token key atrule">branches</span><span class="token punctuation">:</span> <span class="token punctuation">[</span> master <span class="token punctuation">]</span>
  <span class="token key atrule">pull_request</span><span class="token punctuation">:</span>
    <span class="token key atrule">branches</span><span class="token punctuation">:</span> <span class="token punctuation">[</span> master <span class="token punctuation">]</span>

<span class="token key atrule">jobs</span><span class="token punctuation">:</span>
  <span class="token key atrule">build-and-deploy-vuepress</span><span class="token punctuation">:</span>
    <span class="token key atrule">runs-on</span><span class="token punctuation">:</span> ubuntu<span class="token punctuation">-</span>latest

    <span class="token key atrule">steps</span><span class="token punctuation">:</span>
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> Checkout
        <span class="token key atrule">uses</span><span class="token punctuation">:</span> actions/checkout@v2

      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> Setup Node
        <span class="token key atrule">uses</span><span class="token punctuation">:</span> actions/setup<span class="token punctuation">-</span>node@v2.1.2
        <span class="token key atrule">with</span><span class="token punctuation">:</span>
          <span class="token key atrule">node-version</span><span class="token punctuation">:</span> <span class="token string">'12'</span>

      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> Get yarn cache
        <span class="token key atrule">id</span><span class="token punctuation">:</span> yarn<span class="token punctuation">-</span>cache
        <span class="token key atrule">run</span><span class="token punctuation">:</span> echo "<span class="token punctuation">:</span><span class="token punctuation">:</span>set<span class="token punctuation">-</span>output name=dir<span class="token punctuation">:</span><span class="token punctuation">:</span>$(yarn cache dir)"

      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> Cache dependencies
        <span class="token key atrule">uses</span><span class="token punctuation">:</span> actions/cache@v2
        <span class="token key atrule">with</span><span class="token punctuation">:</span>
          <span class="token key atrule">path</span><span class="token punctuation">:</span> $<span class="token punctuation">{</span><span class="token punctuation">{</span> steps.yarn<span class="token punctuation">-</span>cache.outputs.dir <span class="token punctuation">}</span><span class="token punctuation">}</span>
          <span class="token key atrule">key</span><span class="token punctuation">:</span> $<span class="token punctuation">{</span><span class="token punctuation">{</span> runner.os <span class="token punctuation">}</span><span class="token punctuation">}</span><span class="token punctuation">-</span>yarn<span class="token punctuation">-</span>$<span class="token punctuation">{</span><span class="token punctuation">{</span> hashFiles('<span class="token important">**/yarn.lock')</span> <span class="token punctuation">}</span><span class="token punctuation">}</span>
          <span class="token key atrule">restore-keys</span><span class="token punctuation">:</span> <span class="token punctuation">|</span><span class="token scalar string">
            ${{ runner.os }}-yarn-</span>
      
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> Build
        <span class="token key atrule">run</span><span class="token punctuation">:</span> <span class="token punctuation">|</span><span class="token scalar string">
          yarn install --frozen-lockfile
          yarn build</span>
      
      <span class="token punctuation">-</span> <span class="token key atrule">name</span><span class="token punctuation">:</span> Deploy
        <span class="token key atrule">uses</span><span class="token punctuation">:</span> peaceiris/actions<span class="token punctuation">-</span>gh<span class="token punctuation">-</span>pages@v3
        <span class="token key atrule">with</span><span class="token punctuation">:</span>
          <span class="token key atrule">github_token</span><span class="token punctuation">:</span> $<span class="token punctuation">{</span><span class="token punctuation">{</span> secrets.GITHUB_TOKEN <span class="token punctuation">}</span><span class="token punctuation">}</span>
          <span class="token key atrule">publish_dir</span><span class="token punctuation">:</span> dist
          <span class="token key atrule">cname</span><span class="token punctuation">:</span> zxh.io
</code></pre></div><h2 id="优化" tabindex="-1"><a class="header-anchor" href="#优化" aria-hidden="true">#</a> 优化</h2>
<h3 id="减少依赖" tabindex="-1"><a class="header-anchor" href="#减少依赖" aria-hidden="true">#</a> 减少依赖</h3>
<p>通过砍掉一些觉得用不太上的功能，和直接手写一些比较简单的功能，去掉了一些依赖。但我已经忘了去掉了哪些了...</p>
<h3 id="合并依赖" tabindex="-1"><a class="header-anchor" href="#合并依赖" aria-hidden="true">#</a> 合并依赖</h3>
<p>为了减少请求次数，可以尽量把多个 JS 或 CSS 文件合并压缩成一个。上 Gulp 之类的工具当然也可以，不过 jsDelivr 自带了合并功能，比如要合并以下两个库：</p>
<ul>
<li><a href="https://cdn.jsdelivr.net/npm/chart.js@2.9.3/dist/Chart.min.js" target="_blank" rel="noopener noreferrer">https://cdn.jsdelivr.net/npm/chart.js@2.9.3/dist/Chart.min.js</a></li>
<li><a href="https://cdn.jsdelivr.net/npm/mermaid@8.4.8/dist/mermaid.min.js" target="_blank" rel="noopener noreferrer">https://cdn.jsdelivr.net/npm/mermaid@8.4.8/dist/mermaid.min.js</a></li>
</ul>
<p>只需要：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>https://cdn.jsdelivr.net/combine/npm/chart.js@2.9.3/dist/Chart.min.js,npm/mermaid@8.4.8/dist/mermaid.min.js
</code></pre></div><p>于是我把除了 Katex 和 Mathjax（因为这俩涉及到引用字体的问题）以外的文件都合并成了一个 <code v-pre>lib.min.js</code>。</p>
<h3 id="cdn" tabindex="-1"><a class="header-anchor" href="#cdn" aria-hidden="true">#</a> CDN</h3>
<p>又一次喜闻乐见的白嫖 jsDelivr，对于托管在 Github 上的博客来说，在静态资源路径前加上以下 URL 即可：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>https://cdn.jsdelivr.net/gh/{{ 用户名 }}/{{ 仓库名 }}@{{ 分支名 }}
</code></pre></div><p>比如：</p>
<div class="language-text ext-text"><pre v-pre class="language-text"><code>https://cdn.jsdelivr.net/gh/Renovamen/renovamen.github.io@master/js/lib.min.js
</code></pre></div><h2 id="vercel" tabindex="-1"><a class="header-anchor" href="#vercel" aria-hidden="true">#</a> Vercel</h2>
<p>我一直以来都在听说双线部署甚至多线部署这种提高访问速度的操作，但一直没有去折腾。因为 Github Pages 虽然境内访问慢了点，但至少还是能访问的，那么能用就行了。直到几个月前，我发现 Github Pages 境内完全访问不上了...</p>
<p>间歇性抽风想想还是很让人不安，那行吧行吧双线部署我来了...</p>
<p>那时候我所知的有境内节点的静态网站托管服务就 <a href="https://gitee.com/help/articles/4136" target="_blank" rel="noopener noreferrer">Gitee Pages</a> 和 <a href="https://help.coding.net/docs/devops/cd/static-website.html" target="_blank" rel="noopener noreferrer">Coding Pages</a> 俩，而 Gitee Pages 不能免费自定义域名，于是就把境内的线路解析到了 Coding Pages 上。</p>
<p>然而不久之前，新版 Coding Pages 的静态网站合并到腾讯云静态网站，并开始收费了。之前旧版的静态网站还能正常部署和访问，但之后的网站就都得用新版。收费倒是并不贵且也没有什么问题，问题在于如果域名不备案的话，即使是境内的访问，Coding 也会强制给你上境外 CDN 加速，于是速度会很慢，估计比直接访问 Github Pages 还慢。</p>
<p>于是就手忙脚乱的把境内线路的博客扔到了 Vercel 上。只能说 Vercel 至少到目前为止还没发现什么毛病，有境内节点（不过感觉还是没有以前的 Coding Pages 快？），而且是直接自动从 Github 仓库上拉代码然后打包加部署，对菜鸡相当友好。</p>
<p>以前为啥就没发现呢。</p>
</div></template>
