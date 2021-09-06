1. How to protect singleton from reflection api? ->
2. How to protect singleton from deserialization? - declare method as static as static members are not part of serialization.
3. How to create Doubleton?
4. collection vs stream
5. How to reverse string -> a)using reverse on SB b)iterative method c)recursion
6. Remove duplicate element from arraylist
7. notify() vs notifyAll()
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
    composition- belongs to, strong kind if has-a relationship, the objects lifecycles are tied. 
    It means that if we destroy the owner object, its members also will be destroyed with it. Example-building and room
    aggregation - has-a relationship,the lifecycles of the objects aren't tied: every one of them can exist independently of each other.Example-car and wheels
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
    | Shallow Copy stores the references of objects to the original memory address | Deep copy stores copies of the object’s value |
    | Shallow Copy reflects changes made to the new/copied object in the original object | Deep copy doesn’t reflect changes made to the new/copied object in the original object |
    | Shallow Copy stores the copy of the original object and points the references to the objects | Deep copy stores the copy of the original object and recursively copies the objects as well |
    | Shallow copy is faster | Deep copy is comparatively slower |
        
50. How to create immutable class in java
    1. Declare the class as final so it can’t be extended.
    2. Make all fields private so that direct access is not allowed.
    3. Don’t provide setter methods for variables.
    4. Make all mutable fields final so that its value can be assigned only once.
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

