---
layout: post
title: 如何造发动机
tags: [热机]
---

== 热机品种

最近突然对造发动机起了兴趣，遂研究研究如何造发动机。

发动机，就是热机，把热能转换成机械能的机器是也。
把热能转换成机械能，谁的热能？气体的热能。

因为，要在大气层里工作，工质必须得是气体才行。

提取气体里的能量，有史以来也就只找到了2种方法：活塞和涡轮。

所以，所有的热机，不外乎往复活塞式，或是涡轮式。

而工作的气体，如果是直接燃料燃烧后的废气，就是内燃机。如果是通过热交换（俗称烧开水）获得的，就是外燃机了，

四个象限下的典型热机

| ------: | :------: | :------: |
|  | 内燃 | 外燃 |
| 往复活塞式 | 汽油发动机 | 瓦特蒸汽机 |
| 涡轮式 | 涡轮发动机 | 蒸汽轮机 |

往复活塞式外燃机，也就是瓦特发明的蒸汽机，已经被淘汰了。
但是烧开的水推动涡轮，一时半会儿还没被淘汰。
外燃机一般使用固体燃料 ------ 煤，如果有液态或者气态的燃料，最好的选择还是内燃机。

内燃机没有体积庞大的换热装置（把废气的能量先转移给水），能减小体积，提高功率密度。

所以，发动机还是内燃的好。

那今天先研究研究往复活塞式内燃机。

== 内燃机循环

说到内燃机，就必须得有内燃机必备的4个循环：吸气，压缩，膨胀，排气
哪怕是涡轮式的也得有这4个循环。

为啥非得有这4个循环呢？其实吸气，膨胀，排气都好理解，为啥非得压缩一下呢？

这就不得不说到热机定律了：*理想热机的效率 = 1 - 排气温度/燃气温度。* 理想热机的定义是，理想热机把能用来转换为机械能的内能全部转换为了机械能，剩下的就只有无法转换的废热，绝无可能再从废热里榨取一丁点的能量了。

为了提高效率，就必须得降低排气温度，提高燃气温度。

而排气温度受限于热力学第二定律的制约，必须高于环境温度。实际工程实践的时候，都远高于环境温度。连接近都做不到。

燃料的热值是一定的，意味着燃烧前后的温差是一定的，于是理想热机的效率可以修正为

    *1 - 排气温度/（燃烧前温度+燃料热值带来的温升）*

提高燃气温度，除了使用热值更高的燃料（只有造导弹的人才能自由的换燃料类型，汽油发动机已经定死了，就是汽油，没得换），还有一个办法就是提高燃烧前的温度。

如果不进行压缩，燃烧前的气体温度，最多也就气温。

所以压缩就是必须的，这个是提高热机效率的必经之路。

== 4个冲程

既然有了4个循环，又设定为是活塞式的内燃机，那内燃机的结构就呼之欲出了。就是一个活塞连在一个偏心轮上。
活塞往复运动完成4个循环，2圈旋转。每2圈完成一个工作流程。

== 提高效率

前面的热机效率推理得知，压缩比越高，热机效率越高。

但是，汽油发动机已经定死了只能烧汽油。只要是汽油，就没法高压缩比。压缩比过高，汽油就会在压缩还未结束的时候就提前燃烧。 也就是说，燃烧前的温度，不能超过汽油的闪点。

而汽油热值也是固定的，没法改。所以想继续提高效率，就只能从排气温度上想办法。

降低排气温度，也能提高效率。而降低排气温度，就是要让气体在膨胀阶段多干活。活干的多了，自然温度就低了。

多膨胀干活，就是提高膨胀比。

如果活塞还是连在曲轴上往复运动，膨胀比就只能等于压缩比。

因此，汽油机要提高效率，就是要在压缩比被锁死的情况下，尽量提高膨胀比。

除非压缩阶段压缩的不是汽油-空气混合气体，而是纯粹的空气，那压缩比倒是不会被锁死。
不过那就成了柴油机烧汽油了。之所以不可行，主要原因就是排放。

汽油这个东西，空气多了，汽油少了，燃烧就会过于充分导致废气里含有氮氧化物。
空气少了汽油多了，燃烧不充分废气就会含有一氧化碳。两个都是空气污染物。
所以空气和汽油必须按比例精确混合。全油门当然可以用柴油机的技术。问题是怠速的时候呢？非全油门工况呢？
还是吸入那么多空气，油喷的少了，排放可就过不了。所以就只能少吸入空气。
少吸入空气，那压缩比就不够，不仅降低效率，而且压缩后温度不够，最后喷油烧不起来，不是罢工了？

所以，必须想办法实现一个合适的压缩比+更大的膨胀比。

=== 进气门晚关

如果在压缩冲程开始的时候，进气门还开着不关。吸进去的气体就会被活塞还回去。
还的差不多了再关闭进气门开始压缩。这样压缩的行程就只有一部分被用上了。
这就降低了压缩比。就实现了低压缩比和高膨胀比的结合了。

为何不是进气门早关？

只考虑改变压缩比这一个策略的话，确实是可以的。但是早关进气门，混合气体就会经历一次真空化的步骤，直到吸气冲程结束，压缩冲程回到关气门的节点。

这个过程会导致飞轮需要提供巨大的能量给活塞以克服大气压强（除非曲轴箱抽真空，但是这是不可能的）。
这无形中就降低了发动机的效率。想想真空吸尘器为了抽气消耗的能量。

== 提高燃烧效率

以上都是介绍从燃气里榨取更多的能量的办法。还有一个提高发动机整体效率的办法，就是别浪费汽油。
尽量让汽油 100% 的燃烧掉，就能再提高效率。

提高燃烧效率的第一点，就是精确控制混合比例。也就是俗称的空燃比。空气和燃料的比值。
不要让汽油里的碳氢找不到氧对象。为此，就得使用电喷+氧传感器取代化油器。
控制电路根据氧传感器测得的废气氧含量，控制燃油的喷射量，榨干汽油里的每一个碳原子和氢原子。

混的比例再好，还是会有一些氧气和一些汽油就是不肯结合，就是多出来了。
或者是混合的不够充分，进去的不是混合气，而是空气+汽油雾滴。
或者是燃烧的速度不够快，膨胀完了还有每烧完的。在排气管了继续烧。但是在排气管里烧的，这部分能量并不会传给活塞做功。

燃烧效率这个东西，和转速息息相关。
因为这个和时间有关。而混合和燃烧，二者都需要时间。
只有恰到好处的时间，才能恰到好处的烧完。

== 上混动

如果只有一个特定转速能完成最好的燃烧，那把这个转速下的效率优化到极致，再利用电机和电池实现削峰填谷。

