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

+ Dynamic Instance Layer

This layer is where all of ACs are deployed and executed, like IoC container in Spring, ACs are loaded into this layer. Based on the IWD file, an AC Instance is instantiated by an AC Type, which then connections between each other are established.

> 所有的PAC实例都被docker-like的容器包裹着以隔离组件，实例容器间通过OSAI Connection进行连接通信

 A PAC instance is wrapped by a docker-like container to perform the isolation between components, and communications among containers are implemented via OSAI Connection.

> 这一层中的所有组件都由Run-time Infrastructure Layer中组件适配管理器CAM管理，an particular component 读取IWD文件，实例化组件，为端口建立连接，接口事件的绑定，并触发控制组件生命周期的事件

Components in this layer are under the management of CAM (Component Adapter Manager) in Run-time Infrastructure Layer, a particular component responsible for reading IWD files, instantiating components, linking connections of ports, bounding interface events, and triggering events that control component run-cycle.

