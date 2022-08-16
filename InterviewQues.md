==== Links =====  
**https://github.com/learning-zone/spring-interview-questions/blob/spring/microservices.md**  
**https://github.com/in28minutes/spring-interview-guide**

**https://www.techiedelight.com/data-structures-and-algorithms-problems/**

**https://winterbe.com/posts/2014/07/31/java8-stream-tutorial-examples/**

**https://github.com/BruceEckel/OnJava8-Examples**  
**https://habr.com/ru/company/luxoft/blog/270383/**    
**https://makeinjava.com/data-structure-interview-questions-problem-solving/**  



1. How to protect singleton from reflection api?  
    throw run-time exception in the constructor if the instance already exists  
    ```
    public class Singleton implements Serializable {
        private static final long serialVersionUID = 3119105548371608200L;
        private static final Singleton singleton = new Singleton();
        
        //reflection
        private Singleton() {
            if (singleton != null) {
                throw new InstantiationError("Creating of this object is not allowed.");
            }
        }

        public static Singleton getInstance() {
            return singleton;
        }
        
        //cloning
        @Override
        protected Object clone() throws CloneNotSupportedException {
            throw new CloneNotSupportedException("Cloning of this class is not allowed");
        }

        //deserialization
        protected Object readResolve() {
            return singleton;
        }
    }
    ```
2. How to protect singleton from deserialization?  
    - use enum for singleton  
    - override readResolve() and return same instance or throw exception
    - declare method as static as static members are not part of serialization.  
* How to protect singleton from cloning?  
    overide clone() method and throw exception or return same instance
3. How to create Doubleton?  
    Doubleton class has two instances of it.  
    ```
    public class Doubleton {
        private static volatile Doubleton INSTANCE1;
        private static volatile Doubleton INSTANCE2;
        private static int call = 0;

        private Doubleton() {
        }

        public static Doubleton getInstance() {
            if (call++ % 2 == 0) {
                if (INSTANCE1 == null) {
                    synchronized (Doubleton.class) {
                        if (INSTANCE1 == null) {
                            INSTANCE1 = new Doubleton();
                        }
                    }
                }
                return INSTANCE1;
            } else {
                if (INSTANCE2 == null) {
                    synchronized (Doubleton.class) {
                        if (INSTANCE2 == null) {
                            INSTANCE2 = new Doubleton();
                        }
                    }
                }
                return INSTANCE2;
            }
        }
    }
    ```

4. collection vs stream
    | Collection | Stream  |
    | ---------- | ------- |
    | Collection is used to store and group the data in particular data structure like List,Map,Set etc | Stream is used to perform complex operations in collection |
    | Source data can be modified | source data can not be modified |
    | collection need to iterate externally | stream iterated internally | 
    | collection can be traversed multiple times | streams once consumed can not traversed again |
    | collection is eagerly constructed | lazily constructed |  

    
5. How to reverse string  
    * using reverse on StringBuilder 
    * iterative method(for loop, iterate from length-1)
    * recursion  
    * String has toCharArray() method iterate from last
6. Remove duplicate element from arraylist?  
    * Pass list to set constructor but insertion order is not maintained
    * using LinkedHashSet, maintains insertion order also   
7. notify() vs notifyAll()  
    notify - wakes up single thread whereas notifyAll wakes up all waiting thread  
* thread can acquire lock by entering into  
    - synchronized method  
    - synchronized block  
* wait(), notify()  and notifyAll() methods are always called from Synchronized block or synchronized methods only and as soon as thread enters synchronized block it acquires object lock (by holding object monitor).   

* Though we call notify/notifyAll, object do not relase lock immediately, lock will only get released once synchronized block/method completed.  

* Multiple threads may exist on same object but only one thread of that object can enter synchronized method at a time. Threads on different object can enter same method at same time.  

* Object lock vs class lock  

    | Object lock | class lock |
    | ---------- | ------------ |
    | Thread can acquire object lock by 1) entering synchronized block 2) entering synchronized method | Thread can acquire lock on class's class object by 1) entering synchronized block 2) entering static synchronized method |
    | Multiple threads may exist on same object but only one thread of that object can enter synchronized method at a time. Threads on different object can enter same method at same time. | Multiple threads may exist on same or different objects of class but only one thread can enter static synchronized method at a time. |
    | Multiple objects of class may exist and every object has it’s own lock. | Multiple objects of class may exist but there is always one class’s class object lock available. |
    | ex - synchronized(myClass) | ex - synchronized(MyClass.class) |  

*Object level locking:*  
```
public class DemoClass { 
  public synchronized void demoMethod(){} 
} 

or 

public class DemoClass { 
  public void demoMethod(){ 
  synchronized (this) 
  { 
   //other thread safe code 
  } 
 } 
} 

or 

public class DemoClass { 
  private final Object lock = new Object(); 
  public void demoMethod(){ 
  synchronized (lock) { 
  //other thread safe code 
 } 
} 
```

*Class level locking:*
```
public class DemoClass 
{ 
  public synchronized static void demoMethod(){} 
} 

or 

public class DemoClass 
{ 
  public void demoMethod(){ 
  synchronized (DemoClass.class) 
  { 
   //other thread safe code 
  } 
 } 
} 

or 

public class DemoClass 
{ 
 private final static Object lock = new Object(); 
 public void demoMethod(){ 
 synchronized (lock) 
  { 
   //other thread safe code 
  } 
 } 
} 
```

* Suppose you have 2 threads (Thread-1 on object1 and Thread-2 on object2). Thread-1 is in static
  synchronized method1(), can Thread-2 enter static synchronized method2() of same class at same time in java?  
  No, it might confuse you a bit that threads are created on different objects. But, not to forgot that multiple objects may exist but there is always one class’s class object lock available.  
  Here, when Thread-1 is in static synchronized method1() it must be holding lock on class class’s object and will release lock on class’s class object only when it exits static synchronized method1(). So, Thread-2 will have to wait for Thread-1 to release lock on class’s class object so that it could enter static synchronized method2().  
  Likewise, Thread-2 even cannot enter static synchronized method1() which is being executed by Thread-1. Thread-2 will have to wait for Thread-1 to release lock on  class’s class object so that it could enter static synchronized method1().  
    Program - 
    ```
    class MyRunnable1 implements Runnable {

        @Override
        public void run() {
            if (Thread.currentThread().getName().equals("Thread-1"))
                method1();
            else
                method2();
        }

        static synchronized void method1() {
            System.out.println(Thread.currentThread().getName() + " in synchronized void method1() started");
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + " in synchronized void method1() ended");
        }

        static synchronized void method2() {
            System.out.println(Thread.currentThread().getName() + " in synchronized void method2() started");
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + " in synchronized void method2() ended");
        }
    }

    public class MyClass {
        public static void main(String args[]) throws InterruptedException {

            MyRunnable1 object1 = new MyRunnable1();
            MyRunnable1 object2 = new MyRunnable1();

            Thread thread1 = new Thread(object1, "Thread-1");
            Thread thread2 = new Thread(object2, "Thread-2");
            thread1.start();
            Thread.sleep(10);// Just to ensure Thread-1 starts before Thread-2
            thread2.start();

        }
    }

    ```

* Suppose you have 2 threads (Thread-1 and Thread-2) on same object. Thread-1 is in synchronized method1(), can Thread-2 enter synchronized method2() at same time in java?  
    No, here when Thread-1 is in synchronized method1() it must be holding lock on object’s monitor and will release lock on object’s monitor only when it exits synchronized method1(). So, Thread-2 will have to wait for Thread-1 to release lock on object’s monitor so that it could enter synchronized method2().  
    Likewise, Thread-2 even cannot enter synchronized method1() which is being executed by Thread-1. Thread-2 will have to wait for Thread-1 to release lock on object’s monitor so that it could enter synchronized method1().  
  

* Suppose you have thread and it is in synchronized method and now can thread enter other synchronized
    method from that method in java? -> Yes  

* Suppose you have thread and it is in static synchronized method and now can thread enter other static 
    synchronized method from that method in java? -> Yes  

* Suppose you have thread and it is in static synchronized method and now can thread enter other non static synchronized method from that method in java? -> Yes  

* Suppose you have thread and it is in synchronized method and now can thread enter other static synchronized method from that method in java? -> Yes  

* Does Thread implements their own Stack, if yes how? -> Yes
* When we implement Runnable interface, same object is shared amongst multiple threads, but when we extend Thread class each and every thread gets associated with new object.   

* How can you ensure all threads that started from main must end in order in which they started and also main should end in last?  
by calling join method on threads, in short it waits for thread to die on which thread has been called.  

* wait vs sleep?
    | wait      | sleep     |
    | -------   | -------   |
    | releases object lock | holds object lock |
    | belongs to Object class | belongs to Thread class | 
    | called on object | called on thread  |
    | always called from synchronized block | need not to be called from synchronized block |

* solve Producer-consumer problem using wait and notify?
    ```
        import java.util.LinkedList;
        import java.util.List;

        class Producer implements Runnable {

            private List<Integer> sharedQueue;
            private int maxSize = 2; // maximum number of products which sharedQueue can hold at a time.

            public Producer(List<Integer> sharedQueue) {
                this.sharedQueue = sharedQueue;
            }

            @Override
            public void run() {
                for (int i = 1; i <= 10; i++) { // produce 10 products.
                    try {
                        produce(i);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }

            private void produce(int i) throws InterruptedException {

                synchronized (sharedQueue) {
                    // if sharedQuey is full wait until consumer consumes.
                    while (sharedQueue.size() == maxSize) {
                        System.out.println("Queue is full, producerThread is waiting for "
                                + "consumerThread to consume, sharedQueue's size= " + maxSize);
                        sharedQueue.wait();
                    }
                }

                synchronized (sharedQueue) {
                    System.out.println("Produced : " + i);
                    sharedQueue.add(i);
                    Thread.sleep((long) (Math.random() * 1000));
                    sharedQueue.notify();
                }
            }
        }

        class Consumer implements Runnable {
            private List<Integer> sharedQueue;

            public Consumer(List<Integer> sharedQueue) {
                this.sharedQueue = sharedQueue;
            }

            @Override
            public void run() {
                while (true) {
                    try {
                        consume();
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }

            private void consume() throws InterruptedException {

                synchronized (sharedQueue) {
                    // if sharedQuey is empty wait until producer produces.
                    while (sharedQueue.size() == 0) {
                        System.out.println("Queue is empty, consumerThread is waiting for "
                                + "producerThread to produce, sharedQueue's size= 0");
                        sharedQueue.wait();
                    }
                }
                synchronized (sharedQueue) {
                    Thread.sleep((long) (Math.random() * 2000));
                    System.out.println("CONSUMED : " + sharedQueue.remove(0));
                    sharedQueue.notify();
                }
            }

        }

        public class ProducerConsumerWaitNotify {

            public static void main(String args[]) {
                List<Integer> sharedQueue = new LinkedList<Integer>(); // Creating shared object

                Producer producer = new Producer(sharedQueue);
                Consumer consumer = new Consumer(sharedQueue);

                Thread producerThread = new Thread(producer, "ProducerThread");
                Thread consumerThread = new Thread(consumer, "ConsumerThread");
                producerThread.start();
                consumerThread.start();
            }
        }
    ```

* solve Producer-consumer problem using blocking queue?
    ```
    import java.util.concurrent.BlockingQueue;
    import java.util.concurrent.LinkedBlockingQueue;

    class Producer implements Runnable {
        private final BlockingQueue<Integer> sharedQueue;
    
        public Producer(BlockingQueue<Integer> sharedQueue) {
            this.sharedQueue = sharedQueue;
        }
    
        @Override
        public void run() {
            for(int i=1; i<=10; i++){
            try {
                System.out.println("Produced : " + i);
                //put/produce into sharedQueue.
                sharedQueue.put(i);          
            } catch (InterruptedException ex) {
                
            }
            }
        }
    }

    class Consumer implements Runnable{
    
        private BlockingQueue<Integer> sharedQueue;
    
        public Consumer (BlockingQueue<Integer> sharedQueue) {
            this.sharedQueue = sharedQueue;
        }
    
        @Override
        public void run() {
            while(true){
            try {
            //take/consume from sharedQueue.
                System.out.println("CONSUMED : "+ sharedQueue.take());  
            } catch (InterruptedException ex) {
                
            }
            }
        }
    }
    public class ProducerConsumerBlockingQueue {
    
        public static void main(String args[]){
        
        //Creating shared object  
        BlockingQueue<Integer> sharedQueue = new LinkedBlockingQueue<Integer>();
        
        Producer producer=new Producer(sharedQueue);
        Consumer consumer=new Consumer(sharedQueue);
        
        Thread producerThread = new Thread(producer, "ProducerThread");
        Thread consumerThread = new Thread(consumer, "ConsumerThread");
        producerThread.start();
        consumerThread.start();
    
        }
    }

    ```

