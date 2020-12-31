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



### 15.4 Deadlock-free Class

Design a class which provides a lock only if there are no possible deadlocks.

+ Detect deadlock using graph of locks: (left wait for right, detect if has circles)
  $$
  \begin{array}{l}
  A=\{1,2,3,4\} \\
  B=\{1,3,5\} \\
  C=\{7,5,9,2\}
  \end{array}
  $$

  ```mermaid
  graph LR;
  	1 --> 2;
  	2 --> 3;
  	3 --> 4;
  	1 --> 3;
  	3 --> 5;
  	7 --> 5;
  	5 --> 9;
  	9 --> 2;
  ```

+ DFS for cycle detection Pseudocode:

  ```java
  boolean checkForCycle(locks[] locks){
      touchedNodes = hash table(lock -> boolean);
      initialize touchedNodes to false for each lock in locks;
      for each (lock x in processes.locks){
          if(touchNodes[x]==false){
              if(hasCycle(x, touchedNodes)){
                  return true;
              }
          }
      }
      return false
  }
  
  boolean hasCycle(node x, touchedNodes){
      touchedNodes[r] = true;
      if(x.state==VISITING){
          return true;
      } else if(x.state==FRESH){
          ...
      }
  }
  ```

+ ```java
  public class LockFactory {
  	private static LockFactory instance;
  	
  	private int numberOfLocks = 5; /* default */
  	private LockNode[] locks;
  	
  	/* Maps from a process or owner to the order that the owner claimed it would call the locks in */
  	private HashMap<Integer, LinkedList<LockNode>> lockOrder;
  	
  	private LockFactory(int count) {
  		numberOfLocks = count;
  		locks = new LockNode[numberOfLocks];
  		lockOrder = new HashMap<Integer, LinkedList<LockNode>>();
  		for (int i = 0; i < numberOfLocks; i++) {
  			locks[i] = new LockNode(i, count);
  		}
  	}
  	
  	public static LockFactory getInstance() {
  		return instance;
  	}
  	
  	public static LockFactory initialize(int count) {
  		if (instance == null) {
  			instance = new LockFactory(count);
  		}
  		return instance;
  	}
  	
  	public boolean hasCycle(HashMap<Integer, Boolean> touchedNodes, int[] resourcesInOrder) {
  		/* check for a cycle */
  		for (int resource : resourcesInOrder) {
  			if (touchedNodes.get(resource) == false) {
  				LockNode n = locks[resource];
  				if (n.hasCycle(touchedNodes)) {
  					return true;
  				}
  			}
  		}
  		return false;
  	}
  
  	/* To prevent deadlocks, force the processes to declare upfront what order they will
  	 * need the locks in. Verify that this order does not create a deadlock (a cycle in a directed graph)
  	 */
  	public boolean declare(int ownerId, int[] resourcesInOrder) {
  		HashMap<Integer, Boolean> touchedNodes = new HashMap<Integer, Boolean>();
  		
  		/* add nodes to graph */
  		int index = 1;
  		touchedNodes.put(resourcesInOrder[0], false);
  		for (index = 1; index < resourcesInOrder.length; index++) {
  			LockNode prev = locks[resourcesInOrder[index - 1]];
  			LockNode curr = locks[resourcesInOrder[index]];
  			prev.joinTo(curr);
  			touchedNodes.put(resourcesInOrder[index], false);
  		}
  		
  		/* if we created a cycle, destroy this resource list and return false */
  		if (hasCycle(touchedNodes, resourcesInOrder)) {
  			for (int j = 1; j < resourcesInOrder.length; j++) {
  				LockNode p = locks[resourcesInOrder[j - 1]];
  				LockNode c = locks[resourcesInOrder[j]];
  				p.remove(c);
  			}
  			return false;
  		}
  		
  		/* No cycles detected. Save the order that was declared, so that we can verify that the
  		 * process is really calling the locks in the order it said it would. */
  		LinkedList<LockNode> list = new LinkedList<LockNode>();
  		for (int i = 0; i < resourcesInOrder.length; i++) {
  			LockNode resource = locks[resourcesInOrder[i]];
  			list.add(resource);
  		}
  		lockOrder.put(ownerId, list);
  		
  		return true;
  	}
  	
  	/* Get the lock, verifying first that the process is really calling the locks in the order
  	 * it said it would. */
  	public Lock getLock(int ownerId, int resourceID) {
  		LinkedList<LockNode> list = lockOrder.get(ownerId);
  		if (list == null) {
  			return null;
  		}
  		
  		LockNode head = list.getFirst();
  		if (head.getId() == resourceID) {
  			list.removeFirst();
  			return head.getLock();
  		}
  		return null;
  	}
  }
  ```

+ ```java
  public class LockNode {
  	public enum VisitState {
  		FRESH, VISITING, VISITED
  	};
  	
  	private ArrayList<LockNode> children;
  	private int lockId;
  	private Lock lock;
  	private int maxLocks;
  	
  	public LockNode(int id, int max) {
  		lockId = id;
  		children = new ArrayList<LockNode>();
  		maxLocks = max;
  	}
  	
  	/* Join "this" to "node", checking to make sure that it doesn't create a cycle */
  	public void joinTo(LockNode node) {
  		children.add(node);
  	}
  	
  	public void remove(LockNode node) {
  		children.remove(node);
  	}
  	
  	/* Check for a cycle by doing a depth-first-search. */	
  	public boolean hasCycle(HashMap<Integer, Boolean> touchedNodes) {
  		VisitState[] visited = new VisitState[maxLocks];
  		for (int i = 0; i < maxLocks; i++) {
  			visited[i] = VisitState.FRESH;
  		}
  		return hasCycle(visited, touchedNodes);
  	}
  	
  	private boolean hasCycle(VisitState[] visited, HashMap<Integer, Boolean> touchedNodes) {
  		if (touchedNodes.containsKey(lockId)) {
  			touchedNodes.put(lockId, true); 
  		}
  		
  		if (visited[lockId] == VisitState.VISITING) {
  			return true;
  		} else if (visited[lockId] == VisitState.FRESH) {
  			visited[lockId] = VisitState.VISITING;
  			for (LockNode n : children) {
  				if (n.hasCycle(visited, touchedNodes)) {
  					return true;
  				}
  			}
  			visited[lockId] = VisitState.VISITED;
  		}
  		return false;
  	}
  	
  	public Lock getLock() {
  		if (lock == null) {
  			lock = new ReentrantLock();
  		}
  		return lock;
  	}
  	
  	public int getId() {
  		return lockId;
  	}
  }
  ```

+ 

+ Tips: implementation of cycle detection

  ```java
  	private boolean hasCycle(VisitState[] visited, HashMap<Integer, Boolean> touchedNodes) {
  		if (touchedNodes.containsKey(lockId)) {
  			touchedNodes.put(lockId, true); 
  		}
  		
  		if (visited[lockId] == VisitState.VISITING) {
  			return true;
  		} else if (visited[lockId] == VisitState.FRESH) {
  			visited[lockId] = VisitState.VISITING;
  			for (LockNode n : children) {
  				if (n.hasCycle(visited, touchedNodes)) {
  					return true;
  				}
  			}
  			visited[lockId] = VisitState.VISITED;
  		}
  		return false;
  	}
  ```

  

### 15.5 Call in order

following code:

```java
public class foo{
    public Foo(){...}
    public void first(){...}
    public void second(){...}
    public void third(){...}
}
```

The same instance of Foo will be passed to three different threads. Thread A will call first, thread B will call second, and thread C will call third. Design a mechanism to ensure that first is called before second and second is called before third.



