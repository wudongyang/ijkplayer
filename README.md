# ijkplayer

 Platform | Build Status
 -------- | ------------
 Android | [![Build Status](https://travis-ci.org/bbcallen/ci-ijk-ffmpeg-android.svg?branch=master)](https://travis-ci.org/bbcallen/ci-ijk-ffmpeg-android)
 iOS | [![Build Status](https://travis-ci.org/bbcallen/ci-ijk-ffmpeg-ios.svg?branch=master)](https://travis-ci.org/bbcallen/ci-ijk-ffmpeg-ios)

Video player based on [ffplay](http://ffmpeg.org)

### Download

- Android:
 - Gradle
```
# required
allprojects {
    repositories {
        jcenter()
    }
}

dependencies {
    # required, enough for most devices.
    compile 'tv.danmaku.ijk.media:ijkplayer-java:0.6.0'
    compile 'tv.danmaku.ijk.media:ijkplayer-armv7a:0.6.0'

    # Other ABIs: optional
    compile 'tv.danmaku.ijk.media:ijkplayer-armv5:0.6.0'
    compile 'tv.danmaku.ijk.media:ijkplayer-arm64:0.6.0'
    compile 'tv.danmaku.ijk.media:ijkplayer-x86:0.6.0'
    compile 'tv.danmaku.ijk.media:ijkplayer-x86_64:0.6.0'

    # ExoPlayer as IMediaPlayer: optional, experimental
    compile 'tv.danmaku.ijk.media:ijkplayer-exo:0.6.0'
}
```
- iOS
 - in coming...

### My Build Environment
- Common
 - Mac OS X 10.11.5
- Android
 - [NDK r10e](http://developer.android.com/tools/sdk/ndk/index.html)
 - Android Studio 2.1.2
 - Gradle 2.10
- iOS
 - Xcode 7.3 (7D175)
- [HomeBrew](http://brew.sh)
 - ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
 - brew install git

### Latest Changes
- [NEWS.md](NEWS.md)

### Features
- Common
 - remove rarely used ffmpeg components to reduce binary size [config/module-lite.sh](config/module-lite.sh)
 - workaround for some buggy online video.
- Android
 - platform: API 9~23
 - cpu: ARMv7a, ARM64v8a, x86 (ARMv5 is not tested on real devices)
 - api: [MediaPlayer-like](android/ijkplayer/ijkplayer-java/src/main/java/tv/danmaku/ijk/media/player/IMediaPlayer.java)
 - video-output: NativeWindow, OpenGL ES 2.0
 - audio-output: AudioTrack, OpenSL ES
 - hw-decoder: MediaCodec (API 16+, Android 4.1+)
 - alternative-backend: android.media.MediaPlayer, ExoPlayer
- iOS
 - platform: iOS 6.0~9.3.x
 - cpu: armv7, arm64, i386, x86_64, (armv7s is obselete)
 - api: [MediaPlayer.framework-like](ios/IJKMediaPlayer/IJKMediaPlayer/IJKMediaPlayback.h)
 - video-output: OpenGL ES 2.0
 - audio-output: AudioQueue, AudioUnit
 - hw-decoder: VideoToolbox (iOS 8+)
 - alternative-backend: AVFoundation.Framework.AVPlayer, MediaPlayer.Framework.MPMoviePlayerControlelr (obselete since iOS 8)

### NOT-ON-PLAN
- obsolete platforms (Android: API-8 and below; iOS: pre-6.0)
- obsolete cpu: ARMv5, ARMv6, MIPS (I don't even have these types of devices…)
- native subtitle render
- avfilter support

### Before Build
```
# install homebrew, git, yasm
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install git
brew install yasm

# add these lines to your ~/.bash_profile or ~/.profile
# export ANDROID_SDK=<your sdk path>
# export ANDROID_NDK=<your ndk path>

# on Cygwin (unmaintained)
# install git, make, yasm
```

- If you prefer more codec/format
```
cd config
rm module.sh
ln -s module-default.sh module.sh
cd android/contrib
# cd ios
sh compile-ffmpeg.sh clean
```

- If you prefer less codec/format for smaller binary size (include hevc function)
```
cd config
rm module.sh
ln -s module-lite-hevc.sh module.sh
cd android/contrib
# cd ios
sh compile-ffmpeg.sh clean
```

- If you prefer less codec/format for smaller binary size (by default)
```
cd config
rm module.sh
ln -s module-lite.sh module.sh
cd android/contrib
# cd ios
sh compile-ffmpeg.sh clean
```

- For Ubuntu/Debian users.
```
# choose [No] to use bash
sudo dpkg-reconfigure dash
```

- If you'd like to share your config, pull request is welcome.

### Build Android
```
git clone https://github.com/Bilibili/ijkplayer.git ijkplayer-android
cd ijkplayer-android
git checkout -B latest k0.6.0

./init-android.sh

cd android/contrib
./compile-ffmpeg.sh clean
./compile-ffmpeg.sh all

cd ..
./compile-ijk.sh all

# Android Studio:
#     Open an existing Android Studio project
#     Select android/ijkplayer/ and import
#
#     define ext block in your root build.gradle
#     ext {
#       compileSdkVersion = 23       // depending on your sdk version
#       buildToolsVersion = "23.0.0" // depending on your build tools version
#
#       targetSdkVersion = 23        // depending on your sdk version
#     }
#
# Eclipse: (obselete)
#     File -> New -> Project -> Android Project from Existing Code
#     Select android/ and import all project
#     Import appcompat-v7
#     Import preference-v7
#
# Gradle
#     cd ijkplayer
#     gradle

```


### Build iOS
```
git clone https://github.com/Bilibili/ijkplayer.git ijkplayer-ios
cd ijkplayer-ios
git checkout -B latest k0.6.0

./init-ios.sh

cd ios
./compile-ffmpeg.sh clean
./compile-ffmpeg.sh all

# Demo
#     open ios/IJKMediaDemo/IJKMediaDemo.xcodeproj with Xcode
# 
# Import into Your own Application
#     Select your project in Xcode.
#     File -> Add Files to ... -> Select ios/IJKMediaPlayer/IJKMediaPlayer.xcodeproj
#     Select your Application's target.
#     Build Phases -> Target Dependencies -> Select IJKMediaFramework
#     Build Phases -> Link Binary with Libraries -> Add:
#         IJKMediaFramework.framework
#
#         AudioToolbox.framework
#         AVFoundation.framework
#         CoreGraphics.framework
#         CoreMedia.framework
#         CoreVideo.framework
#         libbz2.tbd
#         libz.tbd
#         MediaPlayer.framework
#         MobileCoreServices.framework
#         OpenGLES.framework
#         QuartzCore.framework
#         UIKit.framework
#         VideoToolbox.framework
#
#         ... (Maybe something else, if you get any link error)
# 
```


### Support (支持) ###
- Please do not send e-mail to me. Public technical discussion on github is preferred.
- 请尽量在 github 上公开讨论[技术问题](https://github.com/bilibili/ijkplayer/issues)，不要以邮件方式私下询问，恕不一一回复。


### License

```
Copyright (C) 2013-2016 Zhang Rui <bbcallen@gmail.com> 
Licensed under LGPLv2.1 or later
```

ijkplayer required features are based on or derives from projects below:
- LGPL
  - [FFmpeg](http://git.videolan.org/?p=ffmpeg.git)
  - [libVLC](http://git.videolan.org/?p=vlc.git)
  - [kxmovie](https://github.com/kolyvan/kxmovie)
- zlib license
  - [SDL](http://www.libsdl.org)
- BSD-style license
  - [libyuv](https://code.google.com/p/libyuv/)
- ISC license
  - [libyuv/source/x86inc.asm](https://code.google.com/p/libyuv/source/browse/trunk/source/x86inc.asm)

android/ijkplayer-exo is based on or derives from projects below:
- Apache License 2.0
  - [ExoPlayer](https://github.com/google/ExoPlayer)

android/example is based on or derives from projects below:
- GPL
  - [android-ndk-profiler](https://github.com/richq/android-ndk-profiler) (not included by default)

ios/IJKMediaDemo is based on or derives from projects below:
- Unknown license
  - [iOS7-BarcodeScanner](https://github.com/jpwiddy/iOS7-BarcodeScanner)

ijkplayer's build scripts are based on or derives from projects below:
- [gas-preprocessor](http://git.libav.org/?p=gas-preprocessor.git)
- [VideoLAN](http://git.videolan.org)
- [yixia/FFmpeg-Android](https://github.com/yixia/FFmpeg-Android)
- [kewlbear/FFmpeg-iOS-build-script](https://github.com/kewlbear/FFmpeg-iOS-build-script) 

### Commercial Use
ijkplayer is licensed under LGPLv2.1 or later, so itself is free for commercial use under LGPLv2.1 or later

But ijkplayer is also based on other different projects under various licenses, which I have no idea whether they are compatible to each other or to your product.

[IANAL](https://en.wikipedia.org/wiki/IANAL), you should always ask your lawyer for these stuffs before use it in your product.



### Build Q&A
* [参考1](http://www.liaoxuefeng.com/article/0013738927837699a7f3407ea5f4b5caf8e1ab47997d7c5000)
* [官方 OSX 编译](http://ffmpeg.org/trac/ffmpeg/wiki/MacOSXCompilationGuide)
* Q: yasm/nasm not found or too old. Use --disable-yasm for a crippled build.

  A: install yasm. MAC: brew install yasm

* Q: ijkplayer-sample 总是提示需要 android-support-v7-appcompat 
  
  A: 
  
  	1. 工程中导入自己的 android-support-v7-appcompat
  	2. 如果依然有叹号，并且在关闭项目再打开时提示 .project 缺少 android-support-v7-appcompat 的话，那就要在 java build path - project 中删除原先的 android-support-v7-appcompat
  	3. 

* Q: 提示 ArrayList 需要添加类型

  A: 设置 JRE 1.7

* Q: 提示 android-support-v7-preference 找不到

	A: 先把 /Users/wdy/dev/android-sdk-macosx/extras/android/support/v7/preference 引入到 Eclipse，但总是编译失败。后在 github 上找https://github.com/dandar3/android-support-v7-preference 找到一个 Eclipse 的工程，尝试编译。注意调整了 JAVA Build Path - Projects，Add “v7-appcompat” 和
“V7-recycleview” 以及在 libs 添加了 “android-support-v4.jar”。

	所需的lib都是通过 SDK/extras/android/support/ 中导入Eclipse的工程，v4.jar 是直接从 SDK 下拷贝的。
	
	还有一个方案是把 aar 变成 Eclipse 的工程。[参考](https://commonsware.com/blog/2014/07/03/consuming-aars-eclipse.html)
	
	1. UnZIP the AAR into some directory.
	
	2. Create an empty directory that will be the home for the Android library project. For the rest of these steps, I will refer to this as “the output directory”.
	
	3. Copy the AndroidManifest.xml, res/, and assets/ directories from the AAR into the output directory.
	
	4. Create a libs/ directory in the output directory. Copy into libs/ the classes.jar from the root of the unZIPped AAR, plus anything in libs/ in the AAR (e.g., mediarouter-v7 has its own JAR of proprietary bits).
	
	5. Decide what build SDK you want to try to use. You might just choose the highest SDK version you have installed. Or, you can use the android:minSdkVersion and the -vNN resource set qualifiers to get clues as to what a good build SDK might be. If desired, create a project.properties file with a target=android-NNN line, where NNN is your chosen build SDK. Or, you can address this in Eclipse later on.
	
	6. Import the resulting project into Eclipse, and if needed adjust the build SDK (Project > Properties > Android). Also, you will need to attach to this library project any library projects it depends upon (e.g., mediarouter-v7 depends upon appcompat-v7).
		

* Q: ijkplayer-sample 编译是总提示 java.lang.NullPointerException

  A: 由于Eclipse中使用SVN插件所致。svn在checkout的所有文件的文件夹下都会生成一个.svn文件用于管理版本库，而新版的adt加强了对项目文件中的异常类型文件的识别，所以在遇到.svn时报了空指针的错误。

  用方法二并将API设置成了24（因为 preference-master 对应的是24）
  解决方法一：删除掉工程中所有的.svn目录，重新编译  
  解决方法二：
  	
  	1. Open properties of project in Eclipse then Resources -> Resource filters.
	2. Click the "Add..." button -> Check "Exclude all", "Files and folders", "All children". 
	3. In the text entry box input ".svn" (without quotes).
	4. Restart Eclipse.

* Q: Android SDK Manager 更新失败

  A: 注意看 Manager 里是否使用了代理，使用代理就不用 VPN。用 VPN 一般可以不用使用代理。去掉代理记得去掉下方的： "Force https://...... usering http://... "

* Q: android-support-v7-preference 项目是用 SDK 24中的aar生成的，所以必须用24，用23会提示android:id/list_container找不到。

	A： 可以考虑参考 23 的 preference，把android:id/list_container 改成 +id/list_container。可以解决此问题。
