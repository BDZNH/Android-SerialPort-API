# 描述
这里放一些如何使用 Android Serial Port API 的内容。

[google-official-sourcecode](#/google-official-sourcecode)目录放的是谷歌的官方代码，已经是2009年编写的了。

按照百度或者谷歌能得到的信息，大都是将官方编译好的so和源码内的jni放到app的源码目录内编译。但是实际上这样去做的时候，内置的so是32位的，如果放在64位的系统上运行，再如果app没有标示出自己需要使用32位的so，那么这里就会走一些弯路，特别是没有这方面的经验的情况下。

我觉得就目前来说，更好的一种做法是将jni目录内置到app源码目录内，然后在编译的时候将so编译出来。
下面是我的做法。 

1. 把目录 [jni](#/google-official-sourcecode/android-serialport-api/trunk/android-serialport-api/project/jni) 里的文件放到你的app源码目录的`app/src/main/jni`(此目录可能需要自己建立)
2. 把以下字段添加到你的`app/build.gradle`文件的`android`字段。
```java
    externalNativeBuild {
        ndkBuild {
            path file('src/main/jni/Android.mk')
        }
    }
```
在我这里添加完成后，android字段的全文应该如下
```java
android {
    compileSdkVersion 29
    buildToolsVersion "29.0.3"

    defaultConfig {
        applicationId "com.example.myapplication"
        minSdkVersion 26
        targetSdkVersion 29
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    externalNativeBuild {
        ndkBuild {
            path file('src/main/jni/Android.mk')
        }
    }
}
```

3. 安装 `NDK(side by side)`，如果你在Android Studio里面更改了 `app/build.gradle` 文件，Android Studio 应该会自动提示你安装NDK。

## jniLibs

目录 [jniLibs](#/jniLibs) 里面放了编译好的 `arm64-v8a` 和 `x86-64` 的 so ，如果你懒得自己编译的话，可以尝试使用。