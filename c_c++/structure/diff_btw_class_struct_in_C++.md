### Difference between Structure and Class in C++

##### Key Difference between Information and Knowledge
* The structure is a user-defined data type that combines logically related data items of different data types, whereas a Class is a blueprint or a set of instructions to build specific types of objects.

* The structure is a value type data type, whereas class is a reference type data type.
* You cannot change the default constructor of the structure, whereas you can change class default constructor.
* Structure cannot have a destructor; on the other hand, class can have a destructor.
* Class is used in small programs, while the structure is used in large programs.
* In structure, all the value types are allocated on the stack, while in class, all value types are allocated on the heap.
* Instances of ‘structure’ are called ‘structure variables’ whereas instances of a ‘class’ are called ‘objects.’
* Structure member variables cannot be initiated directly, while class member variables can be initiated directly.


##### What is the Structure in C++?
Structure is a user-defined data type that combines logically related data items of different data types like float, char, int, etc., together.

All the structure elements are stored at contiguous memory locations. Structure type variable helps you store more than one data item of varying data types under one name.

#### Syntax of Structure in C++
Here is a syntax of Structure:
```
struct struct_name
{
// struct data members
}
```
In the above syntax, the struct keyword is used. The struct_name is the name of the structure. The struct members are added within curly braces. These members probably belong to different data types.

#### What is Class in C++?
Class is a blueprint or a set of instructions to build a specific type of object. It is a basic concept of Object-Oriented Programming which revolves around real-life entities. Class in a programming language determines how an object will behave and what the object will contain.

Class is also a user-defined data type which can have different kinds of data types and member functions inside its body.

Syntax of Class in C++
Here is a syntax of Class:
```
class class-name
{
// data
// functions
};
```
In the above syntax, the class name is the name that is assigned to the class. The data is the member of the class, normally declared as variables. The functions here are the class functions.
#### Structure vs Class in C++
Here is the main difference between Structure and Class in C++:

|Structure|Class|
| ----------- | ----------- |
| Structure is a user-defined data type that combines logically related data items of different data types like char, float, int, etc., together. | Class is a blueprint or a set of instructions to build a specific type of object. |
|Structure can be declared using the struct keyword.|It can be declared using the class keyword.|
|It is a value type data type.|It is a reference type data type.|
|You cannot inherit it from other structures or classes.|You can inherit it from other structures or classes.|
|It can instantiate objects without using a new keyword.|It can instantiate an object using a new keyword.|
|You cannot change the default constructor of structure.|You can change class default constructor.|
|Structure cannot have a destructor.|Class can have a destructor.|
|In structure, all the value types are allocated on stack.|In class, all value types are allocated on heap.|
|It is used in small programs.|It is used in large programs.|
|Member functions cannot be abstract or virtual.|Member functions can be abstract or virtual.|
|Structure instances are called ‘structure variables.’|Class instances are called objects.|
|It cannot have null values.|It can have null values.|
|Structure member variables cannot be initiated directly.|Class member variables can be initiated directly.|
|If you have not declared any access specifier, then the members of the structure are public.|If you have not declared any access specifier, then the members of class are private.|
