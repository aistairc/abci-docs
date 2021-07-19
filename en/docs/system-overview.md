# ABCI System Overview

## System Architecture

The ABCI system consists of 1,088 compute nodes with 4,352 NVIDIA V100 GPU accelerators, 120 compute nodes with 960 NVIDIA A100 GPU accelerators and other computing resources, shared file systems and ABCI Cloud Storage with total capacity of approximately 47 PB, InfiniBand network that connects these elements at high speed, firewall, and so on. It also includes software to make the best use of these hardware. And, the ABCI system uses SINET5, the Science Information NETwork, to connect to the Internet at 100 Gbps.

[![ABCI System Overview](img/abci_system_en.svg)](img/abci_system_en.svg)

The main specifications of the ABCI system are as follows:

| Item | Total Performance and Capacity: Compute Node (V) | Total Performance and Capacity: Compute Node (A) | Total Performance and Capacity |
|:--|:--|:--|:--|
| Theoretical Peak Performance (FP64) | 37.2 PFLOPS | 19.3 PFLOPS | 56.6 PFLOPS |
| Effective Performance by HPL | 19.88 PFLOPS[^1] | 11.48 PFLOPS | 22.20 PFLOPS[^2] |
| Effective Performance per Power by HPL | 14.423 GFLOPS/Watt | 21.89 GFLOPS/W | - |
| Theoretical Peak Performance (FP32) | 75.0 PFLOPS | 151.0 PFLOPS | 226.0 PFLOPS |
| Theoretical Peak Performance (FP16) | 550.6 PFLOPS | 300.8 PFLOPS | 851.5 PFLOPS |
| Theoretical Peak Performance (INT8) | 261.1 POPS | 599.0 POPS | 860.1 POPS |
| Total Memory Capacity | 476 TiB | 97.5 TiB | 573.5 TiB |
| Theoretical Peak Memory Bandwidth | 4.19 PB/s | 1.54 PB/s | 5.73 PB/s |
| Total Capacity of Local Storage | 1,740 TB | 480 TB | 2,220 TB |

