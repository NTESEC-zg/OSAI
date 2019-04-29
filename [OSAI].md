# Component Model of Open Software Architecture for Industry (OSAI)

[TOC]

## Abstract



## Introduction



## Background and Related Work

#### Component Models

#### Publish/Subscribe Communication Paradigm

#### A Case Study

## Component Model

In this section, we present the OSAI Component Model  and illustrate how the components are composited, instantiated, executed, and how components interact with each other. 

> 在这一章节中，我们提出了OSAI的组件模型，解释了组件是如何被组合、实例化、执行和组件间如何交互的

The OSAI Component Model distinguishes components from their instance and draws on the object-oriented paradigm. 

> OSAI组件模型区分组件和组件的实例并采用了面向对象的模式

By giving the description of life-cycle, component elements and the Unified Dynamic Bus, we combine the general-purpose software engineering methods with the "OEM-Vendor" industrial cooperating pattern and give a practical component model with both academic and industrial features.

>通过给出组件的生命周期元素和UDB，我们把通用软件工程方法和“OEM-Vendor”工业协作模式相结合并给出包括工业和学术特性的实用组件模型。

#### Component & Composition

A component of OSAI Component Model is called Application Component (AC). 

>一个组件叫做AC

To adopt the object-oriented paradigm, one key feature of OSAI Component Model is that AC can be differentiated between *AC Type* and *AC Instance*, and an AC Type will be instantiated into one or multiple AC Instances at run-time. 

>AC 可以分为 AC Type和AC Instance，AC Type 可以实例化成AC Instance，并可以多实例。

In terms of composition,  ACs can be composed at type-level, bringing two kinds of AC Types: *Atomic AC Type* and *Composite AC Type*. 

>在组合方面，AC可以在类型层面组合，这带来两类AC Types：原子的 和 组合的

An Atomic AC Type encapsulates the specific functionalities and a Composite AC Type aggregates existing AC Types (atomic ones and composite ones). 

>

Service

interface 

Capacity description

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

#### Port & Interface



## Run-time Architecture

#### Unified Dynamic Bus

#### Dynamics



## Evaluation & Implementation

#### Comparision

#### Analysis

#### Case Implementation



## Future work

## Conclusion 









