在类UNIX操作系统中，每个[进程](https://zh.wikipedia.org/wiki/进程)都有自己的根目录。

对于大多数进程，这与系统的实际根目录相同，但可以通过调用[chroot](https://zh.wikipedia.org/wiki/Chroot)系统命令调用来更改它。

**chroot**是起源于Unix系统的一个操作，作用于正在运行的进程和它的[子进程](https://zh.wikipedia.org/wiki/子进程)，改变它外显的[根目录](https://zh.wikipedia.org/wiki/根目录)。一个运行在这个环境下，经由chroot设置根目录的程序，它不能够对这个指定根目录之外的文件进行访问动作，不能读取，也不能更改它的内容。

<u>这通常用于创建隔离环境以运行需要传统库的软件，有时还可以简化软件安装和调试。</u>

 Chroot并不意味着用于增强安全性，因为内部的进程可以透过第二次chroot来获得足够权限，逃出chroot的限制。

FreeBSD提供了一个更强大的[jail()](https://zh.wikipedia.org/wiki/FreeBSD_jail)系统调用，<u>它支持[操作系统层虚拟化](https://zh.wikipedia.org/wiki/作業系統層虛擬化)</u>，并且还用于安全目的，以限制进程可以访问文件系统层次结构的一个子集的文件。

这个技术的目标是：

1. 虚拟化：每个软件监狱（jail）都是在主机机器上运行的一个[虚拟环境](https://zh.wikipedia.org/wiki/虛擬機器)，有它自己的

    - 文件系统，

    - 进程，

    - 用户与超级用户的账户。

        

        **在软件监狱内运行的行程，它面对的环境，跟实际的操作系统环境几乎是一样的。**

        

2. 安全性：每个软件监狱都是独立运作，与其他软件监狱隔离，因此能够提供额外的安全层级。

    

3. 容易删除及创建：因为每个软件监狱的运作范围有限，这使得系统管理者可以在不影响整体系统的前提下，以超级用户的权限，来删除在软件监狱下运作的行程。

这个技术是基于[类Unix](https://zh.wikipedia.org/wiki/类Unix)系统下的[chroot](https://zh.wikipedia.org/wiki/Chroot)机制，进一步发展而来。

chroot jail限制进程<u>只能</u>访问某个部分的文件系统

但是FreeBSD jail机制限制了在软件监狱中运作的行程，不能够影响**操作系统的其他部分**。

也就是说，在软件监狱中的行程，是运作在一个[沙盒](https://zh.wikipedia.org/wiki/沙盒_(電腦安全))上。

**沙盒**（英语：sandbox，又译为**沙箱**）是一种安全机制，为运行中的程序提供的隔离环境。

通常是作为一些来源不可信、具破坏力或无法判定程序意图的程序提供实验之用[[1\]](https://zh.wikipedia.org/wiki/沙盒_(電腦安全)#cite_note-1)。

沙盒通常严格控制其中的程序所能访问的资源。比如，沙盒可以提供[用后即回收](https://zh.wikipedia.org/wiki/塗銷空間)的磁盘及内存空间。在沙盒中，网络访问、对真实系统的访问、对输入设备的读取通常被禁止或是严格限制。

从这个角度来说，沙盒属于[虚拟化](https://zh.wikipedia.org/wiki/虚拟化)的一种。

以下是一些沙盒的具体实现：

- [软件监狱（Jail）](https://zh.wikipedia.org/wiki/FreeBSD_jail)：限制网络访问、受限的文件系统名字空间。软件监狱最常用于[虚拟主机](https://zh.wikipedia.org/wiki/虚拟主机)上。

**虚拟主机**（英语：virtual hosting）或称 **共享主机**（shared web hosting），又称**虚拟服务器**，是一种在单一主机或主机群上，实现多网域服务的方法，可以运行多个[网站](https://zh.wikipedia.org/wiki/網站)或服务的技术。虚拟主机之间完全独立，并可由用户自行管理，虚拟并非指不存在，而是指空间是由实体的服务器延伸而来，其[硬件](https://zh.wikipedia.org/wiki/硬體)系统可以是基于服务器群，或者单个服务器。

实现方式主要有三种：网址名称对应（Name-based）、IP地址对应（IP-based）以及Port端口号对应（Port-based）。

- 网址名称对应（Name-based）是借由识别客户端所以提供的网址，决定其所对应的服务，这个方法有效的减少IP地址的占用，但缺点是必须仰赖[DNS名称对应服务](https://zh.wikipedia.org/wiki/DNS)的支持，若名称对应服务中断，对应此名称的服务也会无法取用。

- IP地址对应（IP-based）是指在同一部服务器上，借由同一份配置设置、不同的IP来管理多个服务。

- 近似于IP地址对应，不过是在同一个IP之下，利用不同的Port端口号来区别不同的服务，藉以快速创建多个虚拟主机。例如：
    - 192.168.0.1:80
    - 192.168.0.1:8080
    - 192.168.0.1:8888

    不过这类的应用大多用在私人或实验性质的服务中，原因是用户无法利用默认的端口号（例如Web服务的默认端口号80）取用提供的服务，除非用户知道提供服务的端口号。

---



2002 年，Linux Kernel 2.4.19 版内核引入了一种全新的隔离机制：[Linux 名称空间](https://en.wikipedia.org/wiki/Linux_namespaces)（Linux Namespaces）。2006年，Linux内核中开发出[cgroups](https://zh.wikipedia.org/wiki/Cgroups)。2007年，被加到Linux 2.6.24版内核中。2008年，基于cgroups，开发出[LXC](https://zh.wikipedia.org/wiki/LXC)，以及Docker。2013年被加入Linux 3.8版中。

Linux 的名称空间是一种由内核直接提供的全局资源封装，是内核针对进程设计的访问隔离机制。进程在一个独立的 Linux 名称空间中朝系统看去，会觉得自己仿佛就是这方天地的主人，拥有这台 Linux 主机上的一切资源，不仅文件系统是独立的，还有着独立的 PID 编号（譬如拥有自己的 0 号进程，即系统初始化的进程）、UID/GID 编号（譬如拥有自己独立的 root 用户）、网络（譬如完全独立的 IP 地址、网络栈、防火墙等设置），等等.

到目前最新的 Linux Kernel 5.6 版内核为止，Linux 名称空间支持以下八种资源的隔离（内核的官网[Kernel.org](https://www.kernel.org/)上仍然只列出了[前六种](https://www.kernel.org/doc/html/latest/admin-guide/namespaces/compatibility-list.html)，从 Linux 的 Man 命令能查到[全部八种](https://man7.org/linux/man-pages/man7/namespaces.7.html)）。

| 名称空间 | 隔离内容                                                     | 内核版本 |
| :------- | ------------------------------------------------------------ | -------- |
| Mount    | 隔离文件系统，功能上大致可以类比`chroot`                     | 2.4.19   |
| UTS      | 隔离主机的[Hostname](https://en.wikipedia.org/wiki/Hostname)、[Domain names](https://en.wikipedia.org/wiki/Domain_name) | 2.6.19   |
| IPC      | 隔离进程间通信的渠道（详见“[远程服务调用](http://icyfenix.cn/architect-perspective/general-architecture/api-style/rpc.html)”中对 IPC 的介绍） | 2.6.19   |
| PID      | 隔离进程编号，无法看到其他名称空间中的 PID，意味着无法对其他进程产生影响 | 2.6.24   |
| Network  | 隔离网络资源，如网卡、网络栈、IP 地址、端口，等等            | 2.6.29   |
| User     | 隔离用户和用户组                                             | 3.8      |
| Cgroup   | 隔离`cgourps`信息，进程有自己的`cgroups`的根目录视图（在/proc/self/cgroup 不会看到整个系统的信息）。`cgroups`的话题很重要，稍后笔者会安排一整节来介绍 | 4.6      |
| Time     | 隔离系统时间，2020 年 3 月最新的 5.6 内核开始支持进程独立设置系统时间 | 5.6      |

对文件、进程、用户、网络等各类信息的访问，都被囊括在 Linux 的名称空间中，即使一些今天仍有没被隔离的访问（譬如[syslog](https://en.wikipedia.org/wiki/Syslog)就还没被隔离，容器内可以看到容器外其他进程产生的内核 syslog），日后也可以随内核版本的更新纳入到这套框架之内，现在距离完美的隔离性就只差最后一步：资源的隔离。

**cgroups**，其名称源自**控制组群**（英语：control groups）的简写，是[Linux内核](https://zh.wikipedia.org/wiki/Linux内核)的一个功能，用来限制、控制与分离一个[进程组](https://zh.wikipedia.org/wiki/行程群組)的[资源](https://zh.wikipedia.org/wiki/資源_(計算機科學))（如CPU、内存、磁盘输入输出等）。

cgroups的一个设计目标是为不同的应用情况提供统一的接口，从控制单一进程（像[nice](https://zh.wikipedia.org/w/index.php?title=Nice&action=edit&redlink=1)）到[操作系统层虚拟化](https://zh.wikipedia.org/wiki/作業系統層虛擬化)（像[OpenVZ](https://zh.wikipedia.org/wiki/OpenVZ)，[Linux-VServer](https://zh.wikipedia.org/w/index.php?title=Linux-VServer&action=edit&redlink=1)，[LXC](https://zh.wikipedia.org/wiki/LXC)）。cgroups提供：

- **资源限制：**组可以被设置不超过设定的[内存](https://zh.wikipedia.org/wiki/内存)限制；这也包括[虚拟内存](https://zh.wikipedia.org/wiki/虚拟内存)。[[3\]](https://zh.wikipedia.org/wiki/Cgroups#cite_note-3) [[4\]](https://zh.wikipedia.org/wiki/Cgroups#cite_note-ols-memcg-4)
- **优先级：**一些组可能会得到大量的CPU[[5\]](https://zh.wikipedia.org/wiki/Cgroups#cite_note-5) 或磁盘IO吞吐量。[[6\]](https://zh.wikipedia.org/wiki/Cgroups#cite_note-6)
- **结算：**用来度量系统实际用了多少资源。[[7\]](https://zh.wikipedia.org/wiki/Cgroups#cite_note-lf-hansen-7)
- **控制：**冻结组或检查点和重启动。