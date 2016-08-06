---
title: debian openresty 安装
date: 2016-08-03 16:24:53 +0800
tags: Debian openresty nginx
categories: Linux
---


### **[openresty 中文官网](https://openresty.org/cn/)**

* 使用wget将安源码下载下来
``` bash
wget https://openresty.org/download/openresty-1.9.15.1.tar.gz
``` 

* 解压
``` bash
tar xzvf openresty-1.9.15.1.tar.gz
```

* 验证安装环境 [可能遇到的问题]
``` bash
cd openresty-1.9.15.1
./configure
```

* 安装
``` bash
make
make install
```

* 配置环境变量
``` bash
vi .profile
在profile文件中增加
export PATH="/usr/local/openresty/nginx/sbin:$PATH"
保存
source .profile
```

* 执行nginx测试


[可能遇到的问题]: #notice "请看下面的备注"

#### <a name="notice">问题</a>

* 未安装Lua,安装Lua
``` bash
wget http://www.lua.org/ftp/lua-5.3.3.tar.gz
tar zxf lua-5.3.3.tar.gz
cd lua-5.3.3
make linux test
export PATH=$PATH:/sbin
```

* 未安装gcc,安装gcc
``` bash
sudo apt-get update
sudo apt-get install build-essential
```

* Lua相应的库找不到readline/readline.h:no such file or directory
``` bash
sudo apt-get install libreadline-dev
```

* Luajit you need to have ldconfig in your PATH env when enabling luajit.
``` bash
是因为找不到命令ldconfig, 这个命令一般是在/sbin/目录下的，所以先执行
export PATH=$PATH:/sbin
```

* 本地无pcre
``` bash
sudo apt-get install libpcre3 libpcre3-dev
```

* 本地无ssl
``` bash
sudo apt-get install libssl-dev
```

* 安装多个依赖包
``` bash
sudo apt-get install libreadline-dev libncurses5-dev libpcre3-dev libssl-dev perl make
```
