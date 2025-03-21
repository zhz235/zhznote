# 同步（synchronization）和死锁
不区分线程进程
## critical section
存在n个进程，每个进程有一些critical section segment of code(比如改变共同变量，更新表等)。那么：

- 只有一个进程可以在critical section
- 每个进程必须获得permission才可以在entry section中进入critical section
- permission必须在exit section被释放


```c
while(true){
    entry section
        critical section
    exit section
        remainder section
}
```

- preemptive：抢占式，目前操作系统一般用
- non-preemtive:非抢占式

### 解决critical section的三个要求
- mutual exclusion（互斥访问）：只有一个进程可以在critical section执行
- progress （空闲让进）：当没有线程在执行临界区代码时，必须在申请进入临界区的线程中选择一个线程，允许其执行临界区代码，保证程序执行的进展
- bounded waiting（有限等待）：当一个进程申请进入临界区后，必须在有限的时间内获得许可并进入临界区，不能无限等待

### Peterson’s solution
- 用来解决两个进程的同步
- 假设load,store是atomic的（不能被打断）
- 两个进程贡献两个变量
  - flag[2]:哪个进程准备进critical section
  - turn：谁进critical section

```c
do{ 
    flag[0] =TRUE; 
    turn =1; 
    while(flag[1] &&turn ==1); 
    critical section 
    flag[0] =FALSE; 
    remainder section 
} while(TRUE); 
```

互斥性：

- Case 1: flag[1]=false-> P1 is out CS
- Case 2: flag[1]=true, turn=1-> P0 is looping, contradicts with P0 is in CS
- Case 3: flag[1]=true, turn=0-> P1 is looping

不足：现代编译器有指令重排（instruction reorder），对于多线程的程序会出错

总之不能用软件层面解决同步

## solution
### memory barriers
- strongly ordered:内存改变其他进程立即可见
- weakly ordered：内存改变其他进程不立即可见

### lock with test-and-set
test_set(lock)是原子性操作，将锁置为true，同时返回原来的锁原来的值。

```c
do{
    lock=false;
    while(test_set(lock));
    critical section
    lock=false;
    remainder section
}while(true)
```

初始化锁为false，第一个调用test_set的进程将锁置为true，同时进入critical section。其他进程将会循环，直到第一个进程执行完critical section然后把锁变回法false。因此互斥是满足的。

但是有busy waiting的情况，如一直是某两个进程来回进入critical section，而第三个进程一直循环waiting。

