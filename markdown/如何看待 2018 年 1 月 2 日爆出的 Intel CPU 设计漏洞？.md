

用一个容易理解的比喻来说明Intel CPU最近的Meltdown Bug：

假定你希望知道你老板最近是不是在北京，但是按照公司规定，你是无法知道老板行踪的。所以你决定尝试窃取这个秘密。

于是，你给老板的秘书打电话说“你能不能帮我查下老板的日程，看看老板是不是在北京出差，如果在北京的话，再帮我去公司CRM系统里面查下，看看我名下近期需要拜访的北京重点客户，这些可能是需要老板帮忙拜访的。”

秘书很敬业，先是查了老板的日程，发现老板确实在北京；然后又花了几分钟时间从CRM系统里把近期需要拜访的北京重点客户名单拿出来。这时，她突然反应过来你是不应该知道老板现在在哪里的。于是她回电话告诉你“不好意思，我觉得你不应该了解老板去了哪里。”

你回复道：“那好吧，那麻烦你直接告诉我有哪些北京重点客户是近期需要拜访的？”

因为秘书刚刚已经从系统里查到了对应的客户，并且你有权限了解这些，她很快就回答你：“近期需要拜访的北京重点客户有A,B,C.......”

通常情况下，从系统里面调取出这些信息需要好几分钟，但是秘书很快就告诉了你答案，那么你可以推测老板就在北京。所以，秘密就这样泄露了。

CPU执行的过程，有一个很重要的加速优化技术叫做“speculative execution", 其实就和以上比喻中秘书的工作方式一样， 她只有在告诉你结果的时候，才发现你不应该了解这个信息。但是此时，通过下一步执行结果的速度变化， 仍然有可能暴露重要的信息，这就是所谓的"time based side channel attack（基于时间的旁路攻击）”。

Intel的CPU在服务器市场可能占据了99%的份额，一旦它出了问题，就意味着有很多服务器上的敏感数据都可能泄露。 因此这是一次非常严重的安全事故，有可能比Heartbleed更严重。
转载至知乎

修正下，分两个漏洞

漏洞1：spectre：AMD、Intel、ARM全部中招。可以击穿JAVA沙箱。KAISER无法拯救。

Hardware. 
We have empirically verified the vulnerability
of several Intel processors to Spectre attacks, including
Ivy Bridge, Haswell and Skylake based processors.
We have also verified the attack’s applicability
to AMD Ryzen CPUs. Finally, we have also successfully
mounted Spectre attacks on several Samsung and
Qualcomm processors (which use an ARM architecture)
found in popular mobile phones.
漏洞2：meltdown：Intel中招，AMD幸免。云厂商虚拟化被严重穿透，虚拟机可以获得host内存的访问权限，及高达503kb/s的系统内存下载速度。对云厂商是致命打击。可以用KAISER拯救。

We also tried to reproduce the Meltdown bug on several
ARM and AMD CPUs. However, we did not manage
to successfully leak kernel memory with the attack described
in Section 5, neither on ARM nor on AMD. The
reasons for this can be manifold.
奉劝某些人看完论文再来评论，谢谢

We evaluated Meltdown running in containers sharing a
kernel, including Docker, LXC, and OpenVZ, and found
that the attack can be mounted without any restrictions.
Running Meltdown inside a container allows to leak information
not only from the underlying kernel, but also
from all other containers running on the same physical
host.
The commonality of most container solutions is that
every container uses the same kernel, i.e., the kernel is
shared among all containers. Thus, every container has
a valid mapping of the entire physical memory through
the direct-physical map of the shared kernel. Furthermore,
Meltdown cannot be blocked in containers, as it
uses only memory accesses. Especially with Intel TSX,
only unprivileged instructions are executed without even
trapping into the kernel.
Thus, the isolation of containers sharing a kernel can
be fully broken using Meltdown. This is especially critical
for cheaper hosting providers where users are not
separated through fully virtualized machines, but only
through containers. We verified that our attack works in
such a setup, by successfully leaking memory contents
from a container of a different user under our control.
分几个player吧：

Intel：涉及太广，产品没法召回（笔记本难道整体返厂么），赶紧明白问题根源。该道歉道歉，企业级客户该赔偿赔偿。按照5%-30%的性能衰减，相当于目前所有CPU退回一代。

AMD：太棒了亲，我没有这问题，欢迎来买！AMD性价比进一步提升。目测股价应该有所表现。

Microsoft&Apple&OtherOS：疯狂给Windows和MacOS和OtherOS搞补丁并推送。IT运维有的忙了。

云厂商：重灾区，本次的问题若被黑客利用后果不敢设想。24小时加班部署补丁，重启服务器。IAAS层的服务会出现中断并影响PAAS层和SAAS层。对于如火如荼的云化是一剂清醒剂。

别的：貌似也帮不上忙。。。



增加下某券商研报：

预测技术，Intel成也萧何败也萧何

