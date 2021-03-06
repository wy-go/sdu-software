## 操作系统

1. 对CPU、内存、外设并发的操作系统有哪些措施？
2. 根据操作系统对资源和进程的管理，写出中断有哪些方面的作用。
> *中断*分为内中断（异常/陷阱）和外中断。*外中断*指来自CPU执行指令以外的事情发生，包括外设请求或人为干预，例如I/O结束中断、时钟中断。
> *内中断*指源自CPU执行指令内部的事件，包括程序出错或系统调用。发生中断时，运行用户态的CPU会立即进入核心态，提供服务。
3. 描述系统调用的工作机制及其参数传递方法。
> *系统调用*把应用程序的请求传给内核，每个系统调用和一个数相关联，通过索引表找到相应的内核函数，调用内核函数完成所需的处理，将处理结果返回给应用程序。  
> (1) 通过**寄存器**来传递参数。(2) 将参数存在**内存的块和表**中，将块的地址通过寄存器来传递。(3) 参数通过程序压入**堆栈**，操作系统弹出。
4. 画出进程NEW、READY、RUNNING、WAITING、TERMINATED的状态图，并说明状态之间变换的原因。
![image](https://user-images.githubusercontent.com/56920038/147334427-4e9e34f4-8632-47e2-b8e6-3b22f153fca9.png)
5. 程序从外存调入内存，在调入、执行、结束过程中发生了什么，又是怎么解决的。
![image](https://user-images.githubusercontent.com/56920038/147334491-5df77ff4-e9a7-45ab-8099-989f7bb27c80.png)
6. 用户级线程和内核级线程是什么？相对的各自有什么优点？
> *用户级线程*仅存在于用户空间中。对于这种线程的创建、撤销、切换、线程之间的同步与通信功能，都无需利用系统调用来实现，而是通过用户级线程库来实现。线程管理在**用户空间**进行，**效率较高**。
> 
> *内核级线程*有操作系统**直接支持和管理**，应用程序没有进行线程管理的代码，只有一个到内核级线程的编程接口。当一个线程被阻塞后，允许另一个线程继续执行，所以**并发能力强**。
7. 多队列调度算法和多级反馈队列调度算法的基本思想，比较这两个算法的好坏。
> *多队列调度*：多级队列调度算法将**就绪**队列分成多个独立队列，根据进程的属性，如内存大小、进程优先级、进程类型，一个进程被**永久地分配**到一个队列。每个队列都有自己的**调度算法**。
> 此外，队列之间通常采用**固定优先级抢占调度**，每个队列与更低队列相比都有绝对的优先级。或者在队列之间**划分时间片**。优点是**低调度开销**，缺点是**不够灵活**。
> 
> *多级反馈队列调度*：允许进程在**队列之间移动**。根据**不同CPU区间**的特点以区分进程。如果进程使用过多CPU时间会被转移到更低优先级队列。
> 在较低优先级队列中等待时间过长的进程会被转移到更高优先级队列。这种形式的老化可以阻止饥饿。最**通用**的CPU调度算法，可被配置以**适应**系统设计，但是**最复杂**。
8. 根据进程的到达和执行时间，画出相应算法的甘特图，并求出平均等待时间。
<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>进程</th>
      <th>到达时间</th>
      <th>运行时间</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>P1</th>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <td>P2</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <td>P3</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>P4</th>
      <td>3</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>
(1) 抢占式最短作业优先调度  
(2) 轮转法调度（时间片大小为2）  
(3) 高响应比

9. 以下是四个进程的到达时间和运行时间。
<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>进程</th>
      <th>到达时间</th>
      <th>运行时间</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>P1</th>
      <td>0</td>
      <td>12</td>
    </tr>
    <tr>
      <td>P2</th>
      <td>1</td>
      <td>8</td>
    </tr>
    <tr>
      <td>P3</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>P4</th>
      <td>3</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>
分别画出FIFO和SJF调度的甘特图，并计算平均等待时间。

10. 临界区设计的基本要求；信号量是怎么设计来满足这些要求的？
> (1) *互斥*：**忙则等待**。进程不同时在临界区内执行。  
> (2) *前进*：**有空让进**。当无进程在临界区执行时，若有进程进入应允许。  
> (3) *有限等待*：进程进入临界区的要求必须在有限时间内得到满足。  
> (4) *让权等待*：等待的时候可以选择释放CPU执行权（非必须）。  
> 临界区是进程中访问临界资源的代码段。不放非必要的代码（并发度降低）。
> 实现进程互斥：分析问题，确定临界区；设置互斥信号量，初值为1；临界区之前对信号量进行P操作；临界区之后对信号量执行V操作。
11. 写出下面程序的输出结果，并解释这样输出的原因。
```cpp
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int a = 0;
int main() {
	pid_t pid = fork();
	if (pid == 0) {
		a = 2;
		printf("child leaving\n");
	} else {
		wait(NULL);
		printf("a=%d\n", a);
	}
	return 0;
}
```
> child leaving  
> a=0
> 
> 程序fork()了一次，产生一个子进程。父进程和子进程并行运行，直到父进程执行wait(NULL)，即wait(0)。
> wait(0)表示父进程会被阻塞，直到子进程的状态发生变化，即从运行态到终止态，才会被唤醒。所以先输出子进程运行结果，后输出父进程运行结果。
> 由于子进程执行a=2时发生写时复制，父子进程有独立的数据段，父进程输出0不变。
12. 两个进程T1和T2并发执行，共享变量x，初值为1，T1使x+1，T2使x-1，过程如下。问两个进程结束后x有多少种可能取值？有哪些方法使结果唯一？选取一种方法修改下面的程序，保证两进程结束后结果唯一。
![image](https://user-images.githubusercontent.com/56920038/147338013-06858276-07c0-4106-a747-c2e82acf7890.png)
> 0,1
13. 简述阻塞、饥饿、死锁、死循环的区别。
> *阻塞*是进程正在等待某一事件而暂停运行，由运行态变成阻塞态，是进程自身的一种**主动**行为。处于阻塞态的进程可能发生死锁或饥饿，也可顺利向前推进。  
> *饥饿、死锁和死循环*都是进程**无法顺利向前推进**的现象（故意设计的死循环除外）。  
> *死锁*一定是“循环等待对方手里的资源”导致的，因此如果有死锁现象，那**至少有两个**进程同时发生死锁。另外，死锁的进程一定处于**阻塞态**。  
> 可能**只有一个**进程发生*饥饿*。发生饥饿的进程既可能是**阻塞态**（如长期得不到需要的I/O设备），也可能是**就绪态**（长期得不到处理机）。  
> 可能**只有一个**进程发生*死循环*。死循环的进程可以上处理机运行（可以是**运行态**），只不过无法像期待的那样顺利推进。  
> *死锁和饥饿*问题是由于**操作系统分配资源不合理**导致的，而*死循环*是由**代码逻辑的错误**导致的。*死锁和饥饿*是**管理者**（操作系统）的问题，*死循环*是**被管理者**的问题。
14. 产生死锁的必要条件。
> *互斥条件*：只有对必须互斥使用的资源的争抢才会导致死锁。将**临界资源改造为可共享**使用的资源（可行性不高）。
> 
> *不剥夺条件*：进程所获得得资源在未使用完之前，不能由其他进程强行夺走，只能主动释放。**让权；剥夺**（考虑优先级）。（实现复杂；剥夺可能导致部分工作实效；反复导致系统开销大；可饥饿）
> 
> *请求和保持条件*：进程已经保持了至少一个资源，但又提出了新的资源请求，而该资源又被其他进程占有，此时请求进程被阻塞，但又对自己已有的资源保持不放。**静态分配**（资源利用率低；可饥饿）。
> 
> *循环等待条件*：存在一种进程资源的循环等待链，链中的每一个进程已获得的资源同时被下一个进程所请求。**顺序资源分配法**（不方便增加新的设备；顺序不一致资源浪费；编程麻烦）。
15. CPU是进程运行必需的资源，为什么进程不会因等待CPU而发生死锁？
> 不满足**不剥夺条件**。对可剥夺的资源（CPU）的竞争是不会引起死锁的。
16. 简述银行家算法避免死锁的过程（包括变量定义、安全判别算法等）。
> 设系统中共有**n个进程**和**m种资源**类型。  
> (1) *安全性算法*：确定计算机系统是否处于安全状态  
> ①向量**finish\[n\]** 存储进程是否已经完成，初始状态为**false**；向量**work[m]**存储当前每种资源的剩余可用量，初始值为**资源总量**。  
> ②寻找是否存在**finish=false**且**所有**资源**need<=work**的进程，如果存在，让这个进程获得所需要的资源执行结束，然后释放资源，令它的**finish=true**，总的**work加上**该进程原来占有的资源。  
> ③**循环**执行②直到没有符和条件的进程。此时若**finish全部为true**，则系统处于安全状态，能获得一个安全序列。
> 
> (2) *资源请求算法*：判断是否可安全允许请求  
> 初始状态：allocation\[n\]\[m\]，need\[n\]\[m\]，available\[m\]  
> ①确定**request<=need**否则**出错**；**request<=available**，否则**等待**；  
> ②按照需求**假分配**，**修改**系统当前的**available、need**，然后根据**安全性算法**判断假分配以后系统状态是否**安全**。如果安全，该分配得到**允许**。
17. 简述死锁预防、死锁避免并比较区别。
> *死锁预防*是一组方法，需要确定**至少一个必要条件不成立**。
> 
> *死锁避免*要求操作系统事先得到有关进程**申请使用资源**的额外信息。当进程申请资源时，若发现满足该资源的请求可能导致死锁发生，则拒绝该申请。
> 
> 两种方法都不允许死锁发生。**动态**的**死锁避免**会因为追踪当前资源分配成本增加运行**成本**，但是相对于**静态**的**死锁预防**方法它允许更多的并发使用资源，所以**系统吞吐量**大于死锁预防。
18. 使用银行家算法判断是否有死锁？如果有死锁，那么哪些进程死锁？
> request<sub>1</sub>(a,b,c) < need<sub>1</sub>(a‘,b’,c‘)  
> request<sub>1</sub>(a,b,c) < available<sub>1</sub>(a‘,b’,c‘)  
> **假分配**，则available变为( , , )  
> **系统状态序列**见下表：  
> <img width="833" alt="Screen Shot 2021-12-24 at 8 24 53 PM" src="https://user-images.githubusercontent.com/56920038/147352457-db671a90-09cf-48ed-90cb-978344dbaa16.png">  
> 进行安全性分析，见下表:
> <img width="850" alt="Screen Shot 2021-12-24 at 8 25 12 PM" src="https://user-images.githubusercontent.com/56920038/147352472-8b0735dd-72a8-45f1-a2e3-f86d6f396c31.png">  
> 根据**安全性算法**求出一个**安全序列<P<sub>0</sub>,P<sub>2</sub>,P<sub>1</sub>,P<sub>3</sub>>**，系统仍处于**安全状态**。请求被**允许**。
19. 使用死锁检测算法判断是否有死锁?如果有死锁，那么哪些进程死锁?
<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th>allocation</th>
      <th>need</th>
      <th>available</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>P<sub>1</sub></th>
      <td>0, 1, 0</td>
      <td>0, 1, 0</td>
      <td>0, 0, 0</td>
    </tr>
    <tr>
      <td>P<sub>2</sub></th>
      <td>2, 0, 0</td>
      <td>1, 0, 0</td>
      <td></td>
    </tr>
    <tr>
      <td>P<sub>3</sub></th>
      <td>0, 0, 3</td>
      <td>0, 0, 0</td>
      <td></td>
    </tr>
    <tr>
      <td>P<sub>4</sub></th>
      <td>2, 1, 1</td>
      <td>2, 1, 0</td>
      <td></td>
    </tr>
     <tr>
      <td>P<sub>5</sub></th>
      <td>0, 0, 2</td>
      <td>0, 0, 2</td>
      <td></td>
    </tr>
  </tbody>
</table>
</div>

> <img width="850" alt="Screen Shot 2021-12-24 at 8 29 35 PM" src="https://user-images.githubusercontent.com/56920038/147353248-7acdb462-bef7-4c00-9a14-6b0f93d1d0d0.png">
> 
> 满足进程P<sub>3</sub>和P<sub>5</sub>的需求后，**work**为(0,0,5)，此时不能满足余下任一进程的需求，系统出现死锁，因此当前系统处在非安全状态。P<sub>1</sub>、P<sub>2</sub>和P<sub>4</sub>死锁。
20. 页大小为1024字节，计算逻辑地址2560和4220对应的物理地址。
<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>页号</th>
      <th>块号</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>20</td>
    </tr>
    <tr>
      <td>1</th>
      <td>30</td>
    </tr>
    <tr>
      <td>2</th>
      <td>60</td>
    </tr>
    <tr>
      <td>3</th>
      <td>10</td>
    </tr>
  </tbody>
</table>
</div>

> 2560=1024\*2+512  
> 4220=1024\*4+124  
> 60\*1024+512=61952  
> 页号4越界。逻辑地址4220非法。
21.   
（1）一个进程的页表见下面的表格，页的大小为1024字节。为执行指令MOV AX, [2560]和MOV BX, [4098]，计算逻辑地址2560和4098对应的物理地址。
<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>页号</th>
      <th>帧号</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>20</td>
    </tr>
    <tr>
      <td>1</th>
      <td>30</td>
    </tr>
    <tr>
      <td>2</th>
      <td>18</td>
    </tr>
    <tr>
      <td>3</th>
      <td>80</td>
    </tr>
  </tbody>
</table>
</div>

注：本题中所有数字均为10进制。[2560]表示访问逻辑地址为2560的存储器，AX及BX均为寄存器名，MOV为移动数据的汇编指令。  
（2）单级页表；TLB命中率为90%，访问TLB需5ns，访问主存为25ns，求有效内存的访问时间。
> （1）2560=1024\*2+512  
> 4098=1024\*4+3  
> 18\*1024+512=18944  
> 页号4越界。逻辑地址4098非法。
> 
> （2）0.9*(5+25)+0.1*(5+25+25)=32.5ns
22. 在某个分页管理系统中，某一个作业有4个页面，被分别装入到主存的第3、4、6、8块中，假定页面和块大小均为1024字节，当作业在CPU上运行时，执行到其地址空间第500号处遇到一条传送命令：MOV 2100,3100，画地址转换图计算出MOV指令中两个操作数的物理地址。
> 在页表中，逻辑页(0,1,2,3)对应物理块(3,4,6,8)，页面大小L为1024字节。 逻辑地址A1=2100，页号P1=(int)(2100/1024)=2，页内偏移量W1=2100-2×1024=52，对应的物理块号为6，则A1对应的物理地址E1=6×1024+52=6196。 逻辑地址A2=3100，页号P2=(int)(3100/1024)=3，页内偏移量W2=3100-3×1024=28，对应的物理块号为8，则A2对应的物理地址E2=8×1024+28=8220。
23. 请求分页系统中，页表项中包含哪些数据项？它们的作用？（14）
> 在请求分页系统中，其页表项中包含的数据项有**页号、内存块号、状态位P、访问字段A、修改位M和外存地址**；  
> 其中*状态位P*指示该页是否调入内存，供程序访问时参考；  
> *访问字段A*用于记录本页在一段时间内被访问的次数，或最近已有多长时间未被访问，提供给**置换算法**选择换出页面时参考；  
> *修改位M*表示该页在调入内存后是否被修改过；  
> *外存地址*用于指出该页在外存上的地址，通常是物理块号，供调入该页时使用。
24. 画出分页内存管理方案的过程图，描述流程、过程中硬件或软件所起的作用。
25. 分页式文件系统，页表的主要数据结构，要使用这个分页结构需要哪些硬件支持。（13）
26. 当前页表如下。页大小为1024字节，该程序分配2个帧，页号0先装入内存。采用先进先出和局部置换策略，现在访问逻辑地址为3000的字节，问在这个过程中发生了什么主要事件并写出置换后的页表。
<div>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th>页号</th>
      <th>帧号</th>
      <th>Valid/Invalid</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>130</td>
      <td>Valid</th>
    </tr>
    <tr>
      <td>1</th>
      <td>570</td>
      <th>Valid</th>
    </tr>
    <tr>
      <td>2</th>
      <td>-1</td>
      <td>Invalid</th>
    </tr>
    <tr>
      <td>3</th>
      <td>-1</td>
      <td>Invalid</th>
    </tr>
  </tbody>
</table>
</div>

> 3000=1024\*2+952，页号为2<4，状态位为Invalid，发生缺页中断，由于2个帧已满，采用FIFO算法和局部置换策略，且页号0先装入内存，置换出，状态位置为Invalid，将页号2对应的帧号修改为130，状态位置为Valid。最后，访问物理地址为130\*1024+952=134072的字节。  
> 置换后的页表为：  
> <img width="773" alt="Screen Shot 2021-12-25 at 8 55 20 PM" src="https://user-images.githubusercontent.com/56920038/147385308-863203d5-8765-4fb9-ad85-e986a47513f8.png">

27. 在一个请求分页系统中，用FIFO，LRU写出下面的置换过程 4,3,2,1,4,3,5,4,3,2,1,5，当分配给该作业的物理块数M分别为 3、4时分别计算缺页次数和缺页率，并说明LRU优于FIFO的原因。（14）
> M=3：  
> <img width="766" alt="Screen Shot 2021-12-25 at 8 56 28 PM" src="https://user-images.githubusercontent.com/56920038/147385358-e6252cda-cf23-4088-bf09-e9c87bbd1667.png">
> 
> FIFO：缺页次数=9，缺页率=75%  
> LRU：缺页次数=10，缺页率=83.3%
> 
> M=4：  
> <img width="766" alt="Screen Shot 2021-12-25 at 8 56 48 PM" src="https://user-images.githubusercontent.com/56920038/147385365-a8897dc7-8616-4db5-8d84-cc70e10021b2.png">
> 
> FIFO：缺页次数=10，缺页率=83.3%  
> LRU：缺页次数=8，缺页率=66.7%
> 
> *FIFO*优先淘汰最先进入内存的页面，但是可能置换出去的是**很久之前初始化并且一直在用**的变量。性能很差，存在**Belady异常**。上述结果可以看出，对FIFO而言，增加分配给作业的内存块数反而出现缺页次数增加的异常现象。给进程分配4个帧产生的页面错误率比分配 3个帧还多。
> 
> *LRU*优先淘汰最近最久没访问的页面。和最优置换（OPT）一样，都**没有Belady异常**（它们属于同一种算法，叫做**栈算法**）。效率较高，是一种经常被使用的页置换算法。




