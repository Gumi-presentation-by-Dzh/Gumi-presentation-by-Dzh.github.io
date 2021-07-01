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

### Publication

* **Zhuohui Duan**, Haikun Liu, Xiaofei Liao, Hai Jin. "A Performance Emulator for Non-volatile Memory", Symposium on Operating Systems Principles Posters (SOSP Posters), 2017. [[slides](https://sosp17posters.hotcrp.com/doc/sosp17posters-paper43.pdf)]

* **Zhuohui Duan**, Haikun Liu, Xiaofei Liao, Hai Jin. "[HME: A Lightweight Emulator for Hybrid Memory](https://ieeexplore.ieee.org/abstract/document/8342227)", Proceeding of 2018 Design, Automation & Test in Europe Conference & Exhibition (DATE), 2018. ([<span style="color:red">Best Paper Award Nominations</span>](https://past.date-conference.com/proceedings-archive/2018/html/bestpaper.html)) [[slides](https://past.date-conference.com/proceedings-archive/2018/pdf/0731.pdf)] [[code](https://github.com/CGCL-codes/HME)]


---
>## 2. AHME: A Glibc-based NVM programming interface for HME

### Overview



### Publication

* **Zhuohui Duan**, Haikun Liu, Xiaofei Liao, Hai Jin. "A Performance Emulator for Non-volatile Memory", Symposium on Operating Systems Principles Posters (SOSP Posters), 2017. [[slides](https://sosp17posters.hotcrp.com/doc/sosp17posters-paper43.pdf)]

* **Zhuohui Duan**, Haikun Liu, Xiaofei Liao, Hai Jin. "[HME: A Lightweight Emulator for Hybrid Memory](https://ieeexplore.ieee.org/abstract/document/8342227)", Proceeding of 2018 Design, Automation & Test in Europe Conference & Exhibition (DATE), 2018. ([<span style="color:red">Best Paper Award Nominations</span>](https://past.date-conference.com/proceedings-archive/2018/html/bestpaper.html)) [[slides](https://past.date-conference.com/proceedings-archive/2018/pdf/0731.pdf)] [[code](https://github.com/CGCL-codes/HME)]