* How to create deadock?
    ```
    class A implements Runnable{
        public void run() {
            synchronized (String.class) {
                    
                    /*
                    * Adding this optional delay so that Thread-2 could enough time
                    * to lock Object class and form deadlock.
                    * If you remove this sleep, because of threads unpredictable
                    * behavior it might that Thread-1
                    * gets completed even before Thread-2 is started and we will
                    * never form deadLock.
                    */
                    try {
                            Thread.sleep(100);
                    } catch (InterruptedException e) {e.printStackTrace();}
                    
                    System.out.println(Thread.currentThread().getName() + "has acquired lock "
                                + "on String class and waiting to acquire lock on Object class...");
                    synchronized (Object.class) {
                            System.out.println(Thread.currentThread().getName() +
                                        " has acquired lock on Object class");
                    }
            }          
            System.out.println(Thread.currentThread().getName()+" has ENDED");
        }
    }
 
    class B extends Thread{
        public void run() {
            
            synchronized (Object.class) {  
                    System.out.println(Thread.currentThread().getName() + " has acquired "
                        + "lock on Object class and waiting to acquire lock on String class...");
                    
                    /*
                    * Adding this optional delay so that Thread-1 could enough
                    * time to lock String class and form deadlock.
                    * If you remove this sleep, because of threads unpredictable
                    * behavior it might that Thread-2
                    * gets completed even before Thread-1 is started and we
                    * will never form deadLock.
                    */
                    try {
                            Thread.sleep(100);
                    } catch (InterruptedException e) {e.printStackTrace();}
                    
                    
                    synchronized (String.class) {
                            System.out.println(Thread.currentThread().getName() +
                                        " has acquired lock on String class");
                    }
            }
            System.out.println(Thread.currentThread().getName()+ " has ENDED");
        }
    }
    public class DeadlockCreation {
        public static void main(String[] args) {
            Thread thread1 = new Thread(new A(), "Thread-1");
            Thread thread2 = new Thread(new B(), "Thread-2");
            thread1.start();
            thread2.start();
        }
    }

    ```

* How to solve/avoid deadlock?  
    - try to avoid nested synchronized blocks/methods
    - lock specific member variables of class rather than locking whole class  
        ```synchronized(customObj.str)```  
    - use join() method(disadvantage is threads start and end in sequentially)  

* Race condition?  
    More than one thread trying to access shared resource without synchronization causes race condition  

* Executors - creates pool of threads and manages life cycle of all threads in it.  
    examples - newFixedThreadPool(), newWorkStealingPool(), newCachedThreadPool(), newScheduledThreadPool().
* wait until all threads finish their work in java using Executor framework?  
    1. Using join() on threads
    2. CountDownLatch, consider CyclicBarrier if want to reset the counter.
    3. iterate through all `Future` objects after submitting to ExecutorService

* How can you catch an exception thrown by another thread in Java?  
This can be done using Thread.UncaughtExceptionHandler(), it is FinctionalInterface in thread class.
```
    // create our uncaught exception handler
    Thread.UncaughtExceptionHandler handler = new Thread.UncaughtExceptionHandler() {
        public void uncaughtException(Thread th, Throwable ex) {
            System.out.println("Uncaught exception: " + ex);
        }
    };

    // create another thread
    Thread otherThread = new Thread() {
        public void run() {
            System.out.println("Sleeping ...");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                System.out.println("Interrupted.");
            }
            System.out.println("Throwing exception ...");
            throw new RuntimeException();
        }
    };

    // set our uncaught exception handler as the one to be used when the new thread
    // throws an uncaught exception
    otherThread.setUncaughtExceptionHandler(handler);

    // start the other thread - our uncaught exception handler will be invoked when
    // the other thread throws an uncaught exception
    otherThread.start();
```


8. how to swap two strings without using third variable/temp variable  
    https://javaconceptoftheday.com/swap-two-string-variables-without-using-third-or-temp-variable-in-java/
9. Generics Wildcard arguements?  
    a) with unknown type(?)--any type (Object) is accepted as typed-parameter.    
    b) with upper bound(? extends Number)  
    c) with lower bound(? super Integer)  
10. can we use abstract and static together? -> No
11. Auto boxing and unboxing?  
Autoboxing refers to the automatic conversion of a primitive type variable to its corresponding wrapper class object, and reverse is unboxing.  
12. spring cloud bus - Whenever we do changes in config-server, we have to restart all applications to load changes in configurations.  
spring cloud bus is used to refresh configuration using POST in config service.
13. What is Spring Cloud Data Flow?
14. Features of microservices?-> decoupling, componentization,  autonomy, agility, decentralized governance
15. API gateway? logging, routing request, 
16. How to configure spring security? 
    - create class extending `WebSecurityConfigurerAdapter` and annotate with `@EnableWebSecurity`
    - override configure(HttpSecurity http) method
17. How to secure a method in spring security using annotation?
    ```
    @Secured("ROLE_ADMIN")
    public void deleteUser(String name);
    ```
18. How to pre-authorize and post-authorize a method in spring security? -> 
    ``` 
    @PreAuthorize ("hasRole('ROLE_WRITE')") and 
    @PostAuthorize ("returnObject.owner == authentication.name")
    ```
19. How to enable method level security in spring?
    ``` 
    <global-method-security secured-annotations="enabled", pre-post-annotations="enabled"/> 
    ```
20. How to enable web security using java configuration in spring?
    ``` @Configuration
        @EnableWebSecurity
        public class SecurityConfig extends WebSecurityConfigurerAdapter {} 
    ```
21. How to configure DispatcherServlet without web.xml in Spring MVC?
    ```
        public class WebAppInitializer implements WebApplicationInitializer {
            public void onStartup(ServletContext servletContext) throws ServletException {  
              AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();  
              ctx.register(AppConfig.class);  
              ctx.setServletContext(servletContext);    
              Dynamic dynamic = servletContext.addServlet("dispatcher", new DispatcherServlet(ctx));  
              dynamic.addMapping("/");  
              dynamic.setLoadOnStartup(1);  
             }  
        }
     ```
22. How to achieve thread safety in java?
    - Stateless Implementation - Method should neither relies on external state nor maintain state at all. Declare all variables in method only.
    - Immutable class
    - Thread local fields - we can create thread-safe classes that don’t share state between threads by making their fields thread-local.  
    `private ThreadLocal threadLocal = new ThreadLocal();`
    - Synchronized collections - `Collections.synchronizedCollection(new ArrayList<>());`
    - Concurrent Collections - `Map<String,String> concurrentMap = new ConcurrentHashMap<>();`
    - Atomic objects - AtomicInteger, AtomicLong, AtomicBoolean, and AtomicReference
    - **Synchronized methods**
    - **Synchronized blocks**
    - **volatile fileld**
    - Extrinsic locking - it uses an external entity to enforce exclusive access to the resource. synchronized methods and blocks
    rely on the this reference.  
    Example -
        ```
        public class ExtrinsicLockCounter {
            private int counter = 0;
            private final Object lock = new Object();
            public void incrementCounter() {
                synchronized(lock) {
                    counter += 1;
                }
            }
            // standard getter
        }
        ```
    - Reentrant lock - preventing queued threads from suffering some types of resource starvation
    - Read/Write lock
23) RequestParam vs Pathvariable?  
    `@RequestParam` - used to request parameter from URL, below date is RequestParam  
    http://localhost:8080/shop/orders/{orderId}/receipts?date=12-05-2017  
    `@PathVariable` - used to extract value from URI. Above exmaple orderId is pathvariable  
    Jersey annotations - PathParam & QueryParam
24) Encapsulation - restricting direct access to variables
25) Abstraction - hiding internal implementation.
26) How to divide two numbers without using division (/) operator in java/ How to find remainder of division of two numbers using minus (-) operator / Java program to find remainder of division using –
27) Restemplate vs WebClient?  
    RestTemplate - synchronous and blocking operation, degrades performance  
    WebClient - non-blocking operation, better performance, WebFlux based.  
28) @Secured vs @PreAuthorize?
    PreAuthorize uses spring expression language  
    `@PreAuthorize("@someComponent.hasRole('ROLE_VIEWER')")`  
29. Object class and methods of it? - equals, hascode, toString, wait(), wait(long,int), wait(long), notify, notifyAll, finalize, clone, getClass
30. What is aggregation and composition?  
    *composition*- belongs to, strong kind of *part-of* relationship, the objects lifecycles are tied.
    It means that if we destroy the owner object, its members also will be destroyed with it.  
    Example-building and room, car and engine 
    ```
	class Car {
		private Engine engine;

		Car(Engine en) {
			engine = en;
		}
	}
    ``` 
    *aggregation* - has-a relationship, weak association, the lifecycles of the objects aren't tied, every one of them can exist independently of each other.  
    Example- Organization and person, car and driver
    ```
    class Organization {
        private List<Person> person;
    }

    class Person {
        String id,name; 
    }
    ```
31. when abstract class and interface are used?  
    classes that extends abstract class, have many common methods or fields(closely related classes-vehicle,car,volvoCar,OtherCar)
32. String immutability and reason?  
    state of object remains constant after its creation, why?-caching, security, synchronization, performance,thread-safe  
