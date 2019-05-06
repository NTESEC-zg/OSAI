# Component Model of Open Software Architecture for Industry (OSAI)

[TOC]

<header>

## Abstract



## Introduction

{I}{ndustrial} Control System(ICS) has got extensive use in manufacture and mechanical control, especially some critical infrastructures like  water conservancy facilities and  nuclear power plant.
%Thus, the reliability of such systems is particularly vital due to the safety-critical application scenarios.
With the growing scale and complexity of ICS, it is of great importance to bring software engineering methods into the development and integration of ICS, in order to meet the evolving industrial requirements and reduce the cost.

Just as manufacturing industry uses pre-defined standard to produce replaceable components, component-based software engineering (CBSE) improves the flexibility and robustness of system by treating it as a combination of subcomponents rather than a monolithic entity\cite{neto2018component,brooks2005towards}. Compared with traditional software development methods, CBSE has significant advantages in terms of improving reusability and portability, and shortening time to market. In addition, CBSE has great potential on enhancing the system performance and specification by modular modeling, which helps to control dependencies between components.The cornerstone of CBSE is based on component models that specify component definition, composition, inter-component interaction and so on.Component models have been widely adopted in general-purpose software domains to date, including EJB, CCM, UML, etc.Even in the field of embedded systems where security and predictability are more demanding, CBSE is considered as a promising technology.

In recent years, concepts such as intelligent robotics, Industrial-Internet-of-Things (IIoT) and Cyber-Physical Systems (CPS) have been prevailed in recent years, which brings rich features along with great challenges into current ICS, particularly when adopting CBSE. As the complexity and functionality keep booming, the software development in industrial domains often requires multi-team and multi-vendor collaboration, asking for great interoperability (cross-platform and cross-company) based on a unified principle. To achieve complex functions and features, high-level programming languages like python, c++, Java are preferred when implementing components. It is desired that the component model can provide supports of multiple programming languages and standardized interfaces for implementation and integration.In addition, on account of the harsh working environment of ICS like long-time high-power operation and electromagnetic disturbance, both the hardware and software are prone to failure, often resulting in frequent replacement or upgrade. However, the downtime cost of updating by rebooting the whole system tends to be intolerable for ICS, especially for the safety-critical ones. Hence, it is desired that software components can be dynamically updated on the fly.

In this paper, we introduce a novel component model, OSAICM (Open Software Architecture for Industrial Component Model), applyed to development of industrial control systems. The model is, on the one hand, rich enough to fit  major industrial requirements and on the other hand, general enough to be applicable in most industrial control systems. More narrowly, OSAICM components are well defined in the whole life-cycle from design time to runtime, supporting for multiple instantiation. In addition, we adopt a XML-like language to describe dependences between components clearly, including assembly/wiring inter components and composition in the components. With the unified interface standard, the model facilitates the collaborative development of manufacturers, improving interoperability and causing easy integration. In the meanwhile, clear and well-defined component interfaces are also critical to software reusability. Besides, as a logic abstraction,  virtual bus provides all of services required for virtual interaction between components. Based on the publish/subscribe model, we use pluggable message middleware to implement multiple communication patterns for its fast communication speed. With the structure of component manager, systems could be less rigid and more reconfigurable.

> 论文结构

