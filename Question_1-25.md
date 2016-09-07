**Q1. A parent class has 2 variables final int a = 20; static int b; and child class has 2 variables with same name int a, b;. **
<br/>Can I change value of a? 
<br/>What would be the value of b among all instances of child class?

**Ans.**
<br/>As per Inheritance rule or Concrete class rule, variables a,b are different variables than super.a, super.b. So their values can be changed. When I say childObj.b, the closest declaration will be used i.e. non-static b.

**Q2. An interface has a variable int a = 20;**
<br/>Can child have member with same name?
<br/>Can child class modify a in its constructor?
<br/>Whether child class member a would become final?

**Ans.**
<br/>As per Inheritance rule or Concrete class, variable a in child class and interface are different. So child class have variable with 'a' name.
<br/>As per Final rule, value of 'a' can't be changed.
<br/>As per Interface rule or Concrete class Rule, a is independent and will not become final.

**Q3. Can we create another instance of same class in its constructor?**
<br/>Ans. Yes. But it may fall in loop and can cause JVM Stack OutOfMemory error.

**Q4. Whether a class/ interface can extend itself?**
<br/>Ans. As per Common Rules for Concrete class and Interface, No.

**Q5. Whether an interface and abstract class can have main()?**
<br/>Ans. As per Abstract class rule, it can have main(). As per Interface rule, it can have `public void main()`, static abstract methods are not allowed .

**Q6. Whether an abstract class can extend**
* abstract class
* concrete class
* Interface

**Ans.**
<br/>As per Abstract class rule, yes. 
<br/>As per Common Rules for Abstract class and Interface, implementation of abstract methods of parent is optional.

**Q7. Whether a class can bypass implementation of abstract methods?**
<br/>Ans. Yes, if it an abstract class. (As per Common Rules for Abstract class and Interface)

**Q8. Why a parent class/interface can refer child class?**
<br/>Ans. To support polymorphism. You can use single entity as entry point to accept all its type.

**Q9. What if a child class extend a class and implement an interface which have method with same signature?**
<br/>Ans. As per Inheritance rule, yes.

**Q10. What if an abstract class and interface has same method signature and a concrete class extends both?**
<br/>Ans. Same as Q9.

**Q11. Can we re-declare field members in child class with other modifiers?**
<br/>Ans. As per Concrete class rule and answer of Q1, yes.

**Q12. If a child class override method() where method() of parent class throws an IOException but child class’s method() doesn’t, do we need to catch it or throw IOException for further level if we create object of child class and call method()?**
<br/>Ans. As per Inheritance Rule, Yes.

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
<br/>Ans. As per Abstract class Rule and Final rule, No.

**Q15. Why a class cannot be static? But a nested class can be static.**
<br/>Ans. Any entity can be static only if it can be used in static way. Since a concrete class can’t have any value because it is just a byte code so it can’t be used in static way. Hence, a concrete class can’t be static.
Nested class follows the rule of member field. So it can be static.

**Q16. Can a non-static nested class have static members? Explain with reason.**
<br/>Ans. As per static rule, No.

**Q17. Can a nested class be abstract or static abstract?**
<br/>Ans. As per Nested class Rule,yes.

**Q18. Can any concrete class have abstract methods?**
<br/>Ans. As per Concrete class Rule, no.

**Q19. Can a non-static abstract class have static members? If it is nested?**
<br/>Ans. Since nested class (whether it is abstract or not) follows rules of member field. So as per Static rule a static nested class can have static or non-static (local variable of static parents are static by default) members. But a non-static nested class can't have static members.

**Q20. Can an anonymous class have abstract methods?**
<br/>Ans. As per Concrete class Rule, No.

**Q21. Can we change interface field value in method/constructor of a class implementing that interface.**
<br/>Ans. As per Concrete class Rule, and Interface Rule, No.

**Q22.Can an abstract class has static final, abstract static final, abstract static, and abstract final methods?**
<br/>Ans. 
<br/>static final – As per Abstract class rule, yes.
<br/>abstract static final – As per Abstract Rule, Final Rule, and Static rule, No.
<br/>abstract static - As per Abstract Rule, and Static rule, No.
<br/>abstract final - As per Abstract Rule, and Final Rule, No.


**Q23. Why Interface variables are static final by default? But abstract class variables are not?**
<br/>Ans. Since member fields never be overridden and interface can't be instantiated so interface variables are static final. As per abstract class rule, abstract class members are simple by default.

**Q24. Can I declare a variable in interface with final and static keyword?**
<br/>Ans. Yes. However it is not required.

**Q25. Can I declare a method in interface with final or static keyword?**
<br/>Ans As per Interface rule, Final Rule, and Static rule, No.
