---
layout: post
title: 无盘PC
tags: [network, 10G, NAS，NFS, diskless]
---

自从上了万兆，把PC上的硬盘拆到nas上，让PC变成无盘的计划就开始执行了。真正的无盘需要 PXE，然而，同时也意味着现有的 NVMe SSD 成了摆设，因为上文提到的，我的 nas 因为主板关系， pcie 通道不足，无法插 M.2 了。所以现有的 ssd 还是继续在主板上服役。M2 只是一个非常小的接口， ssd 插上主板后和主板融为一体了，所以，姑且就算是主板自带的存储好了，如果只有 nvme 那也可以算无盘。



既然保留了 nvme，那么无盘的工作就简单了，只需要将个人文件  aka ``/home`` 目录放到 nas 上通过网络访问即可。由于在完成拷贝之前我就已经迫不及待的把硬盘插上 NAS 了，因此最初的无盘，我使用的 iSCSI。

将 pc 原本的 4T 硬盘通过 iscsi 共享给 pc 自己。连 fstab 文件都不需要修改，因为 iscsi 连上去的磁盘居然还是 /dev/sda 。就好像只是换了一个 sata 口插一样的方便。

但是， iscsi 只是 sata 线的替代品。我真正的目的，是要让我的数据全部由 ZFS 保管。只有 ZFS 才能让我感到安心。

因此， 8T 氦气填充盘到了后，我就开始了乾坤大挪移。rsync 拷贝了数T的数据，把 旧硬盘上的数据全部转移到了 8T盘上的 ZFS。

然后格式化旧盘，将旧盘作为 mirror 和 8T 的盘组成 raid1。

4T 和 8T 怎么组 raid1？

哈，我在 8T 盘上分了一个 4T 的分区，所以是 4T + 4T 的 mirror ZFS。剩下的 4T 则直接是 普通的 ZFS 用于存放不需要保护的数据 aka 下载的电影和小姐姐。

然后 4T 的分区就作为 /home 以 NFSv4 协议挂载了。

遇到的第一个问题，就是发现开机非常非常的慢。发现是 NFS 在等待 networkmanager-wait-online 激活花了好久时间，于是把 ip 地址静态化，省去 dhcp。

还是慢，nfs4 是以 tcp 模式工作的，可能 tcp 不够好吧，于是降级为 nfsv3 换成 udp。

没软用。

接着把 MTU 调整到  9000. 发现启动的时候要从 /home 读取大量的数据。MTU 改大后能显著的减小数据包的数量，降低 nfsd 的 cpu 使用。

速度确实非常的有效，快了不少。

然而，还是够快。NFS 缺乏prefetch功能，因此肯定不如系统自己挂载文件系统快。但是缺乏 prefetch 只是开机速度慢了点，后面还是不慢的。。。。 错！

在 bash 里， ～ 目录下，执行简单的 ls 命令都需要卡上数秒。第二次执行 ls 就瞬间完成了。然后继续 ls ls 都是瞬间完成。但是，只要停个几秒， 再执行 ls ，就要再次卡上数秒。

打开浏览器什么的也是， 每数秒浏览器就要假死一下。

这个现象的原因就很简单了，原因是 NFS 本地缓存问题。默认 NFS 的本地缓存只有 3秒。 3秒过后，重新到 NFS Server 拉文件列表了，连 ls 都会因此卡上一会会。因为我的 /home 文件夹下有百万计的文件。。。除了我自己的大量 git 仓库带来的大量小文件，还有 KDE 的大量 ～/.cache 文件。

最终通过将 nfs 的缓存加大到一整天解决， ls 也再没卡过，即使第一次执行也不卡了，因为 ls 执行需要的文件列表早就在系统启动的时候载入了。

最后本地访问 /home 的读写速度测试后发现在 400MB/s 上下，很明显 nfs 成了瓶颈。因为本地读写实际上是通过 nfs 读写 nas 的内存。虽然本机 drop 了 cache ，但 nas 并没有嘛。我想到了我的网卡只支持 tcp offload ，不支持 udp offload 。。。。 确实我测试读写的时候， nas 上 nfsd 的 cpu 使用飙升到超过 70% 。。。

于是再次回到 nfsv4 + tcp。这下速度超过了 700MB/s， 而 nfsd 的 cpu 使用也只有到了 50% 了。

暂时就这样了。

