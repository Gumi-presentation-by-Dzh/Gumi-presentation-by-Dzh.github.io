---
layout: page
title: Project
comments: true
permalink: /project/
---

* content
{:toc}

# My Projects
---

>## 1. HME: A Lightweight Emulator for Hybrid Memory

### Overview

The advent of NVM technologies has increasingly attracted research interest in hybrid memory systems. Emerging NVM technologies, offers higher memory density, much lower cost per bit and standby power consumption than DRAM. However, because NVM has limited write endurance, and higher write latency and power consumption, a more practical way of using NVM is to organize it in conjunction with commodity DRAM.

There are many open issues about hardware/software design for hybrid memory systems, such as, memory hierarchy, hybrid memory management, and memory allocation schemes from the perspective of computer architectures, operation systems, and programming models, respectively. Moreover, the most interesting fact programmers are expected to know is that how the NVM latencies and bandwidth characteristics affect application performance. This knowledge is especially useful for directing the memory system designs and data placement policies.

While commercial NVM hardware exists for Intel Optane, its high cost of entry and inflexible configuration methods make it impossible to leverage it for introductory NVM research. Most system researchers mainly rely on cycle-accurate architecture simulators to investigate how a new algorithm or system design could improve the application performance under different settings of NVM latencies and bandwidth. However, current simulation approaches are either too slow, or cannot simulate complex and large-scale workloads. 

* We present a lightweight hybrid memory emulator, called HME. HME exploits existing hardware features in commodity Intel CPUs to emulate performance characteristics of NVM using DRAM. Generally, application perceived performance difference between NVM and DRAM is mainly determined by two performance characteristics (i.e., memory latency and bandwidth). We do not mimic the details of each memory access operation accurately, instead we focus on emulating the NVM latency and bandwidth to make it close to a given setting in a period of time.

* We exploit the DRAM thermal control interface provided by commodity Intel CPUs to limit the maximum memory bandwidth. It is more difficult to emulate NVM latency because current commodity hardware does not provide any knob to control the memory latency. Fortunately, we can program hardware performance counters to monitor the number of Last Level Cache (LLC) misses in a small time window (called epoch). The CPU stall time on a LLC miss can reflect the memory access latency. Thus, we inject additional software created delay (the difference between NVM and DRAM latencies) periodically to emulate the NVM latency. In this way, we slow down DRAM to mimic the performance characteristics of NVM

* We implement HME on NUMA based Intel Xeon processors. For a given processor, we use DRAM on a remote NUMA node to emulate the higher-latency and lowerbandwidth NVM. To facilitate programming on hybrid memory systems, we also redesign the memory allocator to explicitly support memory allocation from DRAM or NVM regions. We modify Linux kernel to identify memory zones of NVM, and extend the Glibc library to support nvm malloc interface.

### Publication

