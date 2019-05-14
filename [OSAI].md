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

In fact, the composition in OSAI component model groups existing components and gives a hierarchical views when designing logical software architecture, and then, to  reduce the complexity, a Composite AC Type is instantiated and treated as a single component  in the components interaction, which was considered as a trend in [].

> Composition specification

An AC Type aggregated within a Composite AC Type is called *AC Type Reference*, which implements the usage of the AC Type and can be duplicated.

> Composition AC Type reference

An Atomic AC Type encapsulates the functionalities and internal behaviors and interacts with the outside world through exposed interfaces.

> Atomic AC Type内容 

In our opinion, the artifacts implemented inside an individual Atomic AC essentially include four parts as follow:

> + Function：request-response interface 同步
> + Event：Event-driven
> + Data Area: Data flow 支持异步传输 和 数据区域映射
> + Service：Runnable

+ Operation：Under the semantic of a synchronous client/server communication pattern, a component may be equipped with operations that can be requested by other components. An AC can request operation that another AC provides or response the results of its operation execution by utilizing the *request-reply* interface.

+ Event：Event-driven interaction is also desired for the loosely coupled software components. In OSAI Component Model, an AC has the capacity to trigger or react to some specific types of event. 

  > Through the *event-trigger* interface, events can be raised and transferred to multiple ACs or 

+ Data Area：An AC may contain data areas with different types of data. Data can transferred between different ACs' data area in a asynchronous sender/receiver communication pattern. In addition, data area can also be mapped with other data areas or IO area of the device for data synchronization.

+ Service：In the run-time environment of OSAI, an AC has no direct access to underlying operating system. In order to use mechanism like threading, code fragment of an AC's internal behavior that need to be executed cyclically are wrapped into an artifact called *Service*. Services can be scheduled by run-time infrastructure and mapped with operating system tasks.

The OSAI Component Model also defines 4 types of port to identify the entry points of the ACs for data, event and operation request/reply transfer. As shown in figure x, ports are distinguished between provided ports and required ports.

> port 图例





Figure x gives an example of constructing a Composite AC Type named *COETransport* composing two Atomic AC Type References: *CANopenProtocolSupport* and *EtherCATMaster*. 

> 图例给出了一个组合两个AC Type Ref的例子

*COETransport* provides support for communicating with servo drives that control the axises on a robot arm through EtherCAT (quote); *EtherCATMaster* plays the role of a master protocol stack of EtherCAT that interacts with the EtherCAT slaves (servo drives); *CANopenProtocolSupport* provides functionalities to support CANopen (a high-layer communication protocol for embedded systems used in automation)  over EtherCAT (CoE). 

> CANopenProtocol组件结构

Since the Composition AC Type only exposes the ports on its surface, the *AC Type Reference* will interact with outside world through the port *delegation* and communicate with another *AC Type Reference* , inside the same Composition AC Type, through port *binding*. Such two concepts are both implemented at type-level.

> delegation & binding

In the scene of robot motion control, instructions for moving axis or configuring the operation mode will be handled by the *COETransport AC Type* by invoking the request-reply ports which are delegated to ports of *CANopenProtocolSupport AC Type Reference*. 

These instructions will be transformed and wrapped into SDO (Service Data Object) packages that accord with the object dictionary of CiA 402 (CANopen device profile for motion controllers and drives released by *CAN in Automation* 注释：**CAN in Automation** (CiA) is the international users' and manufacturers' organization that develops and supports CAN-based higher-layer protocols.). 

Then these SDOs, which are written to the *EtherCATMaster AC Type Reference* through the port binding, are transferred to the EtherCAT slaves via EtherCAT along with the process data organized in PDOs (Process Data Objects).

> 描述图例组件

Notice that every AC Type, besides the ports exposed from inner artifacts or port delegation, will implement an extra port for receiving events that manage the lifecycle after instantiated at run-time.

> lifecycle port



![AC_Type](.\img\AC_Type.png)



#### Lifecycle

