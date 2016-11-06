# Java Rule Book
Basic concepts of Java to answer any question about how Java works specially in job interviews. With combining multiple rules you can answer many java question

**This book is only suitable for someone who has worked in Java**

**Feel free to correct any rule or to suggest any correction needed.**

[![Donate to author](https://www.paypalobjects.com/webstatic/en_US/btn/btn_donate_92x26.png)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=KQJAX48SPUKNC)

##Rules (upto Java 7)

####Final
1. No further change ( So should be assigned either the time of declaration or in constructor)
2. JVM allocates same memory location for same constants.
3. constant expressions are computed at compile time

####Static
1. Static members belongs to a class not any instance.
 a. Static member doesn’t belong to any instance hence they can’t access instanced members.
2. One per class. Shared among all class instances. 
 a. Can’t be overridden. But are inherited.

####Concrete class
1. All methods have some functionality.
2. cannot be static
3. Constructor gets called after initializing memory by JVM.
4. Declaration of any member is fully independent from any other class/interface.
5. Can have static member and static block which are executed first in the class and top to bottom.
6. All classes has default constructor, with no statement inside, by default until you create any constructor manually.
7. It can have nested static interface.

####Interface
1. It's all member fields are **public** (since has no implementation private fields can't be used), **final** (since interfance can't be instanciated they can have only 1 value) **static** (since interfance can't be instanciated and member field are not the responsibilities for other class).
2. Methods are public only and can’t be static. (Interface defines external responsibilities (hence public) which may be different for each class (hence non-static).)
3. There is no functionality/implementation, but responsibilities, to be implemented by child class type (class itself or its parent class). So implicitly *abstract* keyword and modifier are optional.
4. Can extend only 1 interface.
5. It can have nested class.

#####Common Rules for Concrete class and Interface
1. Cannot extend/implement itself or one of its own member types. If it inherits itself it creates Cycle. If it inherits any child then its hierarchy in inheritance tree becomes inconsistent.
2. Represent the type of its child, sub-child and so on.

####Abstract class
1. Concrete class + Interface = Abstract class
 * Unlike concrete class, it can’t be instantiated.
 * Unlike interface, abstract methods can have any scope and *abstract* keyword is not optional.
 * Unlike interface, fields can’t be static and final.

(Like any simple java class, it may have simple member , static member, static block, constructor etc. It can extend/implement any concrete class, abstract class, or an interface.)

#####Common Rules for Abstract class and Interface
1. Both can have 0 to N number of abstract methods.
2. Both need not to implement abstract methods declared in their parent abstract class/interface.

####Nested/inner class
1. Can access all members of container class.
2. ~~An independent class inside a class which follows rule of a class member.~~ Concrete class rule 5 (So it can be abstract, static abstract, static)

####Anonymous class (closures)
1. Cannot be implemented by any class (as created dynamically)
2. Can access only final external object/var. (as the values of non-final variable can be changed, the behaviour of annonymous class will be unpredictable.)

####Inheritance
1. A class/interface can reference its own instance or any child class instance whichever can be instantiated. (It means a child class can’t refer to its base class instance.)
2. A reference can call method of referred instance only.
3. Child class need not to throw an exception while overriding a method, it is automatically get thrown by parent class method.
4. Super() must be first statement in child class constructor. If you don’t call super() explicitly, compiler automatically adds empty super().
5. Members can have same name or method signature. In case of ambiguity, *this* is used to represent current class' members and super is used to represent members of parent class. Otherwise the closest declaration will be referred.
6. Until or unless there is no ambiguity of method's declaration inheritance is allowed. (Hence a class can't extend multiple classes but can implement multiple interfaces. Similarly it can extend a class and implement an interface which has methods with same signature.)
7. Child class can't narrow down the scope of method while overriding. (A subclass should always satisfy the contract of the superclass. See Liskov Substitution principle.)
8. **covariant** return type: if child-class is overriding a method(), it can return either object A or B, where B is sub-class of A.

####Garbage Collector
1. It fragment and release only heap memory not stack memory.
2. Whether you System.gc(); or objReference=null; it doesn’t invoke GC until JVM really needs it. But 2nd statement helps GC to collect object quickly.
3. finalize() is called only when object memory is released from heap.

