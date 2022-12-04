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

---
>1. Stringizing Operator (#)

The operator # is known as Stringize Operator in C C++ language, it is used in conjunction with #define directive.
\
Stringize Operator (#) is used to turn the argument into quoted string. It transforms the macro parameter to string. It causes the macro parameter to enclose in double quoted string.

Syntax : -

#define macro_function(argument)    #argument

* This will convert whatever is passed as a parameter argument into a string.
* This will delete Leading and trailing white space.
* If we pass the macro, it won’t expand. It will print the string.
* Some characters cannot be stringized – notably, the comma , because it is used to delimit parameters and the right parenthesis ) because it marks the end of the parameter list.
```c++
#include <stdio.h>

// Macro definition with stringize operator
#define PRINT(msg) #msg

int main()
{
	printf(PRINT(This will be the string));
	
	return 0;
}
output -> This will be the string
```

---
>2. Token Pasting Operator (##)

The token pasting operator ## defined by ANSI enables us to combine two tokens within a macro definition to form a single token.
In another word, The token pasting (##) operator simply eliminates any white space around it and concatenates (joins together) the non-whitespace characters together. It is used to create new tokens.

Limitations
* It can only be used in a macro definition.
* It may not be the first or last characters in the replacement text.
* We can only use this method for valid tokens only.

```c++
#include <stdio.h>
#define CONCAT(A,B) A##B
int main()
{
  printf("value1: %d\n",CONCAT(12,20));
  printf("value2: %d\n",CONCAT(12,20)+10);  
  return 0;
}
output-> value1: 1220
        value2: 1230
```

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



