# Kotlin-Key-Elements


## Safe Check

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

 
 
      
  
