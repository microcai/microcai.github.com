---
layout: post
title: 那些年我们追过的网络库(PartI)
---

#为什么要用 C++ 编写服务端程序？
如果说答案是性能，那么肯定有人会满不在乎。觉得性能不够的话， 只要加机器就可以了。
然而更少的机器，意味着更低的能耗，更少的硬件投入，更少的人力资源投入去维护机器。总而言之，更低的成本。

肯定会有人说，C++的开发速度太慢了。然而这并不是绝对的。C++也可以做到非常快速的开发。有句俗语 * “脚本一时爽，重构火葬场” * 说的正是脚本语言开发的项目进入维护阶段后无穷的灾难。而 C++ 经过了几十年的发展, 拥有庞大的工具链. 不管是动态分析还是静态分析都有大量的工具, 能极大的帮助程序员减少错误.
c++得益于精良的设计，严格的检查，越是大型的工程，越是能降低开发成本。

但这并不意味着C++就不适合小型项目了。小型的项目，也可以快速开发。因为 C++11 开始，已经 *感觉像是全新的语言了*，可以完全以脚本的形式去使用，获得接近甚至超越脚本语言的开发速度，同时得益于编译优化，获得不俗的运行时性能。
C++正是鱼和熊掌得兼的语言。

#为什么要用asio这个库？

事实上如果使用C++开发服务端程序，你有多得数不清的选择。什么 ACE 啦，libuv 啦，libevent 啦，libev 啦，甚至可以直接使用 epoll/iocp 这样的系统API。
为什么要用 asio 呢？

##那些年我们用过的网络库

在计算机史前文明时代, 曾经有个世界难题, 叫 *"c10k problem"*. 这个是继 y2k problem 后的又一个重大攻关项目. 全世界的文艺青年都想拿下解决这个问题的荣誉, 正可谓八仙过海, 各显神通.

那一年, NPTL 还没有研究出来. 还不能创建成千上万个线程 <br>
那一年, windows 还在蓝屏中挣扎, 无暇顾及网络.

然而, 曙光还是有的. 异步的出现带给了人以希望. 古老的UNIX早就想到了, 提供了 select() 系统调用供人驱使.
然而问题还是有的, select 只能支持 1024 个文件描述符, windows 上的 select 更是劣质到只能使用64个. 就算通过修改定义强迫接受一万个文件描述符, 也没有解决实际的问题. select  实在是太慢了.

在这种背景下, IBM 老大哥带领着MS老弟先搞了 IOCP . 然而开源的人有开源的做法, 在 NIH 综合症的影响下, BSD 的人敢为天下所不齿, 发明了 Kqueue. 同样在 NIH 综合症影响下, Linux 的一群 M* 的猴子捣鼓出了 epoll.

分裂, 让人头疼.

然而, 他们都声称自己的新接口对 select 有质的提升, 是破解 c10k 问题的不二法宝. 你用也得用, 不用也得用.
为了让自己编写的网络程序能跨平台, 程序员开始了对3大各自为阵的法宝的膜拜学习. <br>除了需要应对多套互不兼容的 API , 异步本身也需要更高级的抽象, 把程序员从编写异步代码的地狱模式里拯救出来. 于是程序员们急需一个上天入地无所不能的法宝的法宝, 把这3家法宝给统御起来.

率先站出来悳瑟的是 ACE.

### 悳瑟的 ACE
恰乱世刚过, 天下待定, C++ 委员会的老人们却韬光养晦, 不问世事. 所谓乱世出英雄, 英雄出少年, 欧文大学出了名秀才. 凭借其洋洋洒洒的一片雄文 《Pattern-Oriented Software Architecture》 中举去了首府学城, 并为ACE奠定了无可撼动的地位.

ACE 的名字, 也许灵感来自 Adaptive Clubbed Rod, 这也是当年一位英雄少年的宝贝. 既是宝贝, 必需如意. 即是后来的葫芦娃都怕了 "如意宝贝".

ACE 如意在什么地方呢？如意其一, 支持 IOCP/kqueu/epoll/select/you\_name\_it 各种接口, 号曰没有不能跨的平台. 如意其二, 支持多种模型。这些模型都在《Pattern-Oriented Software Architecture》有过详细叙述. ACE 本身就是这篇论文的实践，因为他知道, 纸上得来终觉浅 绝知此事要躬行。 如意其三, 接口和模式排列组合下, 多少种, 竟可不修改代码而适应。

