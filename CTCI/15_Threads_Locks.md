# Threads and Locks

### Definitions

+ Thread vs. process
  + Process: 
    + an instance of a program in execution
    + independent to which system resources are allocated
    + inter-process communications required for inter-communication (share resources)
  + Thread: 
    + within a process
    + a particular execution path of a process
    + share resources (heap space)
    + each has its own registers and stack, but not heap memory

+ Context switch:
  + Definition: time spent switching between two processes 
    + executing process -> wait, into memory, save state information
    + waiting process -> execution
  + Measurement
    + record the timestamps of the last and first instruction of the swapping processes
    + Process:
      1. $\mathrm{P}_{2}$ blocks awaiting data from $\mathrm{P}_{1}$.
      2. $\mathrm{P}_{1}$ marks the start time.
      3. $\mathrm{P}_{1}$ sends token to $\mathrm{P}_{2}$. $T_d$
      4. $\mathrm{P}_{1}$ attempts to read a response token from $\mathrm{P}_{2}$. This induces a context switch.  $T_c$
      5. $\mathrm{P}_{2}$ is scheduled and receives the token. $T_r$
      6. $\mathrm{P}_{2}$ sends a response token to $\mathrm{P}_{1}$. $T_d$
      7. $\mathrm{P}_{2}$ attempts read a response token from $\mathrm{P}_{1}$. This induces a context switch.  $T_c$
      8. $\mathrm{P}_{1}$ is scheduled and receives the token. $T_r$
      9. $\mathrm{P}_{1}$ marks the end time.
    + T: time between 3 and 8, measured by the time marked by P1.
      + $\mathrm{T}=2^{*}\left(\mathrm{~T}_{\mathrm{d}}+\mathrm{T}_{\mathrm{c}}+\mathrm{T}_{\mathrm{r}}\right)$
    + To measure $T_d$ and $T_r$, let $P_1$ sent to and receive from itself. Do many times and use average.
    + Get $T_c$ from $T$ ,$T_d$ and $T_r$.
    + 

### Threads In Java

+ Each thread controlled by a unique object of the `java.lang.Thread`	class.
+ `main thread`: When a standalone application runs, a user thread is automatically created to executes the `main()` method.
+ Two ways to implement threads: 
  + implement `java.lang.Runnable` interface
  + extend `java.lang.Thread` class

#### Implement Runnable Interface

+ Preferred method. Because:

  + Java does not support multiple inheritance. Thus extending Thread class means the subclass cannot extend any other class. But a class implementing the Runnable interface can.
  + For a class only interested in being runnable, inheriting the full overhead of the Thread class would be excessive.

+ Runnable interface:

  ```java
  public interface Runnable{
      void run();
  }
  ```

+ Implement: 

  + create a class of Runnable
  + create instance of Runnable class
  + create an object of type `Thread` with that instance
  + invoke `start()` method of `Thread` object

  ```java
  public class RunnableThreadExample implements Runnable{
      public int count = 0;
      public void run(){ //implement run() method
          System.out.println("RunnableThread starting.");
          try{
              while (count<5){
                  Thread.sleep(500);
                  count++;
              }
          } catch (interruptedException exc){
              System.out.println("RunnableThread interrupted.");
          }
          System.out.println("RunnableThread terminating.");
      }
  }
  
  public static void main(String[] args){
      RunnableThreadExample instance = new RunnableThreadExample();
      Thread thread = new Thread(instance); //use instace to constructe Thread object
      thread.start(); // start() method
      
      while(instance.count!=5){
          try{
              Thread.sleep(250);
          }catch(InterruptedException exc){
              exc.printStackTrace();
          }
      }
  }
  ```

#### Extend Thread Class

+ Override `run()` method of the `Thread` class

+ Create an instance of the Thread class

+ Call `start()` of `Thread` object

  ```java
  public class ThreadExample extends Thread{
      int count = 0;
      public void run(){
          System.out.println("Thread starting.");
          try{
              while (count<5){
                  Thread.sleep(500);
                  System.out.println("In Thread, count is " + count);
                  count++;
              }
              catch(InterruptedException exc){
                  System.out.println("Thread interrupted.");
              }
              System.out.println("Thread terminating.");
          }
      }
  }
  public class ExampleB{
      public static void main(String args[]){
          ThreadExample instance = new ThreadExample();
          instance.start();
          
          while(instance.count!=5){
              Thread.sleep(250);
          }catch(InterruptedException exc){
              exc.printStackTrace();
          }
      }
  }
  ```



### Synchronization and locks

+ Threads within a given process share the same memory space
  + pros: enables threads to share data
  + cons: problem when two threads modify a resource at the same time