[back](#Component Model of Open Software Architecture for Industry (OSAI))

## Background and Related Work

#### Component Models

CBSE has been studied since the end of last century, with some ideas proposed on what features a perfect component model should possess. 
Kung-Kiu Lau and Zheng Wang\cite{lau2005taxonomy} gave a taxonomy of component models and revealed an idealized component life-cycle, discussing desiderata of CBSE: components are pre-existing reusable software unit that can be produced and used by independent parties; components are able to be composed into composite components and copied and instantiated at deployment.
The authors of\cite{bures2006sofa} emphasized three key factors of component models: dynamic reconfigurations, multiple communication styles and non-functional aspects of components (e.g., life-cycle management). 
In \cite{hovsek2010comparison}, a comparison was made among some component frameworks for real-time embedded systems based on a set of criteria such as supports for distributed applications, resource constraints and real-time attributes(e.g., periodic tasks).
There is also a gap between academic and industrial component models where design-oriented features are often difficult to balance with sufficient runtime support\cite{bures2006sofa}.

As an open and standardized  software architecture enjoying extensive concern and broad acceptance in the automotive domain, AUTOSAR (AUTomotive Open System ARchitecture)\cite{autosar.org} was a large source of inspiration. 
The main focus of AUTOSAR is standardization and interoperability of software components, which is exactly one of our pursuits. To provide smooth integration and easy reuse of components from different vendors, AUTOSAR supports multi-type interfaces and multi-instantiation.
AUTOSAR defines Virtual Function Bus (VFB) to support interaction between components and hide implementation and communication details. Thus, system can be integrated early without regard to component physical allocation. 
AUTOSAR Software Components are compiled and linked into specific executable, losing some benefits of the component-based approach during run-time\cite{feljan2009classification}. 

In addition, AUTOSAR does not give much thought to component dynamics.

> TwinCAT (The Windows Control and Automation Technology)\cite{twincat.org} is a PC-based automation software developed by Beckhoff, with the characteristics of openness and modularization. 
> TwinCAT offers TcCOM (TwinCAT Component Object Model) derived from the COM to define characteristics and behaviors of application modules. 
> %TcCOM distinguishes between the module type and instance described in a TMC(TwinCAT Module Class) and TMI(TwinCAT Module Instance)file respectively. 
> TwinCAT also defines concept of state machine, which describes state transitions and corresponding actions at run-time.
> The run-time environment of TcCOMs relies on specific hardware devices of Beckhoff and lack of support for high-level languages other than C/C++.

Up to now, numerous component models have been proposed each with their own characteristics and advantages,  but few of them had a comprehensive consideration or inclusion of aforementioned features.%For example
On the other hand, studies on such component models do not seem to be evolving rapidly these years, which may capture more attention from researchers\cite{neto2018component}.

[back](#Component Model of Open Software Architecture for Industry (OSAI))

#### Publish/Subscribe Communication Paradigm

> 作为一种简单高效的数据通信方式，发布订阅模型得到了广泛的关注。

As an efficient information diffusion paradigm, the publish/subscribe model has recently received increasing attention.

> 这个模型的关键特性是发布者和订阅者不用互相知晓。

The key feature of this paradigm is that the publisher and subscriber are unaware of each other.The publisher sends messages without knowing  names, or even  existences, of the subscribers. And the subscriber receiving messages is also not aware of the publisher out there.

> 基于这个松耦合的特性，在组件模型中应用基于发布订阅模型的可插拔的消息中间件使得组件更替重连更容易。

Based on this loose coupling feature, applying pluggable communication middleware based on pub/sub model to the component model makes it relatively easy to add, remove, or rewire components.

> DCPS基于全局数据空间的概念，使数据分发更便捷。

The Data-Centric Publish-Subscribe (DCPS) model extending the publish/subscribe model is based on the concept of "global data space" and facilitates efficient data transport.

> 如图所示，DCPS有这么些因素。

As shown in Fig.x, the DCPS model consists of entities as follow:



+ DomainParticipant. DCPS is constructed of one or more data domains, every domain contains a set of participants. A domain participants represents the following other entities.
+ Publisher. A publisher sends data to one or more topics of different data types with owning and managing one or several data writers.
+ Subscriber. A subscribe receives and dispatches data for different topics  on behalf of  one or more data readers
+ Data Writer. A data writer is a typed facade to a publisher, sending data of a given type.
+ Data Reader. A data reader is attached to a subscriber, used by a domain participant to receive and access data.
+ Topic. A topic associates data-object between a data writer and a data reader, with a unique name, a data type, and a set of Qos. The type definition provides enough information for the service to manipulate the data. 
  

> 每次数据传输的执行都依据相应的服务质量策略，应用程序用它们来指定可靠性、资源使用和其他要求的服务。

The flexible configuration of communicaition is performed according to corresponding Qos (Quality of Service) policy that the system uses to specify reliability, resource usage, and other required services.

> 另外，DDS的发现机制使得主题的新发布和新订阅随时出现，中间件也能自动连接。

In addition, the discovery mechanism of DDS enables the middleware to automatically connect when new participants appear.

There are some traditional middleware technologies, such as Web Services, OPC (OLE for Process Control) and CORBA (Common Object Request Broker Architecture), but most of them are based on the Client/Server model, do not adapt properly to characteristics of dynamic process. 

> 这是我们选择DDS的原因,DDS是。。。有很多应用。

That's why we choose DDS (Data Distribution Service) as message middleware. DDS is an emerging specification based on pub/sub model which was released by the OMG (Object Management Group). With features of low latency, high throughput, and controllable transmission performance, this technology has been adopted in many fields, like traffic control systems, aerospace, robotics, and IoT. 

> 比如，ROS2相比ROS1加入了DDS的应用，来应对更复杂的机器人系统。

For example, the ROS2 (Robot Operating System), an open-source middileware widely used in robotics domain, adopt the use of DDS compared to ROS1, in order to deal with increasingly complex robot systems\cite{maruyama2016exploring}.

> IEC61499

In \cite{calvo2010control}, authors build IEC61499 Service Interface Function Blocks (SIFB) based on DDS.

[back](#Component Model of Open Software Architecture for Industry (OSAI))

#### A Case Study

![](.\img\HWArch.png)

## Component Model

In this section, we present the OSAI Component Model  and illustrate how the components are composited, instantiated, executed, and how components interact with each other. 

> 在这一章节中，我们提出了OSAI的组件模型，解释了组件是如何被组合、实例化、执行和组件间如何交互的

The OSAI Component Model distinguishes components from their instance and draws on the object-oriented paradigm. 

> OSAI组件模型区分组件和组件的实例并采用了面向对象的模式

By giving the description of life-cycle, component elements and the Unified Dynamic Bus, we combine the general-purpose software engineering methods with the "OEM-Vendor" industrial cooperating pattern and give a practical component model with both academic and industrial features.

>通过给出组件的生命周期元素和UDB，我们把通用软件工程方法和“OEM-Vendor”工业协作模式相结合并给出包括工业和学术特性的实用组件模型。

![SWArch](.\img\SWArch.png)

[back](#Component Model of Open Software Architecture for Industry (OSAI))

#### Component Construction

A component of OSAI Component Model is called Application Component (AC). 

>一个组件叫做AC

To adopt the object-oriented paradigm, one key feature of OSAI Component Model is that AC can be differentiated between *AC Type* and *AC Instance*, and an AC Type will be instantiated into one or multiple AC Instances at run-time. 

>AC 可以分为 AC Type和AC Instance，AC Type 可以实例化成AC Instance，并可以多实例。

In terms of composition,  ACs can be composed at type-level, bringing two kinds of AC Types: *Atomic AC Type* and *Composite AC Type*.

>在组合方面，AC可以在类型层面组合，这带来两类AC Types：原子的 和 组合的

A Composite AC Type aggregates existing AC Types (atomic ones and composite ones). 

> Composite进行组合

The composition in OSAI component model groups existing components and gives a hierarchical views when designing logical software architecture, and then, to  reduce the complexity, a Composite AC Type is instantiated and treated as a single component  in the components interaction, which was considered as a trend in [].

> Composition specification

An AC Type aggregated within a Composite AC Type is called *AC Type Reference*, which implements the usage of the AC Type and can be duplicated.

> Composition AC Type reference

An Atomic AC Type encapsulates the specific functionalities and internal behaviors.

> Atomic AC Type







Service

interface 

Capacity description

![AC_Type](.\img\AC_Type.png)



#### Life-cycle

In [] [] [], the authors proposed an idealized life cycle of software components consisting of three phases: *design*, *deployment* and *run-time*. 

>

We bring these concepts into the development workflow of ICS. 

>

Figure x  describes the three phases and several stages when constructing a distributed system based on OSAI. 

>

In the first place, OEM, the integrators, define the hardware architecture and model a software architecture. 

>

By mapping the software architecture with the hardwares, OEM can determine how many and what kind of components they need and what run-time supports these component require. 

>

An XML-based file called Component Repository Description (CRD) can be generated and it 

>

 In a collaborative development context, the design phase is often conducted by multiple developers (or vendors). 

>

Developers design and construct their own components independently based on a standardized interface definition and then store components in respective  repositories. 

>

Components can be retrieved from the repository and composed into composite components that can be deposited back for further retrieve and reuse.

>

During the deployment phase, components are compiled into object code for target platform and delivered to the integrator (or OEM). 

>

To build the control system, the integrator retrieve components from the object repository and deploy them into the run-time environment (RTE) along with an XML-based instance wiring configuration (IWC) which specifies the dependency and interface connection among component instances.

>

A component may have one or multiple instance. 

>

These instances communicate with each other at run-time via *Unified Dynamic Bus*, which will be detailed later.

> state machine



[back](#Component Model of Open Software Architecture for Industry (OSAI))

#### Port & Interface



## OSAI Run-time Architecture

> OSAI为工业应用场景中的不同设备、多平台、分布式的特点提出一种统一的架构，以供AC能够部署和执行，并且AC实例之间通过一个统一的总线进行交互。

> OSAI的运行时架构针对ICS中多种设备，不同性能配置，包括那些配置较高的能部署OS包括实时OS的设备和那些小的基于MCU的设备。



> 部署在这不同的两类设备的上AC也依此被分为PAC和LAC，在OSAI的分布式架构中，每一台嵌入式设备被称为一个node



> LAC被部署在小型MCU设备上，通常在此类node中只会一个LAC，与其一同部署的还有一个为这个LAC特定低层级的运行时环境（如IEC 61131-3程序运行时）

> 至于能力capability较强的设备，OSAI给出了一个分层架构包括 Containerized Instance Layer 和 Runtime infrastructure layer

+ Containerized Instance Layer

> 这一层是所有AC部署并运行的地方，如同spring的IoC容器，组件被载入到这一层，由AC Type被实例化为AC Instance，随后实例间彼此建立连接。
>
> 所有的PAC实例都被docker-like的容器包裹着以隔离组件，实例容器间通过OSAI Connection进行连接通信

> docker的作用

> 这一层中的所有组件都由组件适配管理器管理

+ Run-time Infrastructure Layer

> 

 

![Architecture](.\img\Architecture.png)

#### Unified Dynamic Bus

#### Dynamics

![statemachine](.\img\statemachine.png)

为了更好地进行组件运行时生命周期的管理以及实现组件模型动态性（即在运行时加载卸载及更新组件），每个组件都包含一个状态机，来描述组件实例在运行时的生命周期及状态更替。

状态机将一个组件实例所可能的状态定义为INIT，LOADED, READY, ACTIVE, DEACTIVE, DELETED 以及为实现动态重配置所需要的BLOCKED, UPDATED状态，如图X所表示，所期望的组件实例运行时的行为如下：

① 事实上，INIT状态是个虚拟状态，组件一旦实例化，就离开这个状态，同时组件实例通过CM注册加载。

> CM对于每个组件实例都有一个引用计数的概念来管理组件实例的生命周期。接口被查询计数＋1，释放-1，计数变成0之前不能停用组件，即直到处理完本次调用为止。

② 通过对CM的查询，组件实例获取所需interface，建立与其他组件的连接*根据port来创建DDS实体*，等待运行。

③ 组件service绑定到task, 组件实例开始执行，在ACTIVE状态，组件可以与其他组件互相通信。

 在组件实例运行过程中，可能出现的状态改变有两种：完成任务结束运行，或是因为更新换代或是组件实例受到破坏等原因需要进行动态更新（即，用新的组件实例来替换旧的），具体的动态更新实现将在后面说明。

④ 在无需更换组件的情况下，组件实例正常执行，等到不需要它了，停止接收新的调用和通信，变成DEACTIVE状态。

⑤ DEACTIVE状态的组件实例完成当前调用执行之后，解绑task，释放资源。

⑥ 回到LOADED状态的组件实例可以等待再次被使用，也可以通过CM注销interface, 卸载组件。

⑦ 当检测到处在运行中的组件实例更新的需要时，我们将新的调用和通信阻塞住，此时的旧组件继续运行将当前调用执行完成并返回，也就是处于BLOCKED状态。

⑧ 等到需要被更换的组件完成当前周期执行，与task解绑，unlink连接，这时候换上新的组件，将引用重定向，连接重新建立，更新完成。

⑨ 此时将先前阻塞的调用唤醒，新的组件实例service绑定到task之后就可以继续运行了。

作为适用于工业控制系统的组件模型，我们对于组件在运行时的添加删除以及更改有足够的支持，以此来对系统修正缺陷，添加功能，运行时优化，同时避免停机更新所造成的巨大花费。

事实上，对于动态更新的实现，我们利用了DCPS模型作为消息中间件，其对qos策略的支持及发现机制使得组件的重连变得容易。在DCPS模型中，每个实体都具有一套Qos策略来描述数据传输。对相应的Qos策略进行配置，能够对通信周期、可靠性、资源分配等进行恰当的约束。与动态更新相关qos策略描述如下：

| Qos策略        | 描述                                                         |
| :------------- | ------------------------------------------------------------ |
| deadline       | 对于DataReader，指定到达数据样本预期的最大传输时间 <br />对于DataWriter，指定在它们之间发布样本的时间不大于传输时间的承诺<br />表示预期更新实例的最大持续时间(截止日期) |
| discovery      | 指定发现域中参与者所需的属性                                 |
| durability     | 指定是否将之前发布的数据储存和传送至新DataReader             |
| history        | 指定必须为DataWriter或DataReader储存多少数据                 |
| liveness       | 指定和配置允许DataReader探测DataWriter何时变为断开或“死亡”的机制 |
| reliability    | 指定是否可靠地传送数据                                       |
| resourcelimits | 如果允许动态分配，控制为实体分配的物理内存的数量以及它们如何发生，也控制Resourcelimits 关键字主题不同实例值间的内存使用 |

借助这些特性，组件能够尽可能快地进行动态更新，同时对其他组件及系统尽量不造成或少造成影响。

[back](#Component Model of Open Software Architecture for Industry (OSAI))

## Evaluation & Implementation

#### Comparision

#### Analysis

#### Case Implementation



## Future work

## Conclusion 









