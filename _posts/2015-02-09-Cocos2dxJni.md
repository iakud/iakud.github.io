---
layout: post
title: "Cocos2d-x通过Jni实现C++与Java相互调用"
date: 2015-02-09 12:00:01 +0800
categories: cocos2d
---

在cocos2dx项目中与运营平台(java sdk)对接时使用了JNI。
<!--more-->
###通过C++调用Java

在JniUtil.h文件中如下实现：
```cpp
#ifndef _JNIUTIL_H_
#define _JNIUTIL_H_

class JniUtil {
public:
	void static login(const char* zoneId, const char* zoneName);
};

#endif // _JNIUTIL_H_
```
在JniUtil.cpp文件中如下实现：
```cpp
#include "JniUtil.h"

#include <jni.h>
#include "platform/android/jni/JniHelper.h"

void JniUtil::login(const char* zoneId, const char* zoneName) {
	JniMethodInfo minfo;
	if (JniHelper::getStaticMethodInfo(minfo, "com/platform/test/JniUtil",
		"login", "(Ljava/lang/String;Ljava/lang/String;)V")) {

		jstring jzoneId = minfo.env->NewStringUTF(zoneId);
		jstring jzoneName = minfo.env->NewStringUTF(zoneName);
		minfo.env->CallStaticVoidMethod(minfo.classID, minfo.methodID, jzoneId, jzoneName);

		minfo.env->DeleteLocalRef(minfo.classID);
		minfo.env->DeleteLocalRef(jzoneId);
		minfo.env->DeleteLocalRef(jzoneName);
	}
}
```
Java的实现：
```java
package com.platform.test;

public class JniUtil {    
	private static void login(String zoneId, String zoneName) {
		// do
	}
}
```
###通过Java调用C++

在java的JniUtil类中定义一个方法，用于提供给java调用C++：
```java
package com.platform.test;

public class JniUtil {
	public static native void onLogin(String result);
}
```
在JniUtil.cpp文件中如下实现：

方法名与java类中的包名+方法名，以下划线连接
```cpp
extern "C" {
	void Java_com_platform_test_JniUtil_onLogin(JNIEnv*  env, jobject thiz, jint jresult) {
		const char* result = env->GetStringUTFChars(jresult, NULL);
		CCLOG("onLogin : %s", result);
		env->ReleaseStringUTFChars(jresult, result);
	}
}
```