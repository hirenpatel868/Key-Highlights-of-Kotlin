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
        
You can override property of the base class in similar way.
        
         // Empty primary constructor
        open class Person() {
            open var age: Int = 0
                get() = field

                set(value) {
                    field = value
                }
        }

        class Girl: Person() {

            override var age: Int = 0
                get() = field

                set(value) {
                    field = value - 5
                }
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
    
Elvis Operator (?:)
This one is similar to safe calls except the fact that it can return a non-null value if the calling property is null even

val result = nullableVariable?.someMethodCall()
                       ?: fallbackIfNullMethodCall()
The Elvis operator will evaluate the left expression and will return it if it’s not null else will evaluate the right side expression. Please note that the right side expression will only be called if the left side expression is null.


The !! Operator
This operator is used to explicitly tell the compiler that the property is not null and if it’s null, please throw a null pointer exception (NPE)

nullableVariable!!.someMethodCall()
    
    
    
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

 
 
      
  ### vararg : Variable number of arguments 
  
We need a function which takes n number as inputs and returns the average of all the inputs. If the size of n variables is not fixed, we usually convert it into an array or list and pass it to function.
  
      fun getAverage(numbersList: List<Int>): Float {

        var sum = 0.0f
        for (item in numbersList) {
            sum += item
        }
        return (sum / numbersList.size)
    }
    val arrayList = arrayListOf(1, 2, 3, 4)
    val result = getAverage(arrayList)
    
Now, what if the function itself takes n inputs and we don’t need to convert it into an array.

       fun getAverage(vararg input: Int): Float {
        var sum = 0.0f
        for (item in input) {
            sum += item
            }
            return (sum / input.size)
        }
        val result1 = getAverage(1, 2, 3)
        val result2 = getAverage(1, 2, 3, 4, 5)
        
        
        
### Infix Notation : Kotlin
Calling a public function of a class without dot and parentheses of the parameter in Kotlin. Kotlin provides infix notation with which we can call a function with the class object without using a dot and parentheses across the parameter.

        infix fun Int.add(b : Int) : Int = this + b
        val x = 10.add(20)
        val y = 10 add 20        // infix call
        
        
### Equality in Kotlin (‘==’, ‘===’ and ‘.equals’)
== operator is used to compare the data of two variables. == operator in Kotlin only compares the data or variables, whereas in Java or other languages == is generally used to compare the references.
=== operator is used to compare the reference of two variable or object.

### when’ operator in Kotlin
when operator is a replacement of ‘switch’ operator in other languages. Different use cases 

        var intNumber = 10
         when (intNumber) {
            1 -> print(“value is 1”)
            2 -> print(“value is 2”)
            else -> {
               print(“value of intNumber is neither 1 nor 2”)
            }
         }
         
         
         var count = 100
         when (count) {
            in 0 until 100 -> {
            //count is between 1 to 99
            count++
         } 
         else -> {
            //count is greater than 99
            //set it to 0
            count = 0
         }
        }
        
        
        val boolValue : Boolean = false
 
         when (boolValue) {
            false -> {
               print(“value is false”)
            }
            true -> {
               print(“value is true”)
            }
         }


        val shape : Shape = Circle()

         when (shape) {
            is Circle -> print(“shape is a Circle class object”)
            is Square -> print(“shape is a Square class object”)
            else -> {
               print(“invalid shape”)
            }
         }
         
         
          val num = 10
         when {
            isPrime(num) -> print(“num is a prime number”)
            isComposite(num) -> print(“num is a composite number”)
            else -> {
               print(“num is neither prime nor composit”)
            }
         }
