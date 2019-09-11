
1. How to protect singleton from reflection api? -> using enum as it dont have constructor
2. How to protect singleton from deserialization? -> implement read resolve in singleton class 
   How to protect singleton from cloning? -> override clone method and throw new CloneNotSupportedException
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
    -> @Secured("ROLE_ADMIN")
       public void deleteUser(String name);
18. How to pre-authorize and post-authorize a method in spring security? -> @PreAuthorize ("hasRole('ROLE_WRITE')") and 
    @PostAuthorize ("returnObject.owner == authentication.name")
19. How to enable method level security in spring?
    -> <global-method-security secured-annotations="enabled", pre-post-annotations="enabled"/>
20. How to enable web security using java configuration in spring?
    ->  @Configuration
        @EnableWebSecurity
        public class SecurityConfig extends WebSecurityConfigurerAdapter {}
21. How to configure DispatcherServlet without web.xml in Spring MVC?
    ->  public class WebAppInitializer implements WebApplicationInitializer {
         public void onStartup(ServletContext servletContext) throws ServletException {  
              AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();  
              ctx.register(AppConfig.class);  
              ctx.setServletContext(servletContext);    
              Dynamic dynamic = servletContext.addServlet("dispatcher", new DispatcherServlet(ctx));  
              dynamic.addMapping("/");  
              dynamic.setLoadOnStartup(1);  
         }  
        }
22. How to achieve thread safety in java?
    a) Stateless Implementation - Method should neither relies on external state nor maintain state at all.Declare all variiables
    in method only.
        example - factorial program
    b) Immutable class
    c) Thread local fields - we can create thread-safe classes that don’t share state between threads by making their fields 
        thread-local.
        public class ThreadA extends Thread {
            private final List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
            @Override
            public void run() {
                numbers.forEach(System.out::println);
            }
        }
    d) Synchronized collections - Collections.synchronizedCollection(new ArrayList<>());
    e) Concurrent Collections - Map<String,String> concurrentMap = new ConcurrentHashMap<>();
    f) Atomic objects - AtomicInteger, AtomicLong, AtomicBoolean, and AtomicReference
    g) Synchronized methods
    h) Synchronized blocks
    i) volatile fileld 
    j) Extrinsic locking - it uses an external entity to enforce exclusive access to the resource. synchronized methods and blocks
    rely on the this reference.
        Example - 
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
    k) Reentrant lock - preventing queued threads from suffering some types of resource starvation
    l) Read/Write lock
23) RequestParam vs Pathvariable?
    @RequestParam - used to get request parameter from URL.Below date is RequestParam
    http://localhost:8080/shop/order/{orderId}/receipts?date=12-05-2017
    @PathVariable - used to extract value from URI.Above exmaple orderId is pathvariable
    Jersey annotations - PathParam & QueryParam
24) Encapsulation - restricting direct access to variables
25) Abstraction - hiding internal implementation.
26) How to divide two numbers without using division (/) operator in java / How to find remainder of
    division of two numbers using minus (-) operator / Java program to find remainder of division using –
    ans - subtrct divisor from dividend 
    while(dividend >= divisor)
   {
      dividend = dividend - divisor;
      quotient++;
   }
27) How to count the number of objects of a class in java? 
    ans - increment counter in constructor
28) Logging levels? ans -> ALL < TRACE < DEBUG < INFO < WARN < ERROR < FATAL < OFF
29) What is spring security?
    1. user name/password authentication
    2. SSO
    3. App level security
    4. Intra app authorization like OAuth
    5. Microservices security like JWT/tokens
    6. method level security
30) core concepts of spring security?
    1) Authentication 2)Authorization 3) Principal 4)Granted autority 5)Roles 
31) Maven dependency scopes? ans-> compile,provided,runtime,test,system,import
32) enum value is instantiated only once.enums don’t have any constructor so it is not possible for Reflection to utilize 
    it. Enums have their by-default constructor, we can’t invoke them by ourself. JVM handles the creation and invocation 
    of enum constructors internally.
33) Reflection? ->
34) Equals and hashcode contract? -> If two objects are equal, then calling hashCode() on both objects must return 
    the same value
35) Iterator vs ListIterator? -> traversal,direction,listItertor allow to add & remove,iterator remove only
36) Which collection classes provide random access of it’s elements? -> ArrayList, HashMap, TreeMap, Hashtable
37) Annotations used in SB? ans-> RestController,RequestMapping,GetMapping,PostMapping,Service,repository,PathVariable
    RequestBody,ResponseStatus
* What are IdentityHashMap? ->  it uses reference equality when comparing elements.
* What are WeakHashMap? -> Storing only weak references allows a key-value pair to be garbage collected when its key
  is no longer referenced outside of the WeakHashMap  
