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
2. Above code can give **Unreachable code** compilation error if there is any statement after finally block.

**Q61. What will be the output of below program?**
```java
public static int method(){
	int i = 1;
	try{
		i++;
		return ++i;
	}catch(Exception e){
		i++;
		return ++i;
	}finally{
		i++;
		System.out.println(i);
		return ++i;
	}
}

System.out.println(method());
```
<br />Ans. 
<br />4
<br />5

**Q62. What will be the output of below program?**
```java
public static int method(){
	int i = 1;
	try{
		i++;
		return ++i;
	}catch(Exception e){
		i++;
		return ++i;
	}finally{
		i++;
		System.out.println(i);
	}
}

System.out.println(method());
```
<br />Ans. 
<br />4
<br />3


**Serialization**

```java
public class GrandParent {
	public GrandParent() {
		System.out.println("Grand Parent");
	}
}

public class Parent extends GrandParent implements Serializable{
	public Parent() {
		System.out.println("Parent");
	}
}

public class Child extends Parent{
	public Child() {
		System.out.println("Child");
	}
	public static void main(String[] args) throws Exception {
		Child child = new Child();
		
		ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("fileName"));
		out.writeObject(child);
		
		System.out.println("Deserialing");
		
		ObjectInputStream in = new ObjectInputStream(new FileInputStream("fileName"));
		Child child2 = (Child) in.readObject();
	}
}

```

Output

```
Grand Parent
Parent
Child
Deserialing
Grand Parent
```

**Q63. If C extends B extends A where**

1. A is serializable
2. B is serializable
3. A and B both are serializable
4. A and B both are non-serializable

**Which constructors will be called by Deserialization?**

<br />Ans. As per serialization rule 3,

1. No constructor call. Because deserialization finds serializable class in starting
2. A()
3. No constructor call
4. A() then B()

**Q64. If C extends B extends A where**

1. A is serializable
2. B is serializable
3. A and B both are serializable
4. A and B both are non-serializable

Where A has no default constructor but parameterized constructor().

**Which constructors will be called by Deserialization?**
<br />Ans. 

1. No constructor call. Because deserialization finds serializable class in starting
2. It’ll give compilation error since, as per Inheritance rule, parent class always must have default constructor or child class constructor must have explicit call to parameterized constructor of parent class.
<br/> However if child class call parameterized constructor of super class then, it'll throw runtime exception at the time of deserialization: "java.io.InvalidClassException".
3. No constructor call
4. Same as point 2

**Enum**

**Q65. Why an enum can not extend another enum or class but an interface while the java compiler translates it into class later?**
<br />And. As per Enum rule and inheritance, it already extends Enum<T>. That class provides all the enum functionality. 

**Q66. Whether an enum can have abstract methods?**
<br />Ans. Yes. Since all the instances declared inside the enum can have body of anonymous class. So they have to override abstract

**Q67. Explain below code**
```java
List<Fruit> fruits = new ArrayList<Fruit>();
fruits.add(new Apple());
fruits.add(new Strawberry());

List<Fruit> fruits = new ArrayList<Apple>();//Compilation Error: not allowed
```
<br/>Ans. As per Generics rule, List<Apple> is not sub-type of List<Fruit>. But since Apple,Strawberry are type of Fruit List.add() allows any type of Fruit.

**Q68. Explain below code**
```java
Apple[] apples = new Apple[1];
Fruit[] fruits = apples;
fruits[0] = new Strawberry();//Runtime Exception

List<Apple> apples = new ArrayList<Apple>();
//List<Fruit> fruits = apples; //Compile time error
List<? extends Fruit> fruits = apples;
//fruits.add(new Strawberry()); //Compile time error
//fruits.add(new Fruit()); //Compile time error
//fruits.add(new Apple()); //Compile time error
Fruit fruit = fruits.get(0);


List<? super Apple> apples = new ArrayList<Fruit>();
apples.add(new Strawberry()); //Compile time error
apples.add(new Fruit()); //Compile time error
apples.add(new Apple()); 

```
<br />Ans. By Generic rules, `List<Apple>` is subtype of  `List<? extends Fruit>` so list of apples can be assigned to list of fruits. 
<br />But since list of fruits can point to list of any type of fruits, `fruits.get()` doesn't gurantee what will come out but it is sure that it'll a fruit. Hence only `Fruit fruit = fruits.get(0)` is allowed.
<br />Now, similarly, since list of fruits can point to list of any type of fruits, adding particular type of fruit may be wrong. So suppose you try to add Apple but list of fruits is pointing to list of Strawberries. So you can not add any type of fruite. Now with the same reason, you can't add `new Fruit()` because parent class object can't be assigned to child type reference.

<br />In case of super, list of fruits or list of objects can be assigned to list of apples.
<br />But while reading, you can't guarantee what will come out. However it is guaranteed to be Object.
<br />While adding, since list of apples can point either list of apples itself or the list of it's super classes so any object of Apple or it's subtype can be added. But instance of Fruit or it's other type can't be added.

**Q69. What is the following class converted to after type erasure?**
```java
public class Pair<K, V> {

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey(); { return key; }
    public V getValue(); { return value; }

    public void setKey(K key)     { this.key = key; }
    public void setValue(V value) { this.value = value; }

    private K key;
    private V value;
}
```
Ans.
```java
public class Pair {

    public Pair(Object key, Object value) {
        this.key = key;
        this.value = value;
    }

    public Object getKey()   { return key; }
    public Object getValue() { return value; }

    public void setKey(Object key)     { this.key = key; }
    public void setValue(Object value) { this.value = value; }

    private Object key;
    private Object value;
}
```

**Q70. What is the following method converted to after type erasure?**
```java
public static <T extends Comparable<T>>
    int findFirstGreaterThan(T[] at, T elem) {
    // ...
}
```

Ans.
```java
public static int findFirstGreaterThan(Comparable[] at, Comparable elem) {
    // ...
}
```
**Q71. Justify bewlow code snippet**
```java
SomeClass<?> someObj = new SomeClass<B>();
//but
someObj.someMethod(new A());//not allowed
someObj.someMethod(new B());//not allowed
someObj.someMethod(null);
```
Ans. ? represents no type. So A,B are definetely not type of ?. Hence only null is allowed.

**Q72. What is the advantage of code given in Q69?**
<br/>Ans. If we write the above code in following way;
```java
SomeClass<B> someBObj = new SomeClass<B>();
someBObj.someMethod(new B());
someBObj.someMethod(new B());
:
SomeClass<?> readOnlyList = someBObj;
B b = (B) readOnlyList.get(0);
```

**Q73 Will the following class compile? If not, why?**
```java
public final class Algorithm {
    public static <T> T max(T x, T y) {
        return x > y ? x : y;
    }
}
```
Ans. No. The greater than (>) operator applies only to primitive numeric types.

**Q74: Does following code is valid?**
```java
T t = new T();

//or
class Foo<T> {
 public void doSomething(T t) {
 	t.sort();
 }
}
```
Ans. As T is unkown, above code is not allowed. However following code can be used instead;
```java
class Foo<T extends SomeClass> {
 public void doSomething(T t) {
 	t.sort();//  sort method presents in SomeClass
 }
}
```
**NIO**

**Q75: Why FileChannel can't be use with Selector?**
<br /> Ans: As FileChannel is blocking, by the rules, it can't be used with Selectors.

**Q76: Does NIO are always faster than IO?**
<br /> Ans: As NIO use less threads, it saves time and memory in thread(context) switching. Even the CPU utilization is less. But performance completely depends on what way code is written and what is being performed. In general NIO are faster.