然而 ACE 毕竟嫩了点, 没过几年就失势了. 现在除了一些老程序员还在用, 新生代的程序员已经不再使用 ACE 了. 为什么呢? 陈硕在他的博客里说, **ACE 过于复杂，甚至比它试图封装的对象更复杂**,
程序员是指望用你的如意宝贝去驾驭另外那三家宝贝的, 结果你比他们还难。ACE 犯了早期 C++ 库都会犯的一个错误，过度设计， 过度java化。所谓 java 化， 就是以对象代替接口， 以虚函数代替回调，以继承代替组合。以虚类代替模板。对象间关系错综复杂，牵一发而动全身。除了作者，已经无人能参与 ACE 的开发了。

与此同时，C语言的回归却在背后悄然进行。C语言的复辟，带来了几个更为糟糕的替代品， libevent 和 libev，以及 乘着nodejs的盛行东风而来的 libuv。

### 原始的 libevent
C语言有着顽强的生命力，当然，这并不是因为C语言有多好，在后续的章节了我们还会深入的探讨C++相对C的改进。C语言的顽强和人天生的懒惰和偏见是有一定关系的。这种惰性表现为随遇而安，表现为固执己见。 非要拼命的否定C++，固守 C , 对 C 的缺点视而不见，诋毁C++相对C的改进。固守的结果就是简陋原始的 libevent . 然而因为保守党巨大的人数优势， libevent 应其群众基础良好而获得了空前的广泛使用。

libevent 就如名字所言，是一个异步事件框架。从 OS 那里获得事件， 然后派发。派发机制就是“回调函数”。异步异步，归根结底就是处理从操作系统获得的事件。iocp也好， epoll也罢，都只是用来获取事件的接口。libevent 去掉了ACE华而不实的包装，保留了异步事件，极大的简化了模型。不得不说软件工程是个糟糕的发明，从来都把简单问题复杂化。libevent把简单问题简单化，让异步网络编程反朴归真，应该来说，本是一个好库。

然而 libevent 因为设计缺陷，例如使用全局变量，定时器无法处理时间跳变，诸如此类的设计缺陷导致了 libev 的出现。
libev 就是为了克服libevent的缺陷而诞生的。然而，libev 就一定好了吗？

### 禁锢的 libev
libev 带着对 libevent 的怨气出世了。 吸收了 libevent 的所有缺点，
虽然承诺过改进。然而 libev 如何改进的了呢？ libev 已经够原始了，向下改进还不如让人直接使用系统的 api, 向上改进，一是会导致和libevent的重叠，二是很快就碰到了 C 语言强加的禁锢。

C 语言因其语法~~简陋~~简洁而著称。然而，缺乏必要的抽象能力，导致 C 语言编写异步程序，就如同安迪拿着小锤子琢开肖生克监狱的墙壁一样。能，但是要耗费巨大的精力和时间。
编写异步程序， 最需要的2个抽象能力， 其一为协程，其二是函数对象，包括匿名函数对象， 也就是lambda。
C统统没有。函数对象是实现闭包比不可少的，如果没有函数对象， 就只能通过携带 `void*` 指针的形式迂回完整，繁琐不说，还特别容易出错。程序里也到处充满了类型强转。到处是临时定义的类型，就为了传给 `void*` 使用。

尽管C 有那么多缺点，然而 libev 还未来得及被C的缺点拖累，因为他不支持 IOCP. 于是 libuv 就出来给 libev 擦屁股了。 支持了 iocp 后的 libuv 就真的只有 C 本身的缺点了吗？

### 混乱的 libuv

libuv 可以说是 C 语言的异步库所能达到的最高高度了。完完全全的触碰到了C语言的自身瓶颈，好在 libuv 只是 nodejs 的底层库，上层软件转移到 javascript 语言而逃避了 C 的禁锢。

真的是这样的吗？ libuv 自身还有什么缺点呢？

开源社区avplayer的大拿jackarain曾经说过，一个网络库好不好，就看他有没有正确的处理 TCP 关闭， read write 实现的ui不对。
libuv 很遗憾的是，不合格。libuv 的 uv_write 没有返回值，允许空回调。也就是忽略write错误。
网络出错的情况下， libuv 的用户只能稀里糊涂的知道出错了， 至于错在哪？数据到底有没有发出去了? 一概不知道。
把数据交给 uv_write 后，就是一笔糊涂账了，大概 TCP 不可靠的说法就是从这里传出来的吧。

---

## ASIO 腾空出世

请期待下篇！








