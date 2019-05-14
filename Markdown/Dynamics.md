## Dynamics

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