---
title: android 命令行签名apk
date: 2016-07-31 16:24:53 +0800
tags: android sign
categories: android
---

### **生成keystore文件**

``` bash
keytool -genkey -alias myalias(别名) -keyalg RSA(加密方式) -validity 20000(有效时间，单位天) -keystore myandroid.keystore(文件名)
``` 

#### **步骤如下**
``` bash
输入密钥库口令:
再次输入新口令:
您的名字与姓氏是什么?
[Unknown]: king
您的组织单位名称是什么?
[Unknown]: test.com
您的组织名称是什么?
[Unknown]: test.com
您所在的城市或区域名称是什么?
[Unknown]: Beijing
您所在的省/市/自治区名称是什么?
[Unknown]: Beijing
该单位的双字母国家/地区代码是什么?
[Unknown]: 86
CN=king, OU=test.com, O=test.com, L=Beijing, ST=Beijing, C=86是否正确?
[否]: y(y确定)

``` 

myandroid.keystore文件生成完成


### **查看生成的keystore文件**

``` bash
keytool -list -keystore "myandroid.keystore"
``` 

### **重新签名apk**
``` bash
jarsigner -verbose -keystore myandroid.keystore -signedjar singggg.apk(签名输出apk) unsign.apk(被签名apk) myalias(别名)
``` 

**可能遇到的异常信息**
jarsigner: 无法对 jar 进行签名: java.util.zip.ZipException: invalid entry compressed size (expected 18246 but got 18572 bytes)

**需要移除原apk中的签名信息**
``` bash
zip -d xxxx.apk META-INF/*
``` 
**重复签名上面的签名步骤**

*备注:* jdk 1.7如果按照上面的步骤签名出现问题,请增加参数**-digestalg SHA1 -sigalg MD5withRSA**
``` bash
jarsigner -verbose -digestalg SHA1 -sigalg MD5withRSA -keystore myandroid.keystore -signedjar singggg.apk(签名输出apk) unsign.apk(被签名apk) myalias(别名)
``` 

