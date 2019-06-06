# DDS/动态更新论文实验

## DDS

### Data distribution service for industrial automation       

 [06489544.pdf](pdf\06489544.pdf) 

- 与socket通信进行比较，主要测量不同场景的通信延迟

- 平台 ubuntu 11.10  ，RT-29 实时补丁，所有的Linux节点都连接到一个交换机上，实现以太网通信，每台机器使用的以太网带宽为1 Gbps

- DDS：开源版本的OpenSplice DDS社区5.4.1

  #### 场景1：Network without Extra Network Load
  
  两个节点A,B，一个Switch, Round Trip Times (RTTs)都在A节点计算，**RoundTripTime =postReadTime-preWriteTime**。put time stamp preReadTime before the message write and postReadTime stamp after the message read function in the resolution of microsecond
  
  - 缩放消息帧来检查性能是否随着消息大小的不同而变化。
  
  - 在两台机器之间建立客户机-服务器通信，以便与DDS进行比较。
  - 不同Qos配置下的DDS性能
  
  使用的QOS：reliability, latency budget and transport priority QoS
  
  #### 场景2：Network with Network Load
  
  4个节点一个switch，A,B两个节点和场景一一样。
  
  #### 指标
  
  测量的主要指标是延迟、抖动和最坏情况下的延迟。每个度量由1.000.000次迭代的平均结果给出。
  
  **延迟**是一个节点发送消息到另一个节点接收消息所经过的时间。latency = R/2
  
  **抖动**是两个连续消息传输之间往返时间的变化。
  
  **最坏情况延迟** 定义为在1.000.000个样本中测量到的最大延迟。
  
  ### 结果
  
  #### 2节点
  
  ![1558616190130](.\img\1558616190130.png)
  
  ![1558616166353](.\img\1558616166353.png)
  
  当消息大时，往返延迟的增加是明显的，而且，与套接字通信相比，DDS对消息大小的增加更敏感。
  
  
  
  - 设置reliable QoS and best-effort QoS
  
  ![1558616350294](.\img\1558616350294.png)
  
  reliable的QoS机制会带来更多的开销。
  
  - 基于1.000.000样本的1024字节消息的DDS和套接字通信的测量延迟分布。
  
  ![1558616375109](.\img\1558616375109.png)
  
  套接字通信在消息传输时间上的变化比DDS小。
  
  - 
  
  ![1558616679259](.\img\1558616679259.png)
  
  较小的消息和较低的延迟预算可以改善延迟，但它们也会导致较低的吞吐量。
  
  ![1558616702614](.\img\1558616702614.png)
  
  当Topic2被赋予比Topic1更高的传输优先级时(如图12所示)，Topic2的平均延迟将小于Topic1。
  
  4	节点 （DDS几乎不受网络负载影响）
  
  ![1558616820030](.\img\1558616820030.png)

## Dynamic

### Dynamic software updates for real-time systems   

 [Wahler, Richter, Oriol - 2009 - Dynamic software updates for real-time systems.pdf](pdf\Wahler, Richter, Oriol - 2009 - Dynamic software updates for real-time systems.pdf) 

![1559121463633](.\img\1559121463633.png)

Sensor 每个周期获取当前时间发送给History,History缓存存储1000条数据。

用 Integrity Event Analyzer 显示进程与事件的生成

S表示计算周期的开始，E表示结束。

![1559122392389](.\img\1559122392389.png)

- 间隔1：一个周期 （273微秒）

- 间隔2：History传递连接和状态给系统

- 间隔3、4：History2接收这些信息

- 结果：在一个周期内完成了更新

### Middleware support for dynamic component updating

 [Balasubramanian et al. - 2005 - Middleware support for dynamic component updating.pdf](pdf\Balasubramanian et al. - 2005 - Middleware support for dynamic component updating.pdf) 

- 环境：850 MHz CPU Intel Pentium III with 512 MB RAM，RedHat Linux 7.1 distribution

- normal operations（无更新）下与标准CCM的比较

  high-resolution timer probes 测量move操作从cm到组件的延迟。200000次调用，不同端口数量

  ![1559183583881](.\img\1559183583881.png)

- updating operations下

  - removal time/ creation time/ reconnect time：在相应函数加计时器探针
  - 测量move操作从cm到组件的延迟。200000次调用，不同端口数量
  - ![1559184142614](.\img\1559184142614.png)
  - ![1559185106257](.\img\1559185106257.png)

  - 结果：延迟随着端口数量线性增长，没有额外开销

- Update Latency Experienced by Clients

  incarnation delay：包括creation time, reconnect time, and processing delay

  - 操作： 移除一个组件

    ​			当客户端调用该组件时启动计时器

    ​			重复200000次

  - ![1559185645532](.\img\1559185645532.png)

结果：与前三个延迟之和大约相等

### 基于OSGi的服务动态演化

 [10.1.1.543.4337.pdf](pdf\10.1.1.543.4337.pdf) 

实验提供一个计算Π值的服务

- 首先对比可动态更新程序与源程序的运行效率

  结果：对性能没什么影响

- 连续调用服务99次，再25，52，81次后分别进行动态更新

  结果：服务动态更新对客户调用产生影响有限

![1559289695097](.\img\1559289695097.png)

### Non-disruptive large-scale component updates for real-time controllers

  [Wahler et al. - 2011 - Non-disruptive large-scale component updates for real-time controllers.pdf](pdf\Wahler et al. - 2011 - Non-disruptive large-scale component updates for real-time controllers.pdf) 

- jitter:实际周期频率与理想周期频率的偏差
- 平台：a Freescale Lite5200B development board containing containing a single-core PowerPC proces- sor running at 396MHz and 256MB of memory
- 系统：RT-Linux
- 分别测量无更新和有更新的抖动，最坏，最好，平均

![1559618370598](.\img\1559618370598.png)

![1559618379094](.\img\1559618379094.png)

![1559618460751](.\img\1559618460751.png)

- 结果：平均抖动相同，最大抖动相同数量级，动态更新不会带来bumps

### A framework for adaptive real-time applications : the declarative real-time OSGi component model   

 [Gui et al. - 2008 - A framework for adaptive real-time applications.pdf](pdf\Gui et al. - 2008 - A framework for adaptive real-time applications.pdf) 

这篇的实验虽然写的是dynamicity但是好像验证的是实时性。

-  The hardware Timer is set in periodic mode. 
- 两种不同负载环境：light&stress
- 两种实现：Pure RTAI user model & DRTCom (文中提出的模型)
- 指标：latency(ns)  (the negative value means the task is scheduled ahead of the predefined time
- 应用有两个组件组成，一个以1000Hz速率做模拟计算，另一个通过读取共享内存以4倍速率显示调度延迟
- It is quite encouraging because it means this implementation can provide the latency guarantee in 30us which is quite nice for much real-time application.

![1558676268045](.\img\1558676268045.png)

### FASA : A Scalable Software Framework for Distributed Control Systems （侧重可拓展分布式和多核系统）

 [Oriol et al. - 2012 - FASA A Scalable Software Framework for Distributed Control Systems.pdf](pdf\Oriol et al. - 2012 - FASA A Scalable Software Framework for Distributed Control Systems.pdf) 

- 四个场景

  1x:单处理器，单核

  2x:单处理器双核

  4x:单处理器四核

  1+1：双处理器单核

- 每个执行周期的头和尾打上时间戳

- 测量了最坏，最好和平均执行时间以及300个周期的标准差，重复5次此过程

![1559614669951](.\img\1559614669951.png)

![1559614755771](.\img\1559614755771.png)

- 结果：不是所有部署都对性能有利