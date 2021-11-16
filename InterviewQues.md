==== Links =====
**https://github.com/learning-zone/spring-interview-questions/blob/spring/microservices.md**
**https://github.com/in28minutes/spring-interview-guide**

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
    + To complete some of the intermediate operations, some state is to be maintained, and such intermediate operations are called stateful intermediate operations e.g sorted(), distinct()  
74.










        
    
    
    


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