####Serialization
1. Transient and static variables are skipped while serialization/deserialization. In alternate of Transient, a class can have *ObjectStreamField[] serialPersistentFields* of class members which can be serialized.
2. While serialization process all default constructor of super classes are called. But deserialization calls default constructor of all non-serialized class from top of inheritance tree till it meets any serialized class. In addition, deserialization and it doesn’t call constructor of current class.
3. At the time of deserialization, first empty constructor gets called then all deserialized values are assigned to relevant property.
4. Impleneting Externalizable, we can override readExternal/writeExternal methods to control serialization process.

Read: [Serialization – a Sci Fi story](https://articlestack.wordpress.com/2016/04/03/serialization-a-sci-fi-story/)

####Multithreading
1. NEW -> RUNNABLE <-> BLOCKED/WAITING/TIMED_WAITING -> TERMINATED
2. More than 1 thread can’t access same monitor at a time, if both are in running state. (Same monitor means who has same memory address)
3. `synchronized method(…` or `Synchronized(this)` locks object itself. Hence `static synchronized method(…` takes lock on whole class. ([External locks](https://articlestack.wordpress.com/2016/01/23/java-multithreading-external-locks/) or ReentrantLock/ReadWriteLock seems better than synchronized blocks)
4. `notify()` or `notifyAll()`, and `wait()` must be in a synchronized block for the object you are waiting on(Otherwise you'll et `IllegalMonitorException`). And the value of the object must not be changed. `notify()` or `notifyAll()` to awake threads which are waiting. `interrupt()` to awake thread which is sleeping.
5. `sleep()` doesn't release the lock. So other threads are supposed to wait<br />**mutual exlusion**: `wait()` release the lock and acquire it again once relevant `notify()` is called. ([CyclicBarrier](https://articlestack.wordpress.com/2016/01/23/cyclicbarrier/) can be used to wait N threads at a time. CountDownLatch can be used to wait N starting threads. `join` can be used to wait until respective thread is completed. `Semaphore` is used to controll number of threads. )
6. Doubles and longs assignment is not atomic. Mark them `volatile` or synchronized their assignment.
7. [**daemon threads**](https://github.com/NaturalIntelligence/Practical-Java/tree/master/Concurrency/src/os/nushi/concurrency/tracer): (eg garbage collector) killed by JVM once all user threads are finished. JVM will exit when only daemon threads remain. If parent thread is daemon, child thread will also be. 
8. `Thread.yield()` gives the scheduler a hint that the current thread is willing to free the processor.

**busy waiting**: wait for an event by performing some active computations that let the thread/process occupy the processor. Eg looping long time to wait instead of using `Thread.sleep()`.
<br/>**ThreadLocal**: allows you to have a variable that will be unique to a given thread. (Thus, if the same code runs in different threads, these executions will not share the value, but instead each thread has its own variable that is local to the thread.)
<br/>**Executor**: Make pooling, scheduling, and interaction with running threads/tasks possible.
<br/>**Callable**: Similar to Runnable but returns some value.
<br/>**Future**: Run by some ExecutorService. Let you poll/block/cancel running task.
<br/>**ThreadPoolExecutor**: perform group operation like set pool size, shutdown pool to stop all running threads etc. `ScheduledThreadPoolExecutor` is extention of this.
<br/>**Fork/Join**: It is an implementation of the ExecutorService. It is designed for work that can be broken into smaller pieces recursively. (CompletableFuture is also worth to check)
<br/>**preemptive scheduling**: Highest priority task get executed first
<br/>**time slicing scheduling**: a task executes for a predefined slice of time and then reenters the pool of ready tasks.

<br/>[Read](https://articlestack.wordpress.com/category/tutorial/java/multithreading/): [Important Terms](https://articlestack.wordpress.com/2016/01/19/java-multithreading/), [Synchronization & Deadlock](https://articlestack.wordpress.com/2016/01/20/synchronization/), [wait & notify](https://articlestack.wordpress.com/2016/01/20/java-multithreading-notify-wait/), [join](https://articlestack.wordpress.com/2016/01/23/java-multithreading-join/), [Test your threading code](Thread Tracer)

####Exception
1. **try** must be followed by either **catch** or **finally** or both
2. finally is always called(until program stops due to fatal error or `System.exit()`). So if `try` and `finally` both have return statement then `return` statement of `try` will be executed but will not return the control.
3. Only 1 catch block is called start from top to bottom.
4. If you call a method throwing some exception, either you need to catch the same or wider exception or throw it again.
5. **Checked exceptions** are checked at compile-time(e.g.IOException,SQLException). But Unchecked exceptions are not.

####String
1. == checks for reference only.
2. It overrides equals() which checks for value and hashcode().
3. Immutable so treated as constant by JVM.
4. **Intern**: When the compiler encounters a String literal, it checks to see if an identical String already exists in the pool, otherwise creates it. (String pool always have unique values.). String.intern() does the same job.
5. **String Pool (String Constant/Memory/Literals Pool)**
 * Literal strings in any class of any package represent references to the same String object.
 * Strings computed by constant expressions are computed at compile time and then treated as if they were literals.
 * A string literal always refers to the same instance of class String.
6. StringBuffer can be considered as thread safe StringBuilder. Both are mutable unlike String. Java uses StringBuilder to concatenate string because it is mutable.

####Enum 
1. Is Self-Type. Declared as  Enum< E extends Enum< E>>
2. Can implement an interface.

####Collection/Map
Have a look on these [charts](https://github.com/NaturalIntelligence/Java-Rule-Book/blob/master/collections.pdf) for better understanding;
1. **HashMap**: key is used to calculate a hash which is used then to find a bucket where that particular key is stored. If there are multiple objects in the same bucket they are stored in linked list( balanced tree after a threshhold in java8) and equals() is used to get an element.
2. **WeakHashMap**: Same as HashMap but entries can be garbage collected.
3. **IdentityHashMap**: Same as HashMap but == is used instead of equals()
4. **HashTable**: Same as HashMap (other than java8 changes) but it is synchronized and doesn't allow `null`.
5. Linked Set/List/Map and ArrayList maintain insertion order.
6. List and Multimap allow duplicate items
7. Sorted Map/Set (Tree Map/Set) requires item to be comparable.( It can help you to maintain insertion order.)

####Generics 

* If B extends A, means B is-a type of A, doesn't mean `SomeClass<B>` is-a type of `SomeClass<A>`. So
* Unbounded Wildcard: `SomeClass<B>` can be assigned to `SomeClass<?>`.

<p text-align="center">
<img src="https://docs.oracle.com/javase/tutorial/figures/java/generics-wildcardSubtyping.gif"/>
</p>

* If A extends B, means B is-a type of A, then `B<E>` is-a type of `A<E>`
* **Upper Bounded Wildcards**(covariance or Narrowing a reference): If A extends B, means B is-a type of A, also means that `SomeClass<B>` is-a type of `SomeClass<? extends A>`.
* **Lower Bounded Wildcards**(contravariance or Widening a reference): `? super T` means T and all classes which are parent of T.
* Multiple bounds: `<T extends A & B>`
* Capture: At the time generic method call, type should be known;
```java
List<? extends Fruit> fruits = new ArrayList<Fruit>();
fruits.set(0, fruits.get(0));//Compile time error
```
* Type inference: `List<A> list = new ArrayList<>()` and `list.<A>add(a); list.add(a);` are allowed
* **Erasure**: During the type erasure process, the Java compiler erases all type parameters and replaces each with its first bound if the type parameter is bounded, or Object if the type parameter is unbounded.
* Cannot Instantiate Generic Types with Primitive Types
* Cannot Declare Static Fields of Parameterized Types
* Cannot Use Casts or instanceof of Parameterized Types
* Cannot Create Arrays of Parameterized Types 
* Cannot Create, Catch, or Throw Objects of Parameterized Types
* Wildcards guideines
 * Use the ? extends wildcard if you need to retrieve object from a data structure
 * Use the ? super wildcard if you need to put objects in a data structure
 * If you need to do both things, don’t use any wildcard.


####IO & NIO
<img src="http://s22.postimg.org/4u4vkcqn5/NIO.png" width="250" align="right" alt="Java NIO"/>
* **Asynchronous**: a non-CPU paralel operation with the caller(Eg JS XmlHttpRequest)
* Stream I/O are sequential. Like: printer port, network port etc.
* **File Descriptor**: A handle (saved as temporary file) to access a file, network socket etc. Number of open files or open network connection equal to number of FDs on disk.
* **Channel** is a nexus for I/O operations. It represents an open connection to an entity which can perform IO operation. It can be synchronous(blocking) or asynchronous(non-blocking). 
* **Selector** is a central object to analyze various asynchronous(non-blocking) Channel. (Since file operations are blocking IO operaion, you can't use FileChannel with a Selector)
* Reading from a channel to buffers is caller **Scatter**.  Writing to a channel from buffers is called **Gather**.
* NIO use 1 thread to access multiple channels (sockets may be) using Selectors. IO uses one thread per socket.

####Networking
TODO

####Other
1. hashCode() returns same value for two objects if obj1.equals(obj2) is true. hashcode is used to narrow up the results of fast searching. But it doesn't gurantee that the objects are same.
2. equals() method must exhibit the following properties:
  * *Symmetry*: For two references, a and b, a.equals(b) if and only if b.equals(a)
  * *Reflexivity*: For all non-null references, a.equals(a)
  * *Transitivity*: If a.equals(b) and b.equals(c), then a.equals(c)
  * *Consistency* with hashCode(): Two equal objects must have the same hashCode() value
3. **JVM**(How to run) is the subset of **JRE** (where to run) is the subset of **JDK** (develop, debug, compile, package ...).
 * JVM has JIT, clasloader, 
   * **JIT**(Just In Time) compiler coverts bytecode('write once and run anywhere') into machine specific instructions and links with other compiled machine code(object code) to save compilation time of same code. Sometimes it also do adaptive optimization by inlining function calls, remove branches etc.
 * JRE has JVM, compiled libraries, libraries to display & run applets on browsers, alone, or with JWS(Java Web Start,  which deploys standalone applications over a network.)
 * **Hotsopt** is one of the implementations of JVM (written in C++) owened by Oracle, which was initially designed to analyze the program’s performance for “hot spots”(frequently or repeatedly executed).
4. A file saved with `.java` extension can have different name than actual class name. So it can also have multiple classes. But compiled class files will be according to class name.
5. Only class members are assigned with default value.


####Class loader
<img src="https://2.bp.blogspot.com/-HCTsr-j_ojw/USTOh1f8JwI/AAAAAAAAAjg/YegPspR5K48/s400/java_classloader_hierarchy.PNG" align="right" />
A class is loaded in Java, when its needed. The sequence of class loader is from top to bottom in diagram.

First request to load a class goes to application class loader which is delegated to parent loaders. If the class is not found or no unique class is found by a loader then it passes to child loader to findClass. if no loader finds unique class, it throws `java.lang.ClassNotFoundException`.


####Memory
<img src="http://i.stack.imgur.com/uDdEk.png" width="400" align="right" />

* **Heap Memory**: memory for all class instances and arrays. h= y+t ;  t=tenured, y=young, h=heap
(The heap is split into several different sections, called **generations**. As objects survive more garbage collections, they are promoted(copied) into higher generations(space). The older generations are not garbage collected as often.)
  * **Young Generation**: Y = E + 2S;  S - Survivor, E - Eden, Y - Young generation
    * **Eden Space**: The pool from which memory is initially allocated for most objects.
    * **Survivor Space**: The pool containing objects that have survived the garbage collection of the Eden space.
  * **Old Generation**
    * **Tenured Space**: The pool containing objects that have existed for some time in the survivor space. This generation is garbage collected much less frequently.
* **Non-heap Memory**: method area; shared among all threads and memory required for the internal processing or optimization 
  * **Permanent Generation**: There is the 4th generation (PermGen). The objects that reside here are not eligible to be garbage collected, and usually contain an immutable state necessary for the JVM to run, such as class definitions and the **Runtime constant pool**. It is replaced with **Metaspace** in java 8.
    * **Runtime constant pool** 
     * can be GCed from 1.7
     * One for each class. Holds class level constants.
     * String constants point to String Pool.
     * Doen't have empty fields
  * **Code Cache**: (native area) Native code generated by JVM resides here. This area holds references and variables of primitive types.

Note: Sizes of each space can be controlled by different JVM parameters, like: NewRatio, NewSize, MaxNewSize, SurvivorRatio etc.

####JDBC
Helps to connect and execute query to the database.

1. Statement: To run a query
2. PreparedStatement: parameterized queries. Query is compiled only once. And the arguments get changed.
3. CallableStatement: to execute execute stored procedures and functions
