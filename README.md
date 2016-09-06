# Java Rule Book
Basic concepts of Java to answer any question about how Java works specially in job interviews. With combining multiple rules you can answer many java question

**Feel free to correct any rule or to suggest any correction needed.**

[![Donate to author](https://www.paypalobjects.com/webstatic/en_US/btn/btn_donate_92x26.png)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=KQJAX48SPUKNC)

##Rules (upto Java 7)

####Final
1. No further change ( So should be assigned either the time of declaration or in constructor)
2. JVM allocates same memory location for same constants.

####Static
1. Static members belongs to a class not any instance.
 a. Static member doesn’t belong to any instance hence they can’t access instanced members.
2. One per class. Shared among all class instances. 
 a. Can’t be overridden. But are inherited.

####Concrete class
1. All methods have some functionality.
2. cannot be static
3. Every member can be written as this.member-name inside member methods.
4. Constructor gets called after initializing memory by JVM.
5. Declaration of any member is fully independent from any other class/interface.
6. You can extend 1 class but multiple interfaces.
7. Can have static member and static block which are executed first in the class and top to bottom.
8. All classes has default constructor, with no statement inside, by default until you create any constructor manually.

####Interface
1. It's all member fields are **public** (since has no implementation private fields can't be used), **final** (since interfance can't be instanciated they can have only 1 value) **static** (since interfance can't be instanciated and member field are not the responsibilities for other class).
2. Methods are public only and can’t be static. (Interface defines external responsibilities (hence public) which may be different for each class (hence non-static).)
3. There is no functionality/implementation, but responsibilities, to be implemented by child class type (class itself or its parent class). So implicitly *abstract* keyword and modifier are optional.
4. Can extend only 1 interface.

#####Common Rules for Concrete class and Interface
1. Cannot extend/implement itself or one of its own member types. If it inherits itself it creates Cycle. If it inherits any child then its hierarchy in inheritance tree becomes inconsistent.
2. Represent the type of its child, sub-child and so on.

####Abstract class
1. Concrete class + Interface = Abstract class
 * Unlike concrete class, it can’t be instantiated.
 * Unlike interface, abstract methods can have any scope and *abstract* keyword is not optional.
 * Unlike interface, fields can’t be static and final.

(Like any simple java class, it may have simple member , static member, static block, constructor etc. It can extend/implement any concrete class, abstract class, or an interface.)

#####Common Rules for Abstract class and Interface
1. Both can have 0 to N number of abstract methods.
2. Both need not to implement abstract methods declared in their parent abstract class/interface.

####Nested/inner class
1. Can access all members of container class.
2. ~~An independent class inside a class which follows rule of a class member.~~ Concrete class rule 5 (So it can be abstract, static abstract, static)


####Anonymous class
1. Cannot be implemented by any class
2. Can access only final external object/var.

####Inheritance
1. A class/interface can reference its own instance or any child class instance whichever can be instantiated.
2. A child class can’t refer to its base class instance.
3. A reference can call method of referred instance only.
4. Child class need not to throw an exception while overriding a method, it is automatically get thrown by child class method.
5. Super() must be first statement in child class constructor. If you don’t call super() explicitly, compiler automatically adds empty super().

####Garbage Collector
1. It fragment and release only heap memory not stack memory.
2. Whether you System.gc(); or objReference=null; it doesn’t invoke GC until JVM really needs it. But 2nd statement helps GC to collect object quickly.
3. finalize() is called only when object memory is released from heap.

####Serialization
1. Transient and static variables are skipped while serialization/deserialization.
2. In alternate of Transient, a class can have *ObjectStreamField[] serialPersistentFields* of class members which can be serialized.
3. While Serialization all default constructor of super classes are called. But deserialization calls default constructor of all non-serialized class from top of inheritance tree till it meets any serialized class. In addition, deserialization 3. doesn’t call constructor of current class.
4. At the time of deserialization, first empty constructor gets called then all deserialized values are assigned to relevant property.

####Threading
1. More than 1 thread can’t access same monitor at a time, if both are in running state.
2. Static synchronized methodName(… takes lock on whole class.
3. synchronized methodName(… takes lock current object.

####Exception
1. **try** must be followed by either **catch** or **finally** or both
2. finally is called ever.
3. Only 1 catch block is called start from top to bottom.

####String
1. == checks for reference only.
2. It overrides equals() which checks for value only.
3. Immutable so treated as constant by JVM.
4. It overrides hashcode().

####Enum 
1. Is Self-Type. Declared as  Enum< E extends Enum< E>>
2. Can implement an interface.

####Other
1. hashCode() returns same value for two objects if obj1.equals(obj2) is true.
2. equals() method must exhibit the following properties:
  * *Symmetry*: For two references, a and b, a.equals(b) if and only if b.equals(a)
  * *Reflexivity*: For all non-null references, a.equals(a)
  * *Transitivity*: If a.equals(b) and b.equals(c), then a.equals(c)
  * *Consistency* with hashCode(): Two equal objects must have the same hashCode() value
