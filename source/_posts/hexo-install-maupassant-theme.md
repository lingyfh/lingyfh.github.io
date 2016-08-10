---
title: hexo install maupassant theme
date: 2016-08-10 21:09:32
tags: hexo
categories: hexo
---

### **maupassant安装 [参考地址](https://www.haomwei.com/technology/maupassant-hexo.html)**
``` bash
git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant
npm install hexo-renderer-jade --save
npm install hexo-renderer-sass --save

```

编辑Hexo目录下的 _config.yml，将theme的值改为maupassant。



### 安装遇到问题  [参考地址](http://www.rockcoding.com/2016/03/02/hexo/)
``` bash
安装cnpm

npm uninstall  hexo-renderer-sass --save
npm uninstall  hexo-renderer-jade --save

cnpm install hexo-renderer-jade --save
cnpm install hexo-renderer-sass --save

```

