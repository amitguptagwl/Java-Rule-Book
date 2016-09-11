**Q26. Can a class have static final methods?**
<br />Ans. Since both are not contradictory, so yes.

**Q27. Can an abstract class implementing an interface have same abstract method declared in interface?**
<br />Ans. Yes by rules.

**Q28. Why and when to make a class abstract?**
<br/>Ans.

1. As Abstract class = Interface + Concrete class, so when we want the child class to implement some responsibilities as well as there is something which can be implemented in the parent class, like something common among all children then abstract class can be used. However, this can also be achieved by extending one concrete class and implementing an interface by all the children. But in this case, the parent is not asking or rather forcing a child to fulfill some responsibilities. This becomes optional.
2. one to many: my all child classes should fulfill these responsibilities. <br/>one to one : current class should fulfill these responsibilities.
3. Parent class: type of <br/>Interface : marker, tag


**Q29. Why to make a class final?**
<br/>Ans. 
<br/>To stop inheritance: "The interaction of inherited classes with their parents can be surprising and unpredicatable if the ancestor wasn't designed to be inherited from."
<br/>"to favour composition over inheritance" - Effective Java
<br/>It may not be useful if you are writing internal code. But it can be useful when writing external libraries.
<br/> It guarantees immutability.


**Q30. Whether a final class can have final or static methods?**
<br />Ans. By Concrete class rule it is possible. But there is no sens to have final methods in final class. Static is fine.

**Q31. Whether you can change field members value of final object?**
<br />Ans. Making an object final means it's reference can't be changed, but the value of field members can be.

**Q32. What is the advantage of creating a final variables?**
<br />Ans:

* It guarantees immutability which is supported by annonymous classes, String, and other places.
* JVm assigns single memory location for same value variables.
* It gives clarity that this variable is read only. and saves you from changing variable value accidentally.

**Q33. What is the advantage of creating a final parameter for a method where parameter is never modified in method?**
<br />Ans. Read Q32.

**Q34. What is the advantage of private/protected constructor?**
<br />Ans. To implement singleton pattern, or pooling/caching pattern.

**Q35. I am calling System.gc() or objReference=null. Whether it will call finalize()?**
<br />Ans. As per GC rule, it is not certain.

**Q36. How can you pull an object out collected by GC?**
<br />Ans. Do some operation on object in finalize() of that class.

**Q37. Whether an unclosed stream/connection may cause memory leak?**
<br />Ans. No. Connections are treated as file in OS. So there can be resource leak but not the memory leak.

**Q38. Can you serialize a class having non-serialize parent? What if a class has reference of non-serialize class?**
<br />Ans. Yes if it is marked serializable. Although parent class will not be serialize but it’ll not give any error. During deserialization, the fields of non-serializable classes will be initialized using the public or protected no-arg constructor of the class.

**Q39. Whether a class can have static private fields? Is there any advantage to create them?**
<br />Ans. Yes. Static private fields will be shared among all instances but can’t be shared by class name directly. They can be used to keep some common internal information like instance counter.

**Q40. Can we declare local variable as static? Explain.**
<br />Ans. No. Because if a method is non-static, it can’t have static local variables. And if it is static there is a single instance of this method so static variable are senseless. (All variables and parameters of static method are static. And a static method can call only static methods.)

###Multithreading

**Q41. Whether thread t1 &amp; t2, running on 2 separate instances of class A, can access synchronized method() of class A at same time?**
<br />Ans. As per Threading rule, yes. (due to 2 separate instances, there is nothing being shared)

**Q42. Whether thread t1 &amp; t2, running on 2 separate instances of class A, can access separate synchronized methods of class A at same time where 1 of them is static method?**
<br />Ans. As per Threading rule, no. (Since static method is sharable between both instances, second thread will wait until first one comes out from the monitor.)

**Q43. For following program, if thread A and thread B runs at same time on same instance where A calls acquire(), and B calls modify(), whether B will modify value of `a`**

```java
public class SynchoTest {
 	:
    public void acquire() throws Exception{
        synchronized(a){
        	Thread.sleep(10000);
        }
    }
 
    public void modify(int n){
        this.a=n;
    }
}
```

<br />Ans. By rules, yes. Because when thread B calls modify(), it doesn't meet to any monitor. However if both threads call acquire() then second thread will wait.