In [] [] [], the authors proposed an idealized life cycle of software components consisting of three phases: *design*, *deployment* and *run-time*. 

>

We bring these concepts into the development workflow of ICS. 

>

Figure x  describes the three phases and several stages when constructing a distributed system based on OSAI. 

>

In the first place, OEM, the integrator, defines the hardware architecture and model the software architecture of the system. 

>

By mapping the software architecture with the hardwares, OEM can determine how many and what kind of components (ACs) they need and what run-time supports these components require. 

>

Consequently, two kinds of XML-based files will be generated from the architecture: Component Type Definition (CTD) and Instance Wiring Description (IWD).

CTD describes properties of the AC Type and specification of ports on its surface, remaining the internal contents and behaviors unimplemented; IWD gives a overall perspective about how AC Instances are assembled with the connection among ports.

![](.\img\workfolw.png)

 In a collaborative development context, the design phase is often conducted by multiple developers (or vendors). 

CTD files are assigned to vendors for the development of each AC's internal artifacts.

Developers design and implement AC Types independently based on the standardized interface definition (based on XML descriptor) and then store components in respective repositories. 

ACs can be retrieved from the repository and composed into Composite AC Types, which can also be deposited back for further retrieve and reuse, that conform with the specification in CTD.

During the deployment phase, ACs are compiled into object code for target platform and delivered to the integrator (or OEM). The CTD files are also merged back including complete xml-based description of internal behaviors and AC Type References  of each AC Type.

To build the control system, the integrator retrieves components from the object repository and deploy them into the run-time environment (RTE) along with the IWD file which specifies the ports connection and initialization parameters for instantiation.

An AC may have one or multiple instance. 

These instances communicate with each other at run-time via *Unified Dynamic Bus*, which will be detailed later.

> state machine

RTE of OSAI Component Model has the ability to, by means of UDB and *Component Adaption Manager*, have  AC Instances loaded, unloaded or updated.

