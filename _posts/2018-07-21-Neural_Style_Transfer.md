---
layout: post
title: 深度神经风格迁移
date: 2018-07-21
tags: 深度学习
---

## 介绍（由于近期准备校招，博客暂时不更新）

　　神经风格迁移是我研一研二时期主要的研究方向，而从最初的风格迁移出现已经有较长一段时间了。之所以现在写这个博客，第一是因为我的毕业论文定的方向是风格迁移+情感分析；第二是借这篇博客以及之后的学习，能对深度学习更进一步的理解！


<div align="center">
	<img src="/img/posts/stytrans/Dawn Sky.jpg" height="215.2" width="384">

  <img src="/img/posts/stytrans/Dawn_Sky_masked.jpg" height="215.2" width="384">
</div>


### 目录

* [什么是风格迁移?](#What-is-style-transfer)
* [当前不同框架下的风格迁移](#different-application-in-different-platform)
* [如何评价风格迁移的实验效果](#how-to-evaluation)
* [风格迁移能解决的问题](#solve-problems)
* [风格迁移的局限性](#the-limitation-of-style-transfer)
* [展望](#prospect)


### <a name="What-is-style-transfer"></a>什么是风格迁移?

* 最经典的Gatys([paper](https://arxiv.org/abs/1508.06576),[code](https://github.com/anishathalye/neural-style))中给出了风格迁移的流程图，视为最原始的方法。借此图说明风格迁移的基本原理。

* 1. α 图定义为`style image`, p 图定义为`content image`,
* 2. 损失通过VGG-16的前四层来表示，层次越高，内容越抽象。这里列出几个符号定义。

![](/img/posts/stytrans/dic.jpg)

* 3. 将内容图像输入卷积网络中提取图像内容，由公式

![](/img/posts/stytrans/closs.jpg)，计算内容损失。
* 4. 对以上公式求导

![](/img/posts/stytrans/clossDE.jpg)，使用反向传输，使得生成图像在`内容`上接近原输入内容图像。
* 5. 将风格图像输入到同一个网络中提取它的风格信息，风格提取的符号定义为

![](/img/posts/stytrans/stydifi.jpg)

* 6. 计算风格图像的loss

![](/img/posts/stytrans/styExp.jpg)

单独某层的损失函数

![](/img/posts/stytrans/singleloss.jpg)

各层综合的损失函数

![](/img/posts/stytrans/sloss.jpg)

求偏导

![](/img/posts/stytrans/slossDe.jpg),使得生成图像在`风格`上接近原输入风格图像。

* 7.风格损失

![](/img/posts/stytrans/loss.jpg)

* 8.`Gatys-Image-Style-Transfer`中给出的流程图。X是白噪声图像。同时将三张图片输入到同一网络中，对内容图像和风格图像求特征，对白噪声X求导。

![](/img/posts/stytrans/classictrans.png)
<!-- |印象派|后印象派|立体派|
| ----------------------------------------- | -------------------------------------------- | ----------------------------------------- |
| ![](/images/posts/stytrans/shuilian.jpeg) | ![](/images/posts/stytrans/starry_night.jpg) | ![](/images/posts/stytrans/Guernica.jpeg) |
|超现实主义|抽象表现主义|立体派|
|![](/images/posts/stytrans/chaoxianshizhuyi.jpg)|![](/images/posts/stytrans/chouxiang.jpeg) |![](/images/posts/stytrans/paipaipai.jpeg)| -->

<!-- <table>
<tr align='center'>
<td>印象派</td>
<td>后印象派</td>
<td>立体派</td>

</tr>
<tr>
<td><img src='/images/posts/stytrans/shuilian.jpeg' height="168px"></td>
<td><img src='/images/posts/stytrans/starry_night.jpg' height="168px"></td>
<td><img src='/images/posts/stytrans/Guernica.jpeg' height="168px"></td>

</tr>
<table>

<table>
<tr align='center'>
<td>超现实主义</td>
<td>抽象表现主义</td>
<td>立体派</td>

</tr>
<tr>
<td><img src='/images/posts/stytrans/chaoxianshizhuyi.jpg' height="168px" width="260"></td>
<td><img src='/images/posts/stytrans/chouxiang.jpeg' height="168px" width="260"></td>
<td><img src='/images/posts/stytrans/paipaipai.jpeg' height="168px" width="260"></td>

</tr>
<table> -->


### <a name="different-application-in-different-platform"></a>当前不同框架下的风格迁移

几年前，Gatys等人的`风格迁移`[[paper]](https://arxiv.org/abs/1508.06576),[[code]](https://github.com/anishathalye/neural-style)在学术界引起了不错的反响,并催生了后续很多研究成果。虽然在Gatys之前已经有学者做迁移方面的研究，但我把这篇paper看作是`first style transfer paper`。

#### 1.基于图像优化的`Slow Transfer`
* [**A Neural Algorithm of Artistic Style** ][[paper]](https://arxiv.org/abs/1508.06576)
* [**Image Style Transfer Using Convolutional Neural Networks**] [[Paper]](http://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Gatys_Image_Style_Transfer_CVPR_2016_paper.pdf) *(CVPR 2016)*
	*   [Torch-based](https://github.com/jcjohnson/neural-style)
	*   [TensorFlow-based](https://github.com/anishathalye/neural-style)
	*   [TensorFlow-based with L-BFGS optimizer support](https://github.com/cysmith/neural-style-tf)
	*   [Caffe-based](https://github.com/fzliu/style-transfer)
	*   [Keras-based](https://github.com/titu1994/Neural-Style-Transfer)

* [**Demystifying Neural Style Transfer**][[paper]](https://arxiv.org/abs/1701.01036)*(Theoretical Explanation)* *(IJCAI 2017)*

	*   [MXNet-based](https://github.com/lyttonhao/Neural-Style-MMD)


* [**Stable and Controllable Neural Texture Synthesis and Style Transfer Using Histogram Losses**][[paper]](https://arxiv.org/abs/1701.08893)

* [**Combining Markov Random Fields and Convolutional Neural Networks for Image Synthesis**][[paper]](https://arxiv.org/abs/1601.04589)*(CVPR 2016)*

----------------------

#### 2.基于模型优化的`Fast Transfer`
##### **2.1Per-Style-Per-Model-Methods**
* [**Perceptual Losses for Real-Time Style Transfer and Super-Resolution**][[paper]](https://arxiv.org/pdf/1603.08155.pdf) *(ECCV 2016)*
	*   [Torch-based](https://github.com/jcjohnson/fast-neural-style)
	*   [TensorFlow-based](https://github.com/lengstrom/fast-style-transfer)

* [**Precomputed Real-Time Texture Synthesis with Markovian Generative Adversarial Networks**] [[Paper]](https://arxiv.org/pdf/1604.04382.pdf)  *(ECCV 2016)*
 *   [Torch-based](https://github.com/chuanli11/MGANs)

* [**Texture Networks: Feed-forward Synthesis of Textures and Stylized Images**] [[Paper]](http://www.jmlr.org/proceedings/papers/v48/ulyanov16.pdf)  *(ICML 2016)*

* [**Improved Texture Networks: Maximizing Quality and Diversity in Feed-forward Stylization and Texture Synthesis**] [[Paper]](https://arxiv.org/pdf/1701.02096.pdf)  *(CVPR 2017)*
	*   [Torch-based](https://github.com/DmitryUlyanov/texture_nets)

* [**Precomputed Real-Time Texture Synthesis with Markovian Generative Adversarial Networks**] [[Paper]](https://arxiv.org/pdf/1604.04382.pdf)  *(ECCV 2016)*
 *   [Torch-based](https://github.com/chuanli11/MGANs)

 -------------------------------

##### **2.2Multiple-Style-Per-Model-Methods**
* [**A Learned Representation for Artistic Style**] [[Paper]](https://arxiv.org/pdf/1610.07629.pdf)  *(ICLR 2017)*

 *   [TensorFlow-based](https://github.com/tensorflow/magenta/tree/master/magenta/models/image_stylization)

* [**Multi-style Generative Network for Real-time Transfer**] [[Paper]](https://arxiv.org/pdf/1703.06953.pdf)

	*   [PyTorch-based](https://github.com/zhanghang1989/PyTorch-Style-Transfer)
	*   [Torch-based](https://github.com/zhanghang1989/MSG-Net)

* [**StyleBank: An Explicit Representation for Neural Image Style Transfer**] [[Paper]](https://arxiv.org/pdf/1703.09210.pdf)  *(CVPR 2017)*

* [**Diversified Texture Synthesis With Feed-Forward Networks**] [[Paper]](http://openaccess.thecvf.com/content_cvpr_2017/papers/Li_Diversified_Texture_Synthesis_CVPR_2017_paper.pdf)  *(CVPR 2017)*
	*   [Torch-based](https://github.com/Yijunmaverick/MultiTextureSynthesis)

-----------------------------------------

##### **2.3Arbitrary-Style-Per-Model-Methods**
*  [**Fast Patch-based Style Transfer of Arbitrary Style**] [[Paper]](https://arxiv.org/pdf/1612.04337.pdf)
	*   [Torch-based](https://github.com/rtqichen/style-swap)

*  [**Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization**] [[Paper]](https://arxiv.org/pdf/1703.06868.pdf)  *(ICCV 2017)*

	*   [Torch-based](https://github.com/xunhuang1995/AdaIN-style)


### <a name="how-to-evaluation"></a>如何评价风格迁移的实验效果

　
转载请注明：[罗星的博客](http://xingxl.github.io) » [深度神经风格迁移](https://xingxl.github.io/2018/07/Neural_Style_Transfer/)