**Q44.Can following program lead deadlock?**

```java
public synchronized void acquire() throws Exception{
	Thread.sleep(5000);
	synchronized(lock){
		wait(1);
	}
}

public synchronized void modify() throws Exception{
	Thread.sleep(5000);
	synchronized(lock){
		wait(1);
	}
}
```

<br />Ans. Yes.

```java
public synchronized void acquire() throws Exception{ //lock the class object
	Thread.sleep(5000); 
	synchronized(lock){ //lock the "lock" object
		wait(1);
	}
}

public synchronized void modify() throws Exception{ //lock the class object
	Thread.sleep(5000);
	synchronized(lock){ //lock the "lock" object
		wait(1);
	}
}
```

```
A:8
B:9
[A:NEW,B:NEW]						Thread A,B started
[A:RUNNABLE,B:RUNNABLE] 			Both are ready to run
[A:RUNNABLE,B:BLOCKED]  			A: calls acquire() and lock class obj, B: gets blocked
[A:TIMED_WAITING,B:BLOCKED]			A: waiting due to sleep(), B: still blocked
[A:RUNNABLE,B:BLOCKED] 				A: waiting over, enters into another monitor, B: still blocked
[A:TIMED_WAITING,B:TIMED_WAITING]	A: waiting due to wait(), B: signaled to go ahead, calls modify()
[A:BLOCKED,B:TIMED_WAITING] 		A: Can't enter into first monitor as Baquires that lock, B: waiting due to sleep()
[A:BLOCKED,B:RUNNABLE]				A: Blocked due to B, B: sleep() completes
[A:BLOCKED,B:BLOCKED]				A: Blocked due to B, B: Blocked due to A dint release monitor of "lock" obj,
Both threads are blocked
Deadlock detected in
Thread id: 9
Thread id: 8
```

**Q45. Which of the following methods are correct?**

```java
public void method(){
    synchronized(a){
        wait();
    }
}
 
public void method(){
    synchronized(a){
        synchronized(this){
            wait(n);
        }
    }
}
 
public synchronized void method(){
    synchronized(a){
        wait(n);
    }
}

public synchronized void method(){
    synchronized(a){
    	a = 57;
        a.wait();
    }
}

public void method(){
    synchronized(a){
        a.wait();
    }
}
 
public synchronized void method(){
    synchronized(a){
        wait();
    }
}
```
<br />Ans. By the rules, last 2 methods are correct.

**Q46. Is it possible to start a thread twice?**
<br/>Ans: No by rules.

**Q47. Is it possible to convert a normal user thread into a daemon thread after it has been started?**
<br/>Ans: No, you need to call setDaemon() before thread starts.

**Q48. How do we wait in the parent thread for the termination of the child thread?**
<br/>Ans: join()

**Q59. Have a look on below program to create Daemon thread;**
```java
	private static class MyDaemonThread extends Thread {

		public MyDaemonThread() {
			setDaemon(true);
		}

		@Override
		public void run() {
			while (true) {
				try {
					System.out.println(getState());
					Thread.sleep(500);
				} catch (InterruptedException e) {
				}
			}
		}
	}

	public static void main(String[] args) throws InterruptedException {
		Thread thread = new MyDaemonThread();
		thread.start();
		thread.join();
		System.out.println(thread.isAlive());
	}
```
**Answer following**

1. What will be output of above program?
2. What will be output of above program, if we remove `join`?
3. What will be output of above program, if we remove `join`, and `setDaemon(true)` both?

<br/>Ans:

1. The main thread will wait for dameon thread to be completed due to `join()` call. But since it is running in loop, `isAlive()` call never be happen.
2. It'll output `Runnable` then `true`. Now since there is noting to do in main thread or rather there is no other user thread running, dameon thread will be killed by JVM. Hence the above program will not print the state anymore.
3. It'll output `Runnable` then `true`. Since this is not dameon but user thread, JVM will wait it to be completed. Hence it's state `Runnable` will keep printing for forever until something interrupt it.

**Q50. Can primitive values be used for intrinsic locks?**
<br/>Ans: No because this operation is already atomic.

**Q51. Can a constructor be synchronized?**
<br/>Ans: No, because synchronization is needed when same memory location can be accessed by multiple threads. In case of constructor, everytime new object will be created.
