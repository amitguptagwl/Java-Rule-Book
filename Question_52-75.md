##In Progress

**Q52. If B extends A and you create object of B where default constructor of A throws some exception then whether object of B would be created?**
<br />Ans. As per Inheritance rule, child class needs to call `super()` on first line of it's constructor. But as per Exception rule, it needs to be handled or throwwn. So child class constructor will have to throw it again. [See](http://stackoverflow.com/a/28678358/453767). If there is not runtime exception then the object will be created.

**Q53. If C extends B extends A and you create C object by its parameterized constructor then whether constructor of A and B would be called?**
<br />Ans. As per Inheritance rule, yes.

**Q54. Is this scenario is possible? B extends A where A and B both have a parameterized constructor.**
<br />Ans. As per Inheritance rule, child class need to call super class constructor otherwise compiler adds empty super() call. Since parent class have parameterized constructor, this call will fails. However if child can call parent parameterized constructor to make it successful.

**Q55. Whether super.super or this.this is allowed?**
<br />Ans. this.this is senseless. this.super is not allowed. super.super is not allowed because son inherits properties of father only while the father inherits properties of his father. So if C extends B extends A then B can access all visible members of A. Thus C can access members of A through super only. It doesn’t require super.super.

**Q56. Can I have a final member field in a class which is not initialized anywhere in the class?**
<br />Ans. As per Final rules, No. It gives compilation error.

**Q57. A method() accepts String str as parameter then what will str.equals(null) output if I call method(null)?**
<br />Ans. When you set str=null, it free the object it is referring to. So calling any method on this will cause NullPointerException.

**Q58. If**
```
String s1 = “Amit”;
String s2 = new String(“Amit”);
```
**Which is true and why**

1. “Amit”.equals(s1);
2. “Amit” == s1
3. “Amit”.equals(s2);
4. “Amit” == s2
5. s1.equals(s2);
6. s1 == s2

<br/>Ans. As per Final rule, s1 will point to memory location where “Amit” resides. So 2nd statements is true. But 4th is false.
<br/>As per String rule, 1,3,5 are true. and 6 is false.

**Exception Handling**

**Q59. If**
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
**What would be the output?**

<br/>Ans. As per Exception rule, onle 1 catch block can be called. Since `Exception` is parent of all type of exceptions. It'll be always called. Hence compile time error "Unreachable catch block for NullPointerException".

**Q60. If**
```
public int myMethod() {
	int i=0;
	try{
		System.out.println(i);
		return 2;
	}catch(Exception e){
		System.out.println(i);
		return 3;
	}finally{
		System.out.println(i);
		return 4;
	}
}
:
System.out.println("->" +s.myMethod());
```

**What would be the output of above code?**

<br />Ans. Above code will output

<br />0
<br />0
<br />4

**Note:**

1. Since finally block runs ever whether any exception occurs or not so return statements inside try &amp; catch will be ignored. But their expression will be executed (like in above example ++i &amp; i++). <br />Since, in above program, finally returns program control to parent caller and return in try block doesn’t get completed so it gives abnormal completion warning.
2. Above code can give Unreachable code compilation error if there is any statement after finally block.
