# Kotlin-Key-Elements

### Kotlin Constructors

In Kotlin, there are two constructors:

- Primary constructor - concise way to initialize a class
- Secondary constructor - allows you to put additional initialization logic

- Primary constructor : 

The primary constructor is part of the class header. Here's an example:

    class Person(val firstName: String, var age: Int) {
        // class body
    }

The primary constructor has a constrained syntax, and cannot contain any code.
To put the initilization code (not only code to initialize properties), initializer block is used. It is prefixed with init keyword.

    fun main(args: Array<String>) {
        val person1 = Person("joe", 25)
    }

    class Person(fName: String, personAge: Int) {
        val firstName: String
        var age: Int

        // initializer block
        init {
            firstName = fName.capitalize()
            age = personAge

            println("First Name = $firstName")
            println("Age = $age")
        }
    }
    
You can provide default value to constructor parameters 

    class Person(_firstName: String = "UNKNOWN", _age: Int = 0) {
        val firstName = _firstName.capitalize()
        var age = _age

        // initializer block
        init {
            println("First Name = $firstName")
            println("Age = $age\n")
        }
    }
    
- Kotlin Secondary Constructor : 
    In Kotlin, a class can also contain one or more secondary constructors. They are created using constructor keyword
    The most common use of secondary constructor comes up when you need to extend a class that provides multiple constructors that initialize the class in different ways
    
       class Log {
            constructor(data: String) {
                // some code
            }
            constructor(data: String, numberOfData: Int) {
                // some code
            }
        }

### Open - Kotlin inheritance

By default, classes in Kotlin are final. If you are familiar with Java, you know that a final class cannot be subclassed. By using the open annotation on a class, compiler allows you to derive new classes from it.

    open class Person(age: Int) {
        // code for eating, talking, walking
    }

    class MathTeacher(age: Int): Person(age) {
        // other features of math teacher
    }

    class Footballer(age: Int): Person(age) {
        // other features of footballer
    }

    class Businessman(age: Int): Person(age) {
        // other features of businessman
    }

- If the class has a primary constructor, the base must be initialized using the parameters of the primary constructor. 
        
        
        open class Person(age: Int, name: String) {
            // some code
        }

        class Footballer(age: Int, name: String, club: String): Person(age, name) {
            init {
                println("Football player $name of age $age and plays for $club.")
            }

            fun playFootball() {
                println("I am playing football.")
            }
        }
    
   
 - In case of no primary constructor, each base class has to initialize the base (using super keyword), or delegate to another constructor
  
        open class Log {
            var data: String = ""
            var numberOfData = 0
            constructor(_data: String) {

            }
            constructor(_data: String, _numberOfData: Int) {
                data = _data
                numberOfData = _numberOfData
                println("$data: $numberOfData times")
            }
        }

        class AuthLog: Log {
            constructor(_data: String): this("From AuthLog -> + $_data", 10) {
            }

            constructor(_data: String, _numberOfData: Int): super(_data, _numberOfData) {
            }
        }

### Overriding Member Functions and Properties

         // Empty primary constructor
        open class Person() {
            open fun displayAge(age: Int) {
                println("My age is $age.")
            }
        }

        class Girl: Person() {

            override fun displayAge(age: Int) {
                println("My fake age is ${age - 5}.")
            }
        }

        fun main(args: Array<String>) {
            val girl = Girl()
            girl.displayAge(31)
        }
### Safe Check

In Kotlin, A normal property can’t hold a null value and will show a compile error.

    var variable : CustomClass = CustomClass()
    variable = null //compilation error
    
Instead, we can add a ? after the data type of that property which declares that variable as a nullable property
    
    var nullableVariable : CustomClass? = CustomClass()
    nullableVariable = null //works fine
    
    variable.someMethodCall() //works fine as the compiler is sure that
                              //the variable can’t be null
    nullableVariable.someMethodCall() //will highlight compilation error
                                      //as compiler is not sure as
                                      //nullVariable can be null.
Explicit Null Check
    
    if ( null != nullableVariable) {
     nullableVariable.someMethodCall()
    } else {
     // fallback flow
    }
    
    
Safe Calls (?.)
Another way of using a nullable property is safe call operator ?.
This calls the method if the property is not null or returns null if that property is null without throwing an NPE (null pointer exception).

nullableVariable?.someMethodCall()
Safe calls are useful in chains. For example, if Bob, an Employee, may be assigned to a Department (or not), that in turn may have another Employee as a department head, then to obtain the name of Bob’s department head (if any), we write the following

bob?.department?.head?.name
Such a chain returns null if any of the properties in it is null.
    
To perform a certain operation only for non-null values, you can use the safe call operator together with let
     
    val listWithNulls: List<String?> = listOf(“A”, null)
    for (item in listWithNulls) {
     item?.let { println(it) } // prints A and ignores null
    }
    
    
    
### Getter and Setter in kotlin

Java code : 

      private String firstName;
      private String lastName;
      //private String name;
      public String getName() {
            return firstName + " " + lastName;
      }
      
      public void setName(String name) {
            String nameArray[] = name.split(" ");
            firstName = nameArray[0];
            lastName = nameArray[1];
       }
       

What will be the equivalent Kotlin code for this?
       
       private var firstName: String? = null
       private var lastName: String? = null
       var name: String
           get() = firstName + " " + lastName
           set(value) {
               val nameArray = value.split(" ".toRegex())
               firstName = nameArray[0]
               lastName = nameArray[1]
           }
           
 1. How can I make setter private?
    Add private keyword before set.

    public var name: String = "John"
    private set
    
 2. How can I override the getter or setter in an extending class?
    Define the variable as open in the base class and use override keyword in the extending class.

         //Base class
         open var age: Int = 0
         get() = 10
          
         //Extending class
         override var age: Int = 0
         get()= 20
      
      
### companion objects
If you are familiar with Java, you may relate companion objects with static methods, even though how they work internally is totally different. Kotlin doesn’t have static members or member functions. 

Before taking about companion objects, let's take an example to access members of a class.

        class Person {
            fun callMe() = println("I'm called.")
        }

        fun main(args: Array<String>) {
            val p1 = Person()

            // calling callMe() method using object p1
            p1.callMe()    
        }
        
Here, we created an object p1 of the Person class to call callMe() method. That's how things normally work.

However, in Kotlin, you can also call callMe() method by using the class name, i.e, Person in this case. For that, you need to create a companion object by marking object declaration with companion keyword.

        class Person {
            companion object Test {
                fun callMe() = println("I'm called.")
            }
        }

        fun main(args: Array<String>) {
            Person.callMe()
        }
        
The name of the companion object is optional and can be omitted.
        
       class Person {
    
            // name of the companion object is omitted
            companion object {
                fun callMe() = println("I'm called.")
            }
        }

        fun main(args: Array<String>) {
            Person.callMe()
        }

 
 
      
  
