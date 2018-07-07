# App构建过程

1. AAPT(Android Asset Packaging Tool)工具打包应用中的资源文件，如AndroidManifest、layout布局等，并将xml文件编译为二进制形式
- assets中的文件不会被编译，图片及raw文件夹中的资源也会保持原来的形态，需要注意的是raw文件夹中的资源也会生成资源id。
- AAPT编译完成之后生成R.java文件。

2. AIDL工具将所有的aidl接口转化为java接口。

3. 所有的java代码，包括R.java与aidl文件都会被Java编译器编译成.class文件。

4. Dex工具将产生的.class文件及第三库及其他.class文件编译成.dex文件（dex文件是Dalvik虚拟机可以执行的格式），dex文件最终会被打包进APK文件。

5. ApkBuilder工具会将编译过的资源及未编译过的资源（如图片等）以及.dex文件打包成APK文件。

6. 生成APK文件后，需要对其签名才可安装到设备，正式发布时必须使用release版的keystore对应用进行签名。

7. 如果对APK正式签名，还需要使用zipalign工具对APK进行对齐操作，这样做的好处是当应用运行时会提高速度，但是相应的会增加内存的开销。 
