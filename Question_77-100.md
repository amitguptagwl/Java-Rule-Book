##In Progress
**Q77. Why checked exceptions are required be handled?**
<br />Ans. No matter how good you are in coding, there is always a chance of exception due to external events. Eg. drive is not monuted, internet connection is lost, or thread is interuppted etc. Such cases are supposed to be handled.

**Q78. How many objects will be created in the following code?**

```java
String s1="Welcome";  
String s2="Welcome";  
String s3="Welcome";  
```
<br />Ans. Since all 3 reference ar pointing to same string literal, there will be one String literal created in String Pool. Now if this code is written in some method then 3 references will be created in local variable array of JVM frame. Otherwise, if s1,s2, and s3 class fields then 3 references will be created in Constant Pool.

**Q79. How many objects will be created in the following code?**
```java
String welcome = "Welcome";
String s = new String(welcome);
```

```java
String s = new String("Welcome");
```
<br />Ans. 

1. Two objects, As soon as compiler finds literal in program, it checks if identical String already exists in **String constant pool**. If not, it creates one. This referance will be passed to `new String(String)`. `new` will create another object in non-pool(heap). However reference `welcome`, and `s` will be pointing to same value.
2. This is same as above. But it has only one reference ie `s`.

**Q80. How many objects will be created in the heap and non-heap memory area in following code?**
```java
String te = "te";
String st = "st";

String test = new String(te) + new String(st);
```
Will we find "test" as simple text if we take memory dump?
<br />Ans. It'll create 2 string constants in non-heap area "te", "st". There reference will be passed to create 2 new String object which will reside in heap memory. Both will have char[], not string. test will also have `char[]=['t','e','s','t']`, not string.
<br/>Since String constant pool has 2 literals "te", and "st" not "test", you'll not see "test" string in memory dump. Moreover, from JDK7 onwards, String contants can be GCed.


**Q81. We are reading 50 string properties from a property file, and assigning them Property class object. How many constants will be created in String Constant Pool?**
<br />Ans. 0. A string value takes place in constant pool at compilation phase when compiler finds a string literal somewhere in the code.

**Q82. Why is char[] preferred over String for passwords?**
<br />Ans. when we log/print char[], it's hashcode is printed not the actual string.
<br />Strings are like plain test in memory whether in heap area or in non-hea area. So they can be read on memory dump until GCed.

**Q83. What will be the output of following program?**
```java
String a = "text";
String b = new String("text");
System.out.println(a == b);
b = b.intern();
System.out.println(a == b);
String c = "text";
System.out.println(a == c);
```
**Ans**.
```
false
true
true
```
== is used to check the equality of reference. As per String literal rules, Literal strings in any class of any package represent references to the same String object. Hence a,c are pointing to same object. intern() return the object from String pool. So a,b,c are equal in last.

**Q84. What will be the output of following program?**
```java
class MemoryModel { final String s = "abc"; String s5 = "abc";}

:
String s1 = "abc";
MemoryModel mm = new MemoryModel(); 
System.out.println("abc".hashCode() == s1.hashCode());
System.out.println(mm.s.hashCode() == s1.hashCode());
System.out.println(mm.s5.hashCode() == "abc".hashCode());
```
<br/>Ans.
<br/>true
<br/>true
<br/>true

**Q85. Explain the memory distribuion for below code**
```java
class MemoryModel{
  public method(){
      final String s="abc";
      String s1="def";
      final String s2=s+s1;
      String s3=s+"def";
      String s4="abc"+"def";
  }
}
```
[REF](http://stackoverflow.com/a/39687313/453767)
<br/>Ans. Since all the variables are created in method. Nothing will go to Constant Pool. Instead, JVM will create objects for "abc", "def", and "abcdef" in String Literal Pool. And it'll create reference s,s1,s2,s3,s4 in Local variable array of JVM Frame for method(). 


**Q86. What will be the output?**
```java
class MemoryModel{
  public method(){
      final String s="abc";
      String s1="def";
      final String s2=s+s1;
      String s3=s+"def";
      String s4="abc"+"def";
      
      System.out.println(s3 == s4);
      System.out.println(s == "abc");
      System.out.println(s3 == "abcdef");
      System.out.println(s2 == "abcdef");
  }
}
```
<br/>Ans. By the rule of final, String literal and as java use StringBuilder to build strings, the above program will be converted into following;
```java
class MemoryModel{
  public method(){
      final String s="abc";
      String s1="def";
      final String s2=new StringBuilder("abc").append(s1).toString();
      String s3="abc"+"def";
      String s4="abcdef";
      
      System.out.println(s3 == s4);
      System.out.println("abc" == "abc");
      System.out.println(s3 == "abcdef");
      System.out.println(s2 == "abcdef");
  }
}
```
Which will further convert to
```java
class MemoryModel{
  public method(){
    :
      String s3="abcdef";
    :
  }
}
```
Now since "Literal strings in any class of any package represent references to the same String object.";

<br/>true
<br/>true
<br/>true
<br/>false


**Q87. What will be the output?**
```java
class MemoryModel{
  public method(){
      String s="abc";
      String s1="def";
      final String s2=s+s1;
      String s3=s+"def";
      String s4="abc"+"def";
      
      System.out.println(s3 == s4);
      System.out.println(s == "abc");
      System.out.println(s3 == "abcdef");
      System.out.println(s2 == "abcdef");
  }
}
```
<br/>Ans. As 's' is not final, The above code will be converted finally in this;
```java
class MemoryModel{
  public method(){
      String s="abc";
      String s1="def";
      final String s2=new StringBuilder(s).append(s1).toString();
      String s3=new StringBuilder(s).append("def").toString();
      String s4="abc"+"def";
      
      System.out.println(s3 == s4);
      System.out.println(s == "abc");
      System.out.println(s3 == "abcdef");
      System.out.println(s2 == "abcdef");
      System.out.println(s4 == "abcdef");
  }
}
```

<br/>false
<br/>true
<br/>false
<br/>false
<br/>true

**Q88. What is difference between 2 code snippets?**
```java
  String name;
    
    public MemoryModel() {
		name="amit";
	}
```
And 
```java
	String name="amit";
```
<br/>Ans. Both code will compile to same byte code. So no difference.

**Q89. How many objects will be created in below snippet?**
```java
String three ="three";
String s = "one" + "two" + three;
```
Ans. Above code would be tranformed to;
```java
String s = new StringBuilder("onetwo").append(three).toString();
```
So there will 2 objects created for String constant in String pool "onetwo" and "three". Then 1 object s will be created in local variable array.

