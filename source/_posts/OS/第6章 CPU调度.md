---
title: 第6章 CPU调度
date: 2020-10-27 20:30:15
tags: 
categories: 操作系统
---
## CPU调度

### 6.1 CPU调度

#### CPU调度程序(Short-term Scheduler)

<!-- more -->

从内存中选择一个能够执行的进程，并为其分配CPU

#### 调度程序

将CPU控制交给由短期调度程序选择的进程

- 切换上下文
- 切换用户模式
- 跳转到用户程序的合适位置，以便重新启动程序

调度延迟：调度程序停止一个进程而启动另一个所需的时间

### 6.2 调度准则

- CPU利用率
- 吞吐率：在时间单元内进程完成的数量
- 周转时间：从进程提交到进程完成的时间段，包括等待进入内存、在就绪队列中等待、在CPU上执行和I/O执行
- 等待时间：在就绪队列中等待所花时间之和
- 响应时间：从提交请求到产生第一响应的时间

### 6.3 调度算法

#### 先到先服务调度(FCFS)

Example:

- one CPU-bound process
- many I/O-bound process

#### 最短作业优先调度(SJF)

##### 非抢占

一旦CPU被分配给进程，则在CPU执行完前不能抢占

##### 抢占

#### 最短剩余时间有限(SRTF)

