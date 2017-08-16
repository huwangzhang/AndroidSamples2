# AndroidSamples2
NDK学习
1、NDK下载 https://developer.android.com/ndk/downloads/index.html
2、在android studio中配置NDK路径
  点击菜单栏File->ProjectStructure ->SDKLocation->在左侧Android NDK Location加入NDK目录
3、Make当期的工程文件
4、引用本地方法的类
  static {
        System.loadLibrary("myNativeLib");
  }
  并且加入本地测试方法：public native String getStringFromNative();
5、在terminal窗口中，cd到当前项目的java目录下，javah -d ../jni 你的包名.引用本地方法的类的名称
  最后会生成一个jni目录，生成.h头文件
6、配置build.gradle文件
  ndk {
      moduleName"myNativeLib"
      ldLibs "log", "z", "m"
      abiFilters "armeabi", "armeabi-v7a", "x86"
  }
  moduleName与将来在Java类中使用System.loadLobrary(“本地库名称”);以及生成的.so文件名称对应
7、配置local.properties文件和gradle.properties文件
 打开工程目录下的gradle.properties文件加入 android.useDeprecatedNdk=true
8、 编写生成.h文件的源文件
  核心部分：
  JNIEXPORT jstring
  JNICALL
  Java_com_example_huwang_ndkdemo_MainActivity_getStringFromNative
        (JNIEnv *env, jobject jObj) {
    LOGE("log string from ndk.");
    return env->NewStringUTF("HelloFrom JNI!");
  }
9、调用测试
