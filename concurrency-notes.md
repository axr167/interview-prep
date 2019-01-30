
# Concurrency vs Parallelism

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

# Java Memory Model

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

# Volatile vs Atomic

Let us consider the visibility and synchronization problems over t threads t1, t2.

### Visibility problem

- Set x = true in main and call the 2 threads.
- t1 runs: while(x){ // do something };
- t2 runs: x = false;

This loop might keep running as x = true is in shared cache and thread 1 only updates local cache instead of shared cache. Hence the updated value of x is not visible to t2.

To solve this, we make x volatile. Just by doing this all updates are visible to thread 2 and the condition is broken. Here any changes made to a volatile cache are:
- Flushed down to shared cache
- Refreshed back up to the local caches of other cores.

### synchronization problem

- set x = 1 in main and call 2 threads.
- Both t1 and t2 do x++ and return. We want the final answer to be 3.
- But this does not work even if we change type of x to volatile.
    - Because x++ is 2 operations which is: read 1, write 2.
- So t1 runs read 1 (not x), we switch to t2
- t2 runs read 1, switch to t1
- t1 runs store 2 in x, switch to t2
- t2 runs store 2 in t1

The problem here is that thread 2 has already read the value 1 so although changes to x are flushed to shared cache, thread 2 has already stored 1 and x is not needed anymore until it is time to write 2 to x.

To solve this problem we use synchronized. It ensures that only 1 thread can do the compound operation. So use:
        
    synchronized(obj){x++;}

Another way to do this is by using an AtomixInteger. We can do:

    AtomicInteger x = new AtomicInteger(1);
    // in each thread do x.increment();

# ExecutorService and Thread Pools

In java it is easy to run a method in a new thread. Simply create a class that implements runnable, overwrite its run method, create a new thread with the instance of the class and start the thread. This is shown below:
    
    static class Task implements Runnable {
        public void run() {// some code}
    }
    public static void main(string[] args) {
        Thread t1 = new Thread (new Task())
        t1.start();
    }

Similarly if you want to run 10 tasks, you can create the threads in a for loop

    for(int i=0; i<10; i++) {
        Thread t = new Thread(new Task());
        t.start();
    }

If you want to run 1000 tasks asynchronously, the problem is that 1 thread = 1 OS thread and creating a thread is expensive. Hence the ideal thing to do would be to create a fixed number of threads (say 10 threads) created upfront (this is a pool of threads) and submit the 1000 tasks to them.

This is done like so:

    ExecutorService es = Executors.newFixedThreadPool(nThreads:10);
    for(int i=0; i<10; i++) {
        es.execute(new Task());
    }

In es, a blocking queue is implemented. The queue stores all tasks and all th threads will perform the 2 steps:
- Fetch next task
- Execute it concurrently

**Ideal pool size:** For CPU intensive tasks the pool size is the core count (but we might not get access to all cores due to backgopund apps). For IO intensive tasks it is good to have higher number of threads because it lets you submit tasks faster reducing avg. wait time (but it consumes more memory).

### Types of thread pools:

Fixed thread pool:
- Fixed pool size
- n tasks stored in a blocking queue and threads retreive from it

Cached thread pool:
- Here we have a synchronous queue - same as blocking queue but it can hold only 1 task
- It then scans all available threads. If thread is free assign task or if no thread is free create new thread.
- Since many threads can be created it has the ability to kill threads if they are inactive for more than 60 seconds.

Scheduled thread pool:
- For tasks that must be performed after some delay.
- Tasks stored in delay queue where tasks are arranged based on when they must be executed.
- We declare pool size and it has the following methods:
    - schedule(task, delay): Sechdules task after x seconds
    - scheduleAtFixedRate(task, initialDelay, period): runs task every x seconds
    - scheduleWithFixedDelay(task, intialDelay, delay): runs task every x seconds on completion of previous iteration

Single thread executor:
- Same as fixed size exeutor however only 1 thread
- Recreates the thread if killed.

### Fork Join Pool

Same as executor service in that you submit task, execute task and optionally get the return value.

It differs from executor service in that it deals with tasks that produces its own sub-tasks aka fork joins. So here we do the following: break t1 into s1, s2, s3; solve s1..3 individually, then combine them to form result (think recursion). Subtasks are recursive in that it should not share common variables and are isolated.

Another way it differs is that it has per thread queueing and work stealing.

Per thread queueing:
- Think of fixed thread pool from executor service however every queue has its own dqueue (double ended queue)
- When the task is split into subtasks, they are not stored in the common blocking queue. Instead they are stored in the thread's own dqueue.

Work stealing:
- If t1 has more subtasks than t2 and t2 becomes empty
- t2 can take (steal) some tasks from t1.

### Thread Local

The concept of assigning variables/objects to threads itself so as to avoid synchronization for a global object is called thread local.

# Phaser

### Countdown latch

Say we want a thread to wait for 3 threads to initialize. For that we create a countdown latch of size 3. In the dependent services, after the service is done we say latch.countdown() that reduces the count of the latch. When the other 2 services count down, the main thread can continue.

### Cyclic barrier

Say we have a game with 3 players and we want to send a message to all of them at once. In this case we can use a cyclic barrier. All the 3 tasks will get the barrier and will say barrier.await(). Once all threads have come to the point that says barrier.await() only then will they all continue in execution.

### Phaser

This can act as both a countdown latch and a cyclic barrier.

# Asynchronous Programming

Computers these days have multiple cores so to take advantage of that, we create multiple threads. However if we have too many threads problems can occur.
- There is overhead due to context switching (when we have to store stuff from thread operations in shared cache)
- There is overhead due to scheduling different threads
    - This is heightened in IO operations because thread must wait for IO to complete wasting CPU cycles
    - So ideally you want non-blocking IO: Thread triggers operation and when you are done, call a callback method and this is done in a separatre thread. This lets the main thread do other stuff meanwhile.

Hence to increase scalability you need asynchronous API and non blocking IO.
