---
layout: post
title: 折腾了一下GPG和智能卡
tags: [crypto]
---

GPG支持使用智能卡设备存储密钥。
使用文件系统来携带密钥（ 不管是存到 家目录里还是放到U盘里携带）的弊端有很多

    1. 文件泄漏密钥即泄漏
    2. 不能保证密钥的物理唯一性
    3. 文件可以被拷贝，文件可以被任意读取
    4. 无法知道文件是否被拷贝，不能发觉
    

而使用智能卡设备，好处很多

  
    1. 密钥具有物理唯一性，一旦生成即无法读取，即无法制作拷贝
    2. 密钥可以由卡上的CPU生成，更能保证物理唯一性
    3. 智能卡可以随身携带，方便使用不同的电脑
    4. 卡遗失即密钥遗失
    5. 多次尝试破解会导致智能卡自我销毁，保证密钥的安全，所以遗失的卡也不会泄漏密钥
    6. 加解密操作由智能卡硬件完成，即便授权应用程序也无法获取KEY。就别说木马了。
    
    
那么， gpg 支持哪些硬件呢？

gpg 支持 CCID 标准兼容的 USB 读卡器。市面上的智能卡读卡器基本上都基于这个接口。
但是，智能卡有很多种，什么门卡啊，电话卡啊，种类很多， gpg 需要的是一个 "OpenPGP Card" 标准兼容的智能卡。
这种智能卡目前在国内没有购买渠道。
    
所幸的是，gpg通过 gnupg-pkcs11-scd 兼容支持了 PKI 设备。
所谓 PKI 设备就是使用 PKCS\#11 接口进行访问的物理加解密设备，比如电子狗，U盾。

那么，我需要的是一个（国内能购买到的）具备 RSA 或者 DSA 加密运算能力的 PKI 设备，同时能在 Linux 下使用。
能在 Linux 下使用，要么是使用标准的 PC/SD 接口，要么是提供了 Linux 下的驱动（PKCS\#11 接口允许厂商提供一个 .so 作为驱动访问硬件。应用程序只要使用 PKCS\#11 的接口即可。）

一个 “国内能购买到的” 前提条件，让我的选择范围一下子缩小了。最好能使用标准接口，这样无需额外驱动即可使用。
但是这样的好东西国内似乎很难买到，基本上都需要驱动，而且是 Windows Only。
于是退求其次，找到了 “eToken Pro 72k” 这个设备。他支持 RSA 算法，密钥支持 2048bit 长度，同时也提供了 Linux 驱动。

于是赶紧购买。驱动在一个 Overlay 里找了， emerge 一下就好了。

    UPDATE:
    后来看到 debian wiki 里的智能卡页面发现其实也有国产的也能 Linux 下跑啦。





