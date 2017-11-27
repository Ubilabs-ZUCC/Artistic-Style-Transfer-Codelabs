# 4. 艺术风格的学术代表

## 关于这个网络

> 注意：对于使用或者导入网络，理解这个网络的工作原理并不重要，但这里提供了一些背景资料。请随意跳过本节。

&emsp;&emsp;我们正在导入的网络是一些重要发展的结果。第一个风格转换的神经网络论文 [(Gatys, et al. 2015)](http://arxiv.org/abs/1508.06576) 引入了一种利用卷积图像分类网络性质的技术，其中低层级识别简单边缘和形状（风格的组成部分），高层级别识别更复杂的内容，最终生成一个拼图。这种技术适用于任意两个图像，但执行速度缓慢。

&emsp;&emsp;此后提出了一些改进措施，其中包括使用一种对每种风格的网络进行预训练的方法实现权衡 [(Johnson, et al. 2016)](https://arxiv.org/abs/1603.08155) ，从而实时生成图像。

&emsp;&emsp;最后，我们在这个实验室中使用的 [神经网络](https://arxiv.org/abs/1610.07629) 指出不同的神经网络代表不同的风格，可能会复制大量的信息。他们提出了一个用多种风格训练的单一神经网络，并且它有一个有趣的副产品，就是我们在这里所使用的混合风格的能力。

&emsp;&emsp;要对这些网络进行更为技术性的比较，以及对其他网络的评估，请查看 [Cinjon Resnick的评论文章](https://github.com/tensorflow/magenta/blob/master/magenta/reviews/styletransfer.md)。

## 在神经网络中

&emsp;&emsp;产生这个网络的原始 TensorFlow 代码可以在 [Magenta 的 GitHub 页面](https://github.com/tensorflow/magenta) 上获得，特别是 [风格化的图像转换模型](https://github.com/tensorflow/magenta/blob/master/magenta/models/image_stylization/model.py#L28)[(README)](https://github.com/tensorflow/magenta/blob/master/magenta/models/image_stylization/README.md)。

&emsp;&emsp;在使用资源有限的环境（如移动应用程序）之前，这个模型已经被转化为更小的数据类型并且删除了冗余的计算。你可以在 [Graph Transforms](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/tools/graph_transforms/README.md) 的文档中获取关于这个过程的更多信息，并且在 [TensorFlow for Poets II: Optimize for Mobile](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets-2/) 的 Codelab 中尝试。

&emsp;&emsp;最后的结果是 [stylize_quantized.pb](https://github.com/googlecodelabs/tensorflow-style-transfer-android/blob/master/android/assets/stylize_quantized.pb) 。这个文件在下方显示，这就是我们在这个应用程序中将会使用的。transformer 节点包含大多数的图，点击到交互版来展开它。

![](/assets/4.png)

[交互探索这张图](https://googlecodelabs.github.io/tensorflow-style-transfer-android/)