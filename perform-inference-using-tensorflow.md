# 5. 使用 TensorFlow 进行推理

## 添加项目依赖

&emsp;&emsp;为了在项目中添加接口库和他们的依赖，我们需要添加 TensorFlow Android 接口库和 JAVA API。这些已经可以在 [JCenter](https://bintray.com/google/tensorflow/tensorflow-android) 使用，或者你可以在从 [TensorFlow 源码](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/android) 编译。

1. 在 Android Studio 中打开 build.gradle。
1. 通过将 API 添加到 android 代码块中的 dependencies 代码块，从而将 API 导入项目。（注意：这不是 buildscript 代码块）。

### [build.gradle](https://github.com/googlecodelabs/tensorflow-style-transfer-android/blob/codelab-finish/android/build.gradle)

``` gradle
dependencies {
   compile 'org.tensorflow:tensorflow-android:1.2.0-preview'
}
```

3. 点击 Gradle sync 按钮，在 IDE 中使改变生效。

## TensorFlow 推理接口

&emsp;&emsp;当执行 TensorFlow 代码时，你通常既要管理一个计算图，又要管理一个会话。（在 [Getting Started](https://www.tensorflow.org/get_started/get_started#the_computational_graph) 文档中提及）然而作为 Android 开发者将可能想用预编译的图实现接口，TensorFlow 提供了一个帮你管理图和会话的 JAVA 接口：

[TensorFlowInferenceInterface](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/contrib/android/java/org/tensorflow/contrib/android/TensorFlowInferenceInterface.java)

&emsp;&emsp;如果你需要更多的控制，TensorFlow 的 JAVA API 提供了你在 Python API 中熟悉的会话和图对象。

## 风格转换神经网络。

&emsp;&emsp;我们已经把在最后一部分描述的风格转换神经网络放在了项目的 `assets` 目录中，因此它可以供你使用。你也可以 [直接下载](https://storage.googleapis.com/download.tensorflow.org/models/stylize_v1.zip) 或者从 [Magenta 的项目](https://github.com/tensorflow/magenta/blob/master/magenta/models/image_stylization/README.md) 中自行编译。

&emsp;&emsp;你可能值得打开交互式图预览以便看到我们将要很快引用的节点。（提示：点击鼠标悬停后出现的 “+” 图标打开 `transformer` 节点）。

[交互探索这张图](https://googlecodelabs.github.io/tensorflow-style-transfer-android/)

## 添加推理代码

1. 在 `StylizeActivity.java` 中，在类的顶部附近添加以下成员字段（举例：在声明 `NUM_STYLES` 之前）

### [StylizeActivity.java](https://github.com/googlecodelabs/tensorflow-style-transfer-android/blob/codelab-finish/android/src/org/tensorflow/demo/StylizeActivity.java)

``` java
// Copy these lines below
private TensorFlowInferenceInterface inferenceInterface;

private static final String MODEL_FILE = "file:///android_asset/stylize_quantized.pb";

private static final String INPUT_NODE = "input";
private static final String STYLE_NODE = "style_num";
private static final String OUTPUT_NODE = "transformer/expand/conv3/conv/Sigmoid";

// Do not copy this line, you want to find it and paste before it.
private static final int NUM_STYLES = 26;
```

> 注意：这些在图中对应的节点有相同的名称。尝试在上面的交互图工具中找到它们。当你看到 /（斜杠） 的时候你需要展开它来查看子节点。

2. 在相同的类下，找到 `onPreviewSizeChosen` 方法，构造 `TensorFlowInferenceInterface`。我们使用这个方法来初始化，因为一旦向文件系统和相机获取权限，就会调用它。

### [StylizeActivity.java](https://github.com/googlecodelabs/tensorflow-style-transfer-android/blob/codelab-finish/android/src/org/tensorflow/demo/StylizeActivity.java)

``` java
@Override
public void onPreviewSizeChosen(final Size size, final int rotation) {
 // anywhere in here is fine

 inferenceInterface = new TensorFlowInferenceInterface(getAssets(), MODEL_FILE);

 // anywhere at all...
}
```

> 重要：如果你看到了 “无法找到标识 ...” 的警告，你将需要添加 import 语句到此文件中。Android Studio 能帮你做这个事情，把光标移动到红色错误文本上方，按下 Alt-Enter 键，选择 Import ...

3. 现在找到 `stylizeImage ` 方法，添加代码将我们的相机位图和选择的风格样式传递给 TensorFlow，并从图中抓取输出。这个目标在两个循环中间。

### [StylizeActivity.java](https://github.com/googlecodelabs/tensorflow-style-transfer-android/blob/codelab-finish/android/src/org/tensorflow/demo/StylizeActivity.java)

``` java
private void stylizeImage(final Bitmap bitmap) {
    // Find the code marked with: TODO: Process the image in TensorFlow here.
    // Then paste the following code in at that location.
 
    // Start copying here:

    // Copy the input data into TensorFlow.
    inferenceInterface.feed(INPUT_NODE, floatValues, 
    1, bitmap.getWidth(), bitmap.getHeight(), 3);
    inferenceInterface.feed(STYLE_NODE, styleVals, NUM_STYLES);

    // Execute the output node's dependency sub-graph.
    inferenceInterface.run(new String[] {OUTPUT_NODE}, isDebug());

    // Copy the data from TensorFlow back into our array.
    inferenceInterface.fetch(OUTPUT_NODE, floatValues);

    // Don't copy this code, it's already in there.
    for (int i = 0; i < intValues.length; ++i) {
    // ...
}
```

4. 可选：找到 `renderDebug `，在调试覆盖层中添加 TensorFlow 状态文本（当你按下音量键时触发）。

### [StylizeActivity.java](https://github.com/googlecodelabs/tensorflow-style-transfer-android/blob/codelab-finish/android/src/org/tensorflow/demo/StylizeActivity.java)

``` java
private void renderDebug(final Canvas canvas) {
    // ... provided code that does some drawing ...

    // Look for this line, but don't copy it, it's already there.
    final Vector<String> lines = new Vector<>();

    // Add these three lines right here:
    final String[] statLines = inferenceInterface.getStatString().split("\n");
    Collections.addAll(lines, statLines);
    lines.add("");

    // Don't add this line, it's already there
    lines.add("Frame: " + previewWidth + "x" + previewHeight);
    // ... more provided code for rendering the text ...
}
```

> 重要：如果你看到了 “无法找到标识 ...” 的警告，你将需要添加 import 语句到此文件中。Android Studio 能帮你做这个事情，把光标移动到红色错误文本上方，按下 Alt-Enter 键，选择 Import ...

5. 在 Android Studio 中按下 `Run` 按钮，等待项目编译
1. 你现在应该看到了在你的设备中发生的风格转换！

![](/assets/3.png)