为了提高处理效率，当代处理器内嵌有预测技术：通过预测下一条指令，处理器可以填满内部流水线，充分发挥运作效率。Intel的推测执行(Speculative Execution)技术是业界标杆水平，行业内所有公司都在向Intel靠拢，但是这个漏洞说明Intel的推测性执行在芯片内部的访存执行单元Load/Store Unit和重排序缓冲区ROB上存在安全检查漏洞，导致操作系统核心的安全保护出现问题，使得用户程序可以窥测内核数据，包括系统访问历史记录，密码等等隐私，同时会造成KASLR（Kernel Address Space Layout Randomization，内核位址空间布局随机化）的无效化，导致攻击者可以推断出内核地址，进而发动攻击控制控制整个系统，造成严重的安全风险。受影响用户包括服务器环境、PC环境和移动环境。

在Linux的源代码和邮件列表中显示，一开始的补丁也囊括了AMD CPU，但是AMD公司的工程师主动进行了修正，声明AMD的处理器不受影响并且要求取消了对AMD的补丁，目前这个补丁仅在Intel CPU上生效。

操作系统厂商纷纷发布补丁尝试修复此漏洞，但补丁会对性能造成严重影响

本次漏洞无法在芯片层面得到修复，修复只能在操作系统层面进行（或重新购买CPU）。微软预计本周四会推送补丁，且补丁已经包含在去年年底的Windows Insider版本中。苹果的MacOS也未能幸免，需要进行相应的修补工作。

本次补丁的主要功能是通过KPTI（Kernel Page Table Isolation）完全分离系统内核与用户内存，让系统使用另外一个分区表，使得用户程序无法访问系统内核。但是，KPTI会导致CPU频繁地从内核模式切换到用户模式，引发耗时的TLB缓存刷新，拉低系统效能。根据初步测试，Intel CPU效能会降低5%-30%，相当于回退1~2代。举例来说，第八代的酷睿CPU打上补丁后的效能可能低于同档次的第七代的酷睿CPU。

Intel善后花费巨大

综合专家看法，我们认为，研发方面，Intel修复本漏洞需要组织工程师Review几十万行RTL源码来确认问题所在，而发现问题后的问题解决过程将耗费更多的人力物力：我们预计Intel至少需要花费几个月的时间才能提出可行的解决方案，并做完初步测试，而采用新方案的芯片的性能将会是一个未知数；商务方面，Intel需要安抚各大客户的情绪，特别是采购了大量Intel CPU的云厂商，以防新增订单向AMD的转移，同时，对于已有的服务器设备，Intel需要与厂商一起研发解决方案堵住漏洞；品牌形象方面，Intel的高端形象无疑会遭受重创，建立品牌认知将花费较长的时间。

2016年，Intel花费了127亿美元用于研发。假设其中10%用于CPU研发，本次漏洞造成Intel CPU性能的回退至少给Intel带来了12.7亿美元的损失（相当于研发产生的性能提升被抹掉），而整体的善后成本在考虑修复成本、商务成本、品牌重建、市场竞争等因素后或将超过25亿美元。

云厂商面临巨大压力，云服务成本或有较大提升

本次的漏洞会影响所有的云厂商，包括但是不限于Amazon、Microsoft、Google等巨头。云厂商只要使用Intel CPU，就需要通过补丁进行系统加固。然而，由于云平台应用了大量的虚拟化技术，这些补丁比针对个人电脑的补丁更为复杂，由于云厂商的服务器拥有极高的数据吞吐，补丁对服务器系统效能的影响会比对PC更为严重。

Microsoft Azure将于1月10日进行维护和重启，据称跟本漏洞有关；AWS也发送了通知邮件声称本周五将进行重大安全更新。让人担心的是，已经有迹象表明有黑客在利用本漏洞攻击云系统。我们认为短期来看，云厂商的服务成本或有较大提升。

编辑于 2018-01-04
著作权归作者所有
推荐阅读
知乎北鼻版「天真问·认真答」第二季，奶萌回归！
推荐阅读
Friso美素佳儿Friso美素佳儿 的广告
Intel CPU 的三角函数精度问题，会对现实生活产生什么影响？
e一般做sin/cos是先把输入截断到[-Pi/2, +Pi/2]，然后用泰勒级数之类的多项式级数逼近或者cordic算法算出来。 intel里文档里解释产生问题的原因是intel在把大输入数截断到[-Pi/2, +Pi/2]过程，也就是sinf(x)=sinf(x-N * Pi/2)时，把pi的近似值作为常数放到了一个66位的ROM里。 对于大数来说，这个精度是不够的(想想把pi的几亿倍，小数点后剩下的有效数字还有多少)，而sin/cos这种恰恰对小数很敏感。 一般libm里干这个事情的单精…
Shu Zhang
的回答 · 30 赞同
如何看待Intel CPU爆出的重大bug？
最好先等等。intel的发言人说这个是“inaccurate media reports”，他们下周会会同其他的供应商一起做个调查。我没有关心过中文网络的报道，这个事情最初是google的project zero去年提出来的，这是google一个很厉害的安全性小组。他们认为CPU的预测执行功能，会造成跨域访问的bug，要知道这个预测执行是几乎所有现代CPU都使用的功能，很难想象只影响intel CPU。 还是等等吧，先等联合调查结果。
萝魏紫
的回答 · 14 赞同
