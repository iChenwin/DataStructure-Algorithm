#### 数据结构
1. 线性表
  1. 顺序存储结构  
    顺序存储结构就是两个相邻的元素在内存中也是相邻的。  
    优点是查询的时间复杂度为O(1)  
    缺点是插入和删除的时间复杂度最坏能达到O(n)
  2. 链式存储结构  
    优点是插入和删除的时间复杂度为O(1)  
    缺点是访问的时间复杂度最坏为O(n)
2. 链表  
  链表就是链式存储的线性表。  
  根据指针域的不同，链表分为`单向链表`、`双向链表`、`循环链表`等等。
  1. 单向链表(slist)  
    每个元素包含两个域，`值域`和`指针域`，我们把这样的元素称之为`节点`。  
    每个节点的指针域内有一个指针，指向下一个节点，而最后一个节点则指向一个空值。  
    面试题：给定链表的头指针和一个节点指针，在O(1)时间删除该节点。[Google面试题]  
    **分析**：本题与《编程之美》上的「从无头单链表中删除节点」类似。主要思想都是「狸猫换太子」，即用下一个节点数据覆盖要删除的节点，然后删除下一个节点。但是如果节点是尾节点时，该方法就行不通了。  
    
    ```C
    struct Node{
        int data;
        Node* next;
    };
    
    //O(1)时间删除链表节点，从无头单链表中删除节点
    void deleteRandomNode(Node *cur)
    {
        assert(cur != NULL);
        assert(cur->next != NULL);    //不能是尾节点
        Node* pNext = cur->next;
        cur->data = pNext->data;
        cur->next = pNext->next;
        delete pNext;
    }
    ```  
    [面试精选：链表问题集锦 —— Jark](http://wuchong.me/blog/2014/03/25/interview-link-questions/)  

#### 算法分析
##### 一、比较排序算法  
插入排序、归并排序、堆排序、快速排序都是比较排序算法。  
![排序复杂度](http://7i7io5.com1.z0.glb.clouddn.com/sort.PNG)  

1. 插入排序  
有5张牌(arr[5]={5,2,4,3,1})，先取第二张牌(key=arr[j], j=1)，拿key和它之前的每一位比较，比它大的则往后挪(arr[i+1]=arr[i])，最后将key插入，然后取下一个key。
```C
#define LEN 5
void insertion(int arr[])
{
    int i = 0;
    int j = 1;
    int key = 0;
    while (j < LEN)
    {
        key = arr[j];
        i = j - 1;
        while (arr[i] > key && i >= 0)
        {
            arr[i + 1] = arr[i];
            i--;
        }
        arr[i + 1] = key;
        j++;
    }
}
```
#### 面试题
1. 堆和栈的区别：[堆和栈 —— 静影沉璧](http://ichenwin.github.io/2016/05/12/%E5%A0%86%E5%92%8C%E6%A0%88/#more)  
2. 进程间通信的几种主要手段:  
  1. 管道（Pipe）及有名管道（named pipe）：管道可用于具有亲缘关系进程间的通信，有名管道克服了管道没有名字的限制，因此，除具有管道所具有的功能外，它还允许无亲缘关系进程间的通信；  
  2. 信号（Signal）：信号是比较复杂的通信方式，用于通知接受进程有某种事件发生，除了用于进程间通信外，进程还可以发送信号给进程本身；linux除了支持Unix早期信号语义函数sigal外，还支持语义符合Posix.1标准的信号函数sigaction（实际上，该函数是基于BSD的，BSD为了实现可靠信号机制，又能够统一对外接口，用sigaction函数重新实现了signal函数）；  
  3. 报文（Message）队列（消息队列）：消息队列是消息的链接表，包括Posix消息队列system V消息队列。有足够权限的进程可以向队列中添加消息，被赋予读权限的进程则可以读走队列中的消息。消息队列克服了信号承载信息量少，管道只能承载无格式字节流以及缓冲区大小受限等缺点。  
  4. 共享内存：使得多个进程可以访问同一块内存空间，是最快的可用IPC形式。是针对其他通信机制运行效率较低而设计的。往往与其它通信机制，如信号量结合使用，来达到进程间的同步及互斥。  
  5. 信号量（semaphore）：主要作为进程间以及同一进程不同线程之间的同步手段。  
  6. 套接口（Socket）：更为一般的进程间通信机制，可用于不同机器之间的进程间通信。起初是由Unix系统的BSD分支开发出来的，但现在一般可以移植到其它类Unix系统上：Linux和System V的变种都支持套接字。  
  出处：[深刻理解Linux进程间通信（IPC）](https://www.ibm.com/developerworks/cn/linux/l-ipc/)
3. 线程  
进程是资源分配的单位，同一进程中的多个线程共享该进程的资源（如作为共享内存的全局变量）。Linux中所谓的“线程”只是在被创建时clone了父进程的资源，因此clone出来的进程表现为“线程”，这一点一定要弄清楚。因此，Linux“线程”这个概念只有在打冒号的情况下才是最准确的。    
同一个进程的多个线程在同一个地址空间，通信是很容易的事情，因此多线程间要同步就好了。  
同步方式[线程间同步--互斥锁、条件变量、信号量](http://blog.csdn.net/yusiguyuan/article/details/14160081)：  
  1. 互斥锁（mutex）  
  2. 条件变量（condition variable）  
  3. 信号量  
     信号量（Semaphore）和Mutex类似，表示可用资源的数量，和Mutex不同的是这个数量可以大于1。  
[进程与线程的一个简单解释 —— 阮一峰](http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html)
4. 大端字节序的判断。大端字节序，是指数据的低位保存在内存的高地址中，而数据的高位，保存在内存的低地址中。  
    
    ```C
    void IsBigEndian()
    {
        short int a = 0x1122;       //十六进制，一个数值占4位
        char b =  *(char *)&a;      //通过将short(2字节)强制类型转换成char单字节，b指向a的起始字节（低字节）
        if( b == 0x11)              //低字节存的是数据的高字节数据
        {
            return TRUE;            //是大端模式
        }
        else
        {
            return FALSE;           //是小端模式
        }
    }
    ```
5. 快速排序  

    ```C
    #include<stdio.h>

    void QuickSort(int arr[], int left, int right)
    {
        int i    = left;
        int j    = right;
        int temp = 0;
        int mid  = arr[(i + j) / 2];  //取数组中间的值作为“基准值”

        while (i <= j)
        {
            while (arr[i] < mid)
            {
                i++;
            }
            while (arr[j] > mid)
            {
                j--;
            }

            //小于基准值移至左边，大于基准值的移至右边
            if (i <= j)
            {
                temp   = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
                i++;
                j--;
            }
        }

        if (j > left)
        {
            QuickSort(arr, left, j);  //递归左半数组
        }
        if (i < right)
        {
            QuickSort(arr, i, right); //递归右半数组
        }
    }

    #define LEN 10

    //测试程序
    int main(void)
    {
        int arr[LEN] = {0};

        printf("Input %d integers:", LEN);
        for (int i = 0; i < LEN; i++)
        {
            scanf("%d", &arr[i]);
        }

        QuickSort(arr, 0, LEN - 1);

        for (int i = 0; i < LEN; i++)
        {
            printf("%d, ", arr[i]);
        }
        printf("\n");

        return 0;
    }
    ```