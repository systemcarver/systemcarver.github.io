---
layout: post
title:  "Introduction to Become a System Carver"
date:   2015-10-08 15:58:40
categories: datacenter
---
## What is the question?

对于一般互联网服务提供商来说，都需要面临服务器运维和管理方面的工作，尽管绝大多数管理员不需要做到datacenter级别或者涉及到很多的硬件维护操作，运维对DevOps(Development & Operations)理念同样发挥重要作用。在这里想要讨论和实现的目标是：自动化（automation），面对大数据的挑战，使得基础设施、应用可以自动动态地扩展; 最终想要实现的目标是能够在自行购买或通过云服务获得的基础设施上搭建自己的高度可扩展（salability）的、能够适应大数据业务需求的应用。

- Q1: 基于云服务提供的infrastructure，如何在其上构建可扩展的platform和application？
- Q2: 完全自行购买硬件，搭建自己的infrastructure甚至是datacenter，如何进行搭建？然后回到Q1

上述两个问题是相关的，Q2包含Q1，而Q2前半部分的问题在解决Q1问题时是非常有用的。在云计算技术快速发展的条件下，大部分服务提供商可能主要需要面临Q1. 无论是构建infrastructure还是platform，需要考虑的关键特性是：**scalability, flexibility**; 想要能够完美的解决Q2, 构建出一个modern datacenter是一个非常复杂的问题，涉及到IT之外的很多问题，仅就IT相关方面来说也包括非常之多的方面和层次。

------

对于一个极小规模的datacenter，Q2与Q1是几乎相同的问题，可以归结为下面的问题：

- Q: Provide you with servers, lots of servers, how do you use them?

想要解决这个问题首先需要**明确这些servers需要提供什么功能**（用于处理什么样的业务或者需要提供的服务形式）？

## Overall goals
Benjamin Hindman在DockerCon2014的一次报告中将[cluster management](https://blog.docker.com/2014/06/dockercon-video-cluster-management-and-containerization-at-twitter/)分为以下四个方面:

Cluster Management(Server / IT Automation):

1. configuration/package management
    - what/how do things get installed
2. deployment
    - what should run where? how should it be started/stoped?
3. naming
    - how should apps find each other?
4. monitoring

一般来说，一个Internet公司的platform需要满足如下两个要求：

- easy deployment for big data applications and web applications
- flexibility for development and test

回到上面提出的问题，假设提供给一个管理员很多的服务器（云服务获得或自行购买），那么需要搭建一个什么样的platform以满足上述要求呢？这是我们需要考虑的主要问题。
