
## Concurrency vs Parallelism

### Parallelism

Consider the following code where we have 3 different tasks.
    
    
    // Task 1
    new Thread(new Runnable() {
        @override
        public void run() {
            method1(param1);
        }
    }).start();
    
    // Task 2
    new Thread(new Runnable() {
        @override
        public void run() {
            method1(param1);
        }
    }).start();
    
    method3(param 3);
    
The same code can be implemented using a thread pool as follows:

    ExecutorService es = Executors.newFixedThreadPool( nThreads: 4);
    es.submit(()->method1(param1));
    es.submit(()->method2(param2));
    method3(param 3);
    
Here task 1,2 are run on separate threads and Task 3 is run on the main thread. If this program is run on a quad core CPU, we can run the 3 tasks in parallel.

In the OS, the component called scheduler schedules the threads on the CPUs. Since no tasks are dependent on each other all 3 tasks will run separately on 3 separate cores at once in parallel. 
  - Parallelism is about doing a lot of things at once so we can speed up the program.

To enable parallelism we can use either threads or thread pools. However the CPU must have >1 cores so we can run multiple threads parellely.

### Concurrency

Consider the following code (which is incorrect) where we have 3 different tasks.
    
    
    // Task 1
    new Thread(new Runnable() {
        @override
        public void run() {
            //lock.lock();
            if(tickets > 0) {book(); tickets--;}
            //lock.unlock();
        }
    }).start();
    
    // Task 2
    new Thread(new Runnable() {
        @override
        public void run() {
            //lock.lock();
            if(tickets > 0) {book(); tickets--;}
            //lock.unlock();
        }
    }).start();
    
    Thread.sleep(2000);

Here tasks 1,2 are both accessing shared variable tickets. 

- Assuming the CPU has just 1 core, it allots some time to thread 1, then goes to thread 2 then back to thread 1 etc. This is called **thread interleaving**
- The program is wrong because of the following scenario:
  - We have 1 ticket. T1 starts then scheduler switches to T2.
  - T2 sees 1 ticket, starts then switch to T1
  - T1 books ticket then switches to T2
  - T2 books ticket then switches to T1
  - T1 decrements count to 0
  - T2 decrements count to -1
- Hence this is wrong. The same can also happen with multiple cores. If T1 and T2 are running in different cores, we cannot be sure when T1 runs in relation to T2.

To fix this problem with shared resources a solution is using a lock. If thread 1 has a lock, thread 2 must wait until the resource is unlocked to proceed.

Here the 2 threads coordinate using a single lock and that is concurrency.
  - Concurrency is applied when we have either a shared resource or if we need to coordinate multiple tasks.
In java we have several tools to deal with concurrency.
  - Locks/synchronized
  - Atomic classes
  - Concurrent data structures
  - CountdownLatch/Phaser/Semaphore/CyclicBarrier etc

## Java Memory Model

To understand the memory model we must first understand a couple of concepts.

### Out of order execution

Sometimes, the compiler might execute statements out of order to drive performance.

For example if we have:

    a = 3; 
    b = 1; 
    a = a+1;
The instruction set is:

    load a; set a = 3; load b; set b = 1; load a; set a = 4;

We load a twice which is bad so the compiler changes the code internally to
    
    a = 3;
    a = a+1;
    b = 1;
The revised instruction set is:
    
    load a; set a = 3; set a = 4; load b; set b = 1;
    
This is out of order execution.

### Field visibility

This is applicable only in terms of concurrent programming. Let us look at the memory model of a quad core computer.

![memory-model](https://i.imgur.com/0UDzjro.png)

Here we have 
- Registers and L1 cache unique to each core
- L2 cache shared between 2 cores
- L3 cache and RAM shared between all cores.

Consider t threads t1, t2 running on cores 1, 2 having a shared resource x.
- Initially x is in shared resource pool
- t1 increments x by 1
- t2 reads x. Here there can be a problem as t1 may have never sent x back to shared cache

This is the field visibility problem. To solve it declare x as **volatile**. This forces all changes made to the variable to reflect in the memory.

### Java Memory Model (JMM)

JMM is a specification that guarantees visibility of fields amidst reordering of instructions. Suppose we have some non volatile variables a, b, c which get updated before x gets updated 
  - in this case JMM ensures that even a, b, c show updated values to t2. This is called **happens before relationship**
The concept of happens-before and field visibility also applies to locks, synchronized, concurrent collections etc.
