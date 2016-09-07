**Q26. Can a class have static final methods?**

Ans. Yes.

**Q27. Can an abstract class implementing an interface have same abstract method declared in interface?**

Ans. Yes.

**Q28. Why and when to make a class abstract?**

Ans. When client must implement some set of methods while implementation of some method is optional. For example,

batchJob extends Job

On the other hand, classes which represent to some set of responsibilities, features, or services but don’t represent any physical entity

then these classes can be declared as abstract class.

**Q29. Why to make a class final?**

Ans. While creating java API, API developer never wants any programmer overriding default nature of the API class. So making them

final stops any one inheriting it.

**Q30. Whether a final class can have final or static methods?**

Ans. If a class is final no one can override it. Hence no one can override its method too. So both supports each other. Hence it is

allowed. However it has no sense.

**Q31. Whether you can change field members value of final object?**

Ans. If an object is final it means it can&#39;t refer to any other object/instance. But it doesn&#39;t make field member final. So they are changeable.

**Q32. What is the advantage of creating a final variables?**

Ans. As per the performance, there is no advantage. It is just a coding standard to clarify that this variable is read only. It saves you from changing variable value accidentally. On the other hand, anonymous method accept only final external variables. So it is required.

**Q33. What is the advantage of creating a final parameter for a method where parameter is never modified in method?**

Ans. Read Q32.

**Q34. What is the advantage of private/protected constructor?**

Ans. To implement singleton pattern, or pooling/caching pattern.

**Q35. I am calling System.gc() or objReference=null. Whether it will call finalize()?**

Ans. As per GC rule, it is not certain.

**Q36. How can you pull an object out collected by GC?**

Ans. Do some operation on object in finalize() of that class.

**Q37. Whether an unclosed stream/connection may cause memory leak?**

Ans. No

**Q38. Whether String.intern() may cause memory leak?**

Ans. Yeas if you are calling intern() on huge number of strings which are unique (no common string) then they’ll fill up string pool.

**Q39. Can you serialize a class having non-serialize parent? What if a class has reference of non-serialize class?**

Ans. As per Common Rules for Concrete class and Interface, parent class will not be serializable. Although, it’ll not give any error
but values of A will not be serialized. At the time of deserialization, A’s values will be assigned with default values, or with values
assigned at the time of declaration or in default constructor.

**Q40. Whether a class can have static private fields? Is there any advantage to create them?**

Ans. Yes. Static private fields will be shared among all instances but can’t be shared by class name directly. They can be used to keep
some common internal information like instance counter.

**Q41. Can we declare local variable as static? Explain.

Ans. No. Because if a method is non-static, it can’t have static local variables. And if it is static there is a single instance of this
method so static variable are senseless. (All variables and parameters of static method are static. And a static method can call only
static methods.)

**Multithreading**

**Q42. Whether thread t1 &amp; t2, running on 2 separate instances of class A, can access synchronized method() of class A at same
time?**

Ans. As per Threading rule, yes.

**Q43. Whether thread t1 &amp; t2, running on 2 separate instances of class A, can access separate synchronized methods of class A
at same time where 1 of them is static method?**

Ans. As per Threading rule, no.

**Inheritance**

**Q44. If B extends A and you create object of B where default constructor of A throw some exception then whether object of B
would be created?**

Ans. As per Inheritance rule, yes.

**Q45. If C extends B extends A and you create C object by its parameterized constructor then whether constructor of A and B
would be called?**

Ans. As per Inheritance rule, yes.

**Q46. Is this scenario is possible? B extends A where A and B both have a parameterized constructor.**

Ans. As per Concrete class rule 8 and Inheritance rule, no. It will give compilation error.

**Q47. Whether super.super or this.this is allowed?**

Ans. this.this is senseless. this.super is not allowed. super.super is not allowed because son inherits properties of father only while the
father inherits properties of his father. So if C extends B extends A then B can access all visible members of A. Thus C can access
members of A through super only. It doesn’t require super.super.

**Q48. Can I have a final member field in a class which is not initialized anywhere in the class?**

Ans. As per Final rules, No. It gives compilation error.

**Q49. A method() accepts String str as parameter then what will str.equals(null) output if I call method(null)?**

Ans.
Since null belongs to a value not a class so calling any method over it always generate an exception ie NullPointerException. Here,
str.equals(null) will be treated as null.equals(null).

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

0

1

1

**Note:**

1. Since finally block runs ever whether any exception occurs or not so return statements inside try &amp; catch will be ignored. But
their expression will be executed (like in above example ++i &amp; i++).

Since, in above program, finally returns program control to parent caller and return in try block doesn’t get completed so it
gives abnormal completion warning.
2. Above code doesn’t give compilation error since finally block is optional.
3. In last return statement value of I will be increased after return.
4. Above code can give Unreachable code compilation error if there is any statement after finally block.