---
author: Kyle Thielk
comments: true
date: 2014-04-25 16:23:21+00:00
layout: post
slug: include-native-so-library-in-apk-with-android-studio
title: Include native *.so library in APK with Android Studio
wordpress_id: 1042
categories:
- Tech
---

Using the Android NDK is well documented throughout the internet *if* you are still using Eclipse. The process is basically the same with Android Studio until the time comes to build your APK. The APK will build fine, but your library *.so file will be missing from the APK and when you attempt to load it with System.loadLibrary("mylibrary") you will get:

{% codeblock lang:java %}java.lang.UnsatisfiedLinkError: Couldn't load mylibrary from loader dalvik.system.PathClassLoader{% endcodeblock %}

You can view the contents of your APK with the following command to verify libmylibrary.so is not in the APK:

{% codeblock lang:bash %}unzip -l MyApp.apk{% endcodeblock %}

Android Studio's build tool will not look in the usual place:

{% codeblock lang:bash %}
SharedLibrary  : libmylibrary.so
Install        : libmylibrary.so => libs/armeabi/libmylibrary.so
{% endcodeblock %}

The trick (thanks to this thread: [https://groups.google.com/forum/#!msg/adt-dev/nQobKd2Gl_8/Z5yWAvCh4h4J](https://groups.google.com/forum/#!msg/adt-dev/nQobKd2Gl_8/Z5yWAvCh4h4J)) is to move libmylibrary.so to:

{% codeblock lang:bash %}src/main/jniLibs/armeabi/libmylibrary.so{% endcodeblock %}

Voila, your native code is now compiled into your APK. This should work with build tools 0.8+