[back](#Component Model of Open Software Architecture for Industry (OSAI))



## OSAI Run-time Architecture

> OSAI为工业应用场景中的不同设备、多平台、分布式的特点提出一种统一的架构，以供AC能够部署和执行，并且AC实例之间通过一个统一的总线进行交互。

OSAI proposes a unified architecture for diverse devices, multiple platforms, and distributed features in the industrial application scenarios, to guarantee the deployment and execution of AC instances, and interactions between AC instances are implemented by a united bus.

> OSAI的运行时架构针对ICS中多种设备，不同性能配置（具备Heterogeneous 特性），包括那些配置较高的能部署OS包括实时OS的设备和那些小的基于MCU的设备。

The run-time architecture of OSAI aims at a variety of devices with different performance configurations ( the heterogeneous characteristic) in ICS, including both high-profile devices can deploy OS(real-time OS included) and small-scale devices based on MCU.

> 部署在这不同的两类设备的上AC也依此被分为PAC和LAC，在OSAI的分布式架构中，每一台嵌入式设备被称为一个node

ACs deployed on these two types of devices are divided into PAC and LAC accordingly. In the distributed architecture of OSAI, each embedded device is called a *Node*.

> LAC被部署在小型MCU设备上，通常在此类Node中只会一个LAC，与其一同部署的还有一个为这个LAC特定低层级的运行时环境（如IEC 61131-3程序运行时）

LACs are deployed on small MCU devices, typically there is a single LAC in such a Node, along with a specific run-time environment for this LAC at the low level, such as the IEC 61131-3 runtime.

> 至于能力capability较强的设备，OSAI给出了一个分层架构包括 Containerized Instance Layer 和 Runtime infrastructure layer

As for devices with powerful capability, OSAI defines a hierarchical architecture including Dynamic Instance Layer and Runtime Infrastructure Layer.

> Dynamic Instance Layer
> 这一层是所有AC部署并运行的地方，如同spring的IoC容器，组件被载入到这一层，基于IWD文件，由AC Type被实例化为AC Instance，随后实例间彼此建立连接。

- Dynamic Instance Layer

This layer is where all of ACs are deployed and executed, like IoC container in Spring, ACs are loaded into this layer. Based on the IWD file, an AC Instance is instantiated by an AC Type, which then connections between each other are established.

> 所有的PAC实例都被docker-like的容器包裹着以隔离组件，实例容器间通过OSAI Connection进行连接通信

 A PAC instance is wrapped by a docker-like container to perform the isolation between components, and communications among containers are implemented via OSAI Connection.

> 这一层中的所有组件都由Run-time Infrastructure Layer中组件适配管理器CAM管理，an particular component 读取IWD文件，实例化组件，为端口建立连接，接口事件的绑定，并触发控制组件生命周期的事件

Components in this layer are under the management of CAM (Component Adapter Manager) in Run-time Infrastructure Layer, a particular component responsible for reading IWD files, instantiating components, linking connections of ports, bounding interface events, and triggering events that control component run-cycle.

+ Run-time Infrastructure Layer

Run-time Infrastructure Layer runs above OS and contains modules for run-time support.

> RIL运行在OS之上 包括了对运行时的支持

It is a basic layer that provides a configurable RTE for the deployment and execution of PAC including some device-specific IO drivers. 

> 这是一个可扩展的层 为PAC的部署和执行提供了一个可配置的RTE，包括一些设备相关的IO驱动

Modules in the Run-time Infrastructure Layer are scalable for different Nodes, and these modules interact with the PACs in the Dynamic Instance Layer via the UDB. Specifically, the data passing or function calls between an PAC and module in the Run-time Infrastructure Layer are through the similar ports connection as the communication between PACs. 



 while some requisite modules offer essential facilities for PACs on each Node. 



Operating System Abstraction Server (OSAS)  







 

![Architecture](.\img\Architecture.png)

#### Unified Dynamic Bus



#### Dynamics

![statemachine](.\img\statemachine.png)

In order to better perform the component lifecycle management in runtime and support dynamic properties of the component model  (i.e., loading, unloading and updating components on the fly), each component provides a state machine to describe state transitions of component instances during service life.

The state machine defines the possible states that an instance of a component can experience states as INIT, LOADED, READY, ACTIVE, DEACTIVE, DELETED, and BLOCKED, UPDATED needed for dynamic updates, as shown in figure X. The dynamic behavior of a component instance at runtime is as follows:

Each component has a lifecycle interface of event-triggered type,  which enables migrations between component states by calling operations on that interface, such as loaded2ready(), active2deactive(), and so on. (1) - (6) state transitions are the processes of a component from start to end in general, and (7) - (9) are additional parts that components go through when they need to be updated dynamically at runtime.

1) In the INIT state, the instantiation takes place, parameters are initialized, and the lifecycle interface is created. At the same time the component registers and loads with the help of CAM, providing interface information, and resources are allocated by the system.

2) By calling *loaded2ready()*, the corresponding DDS entities with QoS policies are created according to the component  interfaces. Establishing connections with other components, the component waits to run.

3) The role of *ready2active()* is to make the component instance start working by binding services to runtime tasks. In the ACTIVE state, the component can communicate with other components.

4) When the component execution is completed without updating requirement, the *active2deactive()* operation executes, CAM refuses new invocations for the component and does not deactivate the component until the current invocation is processed.

5) After the component completes the execution of the current invocation, the DDS entities are deleted, components are disconnected, services and tasks are unbound, and resources are released.

6) Calling the operation *loaded2deleted() *, the component unloads with logout of interfaces.

7) Especially, if the component needs to be updated, CAM blocks new invocations and communications for this component, while the old component continuing to process the current invocation.

8) Once the component that needs to be replaced completes the current execution, with the operation *blocked2updated() *, CAM unties services with tasks, unlink connections, replaces the new component instance, redirects the reference, resets connections, and dynamic updating completes.

9) Now wake up invocations that previously blocked, the new component return to the ACTIVE state continues to run after binding services to tasks.



