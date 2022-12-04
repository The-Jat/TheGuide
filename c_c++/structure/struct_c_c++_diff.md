## C and C++ structures.
| C structure | C++ structures |
| ----------- | ----------- |
| Only data members are allowed, it cannot have member functions.	  |  Can hold both: member functions and data members |
|   We can not assign a default value to the members|   We can assign the default value to the members.|
|   C structures cannot have static members| It can have static members.  |
| It cannot have constructors and destructors inside a structure.  | Constructor and destructors creation is allowed.  |
| Direct Initialization of data members is not possible.  | Direct Initialization of data members is possible.  |
| Writing the ‘struct’ keyword is necessary to declare structure-type variables.  | Writing the ‘struct’ keyword is not necessary to declare structure-type variables.  |
| Does not have access modifiers.  | It supports access modifiers.  |
| Does not support inheritance.  | It supports inheritance.  |
| Sizeof operator will generate 0  for an empty structure.  | Sizeof operator will generate 1 for an empty structure.  |
| Data Hiding is not possible.  | Data Hiding is possible.  |

---
**1. Member functions ( function inside the structure):**
In C: NO
We can not create a function using the structure. If you will try to implement the function in C structures, you will get the compiler error.
```
struct A
{
    //member function
    void foo() //<---- compiler error
    {
    }
};
 ```

In C++: Yes
We can create a member function in C++ structures like the class.
```
struct A
{
    //member function
    void foo()
    {
    }
};
 ```

 

**2. Default values to member variable:**
In C: No
It is not possible to assign a default value to the structure attribute.
```
struct A
{
    //member variable
    int data = 7; //<--- compiler error
};
```
The structure is a data type. You don`t give values to a data type. You give values to variables of data types. See the below example,
```
#include<stdio.h>
struct A
{
    //member variable
    int data;
};
int main()
{
    //_A is structure variable
    struct A _A = {5};
    printf("_A.data = %d\n", _A.data);
    return 0;
}
Output: _A.data = 5
```
 

In C++: Yes
Yes, we can assign the default value.
```
struct A
{
    //member variable
    int data = 7;
};
 ```

**3. static members variables:**
In C: No
It is not possible to create a static member variable in C.
```
struct A
{
    //member variable
    static int data; //<--- compiler error
};
 ```

In C++: Yes
Yes, we can create a static member variable in C++.
```
struct A
{
    //member variable
    static int data;
};
 ```

**4. Instantiation:**
In C during instantiation, we have to use the struct keyword whereas in C++ we don’t need to use the struct keyword.

In C:
```
#include<stdio.h>
struct A
{
    //member variable
    int data1;
    int data2;
};
int main()
{
    struct A sData = {27, 24};
    printf("%d %d", sData.data1,sData.data2);
    return 0;
}
Output: 27 24
```
 

In C++:
```
#include<stdio.h>
struct A
{
    //member variable
    int data1;
    int data2;
};
int main()
{
    /*Not necessary to use struct
    keyword*/
    A sData = {27, 24};
    printf("%d %d", sData.data1,sData.data2);
    return 0;
}
Output: 27 24
```
 

**5. Constructor and Destructor:**
In C: Not possible.
In C we can not create a constructor inside a structure.
```
#include<stdio.h>
struct A
{
    A() //<--- compiler error
    {
    }
};
int main()
{
    return 0;
}
 ```

In C++: Yes possible.
We can create a constructor inside a structure in C++.
```
#include<stdio.h>
struct A
{
    A()
    {
    }
};
int main()
{
    return 0;
}
 ```

**6. Access modifiers:**
C++ structures have access modifiers but C structures do not have these modifiers. C++ has three access modifiers, public, private, and protected.

 

**7. sizeof():**
A sizeof operator will give 0 for an empty structure in C whereas 1 for an empty structure in C++.

In C:
```
#include<stdio.h>
struct A
{
//empty struct
};
int main()
{
    printf("%d", sizeof(struct A));
    return 0;
}
Output: 0
```
 

In C++:
```
#include<iostream>
struct A
{
 //empty struct
};
int main()
{
    std::cout << sizeof(A);
    return 0;
}
Output: 1
```
 

**8. Inheritance and data hiding:**
C structures do not support inheritance and data hiding because C is not an object-oriented language.

 
