<template><div><p>对图像美感评估（Image Aesthetic Assessment）领域的简单调研，到时候大概可以直接复制粘贴进毕业论文里。</p>
<!-- more -->
<h2 id="surveys" tabindex="-1"><a class="header-anchor" href="#surveys" aria-hidden="true">#</a> Surveys</h2>
<ul>
<li>
<p><strong>Algorithmic Inferencing of Aesthetics and Emotion in Natural Images: An Exposition.</strong> <em>Ritendra Datta, Jia Li, and James Z. Wang.</em> ICIP 2008. <a href="https://www.ri.cmu.edu/pub_files/pub4/datta_ritendra_2008_2/datta_ritendra_2008_2.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
</li>
<li>
<p><strong>On Aesthetics and Emotions in Images: A Computational Perspective.</strong> <em>Dhiraj Joshi, et al.</em> IEEE Signal Processing Magazine 2011. <a href="https://ieeexplore.ieee.org/document/5999579" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="https://storage.googleapis.com/pub-tools-public-publication-data/pdf/37213.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
</li>
<li>
<p><strong>Aesthetic Analysis of Images.</strong> 2015. <a href="https://eg.uc.pt/bitstream/10316/35507/1/Aesthetic%20Analysis%20of%20Images%20Intermediate%20report.pdf" target="_blank" rel="noopener noreferrer">[Report]</a></p>
</li>
<li>
<p><strong>Image Aesthetic Assessment: An Experimental Survey.</strong> <em>Yubin Deng, et al.</em> IEEE Signal Processing Magazine 2017. <a href="https://ieeexplore.ieee.org/abstract/document/7974874" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="https://arxiv.org/pdf/1610.00838.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a></p>
</li>
<li>
<p><strong>A Report: Image Aesthetic Assessment.</strong> <em>Chunbiao Zhu.</em> 2018. <a href="https://www.researchgate.net/publication/325184839_A_Report_Image_Aesthetic_Assessment" target="_blank" rel="noopener noreferrer">[Report]</a></p>
</li>
</ul>
<h2 id="datasets" tabindex="-1"><a class="header-anchor" href="#datasets" aria-hidden="true">#</a> Datasets</h2>
<h3 id="photo-net-pn" tabindex="-1"><a class="header-anchor" href="#photo-net-pn" aria-hidden="true">#</a> Photo.net (PN)</h3>
<p><strong>Algorithmic Inferencing of Aesthetics and Emotion in Natural Images: An Exposition.</strong> <em>Ritendra Datta, Jia Li, and James Z. Wang.</em> ICIP 2008. <a href="https://www.ri.cmu.edu/pub_files/pub4/datta_ritendra_2008_2/datta_ritendra_2008_2.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p>共 20,278 张图片（<a href="https://arxiv.org/pdf/1610.00838.pdf" target="_blank" rel="noopener noreferrer">Deng et al. 2017</a>），平均每张图片 12 个评分，评分范围为 0-7，分越高说明图片质量越高。所有图片的平均得分的峰值在分布右侧，说明整体评价偏高。</p>
<p><img src="/img/in-post/2020-02-29/photo-net-distribution.png" alt="photo-net-distribution"></p>
<p class="desc">PN 数据集的图片平均得分分布</p>
<p>有论文认为 PN 数据集中超过 30% 的图片都被其拍摄者 P 上了一个相框以让它更好看一些，这会导致这些图片的评分偏高（<a href="https://ieeexplore.ieee.org/abstract/document/6126444" target="_blank" rel="noopener noreferrer">Marchesotti, et al. 2011</a>）。</p>
<p>下载：需要根据 <a href="http://ritendra.weebly.com/aesthetics-datasets.html" target="_blank" rel="noopener noreferrer">Dataset File</a> 中的索引号去 <a href="http://photo.net" target="_blank" rel="noopener noreferrer">photo.net</a> 上找图片，有的图片已经被从网上移除了。</p>
<h3 id="dpchallenge" tabindex="-1"><a class="header-anchor" href="#dpchallenge" aria-hidden="true">#</a> DPChallenge</h3>
<p><strong>Algorithmic Inferencing of Aesthetics and Emotion in Natural Images: An Exposition.</strong> <em>Ritendra Datta, Jia Li, and James Z. Wang.</em> ICIP 2008. <a href="https://www.ri.cmu.edu/pub_files/pub4/datta_ritendra_2008_2/datta_ritendra_2008_2.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a>（依然是这篇论文）</p>
<p>共 16,509 张图片，平均每张图片 205 个评分，评分范围为 0-10，分越高说明图片质量越高。</p>
<p><img src="/img/in-post/2020-02-29/dpchallenge-distribution.png" alt="dpchallenge-distribution"></p>
<p class="desc">DPChallenge 数据集的图片平均得分分布</p>
<p><strong>现在大概已经可以被 AVA 数据集所取代。</strong></p>
<p>下载：需要根据 <a href="http://ritendra.weebly.com/aesthetics-datasets.html" target="_blank" rel="noopener noreferrer">Dataset File</a> 中的索引号去 <a href="https://www.dpchallenge.com/" target="_blank" rel="noopener noreferrer">dpchallenge.com</a> 上找图片。</p>
<h3 id="cuhk-pq" tabindex="-1"><a class="header-anchor" href="#cuhk-pq" aria-hidden="true">#</a> CUHK-PQ</h3>
<p><strong>Content-based Photo Quality Assessment.</strong> <em>Wei Luo, Xiaogang Wang, and Xiaoou Tang.</em> ICCV 2011. <a href="https://ieeexplore.ieee.org/abstract/document/6126498" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="http://mmlab.ie.cuhk.edu.hk/archive/2011/cvpr11_WLuo_XWang_XTang.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p><strong>Content-based Photo Quality Assessment.</strong> <em>Xiaoou Tang, Wei Luo, and Xiaogang Wang.</em> TMM 2013. <a href="https://ieeexplore.ieee.org/document/6544270" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="http://mmlab.ie.cuhk.edu.hk/archive/2011/cvpr11_WLuo_XWang_XTang.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p>现在有 30,000 左右张图片，每张图片被按内容分为了 7 个类别。美学标注只有“高质量”和“低质量”两个，高低质量图片比例约为 1:3（<a href="https://arxiv.org/pdf/1610.00838.pdf" target="_blank" rel="noopener noreferrer">Deng et al. 2017</a>）。所以该数据集很难被用来进行评分任务的训练，而且分类难度不算大。因为是直接把从专业摄影网站（dpchallenge.com）和从业余摄影师处收集到的图片混在了一起，所以可能不能代表真实场景（<a href="http://refbase.cvc.uab.es/files/MMP2012a.pdf" target="_blank" rel="noopener noreferrer">Murray et al. 2012</a>）。</p>
<p>下载：<a href="http://mmlab.ie.cuhk.edu.hk/archive/CUHKPQ/Dataset.htm" target="_blank" rel="noopener noreferrer"><v-icon name="ri-link-m" scale="0.9"/>  Multimedia Laboratory, CUHK</a></p>
<h3 id="ava" tabindex="-1"><a class="header-anchor" href="#ava" aria-hidden="true">#</a> AVA</h3>
<p><strong>AVA: A Large-Scale Database for Aesthetic Visual Analysis.</strong> <em>Naila Murray, Luca Marchesotti, Florent Perronnin.</em> CVPR 2012. <a href="https://ieeexplore.ieee.org/document/6247954" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="http://refbase.cvc.uab.es/files/MMP2012a.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a> <a href="https://github.com/mtobeiyf/ava_downloader" target="_blank" rel="noopener noreferrer">[Dataset]</a></p>
<p>收集并标注了来源于 dpchallenge.com 的超过 250,000 张图片。带有美学质量标注（Aesthetic annotations）（78～549 个评分，评分范围为 0-10，分越高说明图片质量越高）、语义标注（Semantic annotations）（共 66 种）和摄影风格标注（Photographic style annotations）（共 14 种，大概就是所谓的“美学因素”）。是第一个带有详细标注的大型图像美感评估数据集，也是图像美感评估领域公认的基准数据集。</p>
<p>该论文比较了 AVA 与其他数据集：</p>
<p><img src="/img/in-post/2020-02-29/ava-compare.png" alt="compare"></p>
<p class="desc">AVA 与其他数据集的比较</p>
<p>对于二分类任务，一般认为平均得分高于 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.7278em;vertical-align:-0.0833em;"></span><span class="mord">5</span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">+</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:0.4306em;"></span><span class="mord mathnormal" style="margin-right:0.03588em;">σ</span></span></span></span> 的图片质量高，低于 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.7278em;vertical-align:-0.0833em;"></span><span class="mord">5</span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">+</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:0.4306em;"></span><span class="mord mathnormal" style="margin-right:0.03588em;">σ</span></span></span></span> 的图片质量低。</p>
<p>但 AVA 的很多图片后期处理痕迹太重。且绝大多数图片来自于专业摄影者而非普通人，所以 AVA 有较大的 bias（<a href="https://arxiv.org/pdf/1606.01621.pdf" target="_blank" rel="noopener noreferrer">Kone et al. 2016</a>）。</p>
<p>下载：<a href="https://github.com/mtobeiyf/ava_downloader" target="_blank" rel="noopener noreferrer"><v-icon name="ri-link-m" scale="0.9"/> Github</a></p>
<h3 id="aadb" tabindex="-1"><a class="header-anchor" href="#aadb" aria-hidden="true">#</a> AADB</h3>
<p><strong>Photo Aesthetics Ranking Network with Attributes and Content Adaptation.</strong> <em>Shu Kong, et al.</em> ECCV 2016. <a href="https://arxiv.org/pdf/1606.01621.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a> <a href="https://www.ics.uci.edu/~skong2/aesthetics.html" target="_blank" rel="noopener noreferrer">[Project]</a> <a href="https://github.com/aimerykong/deepImageAestheticsAnalysis" target="_blank" rel="noopener noreferrer">[Code]</a> <a href="https://drive.google.com/open?id=0BxeylfSgpk1MOVduWGxyVlJFUHM" target="_blank" rel="noopener noreferrer">[Dataset &amp; Model]</a></p>
<p>收集并标注了来自 <a href="https://www.flickr.com/" target="_blank" rel="noopener noreferrer">Flickr</a> 的共 10,000 张图片。带有美学质量标注（5 个人评分）和美学因素标注（共 11 种，二分类，算是对 AVA 的补充）。</p>
<p>相比 AVA，AADB 中所有图片都是真实照片，且注意平衡了专业摄影者和普通拍照者的作品数量，且标注了每个评分的标注者 ID（用于消除标不同标注者评价标准的不同所带来的影响）。</p>
<p>该论文还比较了 AADB 与其他数据集：</p>
<p><img src="/img/in-post/2020-02-29/aadb-compare.png" alt="compare"></p>
<p class="desc">AADB 与其他数据集的比较</p>
<p>但 AADB 的数据量过小，且标注人员过少。</p>
<p>下载：<a href="https://drive.google.com/open?id=0BxeylfSgpk1MOVduWGxyVlJFUHM" target="_blank" rel="noopener noreferrer"><v-icon name="ri-link-m" scale="0.9"/> Google Drive</a></p>
<h3 id="arod" tabindex="-1"><a class="header-anchor" href="#arod" aria-hidden="true">#</a> AROD</h3>
<p><strong>Will People Like Your Image? Learning the Aesthetic Space.</strong> <em>Katharina Schwarz, Patrick Wieschollek, and Hendrik P. A. Lensch.</em> WACV 2018. <a href="https://arxiv.org/pdf/1611.05203.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a> <a href="https://github.com/cgtuebingen/will-people-like-your-image" target="_blank" rel="noopener noreferrer">[Code &amp; Dataset]</a></p>
<p>从 <a href="https://www.flickr.com/" target="_blank" rel="noopener noreferrer">Flickr</a> 上爬了 2004 - 2016 年间的超过 380,000 张图片，及他们的浏览量（views）、评论数、被喜欢次数（faves）、图片标题、在网站上的描述等数据。然后定义了图片的美学质量为：</p>
<p><span class="katex-display"><span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:1em;vertical-align:-0.25em;"></span><span class="mord mathnormal" style="margin-right:0.05764em;">S</span><span class="mopen">(</span><span class="mord mathnormal">i</span><span class="mclose">)</span><span class="mspace" style="margin-right:0.2778em;"></span><span class="mrel">∼</span><span class="mspace" style="margin-right:0.2778em;"></span></span><span class="base"><span class="strut" style="height:2.363em;vertical-align:-0.936em;"></span><span class="mord"><span class="mopen nulldelimiter"></span><span class="mfrac"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height:1.427em;"><span style="top:-2.314em;"><span class="pstrut" style="height:3em;"></span><span class="mord"><span class="mop">lo<span style="margin-right:0.01389em;">g</span></span><span class="mspace" style="margin-right:0.1667em;"></span><span class="mord mathnormal" style="margin-right:0.22222em;">V</span><span class="mopen">(</span><span class="mord mathnormal">i</span><span class="mclose">)</span></span></span><span style="top:-3.23em;"><span class="pstrut" style="height:3em;"></span><span class="frac-line" style="border-bottom-width:0.04em;"></span></span><span style="top:-3.677em;"><span class="pstrut" style="height:3em;"></span><span class="mord"><span class="mop">lo<span style="margin-right:0.01389em;">g</span></span><span class="mspace" style="margin-right:0.1667em;"></span><span class="mord mathnormal" style="margin-right:0.13889em;">F</span><span class="mopen">(</span><span class="mord mathnormal">i</span><span class="mclose">)</span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height:0.936em;"><span></span></span></span></span></span><span class="mclose nulldelimiter"></span></span></span></span></span></span></p>
<p>其中 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:1em;vertical-align:-0.25em;"></span><span class="mord mathnormal" style="margin-right:0.22222em;">V</span><span class="mopen">(</span><span class="mord mathnormal">i</span><span class="mclose">)</span></span></span></span> 为每张图片的 views，<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:1em;vertical-align:-0.25em;"></span><span class="mord mathnormal" style="margin-right:0.13889em;">F</span><span class="mopen">(</span><span class="mord mathnormal">i</span><span class="mclose">)</span></span></span></span> 为 faves。</p>
<p>相当于用一种类似于无监督标注的方法在试图解决 AVA 的 bias 太重和 AADB 数据量和标注人员过少的问题。</p>
<p>该论文还比较了 AROD 和 AVA 与 AADB：</p>
<p><img src="/img/in-post/2020-02-29/arod-compare.png" alt="compare"></p>
<p class="desc">AROD 与其他数据集的比较</p>
<p>下载：<a href="https://github.com/cgtuebingen/will-people-like-your-image" target="_blank" rel="noopener noreferrer"><v-icon name="ri-link-m" scale="0.9"/> Github</a>，<code v-pre>arod/list.txt</code> 中给出了图片的 URL，格式为 <code v-pre>url;faves;views</code>。</p>
<h3 id="gpa" tabindex="-1"><a class="header-anchor" href="#gpa" aria-hidden="true">#</a> GPA</h3>
<p><strong>Gourmet Photography Dataset for Aesthetic Assessment of Food Images.</strong> <em>Kekai Sheng, et al.</em> SIGGRAPH 2018. <a href="https://github.com/Openning07/GPA" target="_blank" rel="noopener noreferrer">[Dataset]</a></p>
<p>一个用于食物图片美感评估的数据集。从社交媒体和食物分类数据集中收集了共 24,000 张图片，然后人工标注（二分类）。</p>
<p>下载：<a href="https://github.com/Openning07/GPA" target="_blank" rel="noopener noreferrer"><v-icon name="ri-link-m" scale="0.9"/> Github</a></p>
<h3 id="pccd" tabindex="-1"><a class="header-anchor" href="#pccd" aria-hidden="true">#</a> PCCD</h3>
<p><strong>Aesthetic Critiques Generation for Photos.</strong> <em>Kuang-Yu Chang, Kung-Hung Lu, and Chu-Song Chen.</em> ICCV 2017. <a href="https://ieeexplore.ieee.org/document/8237642" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="https://www.iis.sinica.edu.tw/~kuangyu/iccv17_aesthetic_critiques.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a> <a href="https://github.com/kunghunglu/DeepPhotoCritic-ICCV17" target="_blank" rel="noopener noreferrer">[Code]</a> <a href="https://github.com/ivclab/DeepPhotoCritic-ICCV17" target="_blank" rel="noopener noreferrer">[Dataset]</a></p>
<p>首次在图像美学数据集中加入了语言评论信息。包含了来自 <a href="https://gurushots.com/" target="_blank" rel="noopener noreferrer">GuruShots</a>（一个专业图片评论网站）上的 4235 张图片，和它们的超过 60,000 条评论信息。评论被按角度（如“color &amp; light”、“depth of field”等）分成了 7 类。并且张图片都带有对图片整体和 7 个角度的打分（评分范围为 1-10）。可以说在美学因素的标注上比 AVA 和 AADB 的二分类更详细。</p>
<p>但数据量过少。</p>
<p><s>我并没有找到这个数据集，该论文的 <a href="https://github.com/kunghunglu/DeepPhotoCritic-ICCV17" target="_blank" rel="noopener noreferrer">Github 主页</a>上似乎也没有。</s> 不久前似乎开源了：<a href="https://github.com/ivclab/DeepPhotoCritic-ICCV17" target="_blank" rel="noopener noreferrer"><v-icon name="ri-link-m" scale="0.9"/> Github</a></p>
<p>我也不明白为啥 2017 年的论文 2020 年才开源数据集...</p>
<h3 id="dpc-caption" tabindex="-1"><a class="header-anchor" href="#dpc-caption" aria-hidden="true">#</a> DPC-Caption</h3>
<p><strong>Aesthetic Attributes Assessment of Images.</strong> <em>Xin Jin, et al.</em> ACM MM 2019. <a href="https://arxiv.org/pdf/1907.04983.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a> <a href="https://github.com/BestiVictory/DPC-Captions" target="_blank" rel="noopener noreferrer">[Dataset]</a></p>
<p>把 AVA 中图片的评论都爬了下来，并按角度分成了 6 类，分类的标准基于从 PCCD 中统计出来的信息。比 PCCD 数据量大得多，但没有每个角度的评分，标注要弱一些。</p>
<p>下载：<a href="https://github.com/BestiVictory/DPC-Captions" target="_blank" rel="noopener noreferrer"><v-icon name="ri-link-m" scale="0.9"/> Github</a></p>
<h3 id="其他" tabindex="-1"><a class="header-anchor" href="#其他" aria-hidden="true">#</a> 其他</h3>
<ul>
<li>
<p><strong>Understanding Aesthetics in Photography Using Deep Convolutional neural Networks.</strong> <em>Maciej Suchecki and Tomasz Trzciski.</em> SPA 2017. <a href="https://ieeexplore.ieee.org/abstract/document/8166855" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="http://ii.pw.edu.pl/~ttrzcins/papers/SPA_2017.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p>该论文用的数据集与 AROD 比较类似，从 Flicker 上爬了大概 170 万张图片，定义的美学分数为：</p>
<p><span class="katex-display"><span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.4306em;"></span><span class="mord text"><span class="mord">score</span></span><span class="mspace" style="margin-right:0.2778em;"></span><span class="mrel">=</span><span class="mspace" style="margin-right:0.2778em;"></span></span><span class="base"><span class="strut" style="height:1em;vertical-align:-0.25em;"></span><span class="mop"><span class="mop">lo<span style="margin-right:0.01389em;">g</span></span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height:0.207em;"><span style="top:-2.4559em;margin-right:0.05em;"><span class="pstrut" style="height:2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight">2</span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height:0.2441em;"><span></span></span></span></span></span></span><span class="mopen">(</span><span class="mord"><span class="mord mathnormal">n</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height:0.3175em;"><span style="top:-2.55em;margin-left:0em;margin-right:0.05em;"><span class="pstrut" style="height:2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight"><span class="mord text mtight"><span class="mord mtight">views</span></span></span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height:0.15em;"><span></span></span></span></span></span></span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">+</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:2.2935em;vertical-align:-0.9721em;"></span><span class="mord"><span class="mopen nulldelimiter"></span><span class="mfrac"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height:1.3214em;"><span style="top:-2.314em;"><span class="pstrut" style="height:3em;"></span><span class="mord"><span class="mord"><span class="mord mathnormal">n</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height:0.3361em;"><span style="top:-2.55em;margin-left:0em;margin-right:0.05em;"><span class="pstrut" style="height:2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight"><span class="mord text mtight"><span class="mord mtight">days</span></span></span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height:0.2861em;"><span></span></span></span></span></span></span></span></span><span style="top:-3.23em;"><span class="pstrut" style="height:3em;"></span><span class="frac-line" style="border-bottom-width:0.04em;"></span></span><span style="top:-3.677em;"><span class="pstrut" style="height:3em;"></span><span class="mord"><span class="mord">1</span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height:0.9721em;"><span></span></span></span></span></span><span class="mclose nulldelimiter"></span></span><span class="mspace" style="margin-right:0.2222em;"></span><span class="mbin">+</span><span class="mspace" style="margin-right:0.2222em;"></span></span><span class="base"><span class="strut" style="height:1em;vertical-align:-0.25em;"></span><span class="mord">1</span><span class="mclose">)</span></span></span></span></span></p>
<p>其中 <span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.5806em;vertical-align:-0.15em;"></span><span class="mord"><span class="mord mathnormal">n</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height:0.3175em;"><span style="top:-2.55em;margin-left:0em;margin-right:0.05em;"><span class="pstrut" style="height:2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight"><span class="mord text mtight"><span class="mord mtight">views</span></span></span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height:0.15em;"><span></span></span></span></span></span></span></span></span></span> 是图片浏览量，<span class="katex"><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.7167em;vertical-align:-0.2861em;"></span><span class="mord"><span class="mord mathnormal">n</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height:0.3361em;"><span style="top:-2.55em;margin-left:0em;margin-right:0.05em;"><span class="pstrut" style="height:2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight"><span class="mord text mtight"><span class="mord mtight">days</span></span></span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height:0.2861em;"><span></span></span></span></span></span></span></span></span></span> 是距离图片上传日期的天数。</p>
<p>然而该论文最后给出的数据集地址已经失效了......</p>
<p> </p>
</li>
<li>
<p><strong>An Image is Worth More than a Thousand Favorites: Surfacing the Hidden Beauty of Flickr Pictures.</strong> <em>Rossano Schifanella, et al.</em> ICWSM 2015. <a href="https://arxiv.org/pdf/1505.03358.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a> <a href="http://www.di.unito.it/~schifane/dataset/beauty-icwsm15/#" target="_blank" rel="noopener noreferrer">[Dataset]</a></p>
<p>研究的是在人气较低的图片中找出高质量图片，它的数据集包含来自 Flicker 上的约 15,000 张图片，带有语义标注（共 4 种）和美学质量标注（评分范围为 1-5）。</p>
<p>下载：需要根据<a href="http://www.di.unito.it/~schifane/dataset/beauty-icwsm15/#" target="_blank" rel="noopener noreferrer">这里</a>的 <code v-pre>flickr_photo_id</code> 通过 <a href="https://www.flickr.com/services/api/" target="_blank" rel="noopener noreferrer">Flicker API</a> 去搞图片。</p>
</li>
</ul>
<h2 id="papers" tabindex="-1"><a class="header-anchor" href="#papers" aria-hidden="true">#</a> Papers</h2>
<h3 id="手工特征" tabindex="-1"><a class="header-anchor" href="#手工特征" aria-hidden="true">#</a> 手工特征</h3>
<p><strong>全局特征：</strong></p>
<ul>
<li>
<p><strong>Classification of Digital Photos Taken by Photographers or Home Users.</strong> <em>Hanghang Tong, et al.</em> PCM 2004.
<a href="http://bigeye.au.tsinghua.edu.cn/english/paper/_PCM04_tong.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p>图像美感评估最早的一篇论文，提取了基本的全局低级特征（blurriness、contrast、colorfulness、saliency），然后用 boosting 来区分来自专业摄影师和普通拍照者的照片。</p>
</li>
<li>
<p><strong>Studying Aesthetics in Photographic Images Using a Computational Approach.</strong> <em>Ritendra Datta, et al.</em> ECCV 2006. <a href="http://infolab.stanford.edu/~wangz/project/imsearch/Aesthetics/ECCV06/datta.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p>靠直觉（论文原话）提取了 56 维低层和高层特征，用 SVM 搞二分类，用 CART 评估美感分数值。展示了用机器学习进行图像美感分析的可行性，是个里程碑。</p>
</li>
<li>
<p><strong>The Design of High-Level Features for Photo Quality Assessment.</strong> <em>Yan Ke, Xiaoou Tang, Feng Jing.</em> CVPR 2006. <a href="http://www-cgi.cs.cmu.edu/~yke/photoqual/cvpr06photo.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p>构造了包括很多高层语义特征在内的 7 维特征，然后用朴素贝叶斯搞二分类。该论文跟上一篇论文都是最早把图像审美问题转换为二分类问题的尝试。</p>
</li>
</ul>
<p><strong>前景背景对比特征：</strong></p>
<ul>
<li>
<p><strong>Photo and Video Quality Evaluation: Focusing on the Subject.</strong> <em>Yiwen Luo and Xiaoou Tang.</em> ECCV 2008. <a href="http://mmlab.ie.cuhk.edu.hk/pdf/luoT_ECCV08.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p>先把主题区域和背景分离，然后考虑了主题区域和背景的清晰度对比度、亮度对比度等特征，最终提取了 5 维特征。分类器分别使用了朴素贝叶斯、SVM 和 Gentle AdaBoost。从它给出的结果来看效果比上面两篇论文提高了很多。它还顺便用这种方法搞了一下视频质量评估。</p>
</li>
<li>
<p><strong>Saliency-enhanced Image Aesthetics Class Prediction.</strong> <em>Lai-Kuan Wong, Kok-Lim Low.</em> ICIP 2009. <a href="https://ieeexplore.ieee.org/document/5413825" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="https://projet.liris.cnrs.fr/imagine/pub/proceedings/ICIP-2009/pdfs/0000997.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p>也分离了主题区域和背景，认为主题区域一定是显著区域，所以在提取了主题区域时用了一个显著性模型。而且在考虑前景背景对比特征的同时也考虑了一些全局特征。然后用 SVM 搞二分类。</p>
</li>
<li>
<p><strong>A Framework for Photo-quality Assessment and Enhancement Based on Visual
Aesthetics.</strong> <em>Subhabrata Bhattacharya, et al.</em> ACM MM 2010. <a href="http://www.cs.cmu.edu/~rahuls/pub/mm2010-rahuls.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p><strong>A Holistic Approach to Aesthetic Enhancement of Photographs.</strong> <em>Subhabrata Bhattacharya, et al.</em> TOMCCAP 2011. <a href="http://www.cs.cmu.edu/~rahuls/pub/tomccap2011-rahuls.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p>对有单个前景的图片提取了 relative foreground position 作为特征（用分割或显著度提取前景的 visual attention point，然后算它跟四个 stress point 之间的距离），对风景图片提取了 visual weight ratio 作为特征（检测地平线，然后算它跟黄金分割线的差距）。然后用 SVR 来对图片的的美感进行打分（范围为 1-5）。它还能根据上述法则提出增强图片美感的建议。</p>
</li>
</ul>
<p><strong>通用局部特征：</strong></p>
<ul>
<li>
<p><strong>Aesthetic Quality Classification of Photographs Based on Color Harmony.</strong> <em>Masashi Nishiyama, et al.</em> CVPR 2011. <a href="http://research.nii.ac.jp/~imarik/resources/papers/CVPR2012-Nishiyama.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
</li>
<li>
<p><strong>Assessing the Aesthetic Quality of Photographs Using Generic Image Descriptors.</strong> <em>Luca Marchesotti, et al.</em> ICCV 2011. <a href="https://ieeexplore.ieee.org/document/6126444" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="http://www.tamaraberg.com/teaching/Fall_13/papers/Marchesotti2011.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
</li>
<li>
<p><strong>A Statistic Approach for Photo Quality Assessment.</strong> <em>Li-Yun Lo, et al.</em> ISIC 2012. <a href="https://ieeexplore.ieee.org/abstract/document/6449719" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="http://www.csie.kuas.edu.tw/~jcchen/pdf/A%20Statistic%20Approach%20for%20Photo%20Quality%20Assessment%20.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
</li>
</ul>
<h3 id="深度学习" tabindex="-1"><a class="header-anchor" href="#深度学习" aria-hidden="true">#</a> 深度学习</h3>
<h4 id="分类" tabindex="-1"><a class="header-anchor" href="#分类" aria-hidden="true">#</a> 分类</h4>
<p><strong>Multi-column：</strong></p>
<ul>
<li>
<p><strong>RAPID: Rating Pictorial Aesthetics Using Deep Learning.</strong> <em>Xin Lu, et al.</em> ACM MM 2014. <a href="http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.710.1251&amp;rep=rep1&amp;type=pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p><strong>Rating Image Aesthetics Using Deep Learning.</strong> <em>Xin Lu, et al.</em> TMM 2015. <a href="https://ieeexplore.ieee.org/document/7243357" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="http://infolab.stanford.edu/~wangz/project/imsearch/Aesthetics/TMM15/lu.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p>首次用神经网络搞图片美感评估。用了双列 CNN，把全局图像和随机提取出的一个局部 patch 各输入一列 CNN，输出的特征会被联合起来输入分类层进行二分类：</p>
<p><img src="/img/in-post/2020-02-29/rapid-dcnn.png" alt="rapid-dcnn"></p>
<p>因为数据集用的是有 style 标注的 AVA，该论文还把 style 属性也输入了一列 CNN，相当于最后是 3 列 CNN：</p>
<p><img src="/img/in-post/2020-02-29/rapid-style-cnn.png" alt="rapid-style-cnn"></p>
</li>
<li>
<p><strong>Deep Multi-Patch Aggregation Network for Image Style, Aesthetics, and Quality Estimation.</strong> <em>Xin Lu, et al.</em> ICCV 2015. <a href="https://ieeexplore.ieee.org/abstract/document/7410476/" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="http://infolab.stanford.edu/~wangz/project/imsearch/Aesthetics/ICCV15/lu.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p>只用了 patch 作为输入。从图片中随机提取一些 patch，用一组 CNN 对每个 patch 提取特征，然后把所有 patch 的特征聚合起来，最后把特征输进 soft-max 层进行二分类。</p>
<p><img src="/img/in-post/2020-02-29/dma-net.png" alt="dma-net"></p>
</li>
<li>
<p><strong>A-Lamp: Adaptive Layout-Aware Multi-Patch Deep Convolutional Neural.</strong> <em>Shuang Ma, et al.</em> CVPR 2017. <a href="http://openaccess.thecvf.com/content_cvpr_2017/papers/Ma_A-Lamp_Adaptive_Layout-Aware_CVPR_2017_paper.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a> <a href="https://arxiv.org/pdf/1704.00248.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a></p>
<p>上一篇论文的 patch 是随机提取的，它们可能大量重叠，或者不能覆盖图片的关键信息。所以该论文用了一个显著性模型提取了显著度最高、更多样、重叠部分更少的一些 patch。</p>
<p>同时该论文还用了另一个显著性模型来提取出图片中的主要物体，从而利用了图片中的物体布局信息。</p>
<p><img src="/img/in-post/2020-02-29/alamp.png" alt="alamp"></p>
</li>
<li>
<p><strong>Attention-based Multi-Patch Aggregation for Image Aesthetic Assessment.</strong> <em>Kekai Sheng, et al.</em> ACM MM 2018. <a href="http://chongyangma.com/publications/am/2018_am_paper.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a> <a href="https://github.com/Openning07/MPADA" target="_blank" rel="noopener noreferrer">[Code]</a></p>
<p>在聚合不同 patch 的特征时使用了 attention 机制。</p>
</li>
<li>
<p><strong>Brain-Inspired Deep Networks for Image Aesthetics Assessment.</strong> <em>Zhangyang Wang, et al.</em> arXiv 2016. <a href="https://arxiv.org/pdf/1601.04155.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a></p>
</li>
<li>
<p><strong>Composition-preserving Deep Photo Aesthetics Assessment.</strong> <em>Long Mai, et al.</em> CVPR 2016.
<a href="https://ieeexplore.ieee.org/document/7780429" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Mai_Composition-Preserving_Deep_Photo_CVPR_2016_paper.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a> <a href="http://web.cecs.pdx.edu/~fliu/project/deep-quality/" target="_blank" rel="noopener noreferrer">[Project]</a></p>
<p>因为全连接层的特征数是固定的，所以 CNN 一般会对输入图片的尺寸做固定要求。为了适应这种要求，往往需要对输入图片进行剪裁拉伸等操作，这就会破坏图片的布局、降低图片分辨率、导致图片失真等，从而破坏图片的美感。</p>
<p>所以该论文提出了一个叫 Multi-Net Adaptive Spatial Pooling ConvNet 的结构，由多个用 adaptive spatial pooling 层替换了普通 pooling 层（卷积层和全连接层之间）的 CNN 组成，每个 CNN 除了 adaptive spatial pooling 不一样其他都一样。每个 CNN 的输入是同一张原始图片，输出是指定大小的特征。最后把所有网络输出的特征聚合起来。</p>
<p>然后还用了另外一个 CNN 来提取图片的场景特征，跟上述特征一起扔进聚合层聚合。</p>
<p><img src="/img/in-post/2020-02-29/mna.png" alt="mna"></p>
<p>跟 SPP 有点像：</p>
<p><strong>Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition.</strong> <em>Kaiming He, et al.</em> TPAMI 2015. <a href="https://ieeexplore.ieee.org/abstract/document/7005506" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="https://arxiv.org/pdf/1406.4729.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a> <a href="https://github.com/yueruchen/sppnet-pytorch" target="_blank" rel="noopener noreferrer">[Code]</a></p>
</li>
</ul>
<p><strong>Multi-task：</strong></p>
<ul>
<li>
<p><strong>Deep Aesthetic Quality Assessment with Semantic Information.</strong> <em>Yueying Kao, Ran He, and Kaiqi Huang.</em> TIP 2017. <a href="https://ieeexplore.ieee.org/abstract/document/7814292" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="https://arxiv.org/pdf/1604.04970.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a></p>
<p>用一个 MTCNN 来提取提取美学特征和场景语义信息。</p>
</li>
<li>
<p><strong>Hierarchical Aesthetic Quality Assessment Using Deep Convolutional Neural Networks.</strong> <em>Yueying Kao, Kaiqi Huang, and Steve Maybank.</em> Signal Processing: Image Communication 2016. <a href="https://core.ac.uk/download/pdf/141224862.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
</li>
<li>
<p><strong>Visual Aesthetic Quality Assessment with Multi-task Deep Learning.</strong> <em>Yueying Kao, Ran He, and Kaiqi Huang.</em> arXiv 2016. <a href="https://www.researchgate.net/profile/Yueying_Kao/publication/301877404_Visual_Aesthetic_Quality_Assessment_with_Multi-task_Deep_Learning/links/573a717108ae9f741b2cad7a/Visual-Aesthetic-Quality-Assessment-with-Multi-task-Deep-Learning.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
</li>
<li>
<p><strong>Photo Aesthetics Ranking Network with Attributes and Content Adaptation.</strong> <em>Shu Kong, et al.</em> ECCV 2016. <a href="https://arxiv.org/pdf/1606.01621.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a> <a href="https://www.ics.uci.edu/~skong2/aesthetics.html" target="_blank" rel="noopener noreferrer">[Project]</a> <a href="https://github.com/aimerykong/deepImageAestheticsAnalysis" target="_blank" rel="noopener noreferrer">[Code]</a> <a href="https://drive.google.com/open?id=0BxeylfSgpk1MOVduWGxyVlJFUHM" target="_blank" rel="noopener noreferrer">[Dataset &amp; Model]</a></p>
<p>提出了 AADB 数据集。</p>
</li>
</ul>
<p><strong>DBM：</strong></p>
<ul>
<li>
<p><strong>Joint Image and Text Representation for Aesthetics Analysis.</strong> <em>Ye Zhou, et al.</em> ACM MM 2016. <a href="http://infolab.stanford.edu/~wangz/project/imsearch/Aesthetics/ACMMM2016/zhou.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a></p>
<p>把 AVA 中所有图片的评论都爬了下来，建了一个叫 AVA-Comments 的数据集。用 DBM 来同时利用图片的视觉特征和评论的文本特征。</p>
</li>
</ul>
<h4 id="评分分布" tabindex="-1"><a class="header-anchor" href="#评分分布" aria-hidden="true">#</a> 评分分布</h4>
<ul>
<li>
<p><strong>NIMA: Neural Image Assessment.</strong> <em>Hossein Talebi and Peyman Milanfar.</em> TIP 2018. <a href="https://arxiv.org/pdf/1709.05424.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a> <a href="https://ai.googleblog.com/2017/12/introducing-nima-neural-image-assessment.html" target="_blank" rel="noopener noreferrer">[Blog]</a> <a href="https://github.com/titu1994/neural-image-assessment" target="_blank" rel="noopener noreferrer">[Code (Reproduction)]</a></p>
</li>
<li>
<p><strong>Predicting Aesthetic Score Distribution through Cumulative Jensen-Shannon Divergence.</strong> <em>Xin Jin, et al.</em> AAAI 2018. <a href="http://jinxin.me/downloads/papers/028-AAAI2018/ScoreDestribution.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a> <a href="https://github.com/BestiVictory/CJS-CNN" target="_blank" rel="noopener noreferrer">[Code]</a></p>
</li>
</ul>
<h4 id="文字描述" tabindex="-1"><a class="header-anchor" href="#文字描述" aria-hidden="true">#</a> 文字描述</h4>
<ul>
<li>
<p><strong>Aesthetic Critiques Generation for Photos.</strong> <em>Kuang-Yu Chang, Kung-Hung Lu, and Chu-Song Chen.</em> ICCV 2017. <a href="https://ieeexplore.ieee.org/document/8237642" target="_blank" rel="noopener noreferrer">[IEEE]</a> <a href="https://www.iis.sinica.edu.tw/~kuangyu/iccv17_aesthetic_critiques.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a> <a href="https://github.com/kunghunglu/DeepPhotoCritic-ICCV17" target="_blank" rel="noopener noreferrer">[Code]</a> <a href="https://github.com/ivclab/DeepPhotoCritic-ICCV17" target="_blank" rel="noopener noreferrer">[Dataset]</a></p>
<p>在每个角度的数据集上跑了一个图像描述模型（直接用了 <a href="#show-and-tell">NeuralTalk2</a>），然后用 Attention 把所有模型输出的隐状态融合起来，作为另一个新的 LSTM 的输入，新的 LSTM 的输出就是涵盖了多个角度的描述。（但从论文给的结果来看角度还是比较单一？）</p>
  <img src="/img/in-post/2020-02-29/pccd.png" width="400px" alt="pccd" />
