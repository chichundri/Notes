==== Links =====  
**https://github.com/learning-zone/spring-interview-questions/blob/spring/microservices.md**
**https://github.com/in28minutes/spring-interview-guide**

1. How to protect singleton from reflection api? ->
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
    * using reverse on SB 
    * iterative method(for loop, iterate from length-1)
    * recursion  
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

* Suppose you have 2 threads (Thread-1 and Thread-2) on same object. Thread-1 is in static synchronized
  method1(), can Thread-2 enter static synchronized method2() at same time in java?  
    No, here when Thread-1 is in static synchronized method1() it must be holding lock on class class’s object and will release lock on class’s class object only when it exits static synchronized method1(). So, Thread-2 will have to wait for Thread-1 to release lock on class’s class object so that it could enter static synchronized method2().  
    Likewise, Thread-2 even cannot enter static synchronized method1() which is being executed by Thread-1. Thread-2 will have to wait for Thread-1 to release lock on  class’s class object so that it could enter static synchronized method1().  

* Suppose you have 2 threads (Thread-1 on object1 and Thread-2 on object2). Thread-1 is in static 
    synchronized method1(), can Thread-2 enter static synchronized method2() at same time in java?  
    No, it might confuse you a bit that threads are created on different objects. But, not to forgot that multiple objects may exist but there is always one class’s class object lock available.  
    Here, when Thread-1 is in static synchronized method1() it must be holding lock on class class’s object and will release lock on class’s class object only when it exits static synchronized method1(). So, Thread-2 will have to wait for Thread-1 to release lock on class’s class object so that it could enter static synchronized method2().  
    Likewise, Thread-2 even cannot enter static synchronized method1() which is being executed by Thread-1. Thread-2 will have to wait for Thread-1 to release lock on  class’s class object so that it could enter static synchronized method1().  

* Suppose you have thread and it is in synchronized method and now can thread enter other synchronized
    method from that method in java? -> Yes  

* Suppose you have thread and it is in static synchronized method and now can thread enter other static 
    synchronized method from that method in java? -> Yes  

* Suppose you have thread and it is in static synchronized method and now can thread enter other non 
    static synchronized method from that method in java? -> Yes  

* Suppose you have thread and it is in synchronized method and now can thread enter other static
    synchronized method from that method in java? -> Yes  

* Does Thread implements their own Stack, if yes how? -> Yes
* When we implement Runnable interface, same object is shared amongst multiple threads, but when we 
    extend Thread class each and every thread gets associated with new object.   

* How can you ensure all threads that started from main must end in order in which they started and also 
    main should end in last? -> by calling join method on threads, in short it waits for thread to die on which thread has been called.  

* wait vs sleep?
    | wait      | sleep     |
    | -------   | -------   |
    | always called from synchronized block | need not to be called from synchronized block |
    | belongs to Object class | belongs to Thread class | 
    | called on object | called on thread  |
    | releases object lock | holds object lock |

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



8. how to swap two strings without using third variable/temp variable
9. Wildcard arguements? ->a) with unknown type b) with upper bound c) with lower bound.
10. can we use abstract and static together? -> No
11. Auto boxing and unboxing
12. spring cloud bus
13. What is Spring Cloud Data Flow?
14. Features of microservices?-> decoupling,componentization, autonomy,agility,decentralized governance
15. API gateway?
16. How to configure spring security?
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
    - Stateless Implementation - Method should neither relies on external state nor maintain state at all.Declare all variiables in method only.
    example - factorial program
    - Immutable class
    - Thread local fields - we can create thread-safe classes that don’t share state between threads by making their fields thread-local.
        ```
        public class ThreadA extends Thread {
            private final List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
            @Override
            public void run() {
                numbers.forEach(System.out::println);
            }
        }
        ```
    - Synchronized collections - Collections.synchronizedCollection(new ArrayList<>());
    - Concurrent Collections - Map<String,String> concurrentMap = new ConcurrentHashMap<>();
    - Atomic objects - AtomicInteger, AtomicLong, AtomicBoolean, and AtomicReference
    - Synchronized methods
    - Synchronized blocks
    - volatile fileld 
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
    @RequestParam - used to request parameter from URL.Below date is RequestParam
    http://localhost:8080/shop/order/{orderId}/receipts?date=12-05-2017
    @PathVariable - used to extract value from URI.Above exmaple orderId is pathvariable
    Jersey annotations - PathParam & QueryParam
24) Encapsulation - restricting direct access to variables
25) Abstraction - hiding internal implementation.
26) How to divide two numbers without using division (/) operator in java / How to find remainder of
    division of two numbers using minus (-) operator / Java program to find remainder of division using –
