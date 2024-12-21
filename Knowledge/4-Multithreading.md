## Thread & Process
A thread is the smallest unit of execution within a program.  It represents a sequence of instructions that can run concurrently with other threads within a process.

A process is an instance of a running program. Each process has its own independent memory space, and processes cannot directly share data.

A thread is an execution flow within a process, and a process can have multiple threads. Threads share resources within a process

### state of thread
A thread executing in the Java virtual machine is in this state. BLOCKED. A thread that is blocked waiting for a monitor lock is in this state. WAITING. A thread that is waiting indefinitely for another thread to perform a particular action is in this state
## wait() notify()
The notify() method is defined in the Object class, which is Java's top-level class. It's used to wake up only one thread that's waiting for an object, and that thread then begins execution. The thread class notify() method is used to wake up a single thread.
The wait() method is used for inter-thread communication.when wait() method is called by a thread, then it gives up the lock on that resource and goes to sleep until some other thread enters the same monitor and invokes the notify() or notifyAll() method



## Volatile
### volatile简介
volatile是小M 提供的轻量级的同步机制。volatile 关键字可以保证并发编程三大特性（原子性atomicity. 、可见性、有序性）中的**可见性和有序性**，但是不能保证原子性。

volatile关键字使得不同线程访问的变量是一致的（有改变立马看见，可见性）。

#### 可见性
volatile 变量的值在一个线程中被修改后，**会立即被其他线程看到**，即保证了变量的可见性。这意味着如果一个线程修改了 volatile 变量的值，其他线程会立即看到这个变化。

当对volatile变量进行写操作的时候，JVM会**向处理器发送一条lock前缀的指令，将这个缓存中的变量回写到系统主存中**。
所以，如果一个变量被volatile所修饰的话，在每次数据变化之后，值都会被强制写入主存。而其他处理器的缓存由于遵守缓存一致性协议，就会把变量的值从主存读取到自己的工作内存中。这就保证了volatile在并发编程中，其值在多个缓存中是可见的。

#### 有序性
禁止编译器和处理器对被修饰的变量进行重排序优化。**Preventing Reordering**
volatille除了可以保证数据的可见性之外，还有一个重要的作用，那就是它可以禁止指令重排优化。volatile是通过内存屏障来禁止指令重排的。

### 内存屏障
硬件层的内存屏障
Intel硬件提供了一系列的内存屏障，主要有：
1.lfence，是一种Load Barrier 读屏障
2.sfence，是一种Store Barrier 写屏障
3.mfence，是一种全能型的屏障，具备ifence和sfence的能力
4.Lock前缀，Lock不是一种内存屏障，但是它能完成类似内存屏障的功能。Lock会对CPU总线和高速缓存加锁，可以理解为CPU指令级的一种锁。它后面可以跟ADD,ADC, AND, BTC, BTR, BTS, CMPXCHG, CMPXCHSB, DEC,INC, NEG,NOT, OR, SBB, SUB, XOR, XADD, and XCHG等指令。

JVM的内存屏障
不同硬件实现内存屏障的方式不同，Java内存模型屏蔽了这种底层硬件平台的差异，由JVM来为不同的平台生成相应的机器码。

![[Pasted image 20230824000259.png]]

![[Pasted image 20230824000334.png]]
![[Pasted image 20230824000418.png]]
![[Pasted image 20230824000427.png]]


## completable future
chain operations 

## lock type

### ReentrantLock（可重入锁）与 ReadWriteLock（读写锁）的区别

1. **ReentrantLock（可重入锁）**:
   - ReentrantLock 是一个互斥锁**Mutex Lock** Mutual Exclusion，即它在同一时间只允许一个线程持有锁。不论是读操作还是写操作，只要一个线程获得了这个锁，其他所有需要这个锁的线程都必须等待。
   - 它**提供了比synchronized关键字更高的灵活性**，比如可以尝试获取锁（**tryLock()**），可以中断等待锁的线程，以及支持公平锁等。
   - 它并不区分读操作和写操作。任何操作都需要完全的独占访问。

