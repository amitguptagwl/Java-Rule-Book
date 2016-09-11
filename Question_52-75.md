##In Progress

###Inheritance

**Q52. If B extends A and you create object of B where default constructor of A throws some exception then whether object of B would be created?**
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