27) Restemplate vs WebClient?
28) @Secured vs @PreAuthorize?
29. Object class and methods of it? - equals, hascode, toString, wait(), wait(long,int), wait(long), notify, notifyAll, finalize, clone, getClass
30. What is aggregation and composition?
    composition- belongs to, strong kind of part-of relationship, the objects lifecycles are tied. 
    It means that if we destroy the owner object, its members also will be destroyed with it. Example-building and room, car and engine
    aggregation - has-a relationship, weak association, the lifecycles of the objects aren't tied: every one of them can exist independently of each other.Example- Organization and person, car and driver
31. when abstract class and interface are used? 
    classes that extends abstract class, have many common methods or fields(closely related classes-vehicle,car,volvoCar,OtherCar)
32. String immutability and reason? state of object remains constant after its creation, why?-caching, security, synchronization, performance,thread-safe
33. Instance lock(attached to single object) vs Static lock(attached to class)?
    Instance lock - Acquiring the instance lock only blocks other threads from invoking a synchronized instance method; it does not block other threads from 
    invoking an un-synchronized method, nor does it block them from invoking a static synchronized method
    (http://www.javapractices.com/topic/TopicAction.do?Id=35)
    example - synchronized(this) acquires the instance lock.
    static lock - Similarly, acquiring the static lock only blocks other threads from invoking a static synchronized method; it does not block other threads 
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
    Employee class should properly override equals() and hashCode() methods to store employee object as key in hashmap. Improper implementation of these
    methods results in wrong data 
37. string reverse without built in method?
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
39. Why Map is not the part of Collection interface?
    Each collection(list,set,queue) stores a single value whereas map stores key-value pair which itselfs is incompatible with collection methods like add,
    addAll, removeAll
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
47. can we override and overload main methods? Yes
48. ClassNotFoundException vs NoClassDefFoundError
    ClassNotFoundException - occurs when you try to load a class at runtime using Class.forName() or loadClass() methods and requested classes are not found in
    classpath. Checked exception. derived from java.lang.Exception
    NoClassDefFoundError - occurs when class was present during compile time and program was compiled and linked successfully but class was not present during
    runtime. It is error which is derived from LinkageError. occurs at runtime

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
51. HashMap working?
52. why wait and notify call in the synchronized block?(https://stackoverflow.com/questions/2779484/why-must-wait-always-be-in-synchronized-block)
    A wait() only makes sense when there is also a notify(), so it's always about communication between threads, and that needs synchronization to work 
    correctly. Mostly wait, notify, notifyAll used in inter thread communication 
    We must use synchronnization to avoid - 
    - IllegalMonitorStateException
    - Any potential race condition between wait and notify method in Java
    Example - producer-consumer
    https://javarevisited.blogspot.com/2011/05/wait-notify-and-notifyall-in-java.html#axzz75hpg9s9z
    
53. Producer-Consumer programm?https://www.geeksforgeeks.org/producer-consumer-solution-using-threads-java/
54. http://java-latte.blogspot.com/2014/04/Semaphore-CountDownLatch-CyclicBarrier-Phaser-Exchanger-in-Java.html
55. java concurrency - https://www.javacodegeeks.com/2015/09/the-java-util-concurrent-package.html
56. CountDownLatch- is used to make sure that a task waits for other threads before it starts. To understand its application, let us consider a server where 
    the main task can only start when all the required services have started.
    Working of CountDownLatch: When we create an object of CountDownLatch, we specify the number of threads it should wait for, all such thread are required to 
    do count down by calling CountDownLatch.countDown() once they are completed or ready to the job. As soon as count reaches zero, the waiting task starts 
    running.

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
    3. request - A single instance will be created and available during complete lifecycle of an HTTP request. Only valid in web-aware 
        Spring ApplicationContext.
    4. session - A single instance will be created and available during complete lifecycle of an HTTP Session. Only valid in web-aware 
        Spring ApplicationContext.
    5. application - A single instance will be created and available during complete lifecycle of ServletContext. Only valid in web-aware 
        Spring ApplicationContext.
    6. websocket - A single instance will be created and available during complete lifecycle of WebSocket. Only valid in web-aware Spring 
        ApplicationContext.
    **  application scoped bean is singleton per ServletContext, whereas singleton scoped bean is singleton per ApplicationContext. 
    Please note that there can be multiple application contexts for single application. **

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
61. SOAP vs REST?
62. What is Spring Cloud?
    system that provides integration with external systems.
    > Versioned and distributed configuration.
    > service discovery
    > service to service call
    > Routing
    > circuit breaker
    > load balancing
    > cluster state and leadership election
    > Global locks and distributed messaging

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
    5. ResponseBody -  tells a controller that the object returned is automatically serialized into JSON and passed back into the            HttpResponse object
    6. pathVariable - indicates method parameter should be bound to a URI template variable
        ```
        @RequestMapping(path="/{name}/{age}")
        public String getMessage(@PathVariable("name") String name, 
                @PathVariable("age") String age) {
            
            var msg = String.format("%s is %s years old", name, age);
            return msg;
        }
        ```
    7. ResponseEntity - Represents HTTP response, including headers, body, and status.`ResponseEntity` allows to add header and status       code whereas `@ResponseBody` puts values into response
    8. Qualifier - use to differentiate beans of same type
    9. Autowired - used to inject object dependency implicitly for a constructor, field or method
    10. Configurable - Used on classes to inject properties of domain objects
    11. Required - Used to mark class members that are mandatory
    12. ComponentScan - Make Spring scan the package for the @Configuration clases.
    13. Configuration - It is used on classes that define beans.
    14. Bean - ndicates that a method produces a bean which will be mananged by the Spring container.
    15. Lazy - Makes a @Bean or @Component to be initialized only if it is requested
    16. Value - t is used to inject values into a bean’s attribute from a property file.
    17. Resource - Annotation used to inject an object that is already in the Appl­ication Context.
    18. Primary - Annotation used when no name is provided telling Spring to inject an object of the annotated class first.
    19. Component - Generic stereotype annotation used to tell Spring to create an instance of the object in the Appl­ication Context
    20. Controller, Repository, Service
    21. Conditional - load beans into Application Context only if the given condition is met

    22. SpringBootApplication - used to qualify the main class for a Spring Boot project
    23. EnableAutoConfiguration - Based on class path settings, property settings, new beans are added by Spring Boot by using this           annotation.

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
67. How to implement Exception Handling in Spring Boot?
68. InitBinder?
# Java 8 
https://www.interviewbit.com/java-8-interview-questions/  
69. What are Java 8 streams?  
    A stream is an abstraction to express data processing queries in a declarative way. 
    represents a sequence of data objects & series of operations on that data is a data pipeline   
70. What are the main components of a Stream?  
    a. data source  
    b. Set of Intermediate Operations to process the data source  
    c. Single Terminal Operation that produces the result  
71. What are Intermediate and Terminal operations?  
72. What are the most commonly used Intermediate operations?  
    `Filter(Predicate<T>)` - Allows selective processing of Stream elements. It returns elements that are satisfying the supplied condition by the predicate.  
    `map(Funtion<T, R>)` - Returns a new Stream, transforming each of the elements by applying the supplied mapper function e.g sorted()  
    `distinct()` - Only pass on elements to the next stage, not passed yet.  
    `limit(long maxsize)` - Limit the stream size to maxsize.  
    `skip(long start)` - Skip the initial elements till the start.  
    `peek(Consumer)` - Apply a consumer without modification to the stream.  
    `flatMap(mapper)` - Transform each element to a stream of its constituent elements and flatten all the streams into a single stream.  
73. What is the stateful intermediate operation? Give some examples of stateful intermediate operations.  
    To complete some of the intermediate operations, some state is to be maintained, and such intermediate operations are called stateful intermediate operations e.g sorted(), distinct()  
74. common type of terminal operations?\
    `collect(), reduce(), count(), min(), max(), anyMatch(), noneMatch(), forEach(), forEachOrdered()`\
75. collection vs stream?\
76. What is the feature of the new Date and Time API in Java 8?\
    - Immutable classes and Thread-safe\
    - Timezone support\
    - Fluent methods for object creation and arithmetic\
    - Addresses I18N issue for earlier APIs\
    - All packages are based on the ISO-8601 calendar system\

77. Define Nashorn in Java 8

78.What is method reference?\
    A method reference is a Java 8 construct that can be used for referencing a method without invoking it.
    
    - Constructor refernce - String::new;\
    - Static method reference - ContainingClass::staticMethodName\
    - Bound instance method reference - str::toString\
    - Unbound instance method reference - String::toString;\

79. Functional Interfaces in the Standard Library?

    - Function – it takes one argument and returns a result
    - Consumer – it takes one argument and returns no result
    - Supplier – it takes no arguments and returns a result
    - Predicate – it takes one argument and returns a boolean
    - BiFunction – it takes two arguments and returns a result
    - BinaryOperator – it is similar to a BiFunction, taking two arguments and returning a result. The two arguments and the result are all of the same types.
    - UnaryOperator – it is similar to a Function, taking a single argument and returning a result of the same type

80. What Is a Functional Interface?\
    interface with one single abstract method.\
    Functional Interface can be used with lambda expression
    e.g Runnable, Comparable
81. default method?
82. lambda expression?
    lambda expression is a function that we can reference and pass around as an object.(Anonymous function)\
83. Final variable can be accessed in lambda expression in java 8? - yes
84. non-Final variable can be accessed in lambda expression in java 8?\ yes,but it is effectively final in lambda expression
Any attempt to modify x will produce compilation error
85. Instance and static variables are accessible and modifiable in lambda expression
86. Byte stream vs Character stream? (FileInputStream vs FileReader and 8-bit byte vs 16-bit unicode)
87. Runtime class vs System class?\
    Runtime class - provide access to java runtime system like memoery usage,available memory,invoking garbage collection\
    System class - provide access to system resources like standard input/output, current time in millis, terminating applications

88. assertions in java? -> testing the correctness of any assumptions.
89. Can we have multiple public classes in a java source file?
90. covarient return type?\ 
    It is possible to have different return type for a overriding method in child class, but child’s return type should be sub-type of parent’s return type. 

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

91. wrapper class?\
    convert primitive into object and object into primitive

92. Reflection API?\
    Java Reflection is the process of analyzing and modifying all the capabilities of a class at runtime. Reflection API in Java is used to manipulate class and its members which include fields, methods, constructor, etc. at runtime


93. What is the final variable, final class, and final blank variable?\
94. How will you invoke any external process in Java? using exec() method of Runtime class\ 
95. How many ways you can take input from the console?\
    - Buffered Reader Class
    - Scanner class
    - Console class - System.console().readLine();   

96. How can you avoid serialization in child class if the base class is implementing the Serializable interface?\
    In subclass override writeObject() method and throw NotSerializableException()  

97. Serializable vs Externalizable?\
98. What are the ways to instantiate the Class class?\
    - using new keyword
    - using Class.forName()
    - Using clone()
    - using deserialization
    - using newInstance() 

99. autoboxing and unboxing?\
    automatic conversion of primitive data types into its equivalent Wrapper type is known as boxing and opposite operation is known as unboxing
100. Difference between creating String as new() and literal?\
101. String vs StringBuffer vs StringBuilder?\

    - String is immutable where as StringBuffer and StringBuilder are mutable
    - StringBuffer is thread safe, StringBuilder is not thread safe

102. What is a Memory Leak? How can a memory leak appear in garbage collected language?\
    objects are no longer being used by the application, but the Garbage Collector is unable to remove them from working memory\
    tools to identify useless objects\

    - HP Openview
    - HP JMeter
    - JProbe
    - IBM Tivoli

103. Why String is popular HashMap key in Java?\
104. Error vs Exception?
105.  throw vs throws?\
    
    - throw - used in method body to throw an exception
    - throws - used in method signature to declare an exception that can occur in statement in method body
106. The difference between Serial and Parallel Garbage Collector?\

Serial Garbage Collector

Serial garbage collector works by holding all the application threads. It is designed for the single-threaded environments. It uses just a single thread for garbage collection. The way it works by freezing all the application threads while doing garbage collection may not be suitable for a server environment. It is best suited for simple command-line programs.

Turn on the -XX:+UseSerialGC JVM argument to use the serial garbage collector.

Parallel Garbage Collector

Parallel garbage collector is also called as throughput collector. It is the default garbage collector of the JVM. Unlike serial garbage collector, this uses multiple threads for garbage collection. Similar to serial garbage collector this also freezes all the application threads while performing garbage collection.

107. What is difference between WeakReference and SoftReference in Java?  

    - Strong reference: This is the default type/class of Reference Object.
    `MyClass obj = new MyClass();`
    - Weak reference: Weak Reference Objects are not the default type/class of Reference Object and they should be explicitly specified while using them.
    - Soft References: In Soft reference, even if the object is free for garbage collection then also its not garbage collected, until JVM is in need of memory badly.
    - Phantom References: The objects which are being referenced by phantom references are eligible for garbage collection. But, before removing them from the memory, JVM puts them in a queue called ‘reference queue’.

108. What is a compile time constant in Java? What is the risk of using it?
    a constant whose value known at compile time and compile replaces constant name with value everywhere in code.  
    `private final int x = 10;`

109.  How bootstrap class loader works in java?  
    - Bootstrap class loader  
    - Extension class loader  
    - System class loader  

110. While overriding a method can you throw another exception or broader exception?
    If a method declares to throw a given exception, the overriding method in a subclass can only declare to throw that exception or its subclass. This is because of polymorphism.

111. checked, unchecked exception and errors?  
    - Checked Exception: These are the classes that extend Throwable except RuntimeException and Error, Example: IOException, SQLException
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
    Initialized with given count and can not reset.

114. CyclicBarrier?  
    2 or more threads wait for each other to reach a common barrier point. When all threads have reached common barrier point.  
    * All waiting threads are released  
    * Event can be triggered as well.  
    Count can be reset hence called cyclic barrier  

115. How to use Exchanger for sharing Object between Threads in Java?  
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

122. 











        
    
    
    


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