![image-20201101135007962.png](https://i.loli.net/2020/11/01/wsDmx8lp2aCIghL.png)

Average waiting time: ((16 - 7) + (7 - 4 - 2) + (5 - 1 - 4) + (11 - 4 - 5)) / 4 = 3

#### 优先级调度

问题：饥饿

策略：老化增加优先级

#### 时间片轮转(RR)

#### 多级队列调度

将就绪队列分成多个单独队列，每个队列有自己的调度算法

- 固定优先级
- 划分时间片

#### 多级反馈队列调度

Three queues:

- Q0 –RR with time quantum 8 milliseconds
- Q1 –RR time quantum 16 milliseconds
- Q2 –FCFS

<img src="https://i.loli.net/2020/11/01/jmMDxZ6p2KiXRTJ.png" alt="image-20201101143850159.png" style="zoom: 67%;" />

### 6.4 线程调度

#### 进程竞争范围(PCS)

PCS发生在同一进程的线程之间，实现多对一和多对多模型的系统线程库会将线程调度到可用LWP上

#### 系统竞争范围(SCS)

SCS用来决定哪个内核级线程调度到处理器上，发生在系统内的所有线程之间，采用一对一模型的系统，只采用SCS调度

### 6.5 多处理器调度

#### 对称多处理器调度(SMP)

- 每个处理器自我调度

#### 静态进程分配

- 每个处理器有自己的就绪队列
- 处理器可能负载不均

#### 动态进程分配

- 进程可被分配到任何空闲的处理器上执行
- 所有处理器共享一个就绪队列
- 高调度开销
- 负载均衡

### 6.6 实时CPU调度

正确性取决于操作系统的时间和功能

#### 单调速率调度（rate-monotonic）

- 优先级与周期成反比

#### 最早截至期限优先(EDF)

- 优先级与deadline成正比

### 6.7 算法评估

#### 确定性模型

采用特定的预先确定的符合，计算在给定负荷下每个算法的性能

#### 排队模型

根据CPU和I/O的执行分布和进程到达系统的时间分布简单估计，得到数学公式

##### 排队网络分析

计算机系统：一个服务器网络，每个服务器都有一个等待进程队列

CPU: 就绪队列

I/O:设备队列(等价于等待队列)

#### 仿真/模拟

通过随机数生成器，根据概率分布生成进程、CPU执行、到达时间、离开时间等

##### 跟踪磁带(trace tape)

通过监视真实系统并记录事件发生顺序，使用这个顺序以便驱动仿真，在针对完全相同的实际输入的情况下，比较两种算法

<img src="https://i.loli.net/2020/11/01/pGkBhc5VAQg8bSN.png" alt="image-20201101150423518.png" style="zoom:80%;" />

### 作业

6.1 A CPU-scheduling algorithm determines an order for the execution
of its scheduled processes. Given n processes to be scheduled on one
processor, how many different schedules are possible? Give a formula
in terms of n.

n!

6.2 Explain the difference between preemptive and nonpreemptive scheduling.

非抢占调度在一个进程CPU执行完之前不能分配CPU给另一个进程，抢占调度可以在一个进程CPU执行时中断其执行，并将CPU分配给其他进程。抢占调度因为要频繁切换上下文，相对于非抢占调度来说会产生额外开销。

6.3 Suppose that the following processes arrive for execution at the times
indicated. Each process will run for the amount of time listed. In
answering the questions, use nonpreemptive scheduling, and base all
decisions on the information you have at the time the decision must be
made.

<img src="https://i.loli.net/2020/11/01/GyR62QmFh4xs3C7.png" alt="image-20201101150923649.png" style="zoom:80%;" />

a. What is the average turnaround time for these processes with the
FCFS scheduling algorithm?

执行顺序为P1、P2、P3

(8 + 12 - 0.4 + 13 - 1) / 3 = 10.53

b. What is the average turnaround time for these processes with the
SJF scheduling algorithm?

因为调度为非抢占的，所以执行顺序为P1、P3、P2

(8 + 9 - 1 + 13 - 0.4) / 3 = 9.53 

c. The SJF algorithm is supposed to improve performance, but notice
that we chose to run process P1 at time 0 because we did not know
that two shorter processes would arrive soon. Compute what the
average turnaround time will be if the CPU is left idle for the first
1 unit and then SJF scheduling is used. Remember that processes
P1 and P2 are waiting during this idle time, so their waiting time
may increase. This algorithm could be called future-knowledge
scheduling.

执行顺序为P3、P2、P1

(2 - 1 + 6 - 0.4 + 14 ) / 3 = 6.87

6.4 What advantage is there in having different time-quantum sizes at
different levels of a multilevel queueing system?

设定合适的时间片长度，可以减少上下文切换，从而减少额外开销。如I/O密集型程序可以设置较小的时间片长度，CPU密集型程序可以设置较长的时间片长度

6.5 Many CPU-scheduling algorithms are parameterized. For example, the
RR algorithm requires a parameter to indicate the time slice. Multilevel
feedback queues require parameters to define the number of queues, the
scheduling algorithm for each queue, the criteria used to move processes
between queues, and so on.
These algorithms are thus really sets of algorithms (for example, the
set of RR algorithms for all time slices, and so on). One set of algorithms
may include another (for example, the FCFS algorithm is the RR algorithm
with an infinite time quantum). What (if any) relation holds between the
following pairs of algorithm sets?
a. Priority and SJF

最短执行时间的进程拥有最高优先级

b. Multilevel feedback queues and FCFS

最低层次的多级反馈队列使用的是FCFS调度

c. Priority and FCFS

最先到达的进程拥有最高优先级

d. RR and SJF

没有关系

6.6 Suppose that a scheduling algorithm (at the level of short-term CPU
scheduling) favors those processes that have used the least processor
time in the recent past. Why will this algorithm favor I/O-bound
programs and yet not permanently starve CPU-bound programs?

I/O密集型程序具有大量短CPU执行，使用更少的处理器时间，所以这个调度算法支持I/O密集型程序。因为I/O密集型程序CPU执行时间较短，需要等待I/O操作，CPU密集型程序有机会执行，所以不会被饿死。

6.7 Distinguish between PCS and SCS scheduling.

PCS调度发生在同一进程的线程间，实现多对一和多对多模型的系统线程库会调度线程到可用LWP上。SCS调度会决定决定哪个内核级线程调度到处理器上，发生在系统的所有线程之间，采用一对一模型的系统只采用SCS调度。

6.9 The traditional UNIX scheduler enforces an inverse relationship between priority numbers and priorities: the higher the number, the lower the priority. The scheduler recalculates process priorities once per second
using the following function:
	Priority = (recent CPU usage / 2) + base
where base = 60 and recent CPU usage refers to a value indicating how
often a process has used the CPU since priorities were last recalculated.
Assume that recent CPU usage is 40 for process P1, 18 for process P2,
and 10 for process P3. What will be the new priorities for these three
processes when priorities are recalculated? Based on this information,
does the traditional UNIX scheduler raise or lower the relative priority
of a CPU-bound process? 

三个进程重新计算后的优先级分别为80、69、65，三个进程的优先级数都增加，即优先级均下降，所以传统的UNIX调度会降低CPU密集型进程的优先级

6.10 Why is it important for the scheduler to distinguish I/O-bound programs from CPU-bound programs?

I/O密集型程序具有大量短CPU执行，而CPU密集型程序具有少量长CPU执行，如果所有进程都是I/O密集型程序，那么就绪队列几乎为空，如果所有进程都是CPU密集型的，那么I/O等待队列几乎为空，设备没有得到使用。为了保持系统的平衡，因此需要区分I/O密集型程序和CPU密集型程序，并合理组合两种程序。



I/O密集型程序具有大量短CPU执行，而CPU密集型程序具有少量长CPU执行，如果所有进程都是I/O密集型程序，那么就绪队列几乎为空，如果所有进程都是CPU密集型的，那么I/O等待队列几乎为空，设备没有得到使用。为了保持系统的平衡，调度器应该给予I/O密集型程序更高优先级，可以更好的利用电脑资源。

6.11 Discuss how the following pairs of scheduling criteria conflict in certain settings.
a. CPU utilization and response time

需要最大化CPU使用率和最小化响应时间。为了最大化CPU使用率需要减少上下文切换以减少CPU的空转，而为了最小化响应时间，需要更频繁的进行上下文切换来输出结果给用户。

b. Average turnaround time and maximum waiting time

需要减少平均周转时间和最大等待时间。为了最小化平均周转时间如采用SJF调度算法，会使得最长执行时间的进程的拥有最大等待时间，并且该时间会很大,甚至造成饥饿。为了最小化最大等待时间，如采用FCFS确保每个进程都能在有限时间内完成，这样的话平均周转时间又会上升。

c. I/O device utilization and CPU utilization

需要最大化I/O设备利用率和最大化CPU利用率。多执行CPU密集型进程可以最大化CPU利用率，多执行I/O密集型进程可以最大化I/O设备利用率。但是如果都执行I/O密集型进程，就绪队列几乎为空，造成CPU调度程序没有什么事可做。如果都执行CPU密集型进程，I/O等待队列几乎为空，造成设备浪费，系统会不平衡。因此需要合理组合I/O密集型进程和CPU密集型进程，也就不能同时最大化I/O设备利用率和CPU利用率。

6.14 Consider the exponential average formula used to predict the length of the next CPU burst. What are the implications of assigning the following
values to the parameters used by the algorithm?
a. α = 0 and τ~0~= 100 milliseconds

T~n+1~ = T~0~ = 100 ms

最近历史没有影响，过去历史为全部影响因素

b. α = 0.99 and τ~0~ = 10 milliseconds

最近历史为主要影响因素，过去历史几乎没有影响

6.15 A variation of the round-robin scheduler is the regressive round-robin
scheduler. This scheduler assigns each process a time quantum and a
priority. The initial value of a time quantum is 50 milliseconds. However,
every time a process has been allocated the CPU and uses its entire time
quantum (does not block for I/O), 10 milliseconds is added to its time
quantum, and its priority level is boosted. (The time quantum for a
process can be increased to a maximum of 100 milliseconds.) When a
process blocks before using its entire time quantum, its time quantum is
reduced by 5 milliseconds, but its priority remains the same. What type
of process (CPU-bound or I/O-bound) does the regressive round-robin
scheduler favor? Explain.

支持CPU密集型进程。I/O密集型进程会因等待I/O阻塞而减少时间片时间，而CPU密集型只有较少的I/O操作，并且在一个时间片执行完后，当前进程会增加时间片长度和优先级。在所有进程都执行完一个时间片长度后，CPU密集型的进程会拥有更高的优先级以及更多的时间片长度，因此该调度支持CPU密集型进程。

6.16 Consider the following set of processes, with the length of the CPU burst given in milliseconds:

<img src="https://i.loli.net/2020/11/01/u6eKBLXo3rQbGfg.png" alt="image-20201101151326074.png" style="zoom:80%;" />

The processes are assumed to have arrived in the order P1, P2, P3, P4, P5,
all at time 0.
a. Draw four Gantt charts that illustrate the execution of these
processes using the following scheduling algorithms: FCFS, SJF,
nonpreemptive priority (a larger priority number implies a higher
priority), and RR (quantum = 2).

FCFS: 

| P1   | P2   | P3   | P4   | P5        |
| ---- | ---- | ---- | ---- | --------- |
| 0    | 2    | 3    | 11   | 15     20 |

SJF:

| P2   | P1   | P4   | P5   | P3       |
| ---- | ---- | ---- | ---- | -------- |
| 0    | 1    | 3    | 7    | 12    20 |

priority: 

| P3   | P5   | P1   | P4   | P2       |
| ---- | ---- | ---- | ---- | -------- |
| 0    | 8    | 13   | 15   | 19    20 |

RR: 

| P1   | P2   | P3   | P4   | P5   | P3   | P4   | P5   | P3   | P5   | P3    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ----- |
| 0    | 2    | 3    | 5    | 7    | 9    | 11   | 13   | 15   | 17   | 18 20 |

b. What is the turnaround time of each process for each of the
scheduling algorithms in part a?

FCFS:	P1: 2	P2: 3	P3: 11	P4: 15	P5: 20

SJF:	P1:3	P2:1	P3:20	P4:7	P5:12

prority:	P1:15	P2:20	P3:8	P4:19	P5:13

RR:	P1:2	P2:3	P3:20	P4:13	P5:18

c. What is the waiting time of each process for each of these scheduling
algorithms?

FCFS:	P1: 0	P2: 2	P3: 3	P4: 11	P5: 15

SJF:	P1:1	P2:0	P3:12	P4:3	P5:7

prority:	P1:13	P2:19	P3:0	P4:15	P5:8

RR:	P1:0	P2:2	P3:12	P4:9	P5:13

d. Which of the algorithms results in the minimum average waiting
time (over all processes)?

FCFS:	(0 + 2 + 3 + 11 + 15) / 5 = 6.2

SJF:	(1 + 0 + 12 + 3 + 7) / 5 = 4.6

proirity:	(13 + 19 + 0 + 15 + 8) / 5 = 11

RR:	(0 + 2 + 12 + 9 + 13) / 5 = 7.2

SJF平均等待时间最短

6.17 The following processes are being scheduled using a preemptive, roundrobin scheduling algorithm. Each process is assigned a numerical
priority, with a higher number indicating a higher relative priority.
In addition to the processes listed below, the system also has an idle
task (which consumes no CPU resources and is identified as Pidle ). This
task has priority 0 and is scheduled whenever the system has no other
available processes to run. The length of a time quantum is 10 units.
If a process is preempted by a higher-priority process, the preempted
process is placed at the end of the queue.

<img src="https://i.loli.net/2020/11/01/I3JahXlD2usxM6S.png" alt="image-20201101151404994.png" style="zoom:80%;" />

a. Show the scheduling order of the processes using a Gantt chart.

| P1   | P1   | Pidle | P2   | P3   | P2   | P3   | P4   | P4   | P2   | P3   | Pidle | P5   | P6   | P5        |
| ---- | ---- | ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ----- | ---- | ---- | --------- |
| 0    | 10   | 20    | 25   | 35   | 45   | 55   | 60   | 70   | 75   | 80   | 90    | 100  | 105  | 115   120 |

b. What is the turnaround time for each process?

P1: 20	P2: 80 - 25 = 55	P3: 90 - 30 = 60	P4: 75 - 60 = 15	P5: 120 - 100 = 20	P6: 115 - 105 = 10

c. What is the waiting time for each process?

P1: 0	P2: 55 - 25 = 30	P3: 60 - 25 = 35	P4: 15 - 15 = 0	P5: 20 - 10 = 10	P6: 10 - 10 = 0

d. What is the CPU utilization rate?

Pidle = 15

(120 - 15) / 120 = 87.5%

6.19 Which of the following scheduling algorithms could result in starvation?
a. First-come, first-served
b. Shortest job first
c. Round robin
d. Priority

b、d

6.20 Consider a variant of the RR scheduling algorithm in which the entries
in the ready queue are pointers to the PCBs.
a. What would be the effect of putting two pointers to the same
process in the ready queue?

这个进程在一轮中会执行两个时间片长度

b. What would be two major advantages and two disadvantages of
this scheme?

优点：可以提高某个进程的优先级，将多个指针指向它让它来执行多次。不用改变原有调度算法，还是基于RR算法的应用。

缺点：优先级较低的进程可能会长时间得不到执行，因为这种策略在极端的情况下效果等同于优先级队列。需要在进程到达前提前知道进程的优先级，会有额外的系统开销。

c. How would you modify the basic RR algorithm to achieve the same
effect without the duplicate pointers?

给更高优先级的进程划分更大的时间片，或者给更高优先级的进程划分多个时间片

6.21 Consider a system running ten I/O-bound tasks and one CPU-bound
task. Assume that the I/O-bound tasks issue an I/O operation once for
every millisecond of CPU computing and that each I/O operation takes
10 milliseconds to complete. Also assume that the context-switching
overhead is 0.1 millisecond and that all processes are long-running tasks.
Describe the CPU utilization for a round-robin scheduler when:
a. The time quantum is 1 millisecond

当时间片为1时，无论哪个进程都会执行1s然后上下文切换，CPU利用率为 1 / (1 + 0.1) ≈ 91%

b. The time quantum is 10 milliseconds

当时间片为10时，I/O密集型任务会在执行1毫秒CPU计算后进行一次上下文切换，一共10个I/O任务，共10*(1 + 0.1) = 11 ms, CPU密集型任务会在执行完10ms后进行一次上下文切换，共10 + 0.1 = 10.1ms，所以CPU利用率为 20 / (11 + 10.1) ≈ 94.8%

6.24 Explain the differences in how much the following scheduling algorithms discriminate in favor of short processes:
a. FCFS

在长进程到达后的短进程会有较长的等待时间，依据这个特性来区别短进程

b. RR

所有的进程都有相同的时间片，短进程因为执行时间较短会先执行完毕离开队列，依据这个特性来区别短进程

c. Multilevel feedback queues

短进程执行时间较短，在第一级队列大多就执行完毕离开队列，依据这个特性来区别短进程