---
title: Mac系统上安装FFmpeg
date: 2020-02-18 16:20:10
tags: 工具
categories: 其他
---

## 1.下载FFmpeg
先进入要存放下载文件的目录，比如要放在/Users/qinjian/Downloads/ffm目录，先执行命令：
```shell script
cd /Users/qinjian/Downloads/ffm
```
再执行下载的命令：
```shell script
git clone https://git.ffmpeg.org/ffmpeg.git
```
## 2.编译FFmpeg
先执行下面命令进入ffmpeg目录：
```shell script
cd /Users/qinjian/Downloads/ffm/ffmpeg
```
再执行下面命令配置configure：
```shell script
./configure --prefix=/usr/local/ffmpeg  --enable-gpl  --enable-nonfree  --enable-libfdk-aac  --enable-libx264  --enable-libx265 --enable-filter=delogo --enable-debug --disable-optimizations --enable-libspeex --enable-videotoolbox --enable-shared --enable-pthreads --enable-version3 --enable-hardcoded-tables --cc=clang --host-cflags= --host-ldflags=
```
如果报错`nasm/yasm not found or too old. Use --disable-x86asm for a crippled build`的话，先执行下面命令安装yasm然后再执行配置configure的命令。
```shell script
brew install yasm
```
如果报错`ERROR: libfdk_aac not found`的话，先执行下面命令安装fdk-aac然后再执行配置configure的命令。
```shell script
brew install fdk-aac
```
## 3.安装FFmpeg
执行下面命令来安装：
```shell script
make && make install

// 如果安装出现下图的错误的话就用这条命令来安装
sudo make && sudo make install
```
安装成功后ffmpeg所在的目录是/usr/local/ffmpeg。
## 4.配置环境变量
安装成功后要输入ffmpeg的全路径才能调用ffmpeg命令：
```shell script
/usr/local/ffmpeg/bin/ffmpeg -version
```
我们可以配置环境变量，配置环境变量后可以直接通过ffmpeg -version来调用命令。
先执行下面命令打开环境变量配置文件
```shell script
vi ~/.bash_profile
```
在配置文件加入ffmpeg的bin文件夹路径：
```shell script
export PATH=$PATH:/usr/local/ffmpeg/bin
```
然后输入:wq保存退出，再执行下面命令让刚配置的环境变量生效：
```shell script
source ~/.bash_profile
```
其他参考: <a href="https://www.jianshu.com/p/85f905ddb36f">地址</a>