* **Zhuohui Duan**, Haikun Liu, Xiaofei Liao, Hai Jin. "A Performance Emulator for Non-volatile Memory", Symposium on Operating Systems Principles Posters (SOSP Posters), 2017. [[slides](https://sosp17posters.hotcrp.com/doc/sosp17posters-paper43.pdf)]

* **Zhuohui Duan**, Haikun Liu, Xiaofei Liao, Hai Jin. "[HME: A Lightweight Emulator for Hybrid Memory](https://ieeexplore.ieee.org/abstract/document/8342227)", Proceeding of 2018 Design, Automation & Test in Europe Conference & Exhibition (DATE), 2018. ([<span style="color:red">Best Paper Award Nominations</span>](https://past.date-conference.com/proceedings-archive/2018/html/bestpaper.html)) [[slides](https://past.date-conference.com/proceedings-archive/2018/pdf/0731.pdf)] [[code](https://github.com/CGCL-codes/HME)]


---
>## 2. HiNUMA: NUMA-aware Data Placement and Migration in Hybrid Memory Systems

### Overview

The advent of NVM technologies has attracted increasing research interest in hybrid memory systems. A typical use of NVM is to organize it as an extension of DRAM in a single (flat) address space. For example, Intel Optane DC Persistent Memory provides an App Direct Mode to use both NVM and DRAM as main memory. Due to the relatively large performance gap between NVM and DRAM, data placement and page migration policies have been widely studied to improve the performance of hybrid memory systems in recent years. Those studies all follow a basic principle that frequently-accessed (hot) data should be placed on fast memory (i.e., DRAM) to reduce the total memory access delay. Data placement policies usually rely on offline profiling techniques to characterize applications’ memory access patterns, which are used to guide the initial memory allocation at the object/block level. In contrast, page migration policies usually exploit online page access monitoring techniques to predict memory behaviors in the future, and then migrate hot data from NVM to DRAM. However, online memory monitoring at the software layer can cause significant performance overhead, while hardwareassistant approaches usually require disruptive modifications of current hardware architectures.

Modern multicore systems are usually based on NonUniform Memory Access (NUMA) architectures. A NUMA system is composed of multiple CPU nodes. Each CPU node has memory attached to it, and accesses its local memory much faster than other CPU’s memory. For the App Direct Mode of Intel Optane DC Persistent Memory, the OS kernel manages NVM and DRAM in different NUMA nodes. Because NVM is slower than DRAM, especially for write operations, hybrid memories would further intensifythe feature of non-uniform memory access latencies in NUMA systems. The traditional NUMA-aware memory allocation and load balancing strategies focus on reducing the cost of internode NUMA communication and improving memory access locality. They are no long effective in hybrid memory systems because the access latency of local NVM may be even higher than that of remote DRAM. The difference of access latency between DRAM and NVM becomes a more important factor than the cost of inter-node NUMA communication. The traditional NUMA policy may even slow down the execution of applications in hybrid memory environments. 

First, the diverse hybrid memory access latencies in NUMA systems make the initial data placement becomes a complicated problem. The NUMA programming library libnuma offers some sample data placement policies, such as local allocation and page interleaving. The local allocation policy only allocates memory from the node where the application process is executing. If the local memory is used up, it cannot use memory resource on remote nodes. This policy can achieve good performance because the local memory has the low access latency without inter-node communications, but cannot utilize memory resource on other NUMA nodes even there is sufficient memory in the machine. In contrast, the page interleaving policy allocates memory pages interleaved on all NUMA nodes in a round-robin manner. Although the memory access latency on the remote NUMA nodes becomes higher than the local accesses, it can fully utilize the memory bandwidth of multiple NUMA nodes and balance the memory load. However, due to different performance features (latency, bandwidth) of DRAM and NVM, data distribution on DRAM and NVM is usually not the best placement using the traditional NUMA policies. For example, frequently-accessed (hot) data may be placed on a remote NUMA node attached to NVM DIMMs. Thus, data placement policies should take into account both performance characteristics of hybrid memories and inter-node communication cost.

Second, the default NUMA memory migration policy is not effective in hybrid memory system. Applications generally perform best when the task accesses memory on the local NUMA node. The automatic NUMA balancing (ANB) strategy always tries to move application data to the memory node closer to the tasks (threads or processes). However, in hybrid memory systems, the access latency of NVM on a local node may be even higher than that of DRAM on remote nodes. Without considering the inherent performance gap between DRAM and NVM, ANB may falsely migrate application data to a place that is even slower than its prior residence. Moreover, ANB would migrate a remote page to local memory once it is accessed twice, and thus may lead to unnecessary page migrations and a waste of memory bandwidth. The traditional ANB does not consider the memory heterogeneity and data access frequency, and thus is not effective in hybrid memory systems and may even hurt application performance.

* We present HiNUMA, an extension of traditional NUMA mechanism for hybrid memory architectures. HiNUMA provides new memory access interfaces to distinguish NVM from DRAM in different NUMA groups. We propose two NUMA topology-ware hybrid memory allocation strategies, i.e., Low-latency and Hi-bandwidth for latencysensitive applications and bandwidth-sensitive applications, respectively. 
* We propose a new NUMA balancing mechanism called HANB for hybrid memory systems. It takes both data hotness and inter-node bandwidth utilization into account for page migrations. The proposed mechanisms are implemented in the operating system (OS) layer, without modifications of hardware and applications.

### Publication

* **Zhuohui Duan**, Haikun Liu, Xiaofei Liao, Hai Jin, Wenbin Jiang, Yu Zhang. "[HiNUMA: NUMA-aware Data Placement and Migration in Hybrid Memory Systems](https://ieeexplore.ieee.org/abstract/document/8988604)", Proceeding of 2019 IEEE 37th International Conference on Computer Design (ICCD), 2019. [[slides](https://scholar.google.com/scholar_url?url=https://wwww.easychair.org/publications/preprint_download/rv3h&hl=zh-CN&sa=T&oi=gsb-gga&ct=res&cd=0&d=1147414222658290591&ei=tcnaYOSNDIegyATW6L6gCA&scisig=AAGBfm0IlAsLh2r-2qc4e0bQN2j76IOHYg)]