2. **ReadWriteLock（读写锁）**:
   - ReadWriteLock 分为两个锁：读锁和写锁。它**允许多个线程同时持有读锁（只要没有线程持有写锁）**，这意味着它支持多线程同时读取操作，提高了并发读的效率。
   - 当**写锁被占用时，所有试图获取读锁或写锁的线程都将被阻塞**，确保写操作的独占性。
   - 它主要用于读多写少的场景，因为读操作可以并行进行，而写操作则需要等待和阻塞其他操作。

### StampedLock 

**StampedLock**:
  - StampedLock 是一种**改进的读写锁**，它比ReadWriteLock**有更好的性能**，特别是在高并发的场景下。
  - StampedLock **同样支持常规的读锁和写锁**，但它还引入了“**乐观读**”模式。这个模式在读取数据时不会立即加锁，而是先检查在读取期间是否有写操作，如果有，则退化为普通的读锁。
  - 它不是重入锁，也不支持条件变量。
  - StampedLock 的设计目的是为了提供比传统的ReadWriteLock更高的并发性能，但它的使用也更加复杂。

总结起来，ReentrantLock 更适合标准的互斥场景，ReadWriteLock 适用于读多写少的情况，而 StampedLock 在高并发的场景下提供了更高的性能，但使用起来更复杂。StampedLock 并不是包含了 ReadWriteLock 的所有功能，但它提供了一种更高效的方式来处理读写操作的并发。


### 不那么重要：
**Condition:** The Condition interface provides a way to manage the execution order of threads waiting for a specific condition to be met. It is typically used with a Lock implementation, such as ReentrantLock. Threads can wait for a condition to become true, signal that the condition has changed, or signal all waiting threads to wake up.

**Semaphore:** A Semaphore is a synchronization primitive that maintains a set of permits. Threads can acquire permits from the semaphore and release them when they are no longer needed. Semaphores are useful for controlling access to a shared resource with a limited capacity.


### 乐观读 和 读 的区别

读锁（Read Lock）和 乐观读锁（Optimistic Read Lock）是两种不同的锁机制，它们在处理并发读取操作时有明显的差异。下面是它们的主要区别：

#### 读锁 (Read Lock)

1. **锁的获取**：当一个线程想要进行读操作时，它需要获取读锁。在获取到读锁之前，该线程会被阻塞。

2. **数据一致性**：一旦获得读锁，线程可以确保在持有锁期间，**不会有其他线程进行写操作**，从而保障了数据的一致性。

3. **多线程共享**：读锁允许多个线程同时持有，只要没有线程持有写锁。这意味着多个线程可以同时进行读操作，但如果有线程需要写操作，它必须等所有读锁释放后才能获取写锁。

4. **适用情况**：适用于读多写少的情况，可以提高并发读的效率。

#### 乐观读锁 (Optimistic Read Lock)

1. **锁的获取**：乐观读锁**不会在读取数据时立即加锁**。相反，它允许线程读取数据而不阻塞其他线程。

2. **数据一致性检查**：乐观读锁依赖于一致性检查机制。**线程在读取数据后，需要检查在读取期间是否有写操作发生**。（**也就是读的同时可以写**）如果有写操作，那么读取的数据可能不一致，线程需要重新尝试读取。

3. **并发度高**：由于不会立即阻塞其他线程，乐观读锁可以提高系统的整体并发性能，特别是在读操作远远多于写操作的场景中。

4. **适用情况**：乐观读锁更适用于数据更新不频繁，且读操作远多于写操作的高并发环境。

#### 总结

- 读锁是一种保守的策略，它在读取期间保证数据的完全一致性，但牺牲了一定的并发性能。
- 乐观读锁是一种更为高效的策略，在大多数情况下允许更高的并发度，但**需要在使用后进行额外的一致性检查**。

