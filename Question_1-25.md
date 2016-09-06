**Q1. A parent class has 2 variables final int a = 20; static int b; and child class has 2 variables with same name int a, b;. 
<br/>Can I change value of a? 
<br/>What would be the value of b among all instances of child class?**

Ans.
<br/>As per Concrete class Rule 3, if we don’t use super.a explicitly then it’ll refer member a of child class which is not final so it is changeable.
<br/>As per Inheritance Rule 2; If we point reference of base class then value of a can’t be changed and value of b will be shared among all instances of base class. If we point reference of child class then value of a can be changed and value of b will not be shared among all instances of base class.

**Q2. An interface has a variable int a = 20;
<br/>Can child have member with same name?
<br/>Can child class modify a in its constructor?
<br/>Whether child class member a would become final?**

Ans.
<br/>As per Concrete class Rule 5, we can declare a in child class. And it’ll not become final.
<br/>As per Interface rule 1 and Concrete class Rule 4, value of a can’t be changed.

**Q3. Can we create another instance of same class in its constructor?**
Ans. Yes. But it may fall in loop and can cause JVM Stack OutOfMemory error.

**Q4. Whether a class/ interface can extend itself?**
Ans. As per Common Rules for Concrete class and Interface 1, No.

**Q5. Whether an interface and abstract class can have main()?**
Ans. As per Abstract class rule 1, it can have main(). As per Interface Rule 2, an interface can’t have main().

**Q6. Whether an abstract class can extend**
* abstract class
* concrete class
* Interface

Ans. 
<br/>As per Common Rules for Abstract class and Interface 2, an abstract class can extend another abstract class or can implement another interface. Implementation of abstract methods, of its parent, is optional. If it is not implemented then child concrete class of this abstract class is supposed to implement all abstract methods.
<br/>As per Abstract class Rule 1, it can extend concrete class as well. Also read Inheritance rule 1.

**Q7. Whether a class can bypass implementation of abstract methods?**
<br/>Ans. Yes, if it an abstract class.

**Q8. Why a parent class/interface can refer child class?**
<br/>Ans. To support polymorphism. You can use single entity as entry point to accept all its type.

**Q9. What if a base class has same method an interface has where interface is implemented by child class?**
<br/>Ans. If a base class method is not private then it is the member of child class too. So it is optional to child class to implement the same method again.

**Q10. What if an abstract class and interface has same method signature and a concrete class extends both?**
<br/>Ans. Both are asking to concrete class to implement same method signature (ie same responsibility). So there is no error or exception.

**Q11. Can we re-declare field members in child class with other modifiers?**
<br/>Ans. As per Concrete class rule 5 and answer of Q1, yes.

**Q12. If a child class override method() where method() of parent class throws an IOException but child class’s method() doesn’t, do we need to catch it or throw IOException for further level if we create object of child class and call method()?**
<br/>Ans. As per Inheritance Rule 4, Yes.

**Q13. Why child class reference can’t refer to object of parent class?**
<br/>Ans. Parent-Child relationship defines “type of” relationship. It means child is type of parent. It doesn’t mean child contains parent. 

```
Animal a = new Fox();
Animal b = new Fish();
b = a;
```

If, suppose, child class can refer parent class object then

```
Animal a = new Fox();
Animal b = new Fish();
Fish f = a;
```
will be correct which is against of inheritance concept.

**Q14. Can an abstract class be final?**
<br/>Ans. As per Abstract class Rule 3 and Final rule 1, No.

**Q15. Why a class cannot be static? But a nested class can be static.**
<br/>Ans. Any entity can be static only if it can be used in static way. Since a concrete class can’t have any value because it is just a byte code so it can’t be used in static way. Hence, a concrete class can’t be static.
Nested class follows the rule of member field. So it can be static.

**Q16. Can a non-static nested class have static members? Explain with reason.**
<br/>Ans. As per static rule 3, No.

**Q17. Can a nested class be abstract or static abstract?**
<br/>Ans. As per Nested class Rule 2,yes.

**Q18. Can any concrete class have abstract methods?**
<br/>Ans. As per Concrete class Rule 1, no.

**Q19. Can a non-static abstract class have static members? If it is nested?**
<br/>Ans. Since nested class (whether it is abstract or not) follows rules of member field. So as per Static rule 3 a static nested class can have static or non-static (local variable of static parents are static by default) members. But a non-static nested class can't have static members.

**Q20. Can an anonymous class have abstract methods?**
<br/>Ans. As per Concrete class Rule 1, No.

**Q21. Can we change interface field value in method/constructor of a class implementing that interface.**
<br/>Ans. As per Concrete class Rule 4, and Interface Rule 1, No.

**Q22.Can an abstract class has static final, abstract static final, abstract static, and abstract final methods?**
<br/>Ans. 
<br/>static final – As per Abstract class rule 1, yes.
<br/>abstract static final – As per Abstract Rule 3, Final Rule 2, and Static rule 1, No.
<br/>abstract static - As per Abstract Rule 3, and Static rule 1, No.
<br/>abstract final - As per Abstract Rule 3, and Final Rule 2, No.


**Q23. Why Interface variables are static final by default? But abstract class variables are not?**
<br/>Ans. Since member fields never be overridden and interface can't be instantiated so interface variables are static final. As per abstract class rule 1, abstract class members are simple by default.

**Q24. Can I declare a variable in interface with final and static keyword?**
<br/>Ans. Yes. However it is not required.

**Q25. Can I declare a method in interface with final or static keyword?**
<br/>Ans As per Interface rule 2, Final Rule 2, and Static rule 1, No.