* Internal working of HashSet? return type of add method?
* difference in HashSet,LinkedHashSet,TreeSet?
* Internal working of hashmap? diff between hashmap,concurrenthashmap?
* how to implement synchronization for hashmap?
* How TreeSet sorts element?
* Implementation of equals and hashcode?
* write a program, i have HashMap it has Employee object,sort values based on custom comparator
* write a program which accepts array of string and print those word which starts with "A"
* How to declare float variable?
* RequestParam vs QueryParam?
* Swagger 
* If resttemplate.exchange throws any exception how to handle it?
* ExceptionHnadling in SB? 
* write a progrm to handle exception making use of AOP?
* How to make RequestParam optional?
* What Response contains?
* How to exclude common dependency in maven?
* class B it has two fields and it is not serializable, class c is serializable it has two fields, c extends b what
  will happen?
* class c is serializable, class b is not serializable, can we create class object of b from c?
* custom serialization without externalizable interface?
* Spring security?
* Docker commands
* how to make @RequestParam value optional 
  -> 
  1) using defaultValue
      @RequestMapping(value = "/myApp", method = { RequestMethod.POST, RequestMethod.GET })
      public String doSomething(@RequestParam(value = "name", defaultValue = "anonymous") final String name) {
        return "viewName";
      }
  2) using attribute required=false
      @RequestMapping(value = "/myApp", method = { RequestMethod.POST, RequestMethod.GET })
      public String doSomething(@RequestParam(value = "name", required=false) final String name) {
        return "viewName";
      }
* 

##Java 8

38) age comparator in java 8 -> listDevs.sort((Developer o1, Developer o2)->o1.getAge()-o2.getAge());
39) print devList -> listDevs.forEach((developer)->System.out.println(developer));
40) salry reverse sort -> 
    Comparator<Developer> salaryComparator = (o1, o2)->o1.getSalary().compareTo(o2.getSalary());
    listDevs.sort(salaryComparator.reversed());
41) Iterate map using java 8
    Map<String, Integer> items = new HashMap<>(); //map
    items.forEach((k,v)->System.out.println("Item : " + k + " Count : " + v));
42) Iterate List in java 8?
    List<String> items = new ArrayList<>(); //list
    items.forEach(item->System.out.println(item));
    using method references -> items.forEach(System.out::println);
43) Filter in java 8?
    List<String> result = lines.stream().filter(line -> !"mkyong".equals(line)).collect(Collectors.toList());
    result.forEach(System.out::println);
    #multiple condition 
     Person result1 = persons.stream()
                .filter((p) -> "jack".equals(p.getName()) && 20 == p.getAge())
                .findAny()
                .orElse(null);
    #filter with map
    String name = persons.stream()
                .filter(x -> "jack".equals(x.getName()))
                .map(Person::getName)                        //convert stream to String
                .findAny()
                .orElse("");
    System.out.println("name : " + name); //name : jack
    List<String> collect = persons.stream().map(Person::getName).collect(Collectors.toList());
    collect.forEach(System.out::println);
    //mkyong jack lawrence
44) Get the unique surnames in uppercase of the first 15 book authors that are 50 years old or over.
    library.stream()
    .map(book -> book.getAuthor())
    .filter(author -> author.getAge() >= 50)
    .distinct()
    .limit(15)
    .map(Author::getSurname)
    .map(String::toUpperCase)
    .collect(toList());
    #Compute the sum of ages of all female authors younger than 25.
    library.stream()
    .map(Book::getAuthor)
    .filter(a -> a.getGender() == Gender.FEMALE)
    .map(Author::getAge)
    .filter(age -> age < 25)
    .reduce(0, Integer::sum);

45) sort arraylist and print using java8 
    arrayList.sort((p1, p2) -> p1.compareTo(p2));
    arrayList.forEach(System.out::print);
46) How to define inheritance in spring configuration file?
    <bean id="BaseCustomerMalaysia" class="com.mkyong.common.Customer">
      <property name="country" value="Malaysia" />
    </bean>
    <bean id="CustomerBean" parent="BaseCustomerMalaysia">
      <property name="action" value="buy" />
      <property name="type" value="1" />
    </bean>
    Here parent class is BaseCustomerMalaysia child class is CustomerBean
47) How to resolve cyclic/circular dependency in spring?
    Bean A → Bean B → Bean A
    when having a circular dependency, Spring cannot decide which of the beans should be created first, since they depend 
    on one another. In these cases, Spring will raise a BeanCurrentlyInCreationException.It only happens in case of constructor injection.
    workarounds - 
    1.  Redesign
    2.  @Lazy - proxy object created and inserted 
    3.  use Setter/Field Injection

