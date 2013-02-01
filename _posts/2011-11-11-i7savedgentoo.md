---
layout: post
title: i7 拯救了 Gentoo
---

好多年以前，我开始了 Linux 之旅。 

我频繁的切换发行版，从 RedFlag 5 到 RedFlag 6 再到 ubuntu 8.10 马上又切到 Fedora 8 然后是 9 10 , 期间又重回 RedFlag 7 过，间或也使用过 debian 和 puppy。 

如此频繁的切换发行版，只不过是我觉得它们都没有满足我的要求。我不停的切换，企图寻找到一个可以一劳永逸的满足我的需求的系统。 我不停的搜索着，尝试着。 

直到有一天，我发现了 Gentoo 。 

那个时候我已经厌倦了 Fedora . 不停的重装，只是为了升级。尝试过 upgrade , 只不过比不得重装快活。也有很多错误，不如重装干脆。 

我实在是厌倦了每6个月重装一下系统，只是为了升级。我也厌倦了每次重装好系统之后都要开始好几天的清理工作，把不需要的东西删除掉，再把需要的软件又重装一次。 

直到我发现了 Gentoo . 

经过漫长的编译后，我有了一个可以启动的 Gentoo 系统。但是没有 X , 没有gnome，没有 firefox . 啥都没有。 

编译 firefox 前我需要 DE ，我选择 gnome, 编译 gnome 前我需要 X . 为了编译 firefox , 5 百多个软件需要被编译。 

又是一个 overnight 编译。 

第3天早上，打开显示器查看的时候终于编译好了。期间因为编译错误中断过几次，手工排除了一些错误，继续编译。不过总的来说，一个可以上网的环境就编译好了。 

接下来是更多的编译。因为我需要的一切都没有安装。编译 编译 编译 编译 编译 从此成了吃饭后的重要工作。 

过了一个多星期，我终于有了和从前“一样”的环境了。却少了很多垃圾。我不需要的统统用 USE="-XXX" 的方式清理掉了。 

当 irc 上的朋友发现我失踪了几天再回来的时候，我已经满口的 Gentooooooooooooooooooooo 了。我不停的向大家赞叹 Gentoo 有多么优秀。只有等到数个月后，我才进入了平静期。 

我享受了近2年的 Gentoo 。 

我厌倦了。 

无休止的编译炸干了我最后一点耐性。Firefox 也进入了快速发布时代，而每次firefox 升级，带来的都是数个小时的编译。期间我的电脑由于编译而开始响应缓慢，数个小时不能流畅使用的状态！ 

而每次 firefox 升级，都意味着 thunderbird 也要升级。 这些都是需要编译数个小时的大块头。 

每天都要升级，否则积累起来可能就是要一天时间都编译不完的更新。 

我差一点就放弃 Gentoo 了！直到我下了一次狠心，入手了一个 Sandy Bridge 的 Xeon E3-1230 . 其实就是 i7 2600 阉割了GPU. 因为我有显卡，不想再为核芯显卡掏钱。而且我只要 NVIDIA 的显卡。别的统统不要。 

组装好 CPU 后，我被 i7 的性能惊呆了！ 

编译 Ooo 居然也不过几十分钟的事情，以前的电脑可是要编译一整个下午的。这次居然只有几十分钟。 

而在编译的过程中，我的电脑一直都处于可用状态，没有因为大量的后台编译而损失性能。 

由于换了 cpu 和主板需要重新编译内核，这次内核的编译时间也让我惊呆了，89s ! 89s! 以前的电脑需要一个多小时啊！89s 只相当于我没有 make clean 而是改动了几个选项重新 make 的时间。 

这次 89s 是全部重新编译的时间。如果我修改了个东西，再编译一下，那只需要几秒到十几秒就好了。 

开了 pgo （需要编译2次） 的 firefox 也只有几分钟就编译好了。 

从此编译再也不是等待，而是一个享受的过程，看到一个巨大的软件，却只需要不到一分钟就编译好了，非常大的软件也只要几分钟就可以了，这不是一种享受是什么？ 

i7 , 让 Gentoo 这种全编译的操作都变得非常快速。 i7 你是一个神话。 

感谢 i7 ， 你拯救了 Gentoo 。 

 