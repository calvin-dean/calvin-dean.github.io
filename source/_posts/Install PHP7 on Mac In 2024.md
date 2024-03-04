---
title: MacInstallPHP7
date: 2024-03-05 00:45:57
tags:
---

众所周知,php7版本已经不维护了,macos安装更加困难,下面会介绍如何安装php7及拓展

#### step1:brew安装别人准备好的php7,来源:https://github.com/shivammathur/homebrew-php/

```shell
brew install shivammathur/php/php@7.4
```

#### step2:写入环境变量(方便以后切换版本),也可以用brew连接使用

```shell
PHP7_HOME=/usr/local/Cellar/php@7.4/7.4.33_5
PHP8_HOME=/usr/local/Cellar/php@8.x/8.x.x
#PHP切换版本用
PHP_VERSION=$PHP7_HOME

PATH=$PHP_HOME/bin:$PATH
PATH=$PHP_HOME/sbin:$PATH
PATH=$PHP_HOME/composer:$PATH
```

#### step3:由于默认php环境变量优先级高于.zshrc,所以要卸载系统自带的php(或brew先连接的)

```shell
brew uninstall php@8.x.x
```

#### step4:我使用的laravel一般要拓展pecl_http,最新的http拓展仅支持php8+,所以要找对应的http拓展版本,推荐使用pecl安装pecl_http-3.3.0,pecl_http git版本地址:https://github.com/m6w6/ext-http/releases

```shell
pecl install pecl_http-3.3.0
```

#### step5:然后就会发现会有很多拓展需要安装,brew安装对应的拓展

```shell
#这是brew可以直接安装的
brew install zlib curl libevent icu4c libidn libidn2
```

#### step6:源码安装idnkit(需要先安装libiconv,open source里面有对应的地址),apple open source地址:https://opensource.apple.com/source/bind9/bind9-57.4/bind9/contrib/idn/idnkit-1.0-src/INSTALL.auto.html,然后就是编译安装3步骤了,安装idnkit的时候可能需要输入libiconv的地址,编译安装的时候要记得安装地址

```shell
make&&make install
```

#### step7:回到安装http拓展的页面对应输入地址

```shell
/usr/local/Cellar/zlib/1.3.1
/usr/local/Cellar/curl/8.6.0
/usr/local/Cellar/libevent/2.1.12_1
/usr/local/Cellar/icu4c/74.2
/usr/local/Cellar/libidn2/2.3.7
/usr/local/Cellar/libidn/1.42
/usr/local/
/usr/local/
```

#### step8:安装完成,一般pecl会自动开启拓展,可以使用php -m查看,没开启的话就找php.ini手动开启

```shell
#查看php拓展
php -m
#查看php信息
php -i
#开启http拓展
extension=http.so
```
