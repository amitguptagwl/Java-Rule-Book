##In Progress
**Q52. Why checked exceptions are required be handled?**
<br />Ans. No matter how good you are in coding, there is always a chance of exception due to external events. Eg. drive is not monuted, internet connection is lost, or thread is interuppted etc. Such cases are supposed to be handled.

**Q53. How many objects will be created in the following code?**

```java
String s1="Welcome";  
String s2="Welcome";  
String s3="Welcome";  
```
<br />Ans. 1 object and 3 reference.

**Q54. How many objects will be created in the following code?**
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

**Q55. How many objects will be created in the heap and non-heap memory area in following code?**
```java
String te = "te";
String st = "st";

String test = new String(te) + new String(st);
```
Will we find "test" as simple text if we take memory dump?
<br />Ans. It'll create 2 string constants in non-heap area "te", "st". There reference will be passed to to create 2 new String object which will reside in heap memory. Both will have char[], not string. test will also have `char[]=['t','e','s','t']`, not string.
<br/>Since String constant pool has 2 literals "te", and "st" not "test", you'll not see "test" string in memory dump. Moreover, from JDK7 onwards, String contants can be GCed.


**Q56. We are reading 50 string properties from a property file, and assigning them Property class object. How many constants will be created in String Constant Pool?**
<br />Ans. 0. A string value takes place in constant pool at compilation phase when compiler finds a string literal somewhere in the code.

**Q57. Why is char[] preferred over String for passwords?**
<br />Ans. when we log/print char[], it's hashcode is printed not the actual string.
<br />Strings are like plain test in memory whether in heap area or in non-hea area. So they can be read on memory dump until GCed.

**Q57. What will be the output of following program?**
```java
String a = "text";
String b = new String("text");
System.out.println(a == b);
b = b.intern();
System.out.println(a == b);
```
<br/>Ans.
<br/>false
<br/>true