48) How to create role based method access?
    * enable global method security
      @Configuration
      @EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true, jsr250Enabled = true)
      public class MethodSecurityConfig extends GlobalMethodSecurityConfiguration {
      }
    * Use @Secured annotation to secure the method
      @Secured("ROLE_VIEWER")
      public String getUsername() {
        //...
      }
    * Using @RoleAllowed Annotation - this is equal to @Secured
      @RolesAllowed("ROLE_VIEWER")
      public String getUsername2() {
          //...
      }
    * Using @PreAuthorize and @PostAuthorize Annotations
      @PreAuthorize("hasRole('ROLE_VIEWER') or hasRole('ROLE_EDITOR')")
      public boolean isValidUsername3(String username) {
          //...
      }
    * Security Annotation at the Class Level
      @PreAuthorize("hasRole('ROLE_ADMIN')")
      public class SystemService {
        //...
      }

49) Exception hnadling in spring?
    * Using HTTP Status Codes
      @ResponseStatus(value=HttpStatus.NOT_FOUND, reason="No such Order")  // 404
      public class OrderNotFoundException extends RuntimeException {
         // ...
      }
    * Controller Based Exception Handling
      @Controller
      public class ExceptionHandlingController {
        //@RequestHandler methods
        ...
        // Exception handling methods
        // Convert a predefined exception to an HTTP Status code
        @ResponseStatus(value=HttpStatus.CONFLICT,
                        reason="Data integrity violation")  // 409
        @ExceptionHandler(DataIntegrityViolationException.class)
        public void conflict() {
          // Nothing to do
        }
        // Specify name of a specific view that will be used to display the error:
        @ExceptionHandler({SQLException.class,DataAccessException.class})
        public String databaseError() {
          // Nothing to do.  Returns the logical view name of an error page, passed
          // to the view-resolver(s) in usual way.
          // Note that the exception is NOT available to this view (it is not added
          // to the model) but see "Extending ExceptionHandlerExceptionResolver"
          // below.
          return "databaseError";
        }
        // Total control - setup a model and return the view name yourself. Or
        // consider subclassing ExceptionHandlerExceptionResolver (see below).
        @ExceptionHandler(Exception.class)
        public ModelAndView handleError(HttpServletRequest req, Exception ex) {
          logger.error("Request: " + req.getRequestURL() + " raised " + ex);
          ModelAndView mav = new ModelAndView();
          mav.addObject("exception", ex);
          mav.addObject("url", req.getRequestURL());
          mav.setViewName("error");
          return mav;
        }
      }
    * Global Exception Handling
      @ControllerAdvice
      class GlobalDefaultExceptionHandler {
        public static final String DEFAULT_ERROR_VIEW = "error";
        @ExceptionHandler(value = Exception.class)
        public ModelAndView
        defaultErrorHandler(HttpServletRequest req, Exception e) throws Exception {
          // If the exception is annotated with @ResponseStatus rethrow it and let
          // the framework handle it - like the OrderNotFoundException example
          // at the start of this post.
          // AnnotationUtils is a Spring Framework utility class.
          if (AnnotationUtils.findAnnotation
                      (e.getClass(), ResponseStatus.class) != null)
            throw e;
          // Otherwise setup and send the user to a default error-view.
          ModelAndView mav = new ModelAndView();
          mav.addObject("exception", e);
          mav.addObject("url", req.getRequestURL());
          mav.setViewName(DEFAULT_ERROR_VIEW);
          return mav;
        }
      }





##Angular

1) Annotation and a Decorator in Angular?
2) What's the difference between binding a value with or without square brackets, i.e.: <input attr="something" /> 
   vs <input [attr]="something" />? -> something is evaluated in later case
3) What is <ng-container>? ->  A grouping element that does not interfere with styles or layout
4) What is <ng-template>? -> It's an Angular element for rendering HTML when using structural directives. 
   The ng-template itself does not render to anything but a comment directly.
5) What are different kinds of directives supported in Angular 2? -> Structural, component, and attribute directives.
6) How do components communicate with each other? -> Input/Output properties, services, ViewChild/ViewContent
7) What are the attributes that you can define in an NgModule annotation? 
   -> Declarations, imports, exports, providers, bootstrap
8) What is the possible order of lifecycle hooks in Angular?
   -> constructor, ngOnChanges, ngOnInit, ngDoCheck, ngAfterContentInit, ngAfterContentChecked, ngAfterViewInit, 
   ngAfterViewChecked, ngOnDestroy.
9) What is metadata? -> decorators
   metadata is used to decorate the class so that it can configure the expected behaviour of the class
   1) class decorator - a) @Component and 
                        b) @NgModule
                        c) @Injectable
                        d) @Pipe
                        e) @Directive
   2) property decorator -> Input and Output
   3) method decorator -> @HostListener
   4) parameter decorator -> @Inject in constructor




======================================================
Shell Scripting
ksh - korn shell
bash - bourne again shell
sh - bourne shell

#!/bin/bash - bash is in bin folder
dd - to delete line
# - comment
echo - print
chmod +x "file_name" or chmod 755 "file_name" or chmod +x *
ls -l


