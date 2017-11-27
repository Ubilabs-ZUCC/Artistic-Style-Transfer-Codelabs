# 2. 获得资料起步

## 获得代码

&emsp;&emsp;有 2 种方法去抓取这个 codelab 的代码：下载包含代码的 ZIP 文件或者从 Github clone 项目。

### 下载 ZIP

&emsp;&emsp;[点击我下载这个 codelab 的全部代码](https://github.com/googlecodelabs/tensorflow-style-transfer-android/archive/codelab-start.zip)

&emsp;&emsp;解压下载的 zip 文件，这将解压出一个根目录（tensorflow-style-transfer-android-codelab-start），目录中包含我们将在这个 codelab 中使用的基本应用程序，包括所有应用程序资源。

### 从 Github 检出

&emsp;&emsp;从 Github 中检出代码

&emsp;&emsp;`git clone https://github.com/googlecodelabs/tensorflow-style-transfer-android`

&emsp;&emsp;这将会创建一个包含你所需要的一切的目录。如果你想更改它，你可以分别使用 `git checkout codelab-start` 和 `git checkout codelab-finish` 在实验室的起始代码和最终代码中切换。

### 从 Android Studio 中读取代码

![](/assets/5.png)

&emsp;&emsp;打开 Android Studio，选择 Open an existing Android Studio Project。在文件对话框中你将需要导航到你上一步中下载或者检出的 android 目录。举个例子，如果你在 home 目录中检出了代码，你将会打开 $HOME/tensorflow-style-transfer-android/android。

&emsp;&emsp;如果出现提示，你应该接受使用 Gradle wrapper，拒绝使用 Instant Run。

> 重要：
> * 你需要 open android 目录，而不是 tensorflow-style-transfer-android 目录。

![](/assets/6.png)

&emsp;&emsp;一旦 Android Studio 导入了项目，使用文件浏览器打开 StylizeActivity 类，这就是我们将要工作的地方。如果你成功读取了文件，那么让我们到下一步去。

> 注意：这一部分的代码已经从 main TensorFlow 知识库中分叉出来，因此你不需要编译和检出整个知识库。如果你想编译最新的源码，你可以 [从 Github 中检出](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/android/README.md#user-content-building-the-demo-from-source)