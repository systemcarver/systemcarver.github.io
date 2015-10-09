---
layout: post
title:  "Introduction to Virtualization"
date:   2015-10-09 16:51:30
categories: infrastructure
tags: virtualization
---
Virtual infrastructure provides tremendous value in improved resource utilization, superior manageability and flexibility, and increased application availability.

从硬件到应用程序的5个虚拟化抽象层次：

<center>![virtualization levels](/img/virtualization-level.png)</center>

其中指令集体系结构级虚拟化通常仅能达到20%左右的效率，难以实际应用;而库支持级和应用程序级在应用灵活性上具有较大限制，因此硬件抽象级和操作系统级是我们讨论的主要问题。

## 硬件虚拟化

### Hypervisor -- type-1 & type-2
 hypervisor也称为virtual machine monitor (VMM)，根据是否直接运行在host's hardware之上分为[两类](https://en.wikipedia.org/wiki/Hypervisor)：

- Type-1: native or bare-metal hypervisors
- Type-2: hosted hypervisors

The Xen Project hypervisor is an open-source type-1 or baremetal hypervisor（有些Xen属于type-2）.

### Guest Types -- 全虚拟化与半虚拟化
The Xen hypervisor supports running two different types of guests: Paravirtualization (PV, 半虚拟化) and Full or Hardware assisted Virtualization (HVM，Hardware Virtual Machine；硬件虚拟化/全虚拟化)。 全虚拟化要求CPU支持虚拟化，对guest OS没有任何要求，可以运行Linux或Windows等; 半虚拟化对CPU无要求，但要求guest OS具有PV-enabled kernel以与hypervisor协作。 http://wiki.xen.org/wiki/Xen_Project_Software_Overview#Guest_Types

全虚拟化结合了二进制翻译和直接执行，对一些临界指令如对IO进行操作的指令采用二进制翻译的方法由VMM代为执行（VMware uses a combination of direct execution and binary translation techniques to achieve full virtualization of an x86 system ）。

- Full Virtualization or Hardware-assisted virtualizion (HVM) uses virtualization extensions from the host CPU to virtualize guests. **HVM requires Intel VT or AMD-V hardware extensions.** The Xen Project software uses Qemu to emulate PC hardware, including BIOS, IDE disk controller, VGA graphic adapter, USB controller, network adapter etc. Fully virtualized guests are usually slower than paravirtualized guests, because of the required emulation. 

- **PV delivers higher performance than full virtualization** because the operating system and hypervisor work together more efficiently, without the overhead imposed by the emulation of the system's resources. **Paravirtualized guests require a PV-enabled kernel and PV drivers**, so the guests are aware of the hypervisor and can run efficiently without emulation or virtual emulated hardware. 半虚拟化无法修改必源的操作系统， **For such unmodified guest operating systems, a virtualization hypervisor must either adopt the full virtualization approach or rely on hardware virtualization in the processor architecture.**

- The **hardware virtualization support** enabled by AMD-V and Intel VT technologies introduces virtualization in the x86 processor architecture itself.  **The emergence of virtualization hardware assist reduces the need to paravirtualize guest operating systems.** In fact, Xen vendors such as Virtual Iron have announced that they are supporting only full virtualization using AMD-V and Intel VT processors and are not supporting paravirtualization.

**KVM (for Kernel-based Virtual Machine) is a full virtualization solution for Linux on x86 hardware containing virtualization extensions (Intel VT or AMD-V). 现在较好的方法应该是直接利用硬件的虚拟化支持采用full virtualization的方法** 

## 操作系统级虚拟化
操作系统级虚拟化在一个操作系统中插入一个虚拟化层来划分机器的物理资源，它使得在一个操作系统内核中可以同时运行多个隔离的虚拟机。这种虚拟机也成为VE（Virtual Execution Environment，虚拟执行环境）、VPS（Virtual Private System，虚拟专用系统）或容器。从用户的视角来看，VE就像真实服务器。这意味着VE有自己的进程、文件系统、用户账号、带有IP地址的网络接口、路由表、防火墙规则及其他个人设置。尽管VE可为不同用户分别定制，但它们仍共享同一个操作系统内核。

一些操作系统级虚拟化工具：Docker，OpenVZ，Linux VServer

### OpenVZ
OpenVZ is operating system-level virtualization based on **a modified Linux kernel** that allows a physical server to run multiple isolated instances known as containers, virtual private servers (VPS), or virtual environments (VE).  OpenVZ is the basis of Virtuozzo, a virtualization solution offered by Odin. Virtuozzo(commerical software, https://openvz.org/Virtuozzo) is optimized for hosters and offers hypervisor (VMs in addition to containers), distributed cloud storage, dedicated support, management tools, and easy installation.

OpenVZ-legacy (RHEL6 based)	 Release Date: 14 Oct 2011  EOL(End of life): Nov 2019 -- https://openvz.org/Releases

OpenVZ容器是一个轻量虚拟机，一个完整的操作系统环境，它的主要用途是充当虚拟私有服务器; OpenVZ的主要用途是应用的运维，包括打包、转移、部署和运行等环节。

https://wiki.centos.org/HowTos/Virtualization/OpenVZ