33. Instance lock(attached to single object) vs Static lock(attached to class)?  
    - Instance lock - Acquiring the instance lock only blocks other threads from invoking a synchronized instance method; it does not block other threads from invoking an un-synchronized method, nor does it block them from invoking a static synchronized method
    (http://www.javapractices.com/topic/TopicAction.do?Id=35)  
    example - synchronized(this) acquires the instance lock.  
    - static lock - Similarly, acquiring the static lock only blocks other threads from invoking a static synchronized method; it does not block other threads 
    from invoking an un-synchronized method, nor does it block them from invoking a synchronized instance method.  
    Example - 
    synchronized(Blah.class), using the class literal
    synchronized(this.getClass()), if an object is available
34. Arraylist and linkedlist? which one is better?  
    ArrayList accessing an element takes constant time [O(1)] and adding an element takes O(n) time [worst case].  
    LinkedList adding an element takes O(n) time and accessing also takes O(n) time but LinkedList uses more memory than ArrayList.  
    Prefer ArrayList for random access  
    Prefer LinkedList for random middle insertion
35. singleton pattern?  
    https://dzone.com/articles/all-about-the-singleton
    1. Eager Initialization
    2. Static Block Initialization
    3. Lazy Initialization
    4. Thread-Safe Singletons
    5. Double-Checked Locking
    6. Bill Pugh Solution
        ```
        public class BillPughSingleton {
            private BillPughSingleton(){}
            private static class SingletonHelper{
                private static final BillPughSingleton INSTANCE = new BillPughSingleton();
            }

            public static BillPughSingleton getInstance(){
                return SingletonHelper.INSTANCE;
            }
        }
        ```
     7. Enum Singleton
     8. Reflection and singleton
     9. serialization and singleton
    
    *Break singleton using reflection*
    ```
    public class ReflectionSingleton {
    public static void main(String[] args)  {
        Singleton objOne = Singleton.getInstance();
        Singleton objTwo = null;
        try {
            Constructor constructor = Singleton.class.getDeclaredConstructor();
            constructor.setAccessible(true);
            objTwo = (Singleton) constructor.newInstance();
        } catch (Exception ex) {
            System.out.println(ex);
        }
        System.out.println("Hashcode of Object 1 - "+objOne.hashCode());
        System.out.println("Hashcode of Object 2 - "+objTwo.hashCode());

        }
    }   
    ```
    *Prevent singleton from reflection* - throw a run-time exception in the constructor if the instance already exists.
    ```
    private Singleton() {
        // Check if we already have an instance
        if (INSTANCE != null) {
           throw new IllegalStateException("Singleton" +
             " instance already created.");
        }
    }
    ```
    *Prevent Singleton Pattern From Deserialization* - override readResolve() method in the Singleton class and return the same Singleton instance.
    ```
    protected Object readResolve() { 
           return instance; 
     }  
    ```
    *Prevent Singleton from clonning* - override clone method 
    ```
    @Override
    protected Object clone() throws CloneNotSupportedException  {
        throw new CloneNotSupportedException();
    }
    ```
36. how will you store the Hashmap of <Emp,values>  
    Employee class should properly override equals() and hashCode() methods to store employee object as key in hashmap. Improper implementation of these methods results in wrong data 
37. string reverse without built in method? -> use charAt() iterate from last  
38. can we print hello widout main?
    Yes,but java 7 onwards it will not work
    ```
    class PrintWithoutMain {
        static
        {
            System.out.println("Hello World!!");
            System.exit(1);
        }
    }
    ```
39. convert List into map in java 8?
    ```
    Map cards2Length = cards.stream()
        .collect(Collectors.toMap(Function.identity(), String::length, (e1, e2) -> e1));
    ```
39. Why Map is not the part of Collection interface?  
    Each collection(list,set,queue) stores a single value whereas map stores key-value pair which itselfs is incompatible with collection methods like add, addAll, removeAll  
40. when Outofmemory will occur?  
41. What is diff between heap memory and permgen memory?  
    Java heap memory - all objects are created in heap memory and garbage collector removes unused objects  
    PermGen - keep information about loaded classes,string pool. Garbage collector is not much effective in this area.
43. metaspace vs permgen? After java 8 permGen is renamed as metaspace
44. What is diff between absraction and encapsulaton?
    | Abstraction  | Encapsulation |
    | ------------- | ------------- |
    | solves problem in design level  | solves problem in implementation level |
    | used for hiding unwanted data and giving relevant data | hiding code and data into single unit to protect data from outside world |
    | abstraction lets you focus on what object does instead of how it does | Encapsulation means hiding internal details of how object does something |
    | outer layout | inner layout |
    
45. Comparable vs Comparator
    | Comparable | Comparator |
    | ------ | ------ |
    | Natural/single sorting sequence | Custom/multiple sorting sequence |
    | compareTo(T o) | compare(T o1, T o2) |
    | part of java.lang | part of java.util |

46. functionality of marker interfaces?  
    A marker interface is an interface that has no methods or constants inside it. It provides run-time type information about objects, so the compiler and JVM
    have additional information about the object.
    Example - Serializable, Cloneable
47. can we override and overload main methods?   
    we can overload main() method but we can not override main() method, because a static method cannot be overridden
48. ClassNotFoundException vs NoClassDefFoundError  
    *ClassNotFoundException* - occurs when you try to load a class at runtime using Class.forName() or loadClass() methods and requested classes are not found in classpath. Checked exception derived from java.lang.Exception  
    *NoClassDefFoundError* - occurs when class was present during compile time and program was compiled and linked successfully but class is not present during runtime. It is error which is derived from LinkageError. occurs at runtime  

49. shallow copy vs deep copy?
    | Shallow copy | Deep copy |
    | ----------- | ------- |
    | Shallow Copy stores the references of objects to the original memory address | Deep copy stores copies of the object’s value |
    | Shallow Copy reflects changes made to the new/copied object in the original object | Deep copy doesn’t reflect changes made to the new/copied object in the original object |
    | Shallow Copy stores the copy of the original object and points the references to the objects | Deep copy stores the copy of the original object and recursively copies the objects as well |
    | Shallow copy is faster | Deep copy is comparatively slower |
        
50. How to create immutable class in java
    1. Declare the class as final so it can’t be extended.
    2. Make all fields private so that direct access is not allowed.
    3. Don’t provide setter methods for variables.
    4. Make all mutable fields final so that its value can be assigned only once through constructor.
    5. Initialize all the fields via a constructor performing deep copy
    6. Perform cloning of objects in the getter methods to return a copy rather than returning the actual object reference.
51. HashMap working?   https://javaconceptoftheday.com/how-hashmap-works-internally-in-java/
52. why wait and notify call in the synchronized block?  
    (https://stackoverflow.com/questions/2779484/why-must-wait-always-be-in-synchronized-block)  
    A wait() only makes sense when there is also a notify(), so it's always about communication between threads, and that needs synchronization to work 
    correctly. Mostly wait, notify, notifyAll used in inter thread communication 
    We must use synchronnization to avoid - 
    - IllegalMonitorStateException
    - Any potential race condition between wait and notify method in Java  
    Example - producer-consumer
    https://javarevisited.blogspot.com/2011/05/wait-notify-and-notifyall-in-java.html#axzz75hpg9s9z

53. Reentrant lock?  
    Thread can re-acquire lock.Every lock it acquires increment the holdCount, and decrease holdCount when unlock the resource.  
    The ReentrantLock is a replacement for the easier-to-use synchronized statement when you need one of the following advanced techniques:  
    - lockInterrupibly - 
        ```
        private final ReentrantLock lock = new ReentrantLock();
        public void m1() throws InterruptedException {
            lock.lockInterruptibly();
            try {
                // method body
            } finally {
                lock.unlock();
            }
        }
        ```
        throws an InterruptedException when thread.interrupt() method called by another thread.
    - tryLock- Similar to lockInterrubtably, this throws a thread `InterruptedException` when the thread is interrupted by another thread. Additionally, it allows you to give a time span for how long to try to acquire the lock.  
    `lock.tryLock(2, TimeUnit.MILLISECONDS)`  
    - lock coupling - The reentrant lock allows you to unlock a lock immediately after you have successfully acquired another lock, leading to a technique called lock coupling.  
    - multiple conditions  
    - fair locks - Threads get the lock in the order they requested it.  
    `private final ReentrantLock lock = new ReentrantLock(true);`

    
53. Producer-Consumer programm?https://www.geeksforgeeks.org/producer-consumer-solution-using-threads-java/
54. http://java-latte.blogspot.com/2014/04/Semaphore-CountDownLatch-CyclicBarrier-Phaser-Exchanger-in-Java.html
55. java concurrency - https://www.javacodegeeks.com/2015/09/the-java-util-concurrent-package.html
56. CountDownLatch- is used to make sure that a task waits for other threads before it starts. To understand its application, let us consider a server where the main task can only start when all the required services have started.  
*Working of CountDownLatch:* When we create an object of CountDownLatch, we specify the number of threads it should wait for, all such thread are required to do count down by calling `CountDownLatch.countDown()` once they are completed or ready to the job. As soon as count reaches zero, the waiting task starts running.

57. File upload and download in sring boot
    application.properties
    ```
        # Enable multipart uploads
        spring.servlet.multipart.enabled=true
        # Threshold after which files are written to disk.
        spring.servlet.multipart.file-size-threshold=2KB
        # Max file size.
        spring.servlet.multipart.max-file-size=200MB
        # Max Request Size
        spring.servlet.multipart.max-request-size=215MB
    ```
58. spring bean scopes?
    1. singleton (default) - Single bean object instance per spring IoC container
    2. prototype - Opposite to singleton, it produces a new instance each and every time a bean is requested.
    3. request - A single instance will be created and available during complete lifecycle of an HTTP request. Only valid in web-aware Spring ApplicationContext.
    4. session - A single instance will be created and available during complete lifecycle of an HTTP Session. Only valid in web-aware Spring ApplicationContext.
    5. application - A single instance will be created and available during complete lifecycle of ServletContext. Only valid in web-aware Spring ApplicationContext.
    6. websocket - A single instance will be created and available during complete lifecycle of WebSocket. Only valid in web-aware Spring ApplicationContext.  
    **application scoped bean is singleton per ServletContext, whereas singleton scoped bean is singleton per ApplicationContext. 
    Please note that there can be multiple application contexts for single application.**

59. What is AOP? what does spring AOP provide?  
    In AOP, aspects enable the modularization of concerns such as transaction management, logging or security that cut across multiple types and objects (often termed crosscutting concerns).
    
    AOP provides the way to dynamically add the cross-cutting concern before, after or around the actual logic using simple pluggable configurations

    1. Aspect: An aspect is a class that implements enterprise application concerns that cut across multiple classes, such as transaction    management.
    2. Join Point: A join point is the specific point in the application such as method execution, exception handling, changing object
       variable values etc. In Spring AOP a join points is always the execution of a method
    3. Advice: Advices are actions taken for a particular join point. In terms of programming, they are methods that gets executed when a    certain join point with matching pointcut is reached in the application.
    4. Pointcut: Pointcut are expressions that is matched with join points to determine whether advice needs to be executed or not.          Pointcut uses different kinds of expressions that are matched with the join points and Spring framework uses the AspectJ pointcut     expression language.
    5. Weaving: It is the process of linking aspects with other objects to create the advised proxy objects. This can be done at compile     time, load time or at runtime. Spring AOP performs weaving at the runtime.

    Types of advices?
    1. Before Advice: These advices runs before the execution of join point methods.
    2. After returning advice: Advice to be executed after a join point completes normally
    3. After throwing advice: Advice to be executed if a method exits by throwing an exception.
    4. After advice: Advice to be executed regardless of the means by which a join point exits
    5. Around advice: Around advice can perform custom behavior before and after the method invocation. 

60. What is difference between DI and IOC in spring?  
    `IOC` - is a programming technique in which object coupling bound at runtime by an assembler object and typically not known at compile time   
    `DI` - is design pattern,The basic principle behind Dependency Injection (DI) is that objects define their dependencies only through constructor arguments, arguments to a factory method, or properties which are set on the object instance after it has been constructed or returned from a factory method.  
61. SOAP vs REST?
62. What is Spring Cloud?
    system that provides integration with external systems.  
    - Versioned and distributed configuration.  
    - service discovery  
    - service to service call  
    - Routing  
    - circuit breaker  
    - load balancing  
    - cluster state and leadership election  
    - Global locks and distributed messaging  

63. How to achieve server side load balancing using Spring Cloud? -> `Netflix Zuul`
64. Annotation in spring
    1. RestController = Controller + ResponseBody
    2. RequestMapping - tells which HTTP request should map to the corresponding method.  
        `@RequestMapping(value = "/index", method = "GET") `
    3. RequestParam - binds web request parameter to method parameter
        ```
        public String getFoos(@RequestParam String id) {
            return "ID: " + id;
        }
        ```
    4. ContextConfiguration - specifies how to load the application context while writing a unit test for the Spring environment
    5. ResponseBody -  tells a controller that the object returned is automatically serialized into JSON and passed back into the HttpResponse object
    6. PathVariable - indicates method parameter should be bound to a URI template variable
        ```
        @RequestMapping(path="/{name}/{age}")
        public String getMessage(@PathVariable("name") String name, 
                @PathVariable("age") String age) {
            
            var msg = String.format("%s is %s years old", name, age);
            return msg;
        }
        ```
    7. ResponseEntity - Represents HTTP response, including headers, body, and status.`ResponseEntity` allows to add header and status code whereas `@ResponseBody` puts values into response
    8. Qualifier - use to differentiate beans of same type
    9. Autowired - used to inject object dependency implicitly for a constructor, field or method
    10. Configurable - Used on classes to inject properties of domain objects
    11. Required - Used to mark class members that are mandatory. `BeanInitializationException`
    12. ComponentScan - Make Spring scan the package for the @Configuration clases.
    13. Configuration - It is used on classes that define beans.
    14. Bean - indicates that a method produces a bean which will be mananged by the Spring container.
    15. Lazy - Makes a @Bean or @Component to be initialized only if it is requested
    16. Value - it is used to inject values into a bean’s attribute from a property file.
    17. Resource - Annotation used to inject an object that is already in the Appl­ication Context.
    18. Primary - Annotation used when no name is provided telling Spring to inject an object of the annotated class first.
    19. Component - Generic stereotype annotation used to tell Spring to create an instance of the object in the Appl­ication Context
    20. Controller, Repository, Service
    21. Conditional - load beans into Application Context only if the given condition is met

    22. SpringBootApplication - used to qualify the main class for a Spring Boot project
    23. EnableAutoConfiguration - Based on class path settings, property settings, new beans are added by Spring Boot by using this           annotation.

65. @Primary vs @Qualifier  
    `Primary` - indicates that a bean should be given preference when multiple candidates are qualified to autowire a single-valued dependency.  
    `Qualifier` - indicates specific bean should be autowired when there are multiple candidates.

65. Actuator endpoints?
    | Endpoint | Description |
    | -------- | --------- |
    | health |	Application health info |
    | info	| Info about the application |
    | env |	Properties from environment |
    | metrics |	Various metrics about the app |
    | mappings |	@RequestMapping Controller mappings |
    | shutdown |	Triggers application shutdown |
    | httptrace |	HTTP request/response log |
    | loggers |	Display and configure logger info |
    | logfile |	Contents of the log file |
    | threaddump |	Perform thread dump |
    | heapdump |	Obtain JVM heap dump |
    | caches |	Check available caches |
    | integrationgraph |	Graph of Spring Integration components |


    ```
        # Disable an endpoint
        management.endpoint.[endpoint-name].enabled=false

        # Specific example for 'health' endpoint
        management.endpoint.health.enabled=false

        # Instead of enabled by default, you can change to mode
        # where endpoints need to be explicitly enabled
        management.endpoints.enabled-by-default=false
    ```

66. What is a CommandLineRunner and ApplicationRunner?  
    `ApplicationRunner` and `CommandLineRunner` interfaces use to execute the code after the Spring Boot application is started.  
67. ApplicationRunner vs CommandLineRunner?  
    `ApplicationRunner`'s run(ApplicationArguments args) method accepts single args whereas `CommandLineRunner`'s run(String... args) methods accepts var-args  
67. How to implement Exception Handling in Spring Boot?  
    create class by extending `ResponseEntityExceptionHandler`,annotate with `@RestControllerAdvice`, override default `@ExceptionHandler`  
    https://reflectoring.io/spring-boot-exception-handling/
68. InitBinder?

# Java 8 

https://www.interviewbit.com/java-8-interview-questions/  

69. What are Java 8 streams?  
    A stream is an abstraction to express data processing queries in a declarative way.  
    It represents a sequence of data objects & series of operations on that data is a data pipeline   
70. What are the main components of a Stream?  
    a. data source  
    b. Set of Intermediate Operations to process the data source  
    c. Single Terminal Operation that produces the result  
71. What are Intermediate and Terminal operations?  
72. What are the most commonly used Intermediate operations?  
    `Filter(Predicate<? super T> pred)` - Allows selective processing of Stream elements. It returns elements that are satisfying the supplied condition by the predicate.  
    `map(Funtion<T, R>)` - Returns a new Stream, transforming each of the elements by applying the supplied mapper function e.g sorted()  
    `distinct()` - Only pass on elements to the next stage, not passed yet.  
    `limit(long maxsize)` - Limit the stream size to maxsize.  
    `skip(long start)` - Skip the initial elements till the start.  
    `peek(Consumer)` - Apply a consumer without modification to the stream.  
    `flatMap(mapper)` - Transform each element to a stream of its constituent elements and flatten all the streams into a single stream.  
73. What is the stateful intermediate operation? Give some examples of stateful intermediate operations.  
    To complete some of the intermediate operations, some state is to be maintained, and such intermediate operations are called stateful intermediate operations.  
    e.g sorted(), distinct()  
74. common type of terminal operations?  
    `collect(), reduce(), count(), min(), max(), anyMatch(), noneMatch(), forEach(), forEachOrdered()`
75. collection vs stream?[que no 4.]  
76. What is the feature of the new Date and Time API in Java 8?  
    - Immutable classes and Thread-safe
    - Timezone support
    - Fluent methods for object creation and arithmetic
    - Addresses I18N issue for earlier APIs
    - All packages are based on the ISO-8601 calendar system

77. Define Nashorn in Java 8

78.What is method reference?  
    A method reference is a Java 8 construct that can be used for referencing a method without invoking it.
    
    - Constructor refernce - String::new;
    - Static method reference - ContainingClass::staticMethodName
    - Bound instance method reference - str::toString
    - Unbound instance method reference - String::toString;

79. Functional Interfaces in the Standard Library?

    - Function – it takes one argument and returns a result
    - Consumer – it takes one argument and returns no result
    - Supplier – it takes no arguments and returns a result
    - Predicate – it takes one argument and returns a boolean
    - BiFunction – it takes two arguments and returns a result
    - BinaryOperator – it is similar to a BiFunction, taking two arguments and returning a result. The two arguments and the result are all of the same types.
    - UnaryOperator – it is similar to a Function, taking a single argument and returning a result of the same type

80. What Is a Functional Interface?\
    interface with one single abstract method.  
    Functional Interface can be used with lambda expression.  
    e.g Runnable, Comparable
81. default method?  
    Java 8 has introduced the concept of default methods which allow the interfaces to have methods with implementation without affecting the classes that implement the interface.
82. lambda expression?
    lambda expression is a function that we can reference and pass around as an object.(Anonymous function)  
83. Final variable can be accessed in lambda expression in java 8? - yes
84. non-Final variable can be accessed in lambda expression in java 8?  
    yes,but it is effectively final in lambda expression. Any attempt to modify x will produce compilation error
85. Instance and static variables are accessible and modifiable in lambda expression
86. Byte stream vs Character stream? (FileInputStream vs FileReader and 8-bit byte vs 16-bit unicode)
87. Runtime class vs System class?\
    Runtime class - provide access to java runtime system like memoery usage,available memory,invoking garbage collection\
    System class - provide access to system resources like standard input/output, current time in millis, terminating applications

88. assertions in java? -> testing the correctness of any assumptions.
89. Can we have multiple public classes in a java source file?
90. covarient return type?  
    It is possible to have different return type for a overriding method in child class, but child’s return type should be sub-type of parent’s return type.  
    OR  
    Covariant return, means that when one overrides a method, the return type of the overriding method is allowed to be a subtype of the overridden method's return type.

    ```
    class SuperClass {
        SuperClass get() {
            System.out.println("SuperClass");
            return this;
        }
    }

    public class Tester extends SuperClass {
        Tester get() {
            System.out.println("SubClass");
            return this;
        }

        public static void main(String[] args) {
            SuperClass tester = new Tester();
            tester.get();
        }
    }
    ```

91. wrapper class?  
    convert primitive into object and object into primitive

92. Reflection API?  
    Java Reflection is the process of analyzing and modifying all the capabilities of a class at runtime. Reflection API in Java is used to manipulate class and its members which include fields, methods, constructor, etc. at runtime


93. What is the final variable, final class, and final blank variable?  

94. How will you invoke any external process in Java?  
using exec() method of Runtime class  

95. How many ways you can take input from the console?
    - Buffered Reader Class
    - Scanner class
    - Console class - System.console().readLine();   

96. How can you avoid serialization in child class if the base class is implementing the Serializable interface?  
In subclass override writeObject() method and throw NotSerializableException()  

97. Serializable vs Externalizable?  
98. What are the ways to instantiate the Class class?\
    - using new keyword
    - using Class.forName()
    - Using clone()
    - using deserialization
    - using newInstance() 


99. autoboxing and unboxing?  
automatic conversion of primitive data types into its equivalent Wrapper type is known as boxing and opposite operation is known as unboxing  

100. Difference between creating String as new() and literal?  
101. String vs StringBuffer vs StringBuilder?  
- String is immutable where as StringBuffer and StringBuilder are mutable  
- StringBuffer is thread safe, StringBuilder is not thread safe  
- String implements `equals()` whereas StringBuffer and StringBuilder do not implement. 

102. What is a Memory Leak? How can a memory leak appear in garbage collected language?\
    objects are no longer being used by the application, but the Garbage Collector is unable to remove them from working memory.\
    tools to identify useless objects  

    - HP Openview
    - HP JMeter
    - JProbe
    - IBM Tivoli

103. Why String is popular HashMap key in Java?  
104. Error vs Exception?
105.  throw vs throws?  
- throw - used in method body to throw an exception
- throws - used in method signature to declare an exception that can occur in statement in method body
106. The difference between Serial and Parallel Garbage Collector?\

Serial Garbage Collector

Serial garbage collector works by holding all the application threads. It is designed for the single-threaded environments. **It uses just a single thread for garbage collection.** The way it works by freezing all the application threads while doing garbage collection may not be suitable for a server environment. It is best suited for simple command-line programs.

Turn on the -XX:+UseSerialGC JVM argument to use the serial garbage collector.

Parallel Garbage Collector

Parallel garbage collector is also called as throughput collector. It is the default garbage collector of the JVM. Unlike serial garbage collector, **this uses multiple threads for garbage collection.** Similar to serial garbage collector this also freezes all the application threads while performing garbage collection.

107. What is difference between WeakReference and SoftReference in Java?  
- Strong reference: This is the default type/class of Reference Object.
    `MyClass obj = new MyClass();`
 - Weak reference: Weak Reference Objects are not the default type/class of Reference Object and they should be explicitly specified while using them.
 - Soft References: In Soft reference, even if the object is free for garbage collection then also its not garbage collected, until JVM is in need of memory badly.
 - Phantom References: The objects which are being referenced by phantom references are eligible for garbage collection. But, before removing them from the memory, JVM puts them in a queue called ‘reference queue’.

108. What is a compile time constant in Java? What is the risk of using it?  
a constant whose value known at compile time and compiler replaces constant name with value everywhere in code.  
`private final int x = 10;`

109.  How bootstrap class loader works in java?  

    - Bootstrap class loader  
    - Extension class loader  
    - System class loader  

110. While overriding a method can you throw another exception or broader exception?  
If a method declares to throw a given exception, the overriding method in a subclass can only declare to throw that exception or its subclass. This is because of polymorphism.

111. checked, unchecked exception and errors?  
- Checked Exception: These are the classes that extend Throwable except RuntimeException and Error.  
Example: IOException, SQLException
- Unchecked Exception: The classes that extend RuntimeException are known as unchecked exceptions.  
Example: ArithmeticException, NullPointerException, ArrayIndexOutOfBoundsException
- Error: irrecoverable situation that is not being handled by try/catch  
Example: OutOfMemoryError, VirtualMachineError, AssertionError

112. ClassNotFoundException vs NoClassDefFoundError?
    Both occurs at runtime
    
`ClassNotFoundException` - occurs when you try to load a class at run time using Class.forName() or loadClass() methods and mentioned classes are not found in the classpath  
    
`NoClassDefFoundError` - occurs when a particular class is present at compile time, but was missing at run time.

113. CountDownLatch?  
    This class enables a java thread to wait until other set of threads completes their tasks.  
    Initialized with given count and can not reset. Count specifies the number of events that must occur before latch is released.  
    example - In amusement park, wait for 3 person to start ride

114. CyclicBarrier?  
    2 or more threads wait for each other to reach a common barrier point. When all threads have reached common barrier point.  
    > All waiting threads are released  
    > Event can be triggered as well.  
Count can be reset hence called cyclic barrier  
Example -  Let’s say 10 friends (friends are threads) have planned for picnic on place A (Here place A is common barrier point). And they all decided to play certain game (game is event) only on everyones arrival at place A. So, all 10 friends must wait for each other to reach place A before launching event.   

* Phaser? - similar in functionality of CyclicBarrier and CountDownLatch but it provides more flexibility than both of them. It provides flexibility of registering and deRegistering parties at any time. In CyclicBarrier parties can be registered in constructor, in Phaser parties can be registered at any time.  

* Exchanger - Exchanger enables two threads to exchange their data between each other. solves produce-consumer problem.  

* If superclass method throws/declare **unchecked/RuntimeException** in java -  
    - overridden method of subclass can declare/throw any unchecked/RuntimeException (superclass or subclass)  
    - overridden method of subclass **cannot** declare/throw any **checked exception** in java  
    - overridden method of subclass can declare/throw same exception in java  
    - overridden method of subclass *may not declare/throw any exception* in java  

* If superclass method throws/declare **checked/compileTime** exception in java -   
    - overridden method of subclass can declare/throw narrower (subclass of) checked exception  
    - overridden method of subclass can declare/throw any unchecked /RuntimeException  
    - overridden method of subclass can declare/throw same exception  
    - overridden method of subclass may not declare/throw any exception in java  

* If superclass method **does not throw/declare** any exception in java -  
    - overridden method of subclass **can** declare/throw any **unchecked/RuntimeException**  
    - overridden method of subclass **cannot** declare/throw any **checked exception**  
    - overridden method of subclass may not declare/throw any exception in java  

* What will happen when catch and finally block both return value, also when try and finally both return value in java?  
    method will ultimately return value returned by finally.

* ClassNotFoundException vs NoClassDefFoundError?

    | ClassNotFoundException | NoClassDefFoundError |
    | ---------------------- | -------------------- |
    | ClassNotFoundException is Checked (compile time) Exception in java. | NoClassDefFoundError is a Error in java. Error and its subclasses are regarded as unchecked exceptions in java. |
    | ClassNotFoundException is thrown when JVM tries to load class from classpath but it does not find that class. | NoClassDefFoundError is thrown when JVM tries to load class which 1. was NOT available at runtime but  2. was available at compile time. |
    




115. How to use Exchanger for sharing Object between Threads in Java?  
    https://javarevisited.blogspot.com/2020/04/how-to-use-exchanger-in-java-with-example.html
116. What is semaphore?  
    semaphore controls access to a shared resource by using permits in java.  
    1. If permits are greater than zero, then semaphore allow access to shared resource.  
    2. If permits are zero or less than zero, then semaphore does not allow access to shared resource.  

117. How do WeakHashMap works?  
    entry in WeakHashMap automatically removed when key is in no longer use.  

118. PriorityQueue?  
     elements are ordered as per their natural ordering or based on a custom Comparator supplied at the time of creation. grows dynamically, not thread-safe use PriorityBlockingQueue for concurrent env  

119. solve producer-consumer problem using BlockingQueue and semaphore?  

120. peek() vs poll vs remove() in queue?  
    peek() - returns the object at the top of the current queue, without removing it. If the queue is empty this method returns null.  
    poll() - returns the object at the top of the current queue and removes it. If the queue is empty this method returns null  
    remove() - same as poll difference is when queue is empty. If queue is empty poll() return null whereas remove()  throws NoSuchElementException  

121. ThreadLocal?  
    The Java ThreadLocal class enables you to create variables that can only be read and written by the same thread.  

122. openSession() vs getCurrentSession()?  
    getCurrentSession - bound to hibernate context, we don't need to close it, once sessionFactory is closed session object also closed, used in single threaded env    
    openSession - always open new session, need to close after each DB operation, used in multi-threaded environment.  

123. How to implement queue functionality into stack?  
124. How to implement stack functionality into queue?  
125. What classes should i prefer to use a key in HashMap in java?  
    All wrapper classes because they are immutable and they implements equals() and hashCode() methods.

126. What do you mean by fail-fast and fast-safe?  
    Iterator returned by few Collection framework Classes are fail-fast, means any structural modification made to these classes during iteration will throw ConcurrentModificationException  
    fail-fast: ArrayList, LinkedList, Vector, HashSet  
    fail-safe: CopyOnWriteArrayList, CopyOnWriteArraySet, ConcurrentSkipListSet

127. What are different ways of iterating over elements in List?  
    ```
    
    1. iterator:
        
        Iterator<String> iterator = arrayList.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }

    2. listIterator:    
        ListIterator<String> listIterator=arrayList.listIterator();

    3. enumeration:
        
        Enumeration<String> listEnum=Collections.enumeration(arrayList); 
        while(listEnum.hasMoreElements()){
        System.out.println(listEnum.nextElement()); 
        }
        
    4. enhanced for loop:
        
        for (String string : arrayList) {
            System.out.println(string);
        }
        
    ```    

128. What are different ways of iterating over keys, values and entry in Map?  
* *Using keys*:  
    1. iterator over keys
    ```
    Iterator<Integer> keyIterator=hashMap.keySet().iterator();
    while (keyIterator.hasNext()) {
        String key = keyIterator.next();
        System.out.println(key + ":" + hashMap.get(key));
    }
    ```
    2. using enhanced for loop - 
    ```
    public void iterateUsingKeySetAndForeach(Map<String, Integer> map){
        for (String key : map.keySet()) {
            System.out.println(key + ":" + map.get(key));
        }
    }
    ```
* *Using values*:  
    1. iterator over values
    ```
    Iterator<String> valueIterator=hashMap.values().iterator();
    while (valueIterator.hasNext()) {
        Integer value = valueIterator.next();
        System.out.println("value :" + value);
    }
    ```  
    2. using enhanced for loop - 
    ```
    public void iterateValues(Map<String, Integer> map) {
        for (Integer value : map.values()) {
            System.out.println(value);
        }
    }
    ```
* *Using entry*: 
    1. iterator over entry  
    ```
    Iterator<Entry<Integer, String>> entryIterator=hashMap.entrySet().iterator();
    while (entryIterator.hasNext()) {
        Map.Entry<String, Integer> entry = entryIterator.next();
        System.out.println(entry.getKey() + ":" + entry.getValue());
    }
    ```
    2. Using enhanced for loop -
    ```
    public void iterateUsingEntrySet(Map<String, Integer> map) {
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + ":" + entry.getValue());
        }
    }
    ```
* Using lambda and forEach()
    ```
    public void iterateUsingLambda(Map<String, Integer> map) {
        map.forEach((k, v) -> System.out.println((k + ":" + v)));
    }
    ```
    ```
    public void iterateByKeysUsingLambda(Map<String, Integer> map) {
	    map.keySet().forEach(k -> System.out.println((k + ":" + map.get(k))));
	}
    ```
    ```
    public void iterateValuesUsingLambda(Map<String, Integer> map) {
        map.values().forEach(v -> System.out.println(("value: " + v)));
    }
    ```
* Using Stream API  
    ```
    public void iterateUsingStreamAPI(Map<String, Integer> map) {
        map.entrySet().stream()
        // ... some other Stream processings
        .forEach(e -> System.out.println(e.getKey() + ":" + e.getValue()));
    }
    ```


129. comparable vs comparator?
130. Can we use null key in TreeMap? Give reason?
131. What is difference between using instanceOf operator and getClass() in equals method?  
    instanceOf it will return true for comparing current class with its subclass as well, but getClass() will return true only if exactly same class is compared.  

132. Inner Class/member inner class?  
    
    ```

    class OuterClass {

        int i = 1; // instanceVariable

        void m() {
        } // instanceMethod

        static int staticI = 1; // staticVariable

        static void staticM() {
        } // staticMethod

        // Inner class
        class InnerClass {
            public void method() {
                System.out.println("In InnerClass's method");

                i = 1; // OuterClass instanceVariable
                m(); // OuterClass instanceMethod

                staticI = 1; // OuterClass staticVariable
                staticM(); // OuterClass staticMethod

                //Inner classes cannot not declare static initialization blocks
                static{} //compilation error

                 //Inner classes cannot not declare member interfaces.
                interface I{} //compilation error

                // Inner classes cannot not declare static members
                static int i = 2; // compilation error

                System.out.println("OuterClass reference=" + OuterClass.this);
                System.out.println("InnerClass reference=" + this);

                OuterClass.this.i = 2;// Accessing OuterClass instanceVariable using OuerClass reference
                OuterClass.this.m();// Accessing OuterClass instanceMethod using OuterClass reference

            }
        } // End InnerClass
    }

    public class InnerClassTest {
        public static void main(String[] args) {
            // Creating instance of InnerClass
            new OuterClass().new InnerClass().method();

        }
    }
    ```


132. static nested class  
    ```

    class OuterClass {
    // StaticNestedClass
    static class StaticNestedClass {
 
           // StaticNestedClass can declare static initialization blocks
           static {
           }
 
           // StaticNestedClass can declare member interfaces.
           interface I {
           }
 
           // StaticNestedClass can declare static members
           static int i = 2;
 
           // StaticNestedClass can declare constant variables
           static final int j = 3;
  
           //StaticNestedClass classes can declare instance initialization blocks
           {}
           
           //StaticNestedClass constructor
           StaticNestedClass() {}

      }
    }
    ```

133. Local inner class?  
    ```

    class OuterClass {
        // LocalInnerClass inside instance block
        {
            class A {
            }
        }

        // LocalInnerClass inside static block
        static {
            class A {
            }
        }

        void myMethod() {
            // LocalInnerClass inside if statement
            if (true) {
                class A {
                }
            }

            // LocalInnerClass inside for loop statement
            for (int i = 0; i < 1; i++) {
                class A {
                }
            }
        }
    }
    ```

134.Anonymous Inner class?  
    - AnonymousInnerClass implement interface  
    
    ```
    interface MyInterface {
        void m();
    }

    public class InnerClassTest {
        public static void main(String[] args) {

            // Anonymous inner class
            new MyInterface() { // implementing interface
                public void m() { 
                    // Provide implementation of MyInterface's m() method
                    System.out.println("implementation of MyInterface's m() method");
                }
            }.m();
        }
    }
    ```

135. What are Method references?  
    There are 4 types of Method reference in Java 8  
* reference to static method  - 
ContainingClass::staticMethodName  
* Reference to an instance method of a particular object - 
containingObject::instanceMethodName  
* Reference to an instance method of an arbitrary object of a particular type - 
ContainingType::methodName  
* Reference to a constructor - 
ClassName::new

136. Multiple DB conections in Spring boot?  

    
    #first db
    spring.datasource.url = [url]
    spring.datasource.username = [username]
    spring.datasource.password = [password]
    spring.datasource.driverClassName = oracle.jdbc.OracleDriver

    #second db ...
    spring.secondDatasource.url = [url]
    spring.secondDatasource.username = [username]
    spring.secondDatasource.password = [password]
    spring.secondDatasource.driverClassName = oracle.jdbc.OracleDriver

    @Bean
    @Primary
    @ConfigurationProperties(prefix="spring.datasource")
    public DataSource primaryDataSource() {
        return DataSourceBuilder.create().build();
    }

    @Bean
    @ConfigurationProperties(prefix="spring.secondDatasource")
    public DataSource secondaryDataSource() {
        return DataSourceBuilder.create().build();
    }
    

137. Parallel stream vs Sequential ?  
138. Qualifier vs Primary?  
    `@Primary` indicates that a bean should be given preference when multiple candidates are qualified to autowire a single-valued dependency.  
    `@Qualifier` indicates specific bean should be autowired when there are multiple candidates.
139. Distributed Transaction in microservices?  
     - 2 phase commit  
     - 3 phase commit  
     - saga pattern - 2 types  
        1. choreography/event - event based, produces and consumes events(MQ)
        2. orchestration/command - coordinator service is responsible for centralizing the saga’s decision making and sequencing business logic   

140. FeignClient?
141. `@AutoConfiguration` ? -> automatically configures the Spring application based on the jar dependencies that we have added. To enable it use `@EnableAutoConfiguration` (it is warpped in `@SpringBootApplication` so not needed)
142. `@SpringBootApplication = @Configuration + @EnableAutoConfiguration + @ComponentScan`
143. How to Inject Prototype Scoped Bean into a Singleton Bean in Spring? -> Beans are instantiated only once at the time of container initialization  
    1. Implementing the ApplicationContextAware interface  
    2. Lookup method injection in Spring  
    3. aop:scoped-proxy  
    4. Using ObjectFactory interface  

144. Can Enum extend any class in Java?  
    No, enum by default extends abstract base class java.lang.Enum, multiple inheriance problem with classes.  
145. Spring beans are thread-safe?  
    No, container doesn't handle synchronization/thread-safety, it handles life cycle of object.  
146. Interface With Default Methods vs Abstract Class 
147. map vs flatMap?  
    Both `map` and `flatMap` can be applied to a `Stream<T>` and they both return a `Stream<R>`. The difference is that the map operation produces ***one output* value for each input value**, whereas the flatMap operation produces an ***arbitrary number* (zero or more) values for each input value**.  
    One line answer: flatMap helps to flatten a `Collection<Collection<T>>` into a `Collection<T>`. In the same way, it will also flatten an `Optional<Optional<T>>` into `Optional<T>`  
    Example -  


    public class Parcel {
        String name;
        List<String> items;

        public Parcel(String name, String... items) {
            this.name = name;
            this.items = Arrays.asList(items);
        }

        public List<String> getItems() {
            return items;
        }

        public static void main(String[] args) {
            Parcel amazon = new Parcel("amazon", "Laptop", "Phone");
            Parcel ebay = new Parcel("ebay", "Mouse", "Keyboard");
            List<Parcel> parcels = Arrays.asList(amazon, ebay);
            System.out.println("-------- Without flatMap() ---------------------------");
            List<List<String>> mapReturn = parcels.stream().map(Parcel::getItems).collect(Collectors.toList());
            System.out.println("\t collect() returns: " + mapReturn);

            System.out.println("\n-------- With flatMap() ------------------------------");
            List<String> flatMapReturn = parcels.stream().map(Parcel::getItems).flatMap(Collection::stream)
                    .collect(Collectors.toList());
            System.out.println("\t collect() returns: " + flatMapReturn);
        }
    }


148. Methods of Stream class?  
    1. Intermediate operation - map, filter, sorted, anyMatch, noneMatch  
    2. Terminal operation - collect, forEach, reduce

149. return type of lambda expression? -> return type of method defined in FunctionalInterface??  convert lambda expression back to anonymous class.
150. JpaRepository vs CrudRepository?  
    PagingAndSorting is added advantage in  JpaRepository  
    JpaRepository -> PagingAndSortingRepository -> CrudRepository -> Repository  

151. Callable vs Runnable?  
    1. Callable - needs to implement call() method, Runnable needs to implement run() method  
    2. Callable return value of Future, but runnable returns void.  
    3. Callable throw checked exception but runnable not  
    4. Thread constructor do not receive callable object so to execute callable task use `ExecutorService`  
    5. To execute callable task use submit() method.
    
*can we convert runnbale task into callable? -> Yes*  
`Callable callable = Executors.callable(Runnable task);`
        

**`StringBuffer` class doesn’t overrides `equals` method of `Object` class and it compares reference and not data**

152. what hashcode returned by Object class?  
    `hashCode` implemented as native method in object class, it marked as `@HotSpotIntrinsicCandidate`

Kiali - Kiali is a management console for Istio service mesh  
Jaeger - is open source software for tracing transactions between distributed services. Internally uses zipkin. Jaeger used in distributed environment to independantly scaled.  
Grafana - metrics visualization  
Prometheus - excels in metric data collection, whereas Grafana champions metric visualizations

153. create thread using java8?  

    Runnable task = () -> {
        String threadName = Thread.currentThread().getName();
        System.out.println("Hello " + threadName);
    };

    task.run();


154. ways to create thread and start?  
* Thread Subclass -
    ```
     public class MyThread extends Thread {
        public void run(){
        System.out.println("MyThread running");
        }
    }
    MyThread myThread = new MyThread();
    myTread.start();
    ```

* Runnable Interface - 
    ```
    Runnable runnable = new MyRunnable();

    Thread thread = new Thread(runnable);
    thread.start();
    ```

* Anonymous subclass of thread -
    ```
     Thread thread = new Thread(){
        public void run(){
        System.out.println("Thread Running");
        }
    }
    thread.start();
    ```
     ```
    new Thread() {
        public void run() {
            System.out.println("blah");
        }
    }.start();    
    ```
* Anonymous implementation of Runnable - 
    ```
    new Thread(new Runnable() {
        public void run() {
            System.out.println("blah");
        }
    }).start();
    ```
    ```
    Runnable myRunnable = new Runnable(){
        public void run(){
            System.out.println("Runnable running");
        }
    };
    Thread t = new Thread(myRunnable);
    t.start();
    ```
* Lambda implementation of Runnable
    ```
    Runnable runnable = () -> {  System.out.println("Lambda Runnable running");};
    ```

155. @ControllerAdvice vs @RestControllerAdvice?  
`@RestControllerAdvice` = `@ControllerAdvice` + `@ResponseBody` - we can use in REST web services.  
`@ControllerAdvice` - We can use in both MVC and Rest web services, need to provide the ResponseBody if we use this in Rest web services.

156. sort list in reverse order in java8?   
    ```
    list.stream().sorted(Collections.reverseOrder()).collect(Collectors.toList());
    ```

Custom comparator -  

    ```
    List<User> sortedList = userList.stream()
        .sorted(customComparator)
        .collect(Collectors.toList());  
    ------------
    Comparator<User> customComparator = new Comparator<User>() {
        @Override
        public int compare(User o1, User o2) {
            if(o1.getAge() == o2.getAge())
                return o1.getName().compareTo(o2.getName());
            else if(o1.getAge() > o2.getAge())
                return 1;
            else return -1;
        }
    };
    ```

157. SOLID principle and how you used in your project?

*map() - transform one object into other by applying a function*  
*filter() - filters elements based upon a condition/predicate*  

158. Does Immutability Really Mean Thread Safety?  
    https://dzone.com/articles/do-immutability-really-means
159. Upper bounded wildcards vs lower bounded wildcards?  
    Upper bounded wildcards -  
    `List<? extends Number> list`  
    Lower bounded wildcards -  
    `List<? super Integer> list`  
160. Fail-safe collections and fail-fast collections?  
    fail-safe: ConcurrentHashMap, CopyOnArrayList, CopyOnArraySet  
    fail-fast: ArrayList, LinkedList, etc

161. third highrst salary?  
    `SELECT * FROM employee ORDER BY salary DESC LIMIT 1 OFFSET 2;`

162. How to reuse stream?  
    create a stream supplier to construct a new stream with all intermediate operations  

    Supplier<Stream<String>> streamSupplier =
    () -> Stream.of("d2", "a2", "b1", "b3", "c")
            .filter(s -> s.startsWith("a"));

    streamSupplier.get().anyMatch(s -> true);   // ok
    streamSupplier.get().noneMatch(s -> true);  // ok
    

163. why you can’t make your spring bean classes final?  
    when you auto-wire a bean of type Sample, Spring in fact doesn’t provide you exactly with an instance of Sample. Instead, it injects a generated proxy class that extends Sample and final class can't be extended, to avoid this bean class cannot be declared as final  

164. Idempotent?  
    When making multiple identical request has same result that is called as idempotent  
    Example - PUT and DELETE  

165. What is OAuth?  
    Oauth is an open-standard authorization protocol or framework that provides applications the ability for “secure designated access.”  

* OpenID vs OAuth2?  
OpenID Connect (OIDC) is an identity layer built on top of the OAuth 2.0 framework.   
While OAuth 2.0 is about resource access and sharing, OIDC is about user authentication.  
https://auth0.com/docs/authenticate/protocols/openid-connect-protocol

166. nth highest salary?

    ```
    SELECT * FROM employee 
    WHERE salary= (SELECT DISTINCT(salary) 
    FROM employee ORDER BY salary DESC LIMIT n-1,1);
    ```  

* second highest salary?  
```
select * from employee 
group by salary 
order by salary desc limit 1,1;
```

167. create a table using existing table?  
    `SELECT * FROM ExistingTable INTO NewTable WHERE 1=2`    
    OR  
    ```
    CREATE TABLE new_table 
    AS (SELECT * 
        FROM old_table WHERE 1=2); 
    ```

168. find how many male and female empolyees from list of 1000 in java8?  
    ```
    Map<String, Map<String, Long>> multipleFieldsMap = employeesList.stream().collect(Collectors.groupingBy(Employee::getGender, Collectors.counting())));
    ```

169. groupBy java8 example?

    ``` 
    import java.util.ArrayList;
    import java.util.Comparator;
    import java.util.IntSummaryStatistics;
    import java.util.List;
    import java.util.Map;
    import java.util.Optional;
    import java.util.Set;
    import java.util.TreeMap;
    import java.util.stream.Collectors;
    
    public class GroupByJava8
    {
        public static void main( String[] args )
        {
            List <Person> personList = new ArrayList<Person>();
            personList.add(new Person("Sharon", 21, "Female"));
            personList.add(new Person("Maria", 18,  "Female"));
            personList.add(new Person("Jack", 21 ,"Male"));
            personList.add(new Person("James", 35,  "Male"));       
            
            Map<String, List<Person>> groupByGenderList = 
                    personList.stream().collect(Collectors.groupingBy(Person::getGender));
            
            //Group by gender List : Female-> Persons and Male -> Persons
            System.out.println("1. Group persons by gender - get result in List: ");
            System.out.println(groupByGenderList.toString());
    
            Map<String, Set<Person>> groupByGenderSet = 
                    personList.stream().collect(Collectors.groupingBy(Person::getGender,Collectors.toSet()));
            
            //Group by gender Set: Female-> Persons and Male -> Persons
            System.out.println("2. Group persons by gender - get result in Set: ");
            System.out.println(groupByGenderSet.toString());
            
            Map<String, Set<String>> groupByGenderAndFirstNameSet
            = personList.stream().collect(Collectors.groupingBy(Person::getGender, TreeMap::new,
                    Collectors.mapping(Person::getName, Collectors.toSet())));    
            System.out.println("3. Group person by gender and get name of person - get result in Set: ");
            System.out.println(groupByGenderAndFirstNameSet.toString());
            
            Map<String, Long> countPersonByGender = personList.stream().
                    collect(Collectors.groupingBy(Person::getGender,Collectors.counting()));
            
            System.out.println("4. Count person objects by gender: ");
            System.out.println(countPersonByGender.toString());     
    
            Map<String, Optional<Person>> personByMaxAge = personList.stream().
                    collect(Collectors.groupingBy(Person::getGender
                            ,Collectors.maxBy(Comparator.comparing(Person::getAge))));      
            System.out.println("5. Group person objects by gender and get person with max age: ");
            System.out.println(personByMaxAge.toString());
    
            Map<String, IntSummaryStatistics> groupPersonsByAge = personList.stream().
                    collect(Collectors.groupingBy(Person::getGender
                            ,Collectors.summarizingInt(Person::getAge)));       
            System.out.println("6. Group person objects by gender and get age statistics: ");
            System.out.println(groupPersonsByAge.toString());
            IntSummaryStatistics malesAge = groupPersonsByAge.get("Male");
            System.out.println("Avgerage male age:"+ malesAge.getAverage());
            System.out.println("Max male age:"+ malesAge.getMax());
            System.out.println("Min male age:"+ malesAge.getMin());
        }    
    }
    ```

170. Sort map based on values?  
    sample output - 
    
    Map<String,Integer> map = new HashMap<>();
    map.put("A" , 50);
    map.put("B" , 40);
    map.put("C" , 60);

    Expected OutPut: 
    List<String> = ["B", "A", "C"]
    
    Solution --------

    Map<String, Integer> map = Map.of("C", 3, "A", 1, "B", 12, "D", 5);
    List<String> list = new ArrayList<>();
    
    map.entrySet().stream().sorted(Map.Entry.comparingByValue())
        .forEachOrdered(e -> {
            System.out.println("Key: "+e.getKey() + " Value: "+e.getValue());
            list.add(e.getKey());
        });
    
    System.out.println(list);

    for reverse order  
    1. sorted(Map.Entry.comparingByValue(Collections.reverseOrder()))  
    2. sorted(Map.Entry.comparingByValue(Comparator.reverseOrder())) 


170. summary of collection?  
    use class `IntSummaryStatistics`

171. thread staration?  
    If a thread is not granted CPU time because other threads grab it all, and thread remains in waiting for long it is starvation.

172. service registry/discovery?
173. Java is pass-by-value or pass-by-reference?
174. How to bring back thread from dead/terminated state?
175. remove duplicates from array without collection classes?

    ```
    for (int i = 0; i < noOfUniqueElements; i++) {
        for (int j = i + 1; j < noOfUniqueElements; j++) {
            if (arrayWithDuplicates[i] == arrayWithDuplicates[j]) {
                arrayWithDuplicates[j] = arrayWithDuplicates[noOfUniqueElements - 1];
                noOfUniqueElements--;
                j--;
            }
        }
    }
    ```

176. How to remove embedded tomcat from spring?
177. How to call stored procedure from spring?  
- Map a Stored Procedure Name Directly  
    ```
    @Procedure
    int GET_TOTAL_CARS_BY_MODEL(String model);
    ```  
    ```
    @Procedure("GET_TOTAL_CARS_BY_MODEL")
    int getTotalCarsByModel(String model);
    ```  
    ```
    @Procedure(procedureName = "GET_TOTAL_CARS_BY_MODEL")
    int getTotalCarsByModelProcedureName(String model);
    ```  
    ```
    @Procedure(value = "GET_TOTAL_CARS_BY_MODEL")
    int getTotalCarsByModelValue(String model);
    ```  
- Reference a Stored Procedure Defined in Entity  
    ```
    @Entity
    @NamedStoredProcedureQuery(name = "Car.getTotalCarsbyModelEntity", 
    procedureName = "GET_TOTAL_CARS_BY_MODEL", parameters = {
        @StoredProcedureParameter(mode = ParameterMode.IN, name = "model_in", type = String.class),
        @StoredProcedureParameter(mode = ParameterMode.OUT, name = "count_out", type = Integer.class)})
    public class Car {
        // class definition
    }
    ```
    ```
    @Procedure(name = "Car.getTotalCarsbyModelEntity")
    int getTotalCarsByModelEntiy(@Param("model_in") String model);
    ```
- Reference a Stored Procedure with @Query Annotation
    ```
    @Query(value = "CALL FIND_CARS_AFTER_YEAR(:year_in);", nativeQuery = true)
    List<Car> findCarsAfterYear(@Param("year_in") Integer year_in);
    ```  

178. Difference between Abstract class and interface after java 8?

| Parameter | Interface | Abstract class |
| --------- | --------- | -------------- |
| Fields | by default public, static, final. do not support non-static, non-final | supports private, protected, public fields, also non-static and non-final |
| methods | supports default, static, abstract methods. don't support final methods | supports final, non-final, static, non-static,  abstract. default methods not allowed |
| Constructors | not allowed | any no. of constructors allowed |
| Multiple inheritence | class can implement multiple interface | class can not implement multiple abstract classes |

179. @Entity vs @Table  
    `
    @Entity(name = "someThing") => this name will be used to name the Entity also table in DB if @Table missing`  
    `@Table(name = "someThing")  => this name will be used to name a table in DB
    `

180. Transactions in NoSQL?  
    Operation on single document is atomic, but for distributed transactions like multiple operation, collections, databases, documents, shards etc use callback API.   
- start transaction -  
    `final ClientSession clientSession = client.startSession();`  
- Optional. Define options to use for the transaction.  
- Define the sequence of operations to perform inside the transactions.   

    ```
    TransactionBody txnBody = new TransactionBody<String>() {
        public String execute() {
            MongoCollection<Document> coll1 = client.getDatabase("mydb1").getCollection("foo");
            MongoCollection<Document> coll2 = client.getDatabase("mydb2").getCollection("bar");
            /*
            Important:: You must pass the session to the operations.
            */
            coll1.insertOne(clientSession, new Document("abc", 1));
            coll2.insertOne(clientSession, new Document("xyz", 999));
            return "Inserted into collections in different databases";
        }
    };
    ```

- Use .withTransaction() to start a transaction,execute the callback, and commit (or abort on error)

    ```
    clientSession.withTransaction(txnBody, txnOptions);
    ```

181. Eventual consistency in DB?  
"eventual consistency" refers to the fact that in a replicated environment, when a write is finished on the primary, it will only be written to the secondaries *eventually*.   
OR  
Eventual consistency is a consistency model used in distributed computing to achieve high availability that informally guarantees that, if no new updates are made to a given data item, eventually all accesses to that item will return the last updated value.  
https://findanyanswer.com/how-does-mongodb-ensure-eventual-consistency  

*Collections in MongoDB is equivalent to the tables in RDBMS. Documents in MongoDB is equivalent to the rows in RDBMS. Fields in MongoDB is equivalent to the columns in RDBMS.*


182. LinkedBlockingQueue vs ArrayBlockingQueue?  

| LinkedBlockingQueue  | ArrayBlockingQueue |
| --------------------- | ----------------- | 
| Internally Uses Linked nodes to store | internally uses array |
| Optionally bounded queue | bounded queue |

* Jaeger - distributed tracing
* Kiali - Observability to service mesh
* prometheus - Data source,metrics-based monitoring system
* grafana - query, visualize, and alert metrics


183. Five classes help common special-purpose synchronization idioms.  
- `Semaphore` is a classic concurrency tool.  
Semaphores are often used to restrict the number of threads than can access some (physical or logical) resource.
- `CountDownLatch` is a very simple yet very common utility for blocking until a given number of signals, events, or conditions hold.  
A synchronization aid that allows one or more threads to wait until a set of operations being performed in other threads completes
- `CyclicBarrier` is a resettable multiway synchronization point useful in some styles of parallel programming.  
A synchronization aid that allows a set of threads to all wait for each other to reach a common barrier point. CyclicBarriers are useful in programs involving a fixed sized party of threads that must occasionally wait for each other. The barrier is called cyclic because it can be re-used after the waiting threads are released.
- `Phaser` provides a more flexible form of barrier that may be used to control phased computation among multiple threads.  
A reusable synchronization barrier, similar in functionality to CyclicBarrier and CountDownLatch but supporting more flexible usage.
- `Exchanger` allows two threads to exchange objects at a rendezvous point, and is useful in several pipeline designs.  
A synchronization point at which threads can pair and swap elements within pairs. Each thread presents some object on entry to the exchange method, matches with a partner thread, and receives its partner's object on return. An Exchanger may be viewed as a bidirectional form of a SynchronousQueue. Exchangers may be useful in applications such as genetic algorithms and pipeline designs.

**https://dzone.com/articles/all-about-the-singleton**

184. Bill Pugh Singleton ?  

```
public class BillPughSingleton {
    private BillPughSingleton() {
    }

    private static class SingletonHelper {
        private static final BillPughSingleton INSTANCE = new BillPughSingleton();
    }

    public static BillPughSingleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```

185. Enum Singleton?

```
public enum SingletonEnum {
	INSTANCE;
}
```

186. Pessimistic Locking vs. Optimistic Locking?  
    *Optimistic Locking* - optimistic locking is based on **detecting changes on entities** by checking their version attribute. If any concurrent update takes place, OptmisticLockException occurs.  
    *Pessimistic locking* - pessimistic locking mechanism involves **locking entities on the database level**. Each transaction can acquire a lock on data. As long as it holds the lock, no transaction can read, delete or make any updates on the locked data  
* PESSIMISTIC_READ  
* PESSIMISTIC_WRITE  
* PESSIMISTIC_FORCE_INCREMENT




187. second level cache in hibernate?  
First level cache works with `session`-scoped object  
second level cache works with `SessionFactory`-scoped  
Enabling Second-Level Caching -  
```
hibernate.cache.use_second_level_cache=true
hibernate.cache.region.factory_class=org.hibernate.cache.ehcache.EhCacheRegionFactory
```
Annotate entity with `@Cacheable`, collections(association objets) are not `cacheable` by default.

188. Why HTTPS and why it makes communication secure?  
    Https uses TLS(SSL) to encrypt normal request and response.
    SSL eventually evolved as TLS. server initiates and handshake with client by sending public key, and private key is stored at server, the data sent by client is encrypted using public key and server decrypt using private key.

189. OPTIONS http method?  
    exposes what other methods are supported by the web server, preflight request. It is required when you submit requests across different origins.


190. CORS? -> @CrossOrigin on method,.. on controller to enable all origin  
CORS(Cross Origin Resource Sharing) was implemented due to the limitations of the single-origin policy. The same-origin policy restricts resources to interact only with resources located in the same domain.  
The host that serves the JS (e.g. example.com) is different from the host that serves the data (e.g. api.example.com). In such a case, CORS enables cross-domain communication.  

* disable CSRF when we have non-browser clients  

191. How to check if there is loop in linked list?

192. How to maintain user session in microservices?  
    Redis  

193. String.valueOf(Object) vs Object::toString()?  
String.valueOf(Object) - returns "*null*"(string type null), defined as *static* method in String class, more safe.  
Object::toString() - throws *NullPointerException*, defined in String class as *non-static*, less safe as it causes NPE  

194. What are the possible states for a docker container?  
    created, restarted, running, paused, exited, dead  

195. checked Exception and unchecked exception?
* checked Exception - Exception and it's subclasses excluding RuntimeException are all chekced exception  
Java verifies checked exceptions at compile-time  
Example - IOException, SQLException, ClassNotFoundException

* unchecked Exception - RuntimeException and its subclasses are unchecked/runtime exception.  
Java does not verify unchecked exceptions at compile-time  
Example -  ArithmaticException, ClassCastException, ArrayStoreException

196. Future vs CompletableFuture?  
    Furure's get() is blocking operation and there is no means to complete this blocking operation whereas in CompletableFuture we can complete() task/operation manually.  
    Multiple Future can be combined using CompletableFuture.


197. what is ACID?
198. can we overload/override static method?  
we can not override the static method in Java, but we can certainly overload a static method in Java.

199. Detect and Remove Loop in a Linked List?
200. How to read 100GB JSON file?  
    https://all-aboutl.blogspot.com/2012/06/how-to-split-large-files-into-smaller.html  
    https://www.admios.com/blog/how-to-split-a-file-using-java

201. Maximum profit by buying and selling a share at most twice  
https://www.geeksforgeeks.org/maximum-profit-by-buying-and-selling-a-share-at-most-twice/?ref=lbp

202. Transaction Isolation Levels in DBMS?  
A transaction isolation level is defined by the following phenomena  
* dirty read
* non repeatable read  
* Phantom read  

The SQL standard defines four isolation levels  
    1. read uncommitted  
    2. read committed  
    3. Repeatable read  
    4. Serializable

203. exceute() vs executeQuery vs executeUpdate?  
> *boolean execute():* Executes the SQL statement in this Prepared Statement object, which may be any kind of SQL statement.

> *ResultSet executeQuery():* Executes the SQL query in this Prepared Statement object and returns the ResultSet object generated by the query.

> *int executeUpdate()*: Executes the SQL statement in this Prepared Statement object, which must be an SQL INSERT, UPDATE or DELETE statement; or an SQL statement that returns nothing, such as a DDL statement.  

204. Prototype design pattern? 
    If object creation is costly process then we make use of clone method to clone existing object.  

205. JVM Architecture?  
https://dzone.com/articles/jvm-architecture-explained

* *Avoid calling abstract methods in your abstract classes’ constructors, as it restricts how those abstract methods can be implemented.*
**https://www.toptal.com/java/interview-questions**

206. How LinkedHashMap maintains insertion order?
207. Can FunctionalInterface extends another interface?  
    yes, without any method in parent interface.

208. Why is catching a RuntimeException not considered a good programming practice?  
Catching any of these general exceptions (including Throwable) is a bad idea because it means you're claiming that you understand every situation which can go wrong, and you can continue on despite that.  
209. How to Get All Endpoints List After Startup, Spring Boot?  
```
@Component
public class EndpointsListener implements ApplicationListener<ContextRefreshedEvent> {

    @Override
    public void onApplicationEvent(ContextRefreshedEvent event) {
        ApplicationContext applicationContext = event.getApplicationContext();
        applicationContext.getBean(RequestMappingHandlerMapping.class).getHandlerMethods()
             .forEach(/*Write your code here */);
    }
}
```

alternatively we can use actuators /endpoint

210. Transaction propogation types?  
* REQUIRED
    - if calling service has transaction then method makes use of existing transaction.
    - If calling service does not have transaction then method will create new transaction.
* SUPPORTS
    - If calling service has transaction then method makes use of existing transaction.
    - If calling service does not have transaction then method will not create new transaction.(run without transaction)
* NOT_SUPPORTED
    - If calling service service method has transaction then method *doesnot make use of existing transaction neither does it creates its own transaction.It run without transaction*
    - If caling service method does not have transaction then method *will not create new transaction and run without transaction.*
* REQUIRES_NEW
    - If calling service method has transaction then method *doesnot make use of existing transaction but creates new transaction.*
    - If calling service method does not have transaction then method will create new transaction
* NEVER
    - If calling service method has transaction then method throws exception
    - If calling service method does not have transaction the method will not create a new one and run without transaction. 
* MANDATORY
    - If calling service method has transaction then method makes use of existing transaction.
    - If calling service method does not have transaction, method will throw exception

211. stub vs mock?  
Object's behaviour not tested in stub whereas behaviour tested in mock.  
For example, for an empty stack, you can create a stub that just returns true for empty() method. So, this does not care whether there is an element in the stack or not.  
Whereas in Mock object is initialized with data and behaviour is tested.
212. serverless vs containers?

213. Why to avoid substraction while comparing two integers using comparator?  
    https://www.baeldung.com/java-comparator-comparable#avoid-subtraction

214. What is the difference between transient and volatile variable in Java?  
    **Volatile:** The volatile modifier tells the JVM that writes to the field should always be synchronously flushed to memory, and that reads of the field should always read from memory. This means that fields marked as volatile can be safely accessed and updated in a multi-thread application without using native or standard library-based synchronization.  
    **Transient:** The transient modifier tells the Java object serialization subsystem to exclude the field when serializing an instance of the class. When the object is then deserialized, the field will be initialized to the default value; i.e. null for a reference type, and zero or false for a primitive type.

215. How to make an ArrayList read only in Java?
    use `Collections.unmodifiableList()` to make list as read only.

216. How does HashMap handle collisions in java?  
    Prior to Java 8, HashMap and all other hash table based Map implementation classes in Java handle collision by chaining, i.e. they use linked list to store map entries which ended in the same bucket due to a collision. If a key end up in same bucket location where an entry is already stored then this entry is just added at the head of the linked list there. In the worst case this degrades the performance of the get() method of HashMap to O(n) from O(1). In order to address this issue in the case of frequent HashMap collisions, Java 8 has started using a balanced tree instead of linked list for storing collided entries. This also means that in the worst case you will get a performance boost from O(n) to O(log n).  
    The threshold of switching to the balanced tree is defined as TREEIFY_THRESHOLD constant in java.util.HashMap JDK 8 code. Currently, it's value is 8, which means if there are more than 8 elements in the same bucket than HashMap will use a tree instead of linked list to hold them in the same bucket.

217. scope of variables in stram? effectively final??
218. Scenario, you must create a cache thread pool using executor framework, but you don’t know the capacity or number of threads needed to achieve that task. how will you determine how much threads you will need based on your requirement? is there any mechanism in executor’s framework to know the capacity of it.?  

219. Microservices Communication Design Patterns?  
    1. One-to-one interactions  
        > synchronous  
        > Asynchronous  
        > one way notification  
    2. one-to-many interaction
    
220. having vs group by?


    
Istio service mesh  
nginx - load balancer/kubernetes networking  
Ingress - is an object that allows access to your Kubernetes services from outside the Kubernetes cluster.  
custom CRD - 
VirtualServices - 
```
   - match:
        - uri:
            prefix: /api/storage/
      route:
        - destination:
            host: storage
            port:
              number: 8080
    - match:
        - uri:
            prefix: /api/search/
      route:
        - destination:
            host: search-service
            port:
              number: 8080
```              
    
AuthorizationPolicies




# Hibernate

https://javatechonline.com/entity-relationship-in-jpa-hibernate-orm/

1. sessionFactory? -> immutable, thread-safe, single instance per application  

2. get()? vs load()?  

    |   get   |   load   |
    | ------- | --------- |
    | always hit DB and return real object | returns proxy object  |
    | return null if data not found | return exception if data not found |
    | used when we are not sure data exists or not | used when we are sure data exists |  

3. save() vs persist()?
    
    | save      |  persist  |
    | --------- | --------- |
    | return serialized object | return void  |
    | can be invoked outside transaction | invke inside transation |

4. *`EntityManagerFactory` instances and, consequently, Hibernate's `SessionFactory` instances, are thread-safe whereas `EntityManager` instances are not thread-safe and are meant to be used in thread-confined environments*
5. what is LazyInitializationException and how to solve?  
    Hibernate throws the `LazyInitializationException` when it needs to initialize a lazily fetched association to another entity without an active session context  

    ```
    EntityManager em = emf.createEntityManager();
    em.getTransaction().begin();
    
    TypedQuery<Author> q = em.createQuery(
            "SELECT a FROM Author a",
            Author.class);
    List<Author> authors = q.getResultList();
    em.getTransaction().commit();
    em.close();
    
    for (Author author : authors) {
        List<Book> books = author.getBooks();
        log.info("... the next line will throw LazyInitializationException ...");
        books.size();
    }
    ```
    How to fix the LazyInitializationException?  
    https://thorben-janssen.com/lazyinitializationexception/  
    load the entity with all required associations in one query.
    1. Initializing associations with a LEFT JOIN FETCH clause
    ```
    EntityManager em = emf.createEntityManager();
    em.getTransaction().begin();
    
    TypedQuery<Author> q = em.createQuery("SELECT a FROM Author a LEFT JOIN FETCH a.books", Author.class);
    List<Author> authors = q.getResultList();
    
    em.getTransaction().commit();
    em.close();
    
    for (Author a : authors) {
        log.info(a.getName() + " wrote the books "
            + a.getBooks().stream().map(b -> b.getTitle()).collect(Collectors.joining(", "))
        );
    }
    ```
    2. Use a @NamedEntityGraph to initialize an association  
    3. Using a DTO projection  



6. pitfalls of @Transactional?  

    ```
    @Transactional
    public void changeName(long id, String name) {
        User user = userRepository.getById(id);
        user.setName(name);
        userRepository.save(user);
    }
    ```
    entities retrieved within transaction are in the managed state, which means that all changes made to them will be populated to the database automatically at the end of the transaction.  
    save() call is redundant in above case.  

7. Hibernate Inheritance strategies?  
    1. Table per hierarchy(Single Table Strategy)
    2. Table per concrete class
    3. Table per subclass(Joined Table Strategy)

8. How to make an class immutable in Hibernate? -> mutable=false

9. What is autamatic dirty checking in Hibernate?  
if object is modified in the transaction, then its state will be updated automatically when you committ the transaction.

10. What is Query cache in Hibernate?  
    need to enable queryCache for specific query, helps in performance improvement, caches query result.
    `hibernate.cache.use_query_cache=true`
11. What is cascading in Hiberante and can you list types of cascading?  
    ALL, DEATCH, MERGE, PERSIST, REMOVE, REFRESH
12. What is the N+1 SELECT problem in Hibernate?  
    The N+1 SELECT problem is a result of lazy loading and load on demand fetching strategy. In this case, Hibernate ends up executing N+1 SQL queries to populate a collection of N elements.
13. What are some strategies to solve the N+1 SELECT problem in Hibernate?  
    1. pre-fetching in batches, will reduce the N+1 problem to N/K + 1 problem where  K is the size of the batch
    2. subselect fetching strategy
    3. disabling lazy loading

14. What is difference between merge() and update() methods in Hibernate?  
    Both update() and merge() methods in hibernate are used to convert the object which is in *detached state into persistence state*.  

15. JPA Cascade Type?
    > ALL - propagates all operations — including Hibernate-specific ones — from a parent to a child entity.  
    > PERSIST - propagates the persist operation from a parent to a child entity. When we save the person entity, the address entity will also get saved.  
    > MERGE - propagates the merge operation from a parent to a child entity.  
    > REMOVE - propagates the remove operation from parent to child entity.  
    > REFRESH - reread the value of a given instance from the database.  
    > DETACH - When we use CascadeType.DETACH, the child entity will also get removed from the persistent context.  

16. Hibernate Cascade Type?
REPLICATE
SAVE_UPDATE
LOCK



# Design pattern
https://javatechonline.com/design-patterns-in-java-3/
https://rahulmitt.github.io/interviewpedia/?course=dp

*Factory Design pattern*
The Factory design pattern is used when we have a super class with multiple sub-classes and based on input, we need to return one of the sub-classes. This pattern takes out the responsibility of instantiation of a class from a client program to the factory class.  
```
    public interface IMobile {
        public void cost();
        public void pictureCapacity();
        public void batteryPower();
    }
```
```
    public class Lenovo implements IMobile {
        ...
    }

    public class Samsung implements IMobile {
        ...
    }

    public class MobileFactory {
        public MobileFactory(){
        }

        IMobile createMobile(String type){

            IMobile mob=null;
            if("len".equalsIgnoreCase(type)){
                mob=new Lenovo();
                System.out.println("Lenovo created");
            }else if("sam".equalsIgnoreCase(type)){
                mob=new Samsung();
                System.out.println("Samsung created");
            }
            return mob;
        }
    }
```
> Factory pattern offers an approach to code for interface rather than implementation.

*Abstract Factory Pattern* - Factory of factories

*Builder pattern* - 
The intent of the Builder Pattern is to separate the construction of a complex object from its representation, so that the same construction process can create different representations.  







# CouchDB
https://www.javatpoint.com/couchdb-interview-questions

What are the main features of CouchDB?  

1. **JSON Documents:** CouchDB stores data in JSON document.  
2. **RESTful Interface:** CouchDB does all tasks like replication, data insertion, etc. via HTTP.  
3. **N-Master Replication:** CouchDB facilitates you to make use of an unlimited amount of 'masters,' making for some very interesting replication topologies.  
4. **Built for Offline:** CouchDB can replicate to devices (like Android phones) that can go offline and handle data sync for you when the device is back online.  
5. **Replication Filters:** CouchDB facilitates you to filter precisely the data you wish to replicate to different nodes.  
6. **ACID semantics:** The CouchDB file layout follows all the features of ACID properties. Once the data is entered into the disc, it will not be overwritten. Document updates (add, edit, delete) follow Atomicity, i.e., they will be saved completely or not saved at all. The database will not have any partially saved or edited documents. Almost all of these update are serialized, and any number of clients can read a document without waiting and without being interrupted.  
7. **Document storage:** CouchDB is a NoSQL database which follows document storage. Documents are the primary unit of data where each field is uniquely named and contains values of various data types such as text, number, Boolean, lists, etc. Documents don't have a set limit to text size or element count.
8. **Eventually consistency:** CouchDB guarantees to provide availability and partition tolerance.
9. **Authentication and Session Support:** CouchDB facilitates you to keep authentication open via a session cookie like a web application.
10. **Security:** CouchDB also provides database-level security. The permissions per database are separated into readers and admin. Readers can both read and write to the database.
11. **Validation:** You can validate the inserted data into the database by combining with authentication to ensure the creator of the document is the one who is logged in.
12. **Map/Reduce List and Show:** The main reason behind the popularity of MongoDB and CouchDB is map/reduce system.





***Spring Security***

* How to get user details in spring security?
    ```
    UserDetails userDetails = (UserDetails)SecurityContextHolder.getContext().getAuthentication().getPrincipal(); 
    ```  

* How to secure a method in spring security using annotation?
    ```
    @Secured("ROLE_ADMIN")
    public void deleteUser(String name); 
    ```

* How to pre-authorize and post-authorize a method in spring security?  
    ```
    @PreAuthorize ("hasRole('ROLE_WRITE')")
    public void addBook(Book book);
    @PostAuthorize ("returnObject.owner == authentication.name")
    public Book getBook(); 

    ```
    returnObject is built-in keyword provided by spring security.  

* @Secured vs @RolesAllowed vs @PreAuthorize?  
    @Secured and @RolesAllowed are the same the only difference is @RolesAllowed is a standard annotation (i.e. not only spring security) whereas @Secured is spring security only.  
    @secured - `@EnableGlobalMethodSecurity(securedEnabled=true)`  
    @RolesAllowed - `@EnableGlobalMethodSecurity(jsr250Enabled=true)`

    @PreAuthorize is different in a way that it is more powerful then the other two. It allows for SpEL expression for a more fine-grained control.  
    @preAuthorize - `@EnableGlobalMethodSecurity(prePostEnabled=true)`

* What is the role of @PreFilter and @PostFilter in spring security?  
* How to enable method level security in spring?  
    `@EnableGlobalMethodSecurity(securedEnabled=true, prePostEnabled=true)`

* What are the new annotations introduced in spring 4 security for JUnit test?  
    `@WithMockUser and @WithUserDetails`
    ```
    @Test 
    @WithMockUser(username = "ram", roles={"ADMIN"})
    public class SpringSecurityTest {} 
    ```    

* Which filter class is needed for spring security?  
    ```
    import org.springframework.security.web.context.AbstractSecurityWebApplicationInitializer;
    public class SecurityInitializer extends AbstractSecurityWebApplicationInitializer {
    } 
    ```    

* What is minimum web.xml configuration to run Spring MVC?  
    - define DispatcherServlet
    - define contextConfigLocation
    - define ContextLoaderListener

    ```
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/dispatcher-servlet.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener> 

    ```

* How to handle views in Spring MVC using XML?  
    configure InternalResourceViewResolver  
    ```
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/pages/"/>
    <property name="suffix" value=".jsp"/> 
    </bean> 
    ```    

* How to configure DispatcherServlet without web.xml in Spring MVC?  
    - Implement WebApplicationInitializer interface
    - override onStartup()  
    ```
    public class WebAppInitializer implements WebApplicationInitializer {
    public void onStartup(ServletContext servletContext) throws ServletException {
            AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();  
            ctx.register(AppConfig.class);  
            ctx.setServletContext(servletContext);    
            Dynamic dynamic = servletContext.addServlet("dispatcher", new DispatcherServlet(ctx));  
            dynamic.addMapping("/");  
            dynamic.setLoadOnStartup(1);  
        }  
    } 
    ```

* How to define Spring MVC view in @Configuration class without spring XML?  
    ```
    @Bean  
    public UrlBasedViewResolver setupViewResolver() {  
        UrlBasedViewResolver resolver = new UrlBasedViewResolver();  
        resolver.setPrefix("/views/");  
        resolver.setSuffix(".jsp");  
        resolver.setViewClass(JstlView.class);
        return resolver;  
    } 
    ```

* @RestController = @Controller + @ResponseBody

* Hystrix?  
    Hystrix is a latency and fault tolerance library designed to isolate points of access to remote systems, services and 3rd party libraries, stop cascading failure and enable resilience in complex distributed systems where failure is inevitable.  

* What is Hystrix Circuit Breaker? Need for it?  
    In case of any exception in exposed service fallback method is defined to return default value.  

* What are different ways to configure a class as Spring Bean?
    1. XML based  
        ```
        <bean name="myBean" class="com.journaldev.spring.beans.MyBean"></bean>
        ```
    2. Annotation based 
    3. Java based   
    ```
    @Configuration
    @ComponentScan(value="com.journaldev.spring.main")
    public class MyConfiguration {
        @Bean
        public MyService getService(){
            return new MyService();
        }
    }
    ```

*Spring boot eliminates all above config* 

* Spring autowiring?  
1. *no:* It’s the default autowiring mode. It means no autowiring.
2. *byName:* The byName mode injects the object dependency according to name of the bean. In such a case, the property and bean name should be the same. 
3. *byType:* The byType mode injects the object dependency according to type. So it can have a different property and bean name. 
4. *constructor:* The constructor mode injects the dependency by calling the constructor of the class.
5. *autodetect:* In this mode, Spring first tries to autowire by the constructor. If this fails, it tries to autowire by using byType.

*Hateoas*: Hypermedia as engine of application state, used to build links to other methods/api.  
```
Resource<User> resource = new Resource<User>(user);
ControllerLinkBuilder linkTo = 
		linkTo(methodOn(this.getClass()).retrieveAllUsers())
```
@RequestHeader - to read particular header  
LocaleContextHolder.getLocale() - read locale from request(Accept-Language)

* How to filter Json values/properties dynamically? -> MappingJacksonValue

* To use data.sql, you need to add this to application.properties -  
spring.jpa.defer-datasource-initialization=true

latest spring cloud changes-(springboot v2.5.0)  
Spring cloud load balancer instead ribbon  
Spring cloud gateway instead of zuul  
Resilience4j instead of hystrix  

* Spring Config Server
    spring.config.import





***Design patterns***
* SOLID Design principles
    1. Single responsibility principle - one class has one responsibility.
    2. Open-Closed principle - specification, open for extension closed for modification.i.e add new feature only by adding new code.
    3. Liskov Substitution principle - Subtypes must be substitutable for their base types.
    4. Interface-segregation principle - Clients should not be forced to depend on methods that they do not use, recommends split interface into smaller interfaces.
    5. Depedency Inversion Principle - 

* Builder design pattern -  
     construct a complex object through a step-by-step process.

* Top 10 Object oriented design principles
    1. DRY(Don't repeat yourself) - avoid duplication in code.
    2. Encapsulate what changes - hides implementation details, helps in maintainance.
    3. Open-Closed principle - open for extension closed for modification
    4. SRP(Single Responsibility Principle) - one class should do one thing and do it well.
    5. DIP(Dependency Inversion Principle) - don't ask, let framework give to you.
    6. Favor composition over inheritance - code reuse without cost of inflexibility.
    7. LSP(Liskov substitution Principle) - Subtype must be substitutable for super type.
    8. ISP(Interface Segregation Principle) - Avoid monolithic interface, reduce pain on client side
    9. Programming for interface - helps in maintainance,improves flexibility
    10. Delegation principle - Don't do all things by yourself,delegate it.











        
    
    
    


======================================================
``` Shell Scripting
ksh - korn shell
bash - bourne again shell
sh - bourne shell

#!/bin/bash - bash is in bin folder
dd - to delete line
echo - print
chmod +x "file_name" or chmod 755 "file_name" or chmod +x *
ls -l
```

