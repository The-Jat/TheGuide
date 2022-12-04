### What is Macro? 
A macro is a piece of code in a program that is replaced by the value of the macro.
\
Macro is defined by #define directive. Whenever a macro name is encountered by the compiler, it replaces the name with the definition of the macro.
\
Macro definitions need not be terminated by a semi-colon(;).

### Types of macros

There are four types of macros:

*    Chain macros

*    Object-like macros

*    Function-like macros

*    Multi-line macros


## Chain macro
Chain macros are defined as the macros inside the macros.
The parent macro is expanded in the first instance and then the child macro is expanded.

```c++

#include <iostream>
using namespace std;
#define Vehicles CARS
#define CARS 5
int main()
{
    cout << "I have " << CARS << " Cars";
 
    return 0;
}

```
Line 7: Here, the vehicle is expanded to produce CARS. After this, the macro is expanded to produce the outcome.

## object-like macros
An object-like macro is defined as the simple identifier that is replaced by the code fragment.
It looks like an object in code. Therefore, it is called an object-like macro.

```c++

#include <iostream>
using namespace std;
#define DATE 15
int main()
{
    cout << "The deadline is " << DATE << "-MAY-2022";
    return 0;
}

```

## function-like macros
Function-like macros work the same as the function call.
It is used to replace the entire code instead of the function name.
Function-like macros can take arguments, just like true functions. To define a macro that uses arguments, you insert parameters between the pair of parentheses in the macro definition that make the macro function-like.
```c++
#define min(X, Y)  ((X) < (Y) ? (X) : (Y))
  x = min(a, b);          →  x = ((a) < (b) ? (a) : (b));
  y = min(1, 2);          →  y = ((1) < (2) ? (1) : (2));
  z = min(a + 28, *p);    →  z = ((a + 28) < (*p) ? (a + 28) : (*p));
```

```c++

#include <iostream>
using namespace std;
#define  PI  3.1416
#define  AREA(r)  (PI*(radius)*(radius))
 
int main() {
     
    float radius = 7;    
       
    cout<<"Area of Circle with Radius " << radius <<": "<< AREA(r);
   
    return 0;
}

```

>Stringizing Operator (#) in C/C++

The operator # is known as Stringize Operator in C language, it is used in conjunction with #define directive.
\
Stringize Operator (#) is used to turn the argument into quoted string.

> Defining Macro with Stringize Operator
\
#define macro_function(argument)    #argument


> Stringizing
Sometimes you may want to convert a macro argument into a string constant. Parameters are not replaced inside string constants, but you can use the ‘#’ preprocessing operator instead.
When a macro parameter is used with a leading ‘#’, the preprocessor replaces it with the literal text of the actual argument, converted to a string constant. Unlike normal parameter replacement, the argument is not macro-expanded first. This is called stringizing.

## multi-line macros
An object-line macro may be of multiple lines.
Therefore, if we want to create a multi-line macro, we have to use the backslash-newline.

```c++

#include <iostream>
using namespace std;
 #define table 2, \
            4, \
            6, \
            8, \
            10, \
            12, \
            14, \
            16, \
            18, \
            20, \
 
int main()
{
    int arr[] = { table };
     printf("The table of 2 is :\n");
 
    for (int i = 0; i < 10; i++) {
        cout << arr[i] << ' ';
    }
    return 0;
}

```