As a component model applies to ICS, in order to better deal with system failures or upgrade requirements when the system is on running, we provide enough support to component dynamic updating, which is a great help in bug fixing, adding functionalities, runtime optimization, and avoiding the huge cost of downtime. 

In fact, we benefit a lot from choosing the DCPS model as message middleware to implement the dynamic update. Flexible configuration of QoS policies and the discovery mechanism make it easy to reconnect components, with minimal or no impact on other components and the system.

In the DCPS model, each entity has a set of QoS policies to describe data transmission. The corresponding QoS policies can be configured to properly constrain the communication cycle, reliability, resource allocation, etc. There are some QoS policies that provide effective support for dynamic update of components, such as:

- durability- specifies whether or not the context will store and deliver previously published data to new entities.
- liveness - specifies and configures the mechanism that allows the DataReader to detect when the DataWriter becomes disconnected or "dead.
- resource limits - controls the amount of physical memory allocated for entities and how they occur if the dynamic allocation is allowed.

To be specific, QoS policies can be used to block data when the component receiving messages needs to be updated dynamically. After the update operation starts, the old component exists, i.e., the original DataReader offline. DDS uses the durability QoS policy to preserve the unreceived data on permanent storage. The update finishes and the new DataReader broadcasts the subscription, DDS sends the staging data. It is similar if there is a component gets a need for  dynamic updating when it sends messages. After the old component is removed, DataReader detects the disconnection of the original DataWriter via the liveness QoS, so it begins to receive the data sent by the new component.

[back](#Component Model of Open Software Architecture for Industry (OSAI))

## Evaluation & Implementation

#### Comparison

In order to reflect the characteristics and evaluate the properties of the OSAI component model more systematically, we selected some similar component models for comparison. We choose both domain-specific and general-purpose models to make this comparison more meaningful. Following the dimensions in basic requirements and advanced features, the information of these component models is collected in table Ⅰ. From the contract,  we can summarize these models roughly as follows (in alphabetic order) : 

- AUTOSAR (AUTomotive Open System ARchitecture) is a standardized software architecture in the automotive field. The standard supports both client-server and sender-receiver communication modes. AUTOSAR provides hierarchical architecture at design-time and supports multiple instantiation at runtime.
- CCM (CORBA Component Model) is a general-purpose component model based on the CORBA middleware. Thus components are not tied to any particular language or platform as long as the CORBA middleware is used. Component interfaces are described in the CORBA IDL and they provide synchronous or asynchronous communication.
- IEC 61499 provides a component solution for automation and distributed control systems extended by IEC 61131. The execution of components called Function Blocks(FBs) in this standard is event-driven and data transmission is via pipe&filter way. The implementation platform and language with IEC 61499 are also not limited.
- Rubus is a domain-specific component model focusing on real-time properties and resource-constraint problems of embedded systems. It runs on the base of the Rubus OS. Implementation is done by C and communication style is pipe&filter.
- SOFA 2.0 is an extension of the SOFA(Software Appliances) component model with hierarchical architecture and features like dynamic reconfiguration, control parts, and multiple communication patterns. SOFA 2.0 has to be developed by Java language, so it is only compatible with platforms supporting JVM(Java Virtual Machine).
- TwinCAT(The Windows Control and Automation Technology) is a PC-based automation software developed by Beckhoff. TwinCAT offers TcCOM (TwinCAT Component Object Model) derived from the COM (Component Object Model) to define characteristics and behaviors of application modules. Modules in TwinCAT support multiple instantiation and periodic tasks. However, the run-time environment of TwinCAT relies on specific hardware devices.

From the table, we can observe that OSAI component model not only possesses characteristics like  hierarchical design and the independency of language and platform, but also provides advanced features including multiple communication styles based on DCPS model, multiple instantiation, and run-time dynamics. 

Equipped with comparatively comprehensive features above, OSAI component model, to some extent, can be applied to embedded distributed systems in more domains, not merely for ICS.   

#### Analysis

#### Case Implementation



## Future work

## Conclusion 