</li>
<li>
<p><strong>Aesthetic Attributes Assessment of Images.</strong> <em>Xin Jin, et al.</em> ACM MM 2019. <a href="https://arxiv.org/pdf/1907.04983.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a> <a href="https://github.com/BestiVictory/DPC-Captions" target="_blank" rel="noopener noreferrer">[Dataset]</a></p>
<p>搞了一个比 PCCD 大得多的数据集，跑了一个浮夸的模型，输出指定角度的描述。</p>
<p><img src="/img/in-post/2020-02-29/aman.png" alt="aman"></p>
</li>
<li>
<p><strong>Neural Aesthetic Image Reviewer.</strong> <em>Wenshan Wang, et al.</em> IET Computer Vision 2019. <a href="https://arxiv.org/pdf/1802.10240.pdf" target="_blank" rel="noopener noreferrer">[arxiv]</a></p>
</li>
<li>
<p><strong>Aesthetic Image Captioning From Weakly-Labelled Photographs.</strong> <em>Koustav Ghosal, Aakanksha Rana, and Aljosa Smolic.</em> ICCV Workshop 2019. <a href="http://openaccess.thecvf.com/content_ICCVW_2019/papers/CROMOL/Ghosal_Aesthetic_Image_Captioning_From_Weakly-Labelled_Photographs_ICCVW_2019_paper.pdf" target="_blank" rel="noopener noreferrer">[Paper]</a> <a href="https://github.com/V-Sense/Aesthetic-Image-Captioning-ICCVW-2019" target="_blank" rel="noopener noreferrer">[Code]</a></p>
</li>
</ul>
</div></template>
