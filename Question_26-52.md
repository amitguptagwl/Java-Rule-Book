##In Progress

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

**On Hold**
Q37. Whether an unclosed stream/connection may cause memory leak?
<br />Ans. No

**On Hold**
**Q38. Whether String.intern() may cause memory leak?**
<br />Ans. Yeas if you are calling intern() on huge number of strings which are unique (no common string) then they’ll fill up string pool.

**On Hold**
**Q39. Can you serialize a class having non-serialize parent? What if a class has reference of non-serialize class?**
<br />Ans. As per Common Rules for Concrete class and Interface, parent class will not be serializable. Although, it’ll not give any error but values of A will not be serialized. At the time of deserialization, A’s values will be assigned with default values, or with values assigned at the time of declaration or in default constructor.

**Q40. Whether a class can have static private fields? Is there any advantage to create them?**
<br />Ans. Yes. Static private fields will be shared among all instances but can’t be shared by class name directly. They can be used to keep some common internal information like instance counter.

**Q41. Can we declare local variable as static? Explain.**
<br />Ans. No. Because if a method is non-static, it can’t have static local variables. And if it is static there is a single instance of this method so static variable are senseless. (All variables and parameters of static method are static. And a static method can call only static methods.)

###Multithreading

**Q42. Whether thread t1 &amp; t2, running on 2 separate instances of class A, can access synchronized method() of class A at same time?**
<br />Ans. As per Threading rule, yes. (due to 2 separate instances, there is nothing being shared)

**Q43. Whether thread t1 &amp; t2, running on 2 separate instances of class A, can access separate synchronized methods of class A at same time where 1 of them is static method?**
<br />Ans. As per Threading rule, no. (Since static method is sharable between both instances, second thread will wait until first one comes out from the monitor.)

**Q44. For following program, if thread A and thread B runs at same time on same instance where A calls acquire(), and B calls modify(), whether B will modify value of `a`**

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

**Q45.Can following program lead deadlock?**

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

**Q46. Which of the following methods are correct?**

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

###Inheritance

**Q44. If B extends A and you create object of B where default constructor of A throw some exception then whether object of B would be created?**
<br />Ans. As per Inheritance rule, yes.

**Q45. If C extends B extends A and you create C object by its parameterized constructor then whether constructor of A and Bwould be called?**

Ans. As per Inheritance rule, yes.

**Q46. Is this scenario is possible? B extends A where A and B both have a parameterized constructor.**

Ans. As per Concrete class rule 8 and Inheritance rule, no. It will give compilation error.

**Q47. Whether super.super or this.this is allowed?**

Ans. this.this is senseless. this.super is not allowed. super.super is not allowed because son inherits properties of father only while the father inherits properties of his father. So if C extends B extends A then B can access all visible members of A. Thus C can access members of A through super only. It doesn’t require super.super.

**Q48. Can I have a final member field in a class which is not initialized anywhere in the class?**
<br />Ans. As per Final rules, No. It gives compilation error.

**Q49. A method() accepts String str as parameter then what will str.equals(null) output if I call method(null)?**
<br />Ans.
Since null belongs to a value not a class so calling any method over it always generate an exception ie NullPointerException. Here, str.equals(null) will be treated as null.equals(null).

When you set str=null, it free the object it is referring to. So calling any method on this will cause NullPointerException.

Q50. If
```
String s1 = “Amit”;
String s2 = new String(“Amit”);
```
Which is true and why

* “Amit”.equals(s1);
* “Amit” == s1
* “Amit”.equals(s2);
* “Amit” == s2
* s1.equals(s2);
* s1 == s2

Ans. As per final rule 5, s1 will point to memory location where “Amit” resides. So first 2 statements are true.

As per Concrete class rule 4, s2 will point to new memory location having value “Amit. So 3 rd statement is true but 4th is false.
5th & 6th are similar to 3 rd &amp; 4 th statements.

**Exception Handling**

Q51. If
```
try{
	String n = null;
	System.out.println(n.equals(null));
	System.out.println(1);
}catch(Exception e){
	System.out.println(2);
}catch(NullPointerException e){
	System.out.println(3);
}
```
What would be the output?

Ans. As per Q49, there would be NullPointerException. So 1 will not be printed. As per Exception rule 3, it’ll print 3 only.

Q52. If
```
public int myMethod() {
	int i=0;
	try{
		System.out.println(i);
		return ++i;
	}catch(Exception e){
		System.out.println(i);
		return i++;
	}finally{
		System.out.println(i);
		return i++;
	}
}
:
System.out.println(s.myMethod());
```

Whether above code will give compilation error? If yes then where and why? If no then why?

Ans. Above code will output

<br />0
<br />1
<br />1

**Note:**

1. Since finally block runs ever whether any exception occurs or not so return statements inside try &amp; catch will be ignored. But
their expression will be executed (like in above example ++i &amp; i++).

Since, in above program, finally returns program control to parent caller and return in try block doesn’t get completed so it
gives abnormal completion warning.
2. Above code doesn’t give compilation error since finally block is optional.
3. In last return statement value of I will be increased after return.
4. Above code can give Unreachable code compilation error if there is any statement after finally block.
