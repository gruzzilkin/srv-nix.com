---
title: "Сборка и установка модуля php-sweph"
published: true
layout: post
date: 2014-04-23 13:25:55 +0600
comments: true
categories: linux
author: A. Semenov
tags: 
- centos
- linux
- php
- build
---

Просто абзац

<!--more-->

{% highlight bash %}
yum install php-devel php subversion -y
BUILD=/usr/src
SWEURL=ftp://ftp.astro.com/pub/swisseph/swe_unix_src_1.80.00.tar.gz
INCLUDEDIR=/usr/local/include
LIBDIR=/usr/local/lib
PHPMODDIR=/usr/lib64/php/modules
pushd $BUILD
wget $SWEURL
mkdir -p $BUILD/swe
tar xf $(basename $SWEURL) -C $BUILD/swe
ln -s $BUILD/swe/src/libswe.a $LIBDIR
ln -s $BUILD/swe/src/sweodef.h $INCLUDEDIR
ln -s $BUILD/swe/src/swephexp.h $INCLUDEDIR
svn checkout http://php-sweph.googlecode.com/svn/trunk/ php-sweph
pushd php-sweph
phpize
./configure  --enable-sweph
make
rm $LIBDIR/libswe.a $INCLUDEDIR/{sweodef.h,swephexp.h}
ln -sf $BUILD/php-sweph/modules/sweph.so $PHPMODDIR
cat >/etc/php.d/sweph.ini <<EOF
extension=sweph.so
EOF
php -m | grep sweph
{% endhighlight %}