+ Synchronization & lock: control access to shared resources
  + `synchronized`L restricts multiple threads from executing the code simultaneously on the same object
  + `lock`: a thread gets access to a shared resource by first acquiring the lock associated with the resource, only one thread can hold lock at a given time.
    + used when resource is accessed from multiple places

#### Synchronized Methods

```java
public class MyClass extends Thread{
    private Sring name;
    private MyObject myObj;
    
    public MyClass(MyObject obj, String n){
        name = n;
        myObj = obj;
    }
    public void run(){
        myObj.foo(name);
    }
}
public class MyObject{
    public synchronized void foo(String name){...}
}

// Different instances hold different references (memory space), thus can call synchronized function the same time
MyObject obj1 = new MyObject();
MyObject obj2 = new MyObject();
MyClass thread1 = new MyClass(obj1, "1");
MyClass thread2 = new MyClass(obj2, "2");
thread1.start();
thread2.start();

//Same instance, only one thread can call, another thread needs to wait
Myobject obj = new MyObjec();
MyClass thread1 = new MyClass(obj, "1");
MyClass thread2 = new MyClass(obj,"2");
thread1.start();
thread2.start();

//Static methods synchronize on the class lock. Only one thread can excute any static methods on the same class at the same time. When thread1 executes foo, thread2 has to wait to execute bar.
public class MyObjct{
    public static synchronized void foo(String name){...}
    public static synchronized void bar(String name){...}
}

//synchronized blocks
public class MyObjct{
    public void foo(String name){
        synchronized(this){
            ...
        }
    }
}
```



#### Locks

```java
public class LockedATM{
    private Lock lock;
    private int balance = 100;
    
    public LockedATM(){
        lock = new ReentrantLock();
    }
    public int withdraw(int value){
        lock.lock();
        try{...} catch (InterruptedException e){...}
        lock.unlock();
        return ...;
    }
}
```

### Deadlocks

+ Definition: thread1 waits for thread2 to release a lock, thread2 waits for thread1 to release a lock
+ Conditions
  + Mutual exclusion of resource access
  + Hold and wait
  + No preemption: cannot force to remove another process' resource
  + Circular wait
+ Prevention: avoid circular wait



p 186



## Interview Questions

p459

### 15.3 Dining Philosophers

In the famous dining philosophers problem, a bunch of philosophers are
sitting around a circular table with one chopstick between each of them. A philosopher needs both chopsticks to eat, and always picks up the left chopstick before the right one. A deadlock could potentially occur ifall the philosophers reached for the left chopstick at the same time. Using threads and locks, implement a simulation of the dining philosophers problem that prevents deadlocks.

+ Simulation

```java
class Chopstick{
    private Lock lock;
    public Chopstick(){
        lock = new ReentrantLock();
    }
    public void pickUP(){
        lock.lock();
    }
    public void putDOwn(){
        lock.unlock();
    }
}
class Philosopher extends Thread{
    private int bites = 10;
    private Chopstick left, right;
    public Philosopher(Chopstick left, chopstick right){
        this.left = left;
        this.right = right;
    }
    public void eat(){
        pickUp();
        chew();
        putDOwn();
    }
    public void pickUp(){
        left.pickUp();
        right.pickUp();
    }
    public void chew(){..}
    public void putDown(){
        left.putDown();
        right.putDown();
    }
    public void run(){
        for(int i=0;i<bites;i++){
            eat();
        }
    }
}
```

Deadlock: if all philosophers have a left chopstick and are waiting for the right one

+ Solution 1: all or nothing

```java
class Chopstick{
...
    public boolean pickUP(){
        return lock.tryLock();
    }
...
}
class Philosopher extends Thread{
...
    public void eat(){
        if(pickUp()){
            chew();
            putDOwn();
        };
    }
    public void pickUp(){
        if(!left.pickUp()){
            return false;
        }
        if(!right.pickUp()){
            left.putDown();
            return false;
        }
        return true;
    }
...
}
```

Issue: if they simultaneously pick up left and put down

+ Solution 2: Prioritized Chopsticks 
  + label all chopsticks with number from 0 to N-1. Each philosopher attempts to pick up the lower numbered chopstick first

```java
class Philosopher extends Thread{
...
    private int index;
    private int bites = 10;
    private Chopstick lower, higher;
    public Philosopher(int i, Chopstick left, chopstick right){
        index = i;
        if(left.getNumber()<right.getNumber()){
            this.lower = left;
            this.higher = right;
        }else{
            this.higher = left;
            this.lower = right;
        }

    }
...
}
```