[^1]: [https://www.top500.org/system/179393/](https://www.top500.org/system/179393/)
[^2]: [https://www.top500.org/system/179954/](https://www.top500.org/system/179954/)

## Computing Resources

Below is a list of the computational resources of the ABCI system.

| Node Type | Hostname | Description | # |
|:--|:--|:--|:--|
| Access Server | *as.abci.ai* | SSH server for external access | 2 |
| Interactive Node (V) | *es* | Login server for Compute Node (V), the frontend of the ABCI system | 2 |
| Interactive Node (A) | *es-a* | Login server for Compute Node (A), the frontend of the ABCI system | 2 |
| Compute Node (V) | *g0001*-*g1088* | Server w/ NVIDIA V100 GPU accelerators | 1,088 |
| Compute Node (A) | *a0001*-*a0120* | Server w/ NVIDIA A100 GPU accelerators | 120 |
| Memory-Intensive Node | *m01*-*m10* | Server w/ Intel Optane memory | 10 |

!!! note
    In the following descriptions, `Interactive Node` refers to both the interactive node (V) and the interactive node (A).
    Similarly, `Compute Node` refers to both the compute node (V) and the compute node (A).

!!! note
    Due to operational and maintenance reasons, some computing resources may not be provided.

Among them, interactive nodes, compute nodes(V), and memory-intensive nodes are equipped with 2 ports of InfiniBand EDR, compute nodes(A) are equipped with 4 ports of InfiniBand HDR and they are connected by InfiniBand switch group together with [Storage Systems](#storage-systems) described later.

Below are the details of these nodes.

### Interactive Node

The ABCI system provides two types of compute nodes: compute node (V) and compute node (A). To improve the convenience of program development for each compute node, we provide two types of interactive nodes: interactive node (V) and interactive node (A).

When developing a program for each compute node application, use the corresponding interactive node. It is possible to submit jobs to both compute nodes from either interactive node.

The interactive node of ABCI system consists of FUJITSU Server PRIMERGY RX2540 M4.
The interactive node is equipped with two Intel Xeon Gold 6148 Processors and 384 GiB of main memory available.

The specifications of the interactive node are shown below:

| Item| Description | # |
|:--|:--|:--|
| CPU | [Intel Xeon Gold 6148 Processor 2.4 GHz, 20 Cores (40 Threads)](https://ark.intel.com/products/120489/Intel-Xeon-Gold-6148-Processor-27-5M-Cache-2-40-GHz-) | 2 |
| Memory | 32 GiB DDR4 2666 MHz RDIMM (ECC) | 12 |
| SSD | SAS-SSD 3.2 TB | 4 |
| Interconnect | InfiniBand EDR (100 Gbps) | 2 |
| | 10GBASE-SR | 2 |

Users can login to the interactive node, the frontend of the ABCI system, using SSH tunneling via the access server.

The interactive node allows users to interactively execute commands, and create and edit programs, submit jobs, and display job statuses. The interactive node does not have a GPU, but users can use it to develop programs for compute nodes.

Please refer to [Getting Started ABCI](getting-started.md) for details of login method and [Job Execution](job-execution.md) for details of job submission method.

!!! warning
    Do not run high-load tasks on the interactive node, because resources such as CPU and memory of the interactive node are shared by many users. If you want to perform high-load pre-processing and post-processing, please the compute nodes.
	Please note that if you run a high-load task on the interactive node, the system will forcibly terminate it.

### Compute Node

The compute node in an ABCI system consists of a compute node (V) and a compute node (A).
To execute the program for the compute node, submit the program to the job management system as a batch job or an interactive job. Interactive jobs allow you to compile and debug programs, and run interactive applications, visualization software and so on. For details, refer to [Job Execution](job-execution.md).

#### Compute Node (V)

The compute node (V) of ABCI system consists of FUJITSU Server PRIMERGY CX2570 M4.
The compute node (V) is equipped with two Intel Xeon Gold 6148 Processors and four NVIDIA V100 GPU accelerators. In the entire system, the total number of CPU cores is 43,520 cores, and the total number of GPUs is 4,352.

The specifications of the compute node (V) are shown below:

| Item | Description | # |
|:--|:--|:--|
| CPU | [Intel Xeon Gold 6148 Processor<br>2.4 GHz, 20 Cores (40 Threads)](https://ark.intel.com/products/120489/Intel-Xeon-Gold-6148-Processor-27-5M-Cache-2-40-GHz-) | 2 |
| GPU | [NVIDIA V100 for NVLink<br>16GiB HBM2](https://www.nvidia.com/en-us/data-center/v100/) | 4 |
| Memory | 32 GiB DDR4 2666 MHz RDIMM (ECC) | 12 |
| NVMe SSD | [Intel SSD DC P4600 1.6 TB u.2](https://ark.intel.com/products/97005/Intel-SSD-DC-P4600-Series-1-6TB-2-5in-PCIe-3-1-x4-3D1-TLC-) | 1 |
| Interconnect | InfiniBand EDR (100 Gbps) | 2 |

Reference: [Block Diagram of Compute Node (V)](img/compute-node-v.png)

#### Compute Node (A)

The compute node (A) of ABCI system consists of FUJITSU Server PRIMERGY GX2570 successor.
The compute node (A) is equipped with two Intel Xeon Platinum 8360Y Processors and eight NVIDIA A100 GPU accelerators. In the entire system, the total number of CPU cores is 8,640 cores, and the total number of GPUs is 960.

The specifications of the compute node (A) are shown below:

| Item | Description | # |
|:--|:--|:--|
| CPU | [Intel Xeon Platinum 8360Y Processor<br>2.4 GHz, 36 Cores (72 Threads)](https://ark.intel.com/content/www/us/en/ark/products/212459/intel-xeon-platinum-8360y-processor-54m-cache-2-40-ghz.html) | 2 |
| GPU | [NVIDIA A100 for NVLink 40GiB HBM2](https://www.nvidia.com/en-us/data-center/a100/) | 8 |
| Memory | 32 GiB DDR4 3200 MHz RDIMM (ECC) | 16 |
| NVMe SSD | [Intel SSD DC P4510 2.0 TB u.2](https://ark.intel.com/content/www/us/en/ark/products/122580/intel-ssd-dc-p4510-series-2-0tb-2-5in-pcie-3-1-x4-3d2-tlc.html) | 2 |
| Interconnect | InfiniBand HDR (200 Gbps) | 4 |

### Memory-Intensive Node

The memory-intensive node of ABCI system consists of Supermicro 4029GR-TRT2.
The memory-intensive node is equipped with two Intel Xeon Gold 6132 Processors and two Intel Optane memory, and up to 2.6 TiB of memory can be used together with the main memory.

The specifications of the memory-intensive node are shown below:

| Item | Description | # |
|:--|:--|:--|
| CPU | [Intel Xeon Gold 6132 Processor<br>2.6 GHz, 14 Cores (28 Threads)](https://ark.intel.com/content/www/us/en/ark/products/123541/intel-xeon-gold-6    132-processor-19-25m-cache-2-60-ghz.html) | 2 |
| Memory | 32 GiB DDR4 2666 MHz RDIMM (ECC) | 24 |
| SSD | [Intel SSD DC S4500 1.9 TB](https://ark.intel.com/content/www/us/en/ark/products/120524/intel-ssd-dc-s4500-series-1-9tb-2-5in-sata-6gb-s-3d1-tlc.html) | 1 |
| Optane SSD | [Intel Optane SSD DC P4800X 1.5 TB](https://ark.intel.com/content/www/us/en/ark/products/187934/intel-optane-ssd-dc-p4800x-series-with-intel-memory-drive-technology-1-5tb-1-2-height-pcie-x4-3d-xpoint.html) | 2 |
| Interconnect | InfiniBand EDR (100 Gbps) | 2 |

To execute the program for the memory-intensive node, submit the program to the job management system as a batch job or an interactive job, as with the compute node.

## Storage Systems

The ABCI system has five storage systems for storing large amounts of data used for AI and Big Data applications, and these are used to provide shared file systems and ABCI Cloud Storage. The total effective capacity is up to 47 PB.

| # | Storage System | Media | Usage |
|:--|:--|:--|:--|
| 1 | DDN SFA 14KX x1<br>DDN SS9012 Enclosure x5 | 7.68 TB SAS SSD x185 | Home area, Application area |
| 2 | DDN ES7990X x3<br>DDN SS9012 Enclosure x6 | 18 TB NL-SAS HDD x801 | Group area |
| 3 | DDN ES400NVX x3 | 7.68 TB NVMe HDD x69 | Fast data area |
| 4 | DDN SFA 14KX x3<br>DDN SS8462 Enclosure x30 | 3.84 TB SAS SSD x216<br>12 TB NL-SAS HDD x2400 | Group areas |
| 5 | HPE Apollo 4510 Gen10 x24 | 12 TB SATA HDD x1440 | ABCI Cloud Storage |

Below is a list of shared file systems and ABCI Cloud Storage provided by the ABCI system using the above storage systems.

| Usage | Mount point | Capacity | File system | Notes |
|:--|:--|:--|:--|:--|
| Home area | /home | 1.0 PB | Lustre | See [Home Area](storage.md#home-area) |
| Group area | /groups | 10.8 PB | Lustre | See [Group Area](storage.md#group-area) |
| Group area 1 | /groups1 | 7.2 PB | GPFS | See [Group Area](storage.md#group-area) |
| Group area 2 | /groups2 | 7.2 PB | GPFS | See [Group Area](storage.md#group-area) |
| Group area 3 | /groups3 | 7.2 PB | GPFS | Reserved for special purposes |
| ABCI Cloud Storage | | 13 PB max. | | See [ABCI Cloud Storage](abci-cloudstorage.md) |
| Fast data area | /bb | 0.3 PB | | Reserved area for the particular application |

Interactive nodes, compute nodes, and memory-intensive nodes mount the shared file systems, and users can access these file systems from common mount points.
However, the group area 1,2,3 cannot be accessed from the compute node (A).

Besides this, these nodes each have local storage that can be used as a local scratch area. The list is shown below.

| Node type | Mount point | Capacity | File system | Notes |
|:--|:--|:--|:--|:--|
| Interactive node | /local | 12 TB | XFS | |
| Compute node (V) | /local | 1.6 TB | XFS | See [Local Storage](storage.md#local-storage) |
| Compute node (A) | /local1 | 2.0 TB | XFS | See [Local Storage](storage.md#local-storage) |
|                 | /local2 | 2.0 TB | XFS | See [Local Storage](storage.md#local-storage) |
| memory-intensive node | /local | 1.9 TB | XFS | See [Local Storage](storage.md#local-storage) |

## Software

The software available on the ABCI system is shown below.

| Category | Software | Interactive/Compute Node (V) Version | Interactive/Compute Node (A) Version |
|:--|:--|:--|:--|
| OS | CentOS | 7.5 | - |
| OS | Red Hat Enterprise Linux | - | 8.2 |
| Job Scheduler | Univa Grid Engine | 8.6.17 | 8.6.17 |
| Development Environment | [CUDA Toolkit](gpu.md#cuda-toolkit) | 8.0.61.2<br>9.0.176.4<br>9.1.85.3<br>9.2.88.1<br>9.2.148.1<br>10.0.130.1<br>10.1.243<br>10.2.89<br>11.0.3<br>11.1.1<br>11.2.2 | 10.0.130.1<br>10.1.243<br>10.2.89<br>11.0.3<br>11.1.1<br>11.2.2 |
| | NVIDIA HPC SDK | 20.11<br>21.2 | 20.11<br>21.2 |
| | PGI Professional Edition | 20.4 | 20.4 |
| | Intel Parallel Studio XE Cluster Edition<br>(compilers and libraries) | 2020 update 4 (2020.4.304) | 2020 update 4 (2020.4.304) |
| | Intel VTune | 2020.3 | 2020.3 |
| | Intel Trace Analyzer and Collector | 2020.0.3 | 2020.0.3 |
| | Intel Inspector | 2020.3 | 2020.3 |
| | Intel Advisor | 2020.3 | 2020.3 |
| | GCC | 4.8.5<br>7.4.0<br>9.3.0 | 7.4.0<br>8.3.1<br>9.3.0 |
| | cmake | 3.11.4<br>3.19 | 3.11.4<br>3.19 |
| | [Python](python.md) | 2.7.18<br>3.6.12<br>3.7.10<br>3.8.7 | 2.7.18<br>3.6.12<br>3.7.10<br>3.8.7 |
| | Ruby | 2.0.0.648-33 | 2.5.5-105 |
| | R | 4.0.4 | 4.0.4 |
| | Java | 1.7.0\_171<br>1.8.0\_242<br>11.0.6\_10 | 1.8.0.242<br>11.0.6.10<br>15.0.2.0.7 |
| | Scala | 2.12.6 | 2.10.6 |
| | Lua | 5.3.6<br>5.4.2 | 5.3.6<br>5.4.2 |
| | Perl | 5.16.3 | 5.26.3 |
| | Go | 1.14<br>1.15 | 1.14<br>1.15 |
| | Julia | 1.0<br>1.5 | 1.0<br>1.5 |
| | Hadoop | 3.3 | 3.3 |
| | Spark | 3.0 | 3.0 |
| File System | [DDN Lustre](storage.md#home-area) | 2.12.6\_ddn13-1 | 2.12.6\_ddn13-1 |
| | [DDN GRIDScaler](storage.md#group-area) | 4.2.3-20 | - |
| | [BeeOND](storage.md#beeond-storage) | 7.2.1 | 7.2.1 |
| Object Storage | Scality S3 Connector | 7.4.8.1 | 7.4.8.1 |
| Container | [Docker](containers.md#docker) | 19.03.15 | 19.03.15 |
| | [SingularityPRO](containers.md#singularity) | 3.7-1 | 3.7-1 |
| MPI | [Open MPI](mpi.md#open-mpi) | 2.1.6<br>3.1.6<br>4.0.5 | 2.1.6<br>3.1.6<br>4.0.5 |
| | [MVAPICH2](mpi.md#mvapich2) | 2.3.5 | 2.3.5 |
| | [MVAPICH2-GDR](mpi.md#mvapich2-gdr) | 2.3.5 | - |
| | [Intel MPI](mpi.md#intel-mpi) | 2019.9 | 2019.9 |
| Library | [cuDNN](gpu.md#cudnn) | 5.1.10<br>6.0.21<br>7.0.5<br>7.1.4<br>7.2.1<br>7.3.1<br>7.4.2<br>7.5.1<br>7.6.5<br>8.0.5<br>8.1.1<br>8.2.0 | 7.3.1<br>7.4.2<br>7.5.1<br>7.6.5<br>8.0.5<br>8.1.1<br>8.2.0<br>8.2.1 |
| | [NCCL](gpu.md#nccl) | 1.3.5-1<br>2.1.15-1<br>2.2.13-1<br>2.3.7-1<br>2.4.8-1<br>2.5.6-1<br>2.6.4-1<br>2.7.8-1<br>2.8.4-1<br>2.9.6-1 | 2.3.7-1<br>2.4.8-1<br>2.5.6-1<br>2.6.4-1<br>2.7.8-1<br>2.8.4-1<br>2.9.6-1<br>2.9.9-1 |
| | gdrcopy | 2.0 | 2.1 |
| | UCX | 1.7.0 | 1.9.0 |
| | libfabric | 1.7.0-1 | 1.9.0rc1-1 |
| | Intel MKL | 2020.0.4 | 2020.0.4 |
| Utility | aws-cli | 2.1 | 2.1 |
| | fuse-sshfs | 3.7.1 | 3.7.1 |
| | s3fs-fuse | 1.87 | 1.87 |
| | sregistry-cli | 0.2.36 | 0.2.36 |
