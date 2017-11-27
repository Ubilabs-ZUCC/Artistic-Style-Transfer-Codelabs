# 3. 读取 Android 应用程序骨架

## 这个应用程序是什么？

&emsp;&emsp;这个应用程序骨架包含一个 Android 应用程序，它从设备的相机中获取帧，并主活动的视图中呈现。

### UI 控制

![](/assets/3.png)

* 第一个按钮，用一个数字标记（默认256）控制显示出图片的大小（最终通过风格转换网络）。更小的数字表示更小的图片，转换的速度更快，但质量低。相反，更大的数字表示更大的图片，但会花费更多的时间转换。
* 第二个按钮，用 `save` 标识，将会保存当前帧到你的设备中，方便你以后使用。
* 缩略图表示可用于转换的样式，每张图像都是一个滑块。你能够混合多个滑块，这将代表你希望应用于相机帧的每种风格的比例，这些比例以及相机帧代表网络的输入。

### 这些额外的代码是什么？

&emsp;&emsp;应用程序代码包括一些需要在本机 TensorFlow 和 Android Java 之间进行连接的帮助器。他们实现的细节并不重要，但你应该了解他们做的是什么。

&emsp;&emsp;`StylizeActivity.onPreviewSizeChosen(...)`

&emsp;&emsp;这个应用程序骨架使用本地相机帧，一旦授权并且可以使用相机，将调用此方法。

&emsp;&emsp;`StylizeActivity.setStyle(...)`

&emsp;&emsp;这使得风格滑块的规范化，使得它们的值总和为1.0，符合我们网络的期望。

&emsp;&emsp;`StylizeActivity.renderDebug(...)`

&emsp;&emsp;当你按设备上的音量向上或向下按钮时显示调试叠加层，包括 TensorFlow 的输出、性能指标、原始和无样式的图像。

&emsp;&emsp;`StylizeActivity.stylizeImage(...)`

&emsp;&emsp;这是我们要工作的地方，已经提供的代码在整数数组之间进行一些转换（由 Android 的 getPixels() 方法提供）。把表单 [0xRRGGBB] 转化成 [0.0,1.0] 的浮点数组，形式为 [r, g, b, r, g, b ...]。

&emsp;&emsp;`ImageUtils.*`

&emsp;&emsp;提供一些图像转换的帮助器。相机提供了 [YUV 空间](https://en.wikipedia.org/wiki/YUV)(因为它被最广泛的支持)，但网络希望 [RGB](https://en.wikipedia.org/wiki/RGB_color_space)，因此我们提供帮助器去转换图像。这些方法中大多数都用本地 C++ 实现来提高速度，代码位于 jni 目录下，但对于这个实验室通过预建的方式提供了 libtensorflow_demo.so 二进制文件。文件位于 libs 目录（在 Android Studio 中被定义为 jniLibs）下。如果这些不能使用，代码将会回到 Java 实现。