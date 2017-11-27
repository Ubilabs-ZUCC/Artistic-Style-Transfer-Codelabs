# 1. 介绍

## 什么是艺术风格转移？

&emsp;&emsp;最近，在深度学习中出现了很多发展方向。其中最令人激动的方向之一就是艺术风格转移，或者说是一种创造新图像的能力。它被称之为 “拼图” ，基于两张输入图像工作：一张图像表示艺术风格，一张图像表示内容。

![](/assets/1.png)

&emsp;&emsp;使用这种技术，我们可以生成各种风格的美丽的新艺术作品。

![](/assets/2.png)

&emsp;&emsp;这个 codelab 将引导你完成在 Android 应用程序中使用艺术风格传递神经网络的过程。别担心，只需要 9 行代码。你还可以使用此 codelab 中涉及的技术来实现已经培训完毕的任何 TensorFlow 网络。

> 想看更多相关内容么？查看已经发布的 [Google Research](https://research.googleblog.com/2016/10/supercharging-style-transfer.html) 和 [Magenta](https://magenta.tensorflow.org/2016/11/01/multistyle-pastiche-generator/) 的博客。

## 我们将要创造什么？

&emsp;&emsp;在这个 codelab 中，你将要使用现有的 Android 应用程序，并在其中添加一个 TensorFlow 模型，最终使用设备的相机生成风格化的图像。你将建立以下技能：

* 在你的应用程序中使用 TensorFlow 的 Android Java 和 NDK 库
* 在 Android 应用程序中导入一个预训练的 TensorFlow 模型
* 在 Android 应用程序中执行逻辑推理
* 在 TensorFlow 图中执行特定张量

![](/assets/3.png)

## 你将要需要什么？

* 一个运行 Lollipop（API 21，v5.0）系统且相机在 Camera2 API 支持下（在API 21中引入）的设备
* v2.2 或者更高版本的 Android Studio
* 包含 v23 或者更高版本的 SDK 编译工具

> 注意：这个实验室专注于在一个 Android 应用程序中使用一个已存在的 TensorFlow 模型。Android 部分的代码将和提供的代码很大程度相同，但我们将说明 TensorFlow 的部分和 Android TensorFlow 专用的部分。