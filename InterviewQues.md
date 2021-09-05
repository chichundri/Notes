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
    ``` <global-method-security secured-annotations="enabled", pre-post-annotations="enabled"/> ```
20. How to enable web security using java configuration in spring?
    ``` @Configuration
        @EnableWebSecurity
        public class SecurityConfig extends WebSecurityConfigurerAdapter {} ```
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
    a. Stateless Implementation - Method should neither relies on external state nor maintain state at all.Declare all variiables in method only.
    example - factorial program
    b. Immutable class
    c. Thread local fields - we can create thread-safe classes that don’t share state between threads by making their fields thread-local.
        ```
        public class ThreadA extends Thread {
            private final List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
            @Override
            public void run() {
                numbers.forEach(System.out::println);
            }
        }
        ```
    d. Synchronized collections - Collections.synchronizedCollection(new ArrayList<>());
    e. Concurrent Collections - Map<String,String> concurrentMap = new ConcurrentHashMap<>();
    f. Atomic objects - AtomicInteger, AtomicLong, AtomicBoolean, and AtomicReference
    g. Synchronized methods
    h. Synchronized blocks
    i. volatile fileld 
    j. Extrinsic locking - it uses an external entity to enforce exclusive access to the resource. synchronized methods and blocks
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
    k. Reentrant lock - preventing queued threads from suffering some types of resource starvation
    l. Read/Write lock
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
    ```
    public final class Singleton {
    private static volatile Singleton instance = null;
    private Singleton() {
    }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
        }
    }
    ```
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

