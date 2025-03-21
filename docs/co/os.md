# os
- a.out 
ELF:executable and linkable format

rodata
data
test
bss

memory lay out of c

objdump,readelf

## os structure 
direct memory access(DMA):it is a way for cpu to let memory-I/O operations(data transform) independently

cache:
- temporal locality:tend to reference addresses it has recently referenced
- spatial locality:tend to reference addresses near the recently referenced

time sharing: multi programing with rapid context-switching

user/kernal mode

- kernel is a running job
- 等待其他job通过event调用操作系统
 
system call:do something privileged

timer:

- process management
- memory management
- storage management
- protection and  security


## processes(进程)
- process is **a unit of resource allocation and protection**.And it is a program in execution.
- process=code(text),data section(global variables),program counter,stack,heap；具体如图：

![](../img/os/1.png)

（注意栈是由高到低的）

对于同一个程序，如果运行两次，产生的2个进程中，stack，heap的大小和内容是不一样的（因为两个程序可能正在调用不同函数等），data段的大小一样但内容不一样（可能不同变量的值被修改），text段的大小，内容一样

### PCB
- process control block(PCB):information associated with each process
- 每一个进程只有一个PCB，在new阶段分配PCB，在termination阶段free PCB.
- PCB储存了process number(pid),program counter,registers,process state,memory management等信息

### Process State(重点)

![](../img/os/2.png)

#### new阶段
核心是两个syscall:folk和execv.

##### fork()
fork会创造一个新的进程。每个子进程只有一个父进程，并且他会有一个新的pid。子进程的内容是父进程的复制，并且所有的resource初始化为0

fork有两个返回值，**fork会把子进程的pid返回给父进程，把0返回给子进程**。另外，通过`getpid()`可以得到自己的pid，通过`getppid()`得到父进程的pid。

```c
pid=fork();
if(pid==0){
    printf("这是子进程");
}else if(pid>0){
    printf("这是父进程");
}
```

注意，在fork()调用完成后，每个进程将会继续执行后续的语句，且每个进程内的变量互不干扰。

**例子一：**

```c
pid1=fork();
printf("hello");
pid2=fork();
printf("hello");
```

第一次fork，进程由1个变成2个，然后输出两个hello；第二次fork，进程由2个变4个，然后输出4个hello。总共输出6个hello。

**例子二：**

```c
fork();
if(fork()){
    fork();
}
fork();
```

第一次fork，进程由1个变2个，if判断条件的fork使进程由2变4，**if语句内的fork只会对父进程执行**，即2个父进程进行fork产生2个新进程。此时共有6个进程，最后执行的fork使进程由6到12个，最终一共产生12个进程。

**pros and cons:**fork的优点是简洁，不需调用复杂参数，保持进程与进程之间的联系；缺点是性能差且有安全性问题

##### execv()
execv会用新的进程映像来替代当前进程，此时**当前进程会被清空**。

```c
int main(int argc,char* argv[]){
    pid_t pid=fork();
    if(pid==0){
        printf("1");
        execv("./callee",argc);
        printf("2");
    }else if(pid>0){
        printf("3");
    }
}
```

在上面的例子中，父进程会正确执行并输出“3”。但是execv的执行会**清空子进程**并进入“callee”进程，此时子进程已经被清空，所以只会输出“1”，不会输出“2”.

#### Terminations 阶段
进程可以通过`exit()`这个system call来终止。