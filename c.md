# C 語言標準 
## C99  1999-12-16
## C11 2011-12-15
## C17 2018
## C23 now
 
編譯器 包含C99 (90%)標準。
Clang
GCC

clang -Wall -Wextra -Werror -o hello hello.c

gcc -Wall -Wextra -Werror -o hello hello.c

-Wall -Wextra -Werror 印出編譯時期警告錯誤
-o 自動程式碼優化

commant

/* this is a comment */

// this is a comment  version >= C99


??/ 被編譯器當作換行



```
int foo = 20; // Start at 20 ??/
int bar = 0;
// The following will cause a compilation error (undeclared variable 'bar')
// because 'int bar = 0;' is part of the comment on the preceding line
bar += foo;
```

# Data Types


\*  "dereference" 

\[] "array"

\() “function call”

thing[x]  x大小的array

thing(x,y,z) function thing taking x,y,z

*thing  a pointer to thing

```
char *names[20];
// 一個size為20的array，20個指向char的pointer

char (*place)[10];

int fn(long, short)
//fn is a function taking long and short and returning int
int *fn(void)
//the  fn is a function taking void and returning a pointer to int

int *ptr
//a pointer to an int;

int **ptr
//ptr is a pointer to a pointer to an int

int (*fp)(void);
//function pointer
//Overriding the precedence of (): fp: a pointer to a function taking int and returning int;

int fn(void), *ptr, (*fp)(int), arr[10][20], num;
//連續宣告, 都指回int

```

```
uint32_t u32 = 32; /* exactly 32-bits wide */
uint8_t u8 = 255;  /* exactly 8-bits wide */
int64_t i64 = -65  /* exactly 64 bit in two's complement representation */


signed char c = 127; /* required to be 1 byte, see remarks for further information. */
signed short int si = 32767; /* required to be at least 16 bits. */
signed int i = 32767; /* required to be at least 16 bits */
signed long int li = 2147483647; /* required to be at least 32 bits. */
```

For char
If you don't explicitly specify the "signed" or "unsigned" keyword before the "char" data type in C, the default behavior is implementation-defined.

This means that the "char" data type may be signed or unsigned depending on the compiler and platform being used. In practice, most modern compilers default to using "signed char" as the default type, but you should not rely on this behavior, especially when writing portable code.

Therefore, it's always a good practice to explicitly specify whether a "char" is signed or unsigned when writing C code, to avoid any potential issues with platform-specific behavior or changes in default behavior in future compiler versions.

  
```
signed long long int li = 2147483647; /* required to be at least 64 bits */
```

```
int d = 42;   /* decimal constant (base10) */
int o = 052;  /* octal constant (base8) */
int x = 0xaf; /* hexadecimal constants (base16) */
int X = 0XAf;
```
以上四個會是相同數值。

suffix for float double。浮點數前綴
```
float f = 0.314f;
long double ld = 0.314l

//double sd is 1200.0
double sd = 1.2e3

//char string 
//if there is no terminator, print char string may error.
char a1[] = "abc"; /* a1 is char[4] holding {'a','b','c','\0'} */
char a3[3] = "abc"; /* a1 is char[3] holding {'a','b','c'}, missing the '\0' */
```

```
//if string defined like here with *. string is read-only.
char* s = "foobar";
s[0] = 'F'; /* undefined behaviour */

char const* s1 = "foobar";  /*好的作法是將這類型的定義加上const*/
s1[0] = 'F'; /* compiler error! */


/* concatenation is implementation defined */
char* s1 = "Hello" ", " "World";
```
```
/* common usages are concatenations of format strings */
char* fmt = "%" PRId16; /* PRId16 macro since C99 */
```
PRId16 is a macro defined in the "inttypes.h" header file in C99 and later standards. The macro is used to format an int16_t type (16-bit signed integer) value as a string for printing or output.
The PRId16 macro is part of a set of macros defined in the "inttypes.h" header file that are used for portable formatting of integer types. The macro expands to a string literal that specifies the appropriate format specifier for an int16_t type, depending on the specific platform and implementation.

For example, on a platform where the int16_t type is equivalent to the "short" type, the PRId16 macro might expand to the string literal "%hd", which is the format specifier for a "short" type in the printf() function. On another platform where int16_t is defined differently, the PRId16 macro might expand to a different format specifier.

Using the PRId16 macro ensures that the code is portable and can be compiled on different platforms with different data type sizes and formatting requirements.
```
#include <stdio.h>
#include <inttypes.h>

int main() {
    int16_t num = -12345;
    printf("The value of num is: %" PRId16 "\n", num);
    return 0;
}
```


```
/*A char is usually 1 byte in size, while a wchar_t is typically 2 or 4 bytes in size.*/
wchar_t* s2 = L"abc";

/* UTF-8 string literal, of type char[] */
char* s3 = u8"abc";

/* 16-bit wide string literal, of type char16_t[] */
char16_t* s4 = u"abc";
/* 32-bit wide string literal, of type char32_t[] */
char32_t* s5 = U"abc";
```

# operator
* &&  條件式同時成立輸出1
* ||  條件式一者成立輸出1
* ==  條件式相等成立輸出1
* !=  條件式不相等輸出1



* ? 三元運算子 ternary operator
```
a = b ? c : d;
```
```
is equivalent to:
if (b)
    a = c;
else
    a = d;
```

ex
```
int x = 5;
int y = 42;
printf("%i, %i\n", 1 ? x : y, 0 ? x : y); /* Outputs "5, 42" */
```
1為成立輸出前者x，0為不成立輸出後者y。



# Bitwise Operators

* & bitwise AND
* | bitwise inclusive OR
* ^ bitwise exclusive OR (XOR)
* ~ bitwise not (one's complement)
* << logical left shift
* \>> logical right shift

```
unsigned int a = 29;    /* 29 = 0001 1101 */  
unsigned int b = 48;    /* 48 = 0011 0000 */
int c = 0;          
c = a & b;              /* 32 = 0001 0000 */
printf("%d & %d = %d\n", a, b, c );
c = a | b;              /* 61 = 0011 1101 */
printf("%d | %d = %d\n", a, b, c );
c = a ^ b;              /* 45 = 0010 1101 */
printf("%d ^ %d = %d\n", a, b, c );
c = ~a;                 /* -30 = 1110 0010 */
printf("~%d = %d\n", a, c );
c = a << 2;             /* 116 = 0111 0100 */
printf("%d << 2 = %d\n", a, c );
c = a >> 2;             /* 7 = 0000 0111 */
printf("%d >> 2 = %d\n", a, c );
return 0;
```

### Masking

使用AND當作mask可以保留目標。mask是0s被濾掉，要保留部分為1s
```
1010101111101
AND
1111000001111
EQUAL
1010000001101
```
中間五個bit被濾掉

使用OR 當作mask 也可以。mask是0s保留，濾掉部分為1s
```
1010101111101
OR
1111000001111
EQUAL
1111101111111
```

The following function uses a mask to display the bit pattern of a variable:
```
#include <limits.h>
void bit_pattern(int u)
{
    int i, x, word;
    unsigned mask = 1;

    word = CHAR_BIT * sizeof(int);
    mask = mask << (word - 1);    /* shift 1 to the leftmost position */
    //mask as 1000000000000000
    for(i = 1; i <= word; i++)
    {
        x = (u & mask) ? 1 : 0;  /* identify the bit */
        printf("%d", x);         /* print bit value */
        mask >>= 1;              /* shift mask to the right by 1 bit */
    }
}
```

> Increment / Decrement Operators

`++ ` and `--`

The placement of the
operator changes the timing of the incrementation/decrementation of the value to before or after assigning it to
the variable.

```
#include <stdio.h>
int main(void)
{
    int a = 1;
    int b = 4;

    int c = 1;
    int d = 4;
    a++;
    printf("a = %d\n",a);    /* Will output "a = 2" */
    b--;
    printf("b = %d\n",b);    /* Will output "b = 3" */
    if (++c > 1) { /* c is incremented by 1 before being compared in the condition */
        printf("This will print\n");    /* This is printed */
    } else {
        printf("This will never print\n");    /* This is not printed */
    }
    if (d-- < 4) {  /* d is decremented after being compared */
        printf("This will never print\n");    /* This is not printed */
    } else {
        printf("This will print\n");    /* This is printed */
    }
}
```

# Access Operators


## Member of object
> `.`
```
struct MyStruct
{
    int x;
    int y;
};
struct MyStruct myObject;
myObject.x = 42;
myObject.y = 123;

printf(".x = %i, .y = %i\n", myObject.x, myObject.y); 
/* Outputs ".x = 42, .y = 123". */
```

## Member of pointed-to object
>`x->y`

same as 

>`(*x).y`

```
struct MyStruct
{
    int x;
    int y;
};
struct MyStruct myObject;
struct MyStruct *p = &myObject;
p->x = 42;
p->y = 123;
printf(".x = %i, .y = %i\n", p->x, p->y); 
/* Outputs ".x = 42, .y = 123". */
printf(".x = %i, .y = %i\n", myObject.x, myObject.y); 
/* Also outputs ".x = 42, .y = 123". */
```

## Address-of
>`&`
```
int x = 3;
int *p = &x;
printf("%p = %p\n", （void *)&x, (void *)p);
/* Outputs "A = A", for some implementation-defined A.*/
```
## Dereference
>`*`
```
int x = 42;
int *p = &x;
printf("x = %d, *p = %d\n", x, *p); 
/* Outputs "x = 42, *p = 42". */
*p = 123;
printf("x = %d, *p = %d\n", x, *p); 
/* Outputs "x = 123, *p = 123". */
```

## Indexing
a[i] is equivalent to *(a + i)
```
int arr[] = { 1, 2, 3, 4, 5 };
printf("arr[2] = %i\n", arr[2]); 
/* Outputs "arr[2] = 3". */
```

## Interchangeability of indexing
Adding a pointer to an integer is a commutative operation (i.e. the order of the operands does not change the
result)
> pointer + integer == integer + pointer.


int arr[] = { 1, 2, 3, 4, 5 };
```
printf("3[arr] = %i\n", 3[arr]); 
/* Outputs "3[arr] = 4". */
```

# Cast Operator
```
int x = 3;
int y = 4;
printf("%f\n", (double)x / y); 
/* Outputs "0.750000". */
```


# Function Call Operator
```
int myFunction(int x, int y)
{
    return x * 2 + y;
}
int (*fn)(int, int) = &myFunction;
int x = 42;
int y = 123;
printf("(*fn)(%i, %i) = %i\n", x, y, (*fn)(x, y)); 
    /* Outputs "fn(42, 123) = 207". */
printf("fn(%i, %i) = %i\n", x, y, fn(x, y));
    /* Another form: you don't need to dereference
explicitly */
```

# Logical AND
```
0 && 0  /* Returns 0. */
0 && 1  /* Returns 0. */
2 && 0  /* Returns 0. */
2 && 3  /* Returns 1. */
```

# Logical OR
```
0 || 0  /* Returns 0. */
0 || 1  /* Returns 1.  */
2 || 0  /* Returns 1.  */
2 || 3  /* Returns 1.  */
```
# Logical 
```
!1 /* Returns 0. */
!5 /* Returns 0. */
!0 /* Returns 1. */
```

# Pointer Arithmetic
Pointer addition(加pointer位置) 

```
#include<stdio.h>
static const size_t N = 5
   
int main()
{
    size_t k = 0;
    int arr[] = {1, 2, 3, 4, 5};
    for(k = 0; k < N; k++)
    {
        printf("\n\t%d", *(arr + k));
    }
    return 0;
}
```
same as
```
#include<stdio.h>
static const size_t N = 5
   
int main()
{
    size_t k = 0;
    int arr[] = {1, 2, 3, 4, 5};
    int *ptr = arr; /* or int *ptr = &arr[0]; */
    for(k = 0; k < N; k++)
    {
        printf("\n\t%d", ptr[k]);
        /* or   printf("\n\t%d", *(ptr + k)); */
        /* or   printf("\n\t%d", *ptr++); */
    }
    return 0;
}
```

# _Alignof

This operator is often accessed through the convenience macro alignof from <stdalign.h>.

```
int main(void)
{
    printf("Alignment of char = %zu\n", alignof(char));
    printf("Alignment of max_align_t = %zu\n", alignof(max_align_t));
    printf("alignof(float[10]) = %zu\n", alignof(float[10]));
    printf("alignof(struct{char c; int n;}) = %zu\n",
            alignof(struct {char c; int n;}));    
}
```
Possible Output:
```
Alignment of char = 1
Alignment of max_align_t = 16
alignof(float[10]) = 4
alignof(struct{char c; int n;}) = 4
```

"sizeof" tells you the total size of an object or type, while "_Alignof" tells you the minimum number of bytes that the object or type requires to be properly aligned in memory. Both operators are important for understanding the memory layout and alignment requirements of C data types.

# Declarations
foo.h
```
#ifndef FOO_DOT_H    /* This is an "include guard" */
#define FOO_DOT_H    /* prevents the file from being included twice. */
                     /* Including a header file twice causes all kinds */
                     /* of interesting problems.*/
/**
 * This is a function declaration.
 * It tells the compiler that the function exists somewhere.
 */
void foo(int id, char *name);
#endif /* FOO_DOT_H */
```
foo.c
```
#include "foo.h"    /* Always include the header file that declares something
                     * in the C file that defines it. This makes sure that the
                     * declaration and definition are always in-sync.  Put this
                     * header first in foo.c to ensure the header is self-contained.
                     */
#include <stdio.h>
                       
/**
 * This is the function definition.
 * It is the actual body of the function which was declared elsewhere.
 */
void foo(int id, char *name)
{
    fprintf(stderr, "foo(%d, \"%s\");\n", id, name);
    /* This will print how foo was called to stderr - standard error.
     * e.g., foo(42, "Hi!") will print `foo(42, "Hi!")`
     */
}
```
main.c
```
#include "foo.h"
int main(void)
{
    foo(42, "bar");
    return 0;
}
```

__Compile and Link__
```
//compile
$ gcc -Wall -c foo.c
$ gcc -Wall -c main.c

//link
$ gcc -o testprogram foo.o main.o
```

## Using a Global Variable

Use of global variables is generally discouraged.
It makes your program more diﬃcult to understand, and harder to
debug. But sometimes using a global variable is acceptable.

global.h
```
#ifndef GLOBAL_DOT_H    /* This is an "include guard" */
#define GLOBAL_DOT_H
/**
 * This tells the compiler that g_myglobal exists somewhere.
 * Without "extern", this would create a new variable named
 * g_myglobal in _every file_ that included it. Don't miss this!
 */
extern int g_myglobal; /* _Declare_ g_myglobal, that is promise it will be _defined_ by
                        * some module. */
#endif /* GLOBAL_DOT_H */
```
global.c
```
#include "global.h" /* Always include the header file that declares something
                     * in the C file that defines it. This makes sure that the
                     * declaration and definition are always in-sync.
                     */
                       
int g_myglobal;     /* _Define_ my_global. As living in global scope it gets initialised to 0
                     * on program start-up. */
```

main.c
```
#include "global.h"
int main(void)
{
    g_myglobal = 42;
    return 0;
}
```

__Using Global Constants__

Headers may be used to declare globally used read-only resources, like string tables for example.

Declare those in a separate header which gets included by any ﬁle ("Translation Unit") which wants to make use of
them. It's handy to use the same header to declare a related enumeration to identify all string-resources:

resources.h:
```
#ifndef RESOURCES_H
#define RESOURCES_H
typedef enum { /* Define a type describing the possible valid resource IDs. */
  RESOURCE_UNDEFINED = -1, /* To be used to initialise any EnumResourceID typed variable to be
                               marked as "not in use", "not in list", "undefined", wtf.
                              Will say un-initialised on application level, not on language level.
Initialised uninitialised, so to say ;-)
                              Its like NULL for pointers ;-)*/
  RESOURCE_UNKNOWN = 0,    /* To be used if the application uses some resource ID,
                              for which we do not have a table entry defined, a fall back in
                              case we _need_ to display something, but do not find anything
                              appropriate. */
  /* The following identify the resources we have defined: */
  RESOURCE_OK,
  RESOURCE_CANCEL,
  RESOURCE_ABORT,
  /* Insert more here. */
  RESOURCE_MAX /* The maximum number of resources defined. */
} EnumResourceID;

extern const char * const resources[RESOURCE_MAX]; /* Declare, promise to anybody who includes
                                      this, that at linkage-time this symbol will be around.
                                      The 1st const guarantees the strings will not change,
                                      the 2nd const guarantees the string-table entries
                                      will never suddenly point somewhere else as set during
                                      initialisation. */
#endif
```

resources.c:
```
#include "resources.h" /* To make sure clashes between declaration and definition are
                          recognised by the compiler include the declaring header into
                          the implementing, defining translation unit (.c file).
/* Define the resources. Keep the promise made in resources.h. */
const char * const resources[RESOURCE_MAX] = {
  "<unknown>",
  "OK",
  "Cancel",
  "Abort"
};
```

main.c:
```
#include <stdlib.h> /* for EXIT_SUCCESS */
#include <stdio.h>
#include "resources.h"

int main(void)
{
  EnumResourceID resource_id = RESOURCE_UNDEFINED;
  while ((++resource_id) < RESOURCE_MAX)
  {
      printf("resource ID: %d, resource: '%s'\n", resource_id, resources[resource_id]);
  }
  return EXIT_SUCCESS;
}
```
```
gcc -Wall -Wextra -pedantic -Wconversion -g  main.c resources.c -o main
```
```
//output:
resource ID: 0, resource: ''
resource ID: 1, resource: 'OK'
resource ID: 2, resource: 'Cancel'
resource ID: 3, resource: 'Abort'
```

## Using the right-left or spiral rule to decipher C declaration
reserved

# Boolean
Version ≥ C99

Using stdbool.h
> Using the system header ﬁle stdbool.h allows you to use bool as a Boolean data type. true evaluates to 1 and
false evaluates to 0.

```
#include <stdio.h>
#include <stdbool.h>
int main(void) {
    bool x = true;  /* equivalent to bool x = 1; */
    bool y = false; /* equivalent to bool y = 0; */
    if (x)  /* Functionally equivalent to if (x != 0) or if (x != false) */
    {
        puts("This will print!");
    }
    if (!y) /* Functionally equivalent to if (y == 0) or if (y == false) */
    {
        puts("This will also print!");
    }
}
```
# Using the Intrinsic (built-in) Type _Bool
Added in the C standard version C99, _Bool is also a native C data type. It is capable of holding the values 0 (for
false) and 1 (for true).

```
#include <stdio.h>
int main(void) {
    _Bool x = 1;
    _Bool y = 0;
    if(x) /* Equivalent to if (x == 1) */
    {
        puts("This will print!");
    }
    if (!y) /* Equivalent to if (y == 0) */
    {
        puts("This will also print!");
    }
}
```
To use nicer spellings bool, false and true you need to use <stdbool.h>.

# Strings
## Tokenisation 
* strtok

strtok breaks a string into a smaller strings, or tokens, using a set of delimiters.

```
#include <stdio.h>
#include <string.h>
int main(void)
{
    int toknum = 0;
    char src[] = "Hello,, world!";
    const char delimiters[] = ", !";
    char *token = strtok(src, delimiters);
    while (token != NULL)
    {
        printf("%d: [%s]\n", ++toknum, token);
        token = strtok(NULL, delimiters);
    }
    /* source is now "Hello\0, world\0\0" */
}
```
```
output:
1: [Hello]
2: [world]
```
The string of delimiters may contain one or more delimiters and diﬀerent delimiter strings may be used with each
call to strtok.

Calls to strtok to continue tokenizing the same source string should not pass the source string again, but instead
pass NULL as the ﬁrst argument. If the same source string is passed then the ﬁrst token will instead be re-tokenized.
That is, given the same delimiters, strtok would simply return the ﬁrst token again.

Note that as strtok does not allocate new memory for the tokens, it modiﬁes the source string. That is, in the above
example, the string src will be manipulated to produce the tokens that are referenced by the pointer returned by
the calls to strtok. This means that the source string cannot be const (so it can't be a string literal). It also means
that the identity of the delimiting byte is lost (i.e. in the example the "," and "!" are eﬀectively deleted from the
source string and you cannot tell which delimiter character matched).

Note also that multiple consecutive delimiters in the source string are treated as one; in the example, the second
comma is ignored.

strtok is neither thread safe nor re-entrant because it uses a static buﬀer while parsing. This means that if a
function calls strtok, no function that it calls while it is using strtok can also use strtok, and it cannot be called by
any function that is itself using strtok.

### problem example
* not re-entrant

An example that demonstrates the problems caused by the fact that strtokis not re-entrant is as follows:
```
char src[] = "1.2,3.5,4.2";
char *first = strtok(src, ",");
do
{
    char *part;
    /* Nested calls to strtok do not work as desired */
    printf("[%s]\n", first);
    part = strtok(first, ".");
    while (part != NULL)
    {
        printf(" [%s]\n", part);
        part = strtok(NULL, ".");
    }
} while ((first = strtok(NULL, ",")) != NULL);
```
Output
```
[1.2]
[1]
[2]
```

> Version ≥ C11
>> C11 has an optional part, Annex K, that oﬀers a thread-safe and re-entrant version named strtok_s. You can test
for the feature with __STDC_LIB_EXT1__. This optional part is not widely supported.
The strtok_s function diﬀers from the POSIX strtok_r function by guarding against storing outside of the string
being tokenized, and by checking runtime constraints. On correctly written programs, though, the strtok_s and
strtok_r behave the same.
Using strtok_s with the example now yields the correct response, like so:

```
* you have to announce that you want to use Annex K */
#define __STDC_WANT_LIB_EXT1__ 1
#include <string.h>
#ifndef __STDC_LIB_EXT1__
# error "we need strtok_s from Annex K"
#endif

char src[] = "1.2,3.5,4.2";  
char *next = NULL;
char *first = strtok_s(src, ",", &next);
do
{
    char *part;
    char *posn;
    printf("[%s]\n", first);
    part = strtok_s(first, ".", &posn);
    while (part != NULL)
    {
        printf(" [%s]\n", part);
        part = strtok_s(NULL, ".", &posn);
    }
}
while ((first = strtok_s(NULL, ",", &next)) != NULL);
```
output:
```
[1.2]
[1]
[2]
[3.5]
[3]
[5]
[4.2]
[4]
[2]
```

## String literals
```
const char *get_hello() {
    return "Hello, World!";  /* safe */
}
```
```
char *foo = "hello";
foo[0] = 'y';  /* Undefined behavior - BAD! */
```
```
const char *foo = "hello";
/* GOOD: can't modify the string pointed to by foo */
```
```
char *foo = "hello";
foo = "World!"; /* OK - we're just changing what foo points to */
```
```
char foo[] = "hello";
foo[0] = 'y';  /* OK! */
```

## Length: strlen()
strlen counts all the bytes from the beginning of the string up to, but not including, the terminating NUL
character, '\\0'. As such, it can only be used when the string is guaranteed to be NUL-terminated.
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
int main(void)
{
    char asciiString[50] = "Hello world!";
    char utf8String[50] = "Γειά σου Κόσμε!"; /* "Hello World!" in Greek */
    printf("asciiString has %zu bytes in the array\\n", sizeof(asciiString));
    printf("utf8String has %zu bytes in the array\\n", sizeof(utf8String));
    printf("\\"%s\\" is %zu bytes\\n", asciiString, strlen(asciiString));
    printf("\\"%s\\" is %zu bytes\\n", utf8String, strlen(utf8String));
}
```
```
asciiString has 50 bytes in the array
utf8String has 50 bytes in the array
"Hello world!" is 12 bytes
"Γειά σου Κόσμε!" is 27 bytes
```
---

```
char * string = "hello world";
```

When initializing a char * to a string constant as above, the string itself is usually allocated in read-only data;
string is a pointer to the ﬁrst element of the array, which is the character 'h'.

Since the string literal is allocated in read-only memory, it is non-modiﬁable1. Any attempt to modify it will lead to
undeﬁned behaviour, so it's better to add const to get a compile-time error like this
```
char const * string = "hello world";
/*GOOD*/
```
To create a modiﬁable string, you can declare a character array and initialize its contents using a string literal, like
so:
```
char modifiable_string[] = "hello world";
```
equivalent to
```
char modifiable_string[] = {'h', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd', '\0'};
```
Since the second version uses brace-enclosed initializer, the string is not automatically null-terminated unless a
'\0' character is included explicitly in the character array usually as its last element.

## Copying strings

You cannot use the = operator to copy strings in C. Strings in C are
represented as arrays of characters with a terminating null-character, so using the = operator will only save the
address (pointer) of a string.

```
#include <stdio.h>
int main(void) {
    int a = 10, b;
    char c[] = "abc", *d;
    b = a; /* Integer is copied */
    a = 20; /* Modifying a leaves b unchanged - b is a 'deep copy' of a */
    printf("%d %d\n", a, b); /* "20 10" will be printed */
    d = c;
    /* Only copies the address of the string -
    there is still only one string stored in memory */
   
    c[1] = 'x';
    /* Modifies the original string - d[1] = 'x' will do exactly the same thing */
    printf("%s %s\n", c, d); /* "axc axc" will be printed */
    return 0;
}
```

* error example
```
#include <stdio.h>
int main(void) {
    char a[] = "abc";
    char b[8];
    b = a; /* compile error */
    printf("%s\n", b);
    return 0;
}
```

## strcpy()
```
#include <stdio.h>
#include <string.h>
int main(void) {
    char a[] = "abc";
    char b[8];
    strcpy(b, a); /* think "b special equals a" */
    printf("%s\n", b); /* "abc" will be printed */
    return 0;
}
```
## snprintf()
To avoid buﬀer overrun, snprintf() may be used.
 效能獲取不是最好(轉換template string)，但是是唯一在標準庫中 limit-safe for copying strings readily-available.

 ```
 #include <stdio.h>
#include <string.h>
int main(void) {
    char a[] = "012345678901234567890";
    char b[8];
#if 0
    strcpy(b, a); 
    /* causes buffer overrun (undefined behavior), so do not execute this here! */
#endif
    snprintf(b, sizeof(b), "%s", a); /* does not cause buffer overrun */
    printf("%s\n", b); /* "0123456" will be printed */
    return 0;
}
```
## strncat()
Better performance.

a buﬀer overﬂow checking version of strcat(): it takes a third argument that tells it the maximum number of bytes to copy:
```
char dest[32];
dest[0] = '\0';
strncat(dest, source, sizeof(dest) - 1);
    /* copies up to the first (sizeof(dest) - 1) elements of source into dest,
    then puts a \0 on the end of dest */
```
strncat() always adds a null byte (good),
but doesn't count that in the size of the string


```
char dst[24] = "Clownfish: ";
char src[] = "Marvin and Nemo";
size_t len = strlen(dst);
strncat(dst, src, sizeof(dst) - len - 1);
printf("%zu: [%s]\n", strlen(dst), dst);
```
output
```
23: [Clownfish: Marvin and N]
```
note: sizeof return the space size, not string length.


## strncpy()
two main gotchas(陷阱)

1. If copying via strncpy() hits the buﬀer limit, a terminating null-character won't be written.(被迫中斷不寫終止)
1. strncpy() always completely ﬁlls the destination, with null bytes if necessary.

only way to use
```
strncpy(b, a, sizeof(b)); /* the third parameter is destination buffer size */
b[sizeof(b)/sizeof(*b) - 1] = '\0'; /* terminate the string */
printf("%s\n", b); /* "0123456" will be printed */
```
Even then, if you have a big buﬀer it becomes very ineﬃcient to use strncpy() because of additional null padding. (被充滿null padding)

## Creating Arrays of Strings
```
char * string_array[] = {
    "foo",
    "bar",
    "baz"
};
```
Remember: when we assign string literals to char *, the strings themselves are allocated in read-only memory.
However, the array string_array is allocated in read/write memory. This means that we can modify the pointers in
the array, but we cannot modify the strings they point to.
```
char modifiable_string_array_literals[][4] = {
    "foo",
    "bar",
    "baz"
};
```
equivalent to
```
char modifiable_string_array[][4] = {
    {'f', 'o', 'o', '\0'},
    {'b', 'a', 'r', '\0'},
    {'b', 'a', 'z', '\0'}
};
```

## "Don't use" Convert Strings to Number: atoi(), atof() 
atoi, atol, atoll and atof are inherently unsafe, because: If the value of the result cannot be
represented, the behavior is undeﬁned.

For strings that start with a number, followed by something else, only the initial number is parsed:
```
#include <stdio.h>
#include <stdlib.h>
int main(int argc, char** argv)
{
    int val;
    if (argc < 2)
    {
        printf("Usage: %s <integer>\n", argv[0]);
        return 0;
    }
    val = atoi(argv[1]);
    printf("String value = %s, Int value = %d\n", argv[1], val);
    return 0;
}
```
output
```
$ ./atoi 0x200
0
$ ./atoi 0123x300
123
```
For strings that start with a number, followed by something else, only the initial number is parsed:

* To convert to long int, use strtol() instead of atol().
* To convert to double, use strtod() instead of atof().
* To convert to long long int, use strtoll() instead of atoll().


## string formatted data read/write

Write formatted data to string

`
int sprintf ( char * str, const char * format, ... );
`

Read formatted data from string

`
int sscanf ( const char * s, const char * format, ...);
`

use sscanf function to parse formatted data.
```
#include <stdio.h>
int main ()
{
  char sentence []="date : 06-06-2012";
  char str [50];
  int year;
  int month;
  int day;
  sscanf (sentence,"%s : %2d-%2d-%4d", str, &day, &month, &year);
  printf ("%s -> %02d-%02d-%4d\n",str, day, month, year);
  return 0;
}
```

## Find ﬁrst/last occurrence of a speciﬁc character
The strchr and strrchr functions ﬁnd a character in a string

`
char *firstOcc = strchr(argv[1], toSearchFor);
`

`
char *lastOcc = strrchr(argv[1], toSearchFor);
`

strrchr: r between str and chr means reversed.

One common use for strrchr is to extract a ﬁle name from a path. For example to extract myfile.txt from
C:\Users\eak\myfile.txt:
```
char *getFileName(const char *path)
{
    char *pend;
    if ((pend = strrchr(path, '\')) != NULL)
        return pend + 1;
    return NULL;
}
```

## Comparsion: strcmp(), strncmp(), strcasecmp(), strncasecmp()
The strcase*-functions are not Standard C, but a POSIX extension.

`
int result = strcmp(lhs, rhs);
`

The functions return a
negative value if the ﬁrst argument appears before the second in lexicographical order, zero if they compare equal,
or positive if the ﬁrst argument appears after the second in lexicographical order.

`
int result = strcasecmp(lhs, rhs);
`

strcasecmp function also compares lexicographically its arguments after translating each character to its
lowercase correspondent:

`
int result = strncmp(lhs, rhs, n)
`

strncmp and strncasecmp compare at most n characters


## Safely convert Strings to Number:
## strtoX functions

* Version ≥ C99

`
double strtod(char const* p, char** endptr);
`

`
long double strtold(char const* p, char** endptr);
`

確保轉換不會 over- or underﬂow:
```
char *check = 0;
double ret = strtod(argv[1], &check); /* attempt conversion */
/* check the conversion result. */
if (argv[1] == check)
    return; /* No number was detected in string */
else if ((ret == HUGE_VAL || ret == -HUGE_VAL) && errno == ERANGE)
    return; /* numeric overflow in in string */
else if (ret == HUGE_VAL && errno == ERANGE)
    return; /* numeric underflow in in string */
/* At this point we know that everything went fine so ret may be used */
```

analogous functions
```
long strtol(char const* p, char** endptr, int nbase);
long long strtoll(char const* p, char** endptr, int nbase);
unsigned long strtoul(char const* p, char** endptr, int nbase);
unsigned long long strtoull(char const* p, char** endptr, int nbase);
```

```
long a = strtol("101",   0, 2 ); /* a = 5L */
long b = strtol("101",   0, 8 ); /* b = 65L */
long c = strtol("101",   0, 10); /* c = 101L */
long d = strtol("101",   0, 16); /* d = 257L */
long e = strtol("101",   0, 0 ); /* e = 101L */
long f = strtol("0101",  0, 0 ); /* f = 65L */
long g = strtol("0x101", 0, 0 ); /* g = 257L */
```

## strspn and strcspn
strspn計算子串列長度，只要單獨包含sepchars中，一個字元。strcspn為排除sepchars中的字元。

```
#include <stdio.h>
#include <string.h>
int main(void)
{
    const char sepchars[] = ",.;!?";
    char foo[] = ";ball call,.fall gall hall!?.,";
    char *s;
    int n;
    for (s = foo; *s != 0; /*empty*/) {
        /* Get the number of token separator characters. */
        n = (int)strspn(s, sepchars);
        if (n > 0)
            printf("skipping separators: << %.*s >> (length=%d)\n", n, s, n);
        /* Actually skip the separators now. */
        s += n;
        /* Get the number of token (non-separator) characters. */
        n = (int)strcspn(s, sepchars);
        if (n > 0)
            printf("token found: << %.*s >> (length=%d)\n", n, s, n);
        /* Skip the token now. */
        s += n;
    }
    printf("== token list exhausted ==\n");
    return 0;
}
```
output
```
skipping separators: << ; >> (length=1)
token found: << ball call >> (length=9)
skipping separators: << ,. >> (length=2)
token found: << fall gall hall >> (length=14)
skipping separators: << !?., >> (length=4)
== token list exhausted ==
```

## String literals

The L preﬁx makes the literal a wide character array, of type wchar_t*. 

For example, L"abcd".
```
preﬁx  base     type encoding
none   char     platform dependent
L      wchar_t  platform dependent
u8     char     UTF-8
u      char16_t usually UTF-16
U      char32_t usually UTF-32
```
後兩者可以檢測， feature test macros去檢查是否有效對UTF encoding.

special characters
```
\b Backspace
\f Form feed
\n Line feed (new line)
\r Carriage return
\t Horizontal tab
\v Vertical tab
\\ Backslash
\' Single quotation mark
\" Double quotation mark
\? Question mark
\nnn Octal value
\xnn... Hexadecimal value

Version ≥ C89
Escape Sequence Represented Character

\a Alert (beep, bell)

Version ≥ C99
Escape Sequence Represented Character

\unnnn Universal character name
\Unnnnnnnn Universal character name
```

## Integer literals
```
Base         Preﬁx      Example
Decimal      None       5
Octal        0          0345
Hexadecimal  0x or 0X   0x12AB, 0X12AB, 0x12ab, 0x12Ab
```

Since C99, `long long` and `unsigned long long` are also supported
```
Suﬃx    Explanation
L, l    long int
LL, ll  (since C99) long long int
U, u    unsigned
```

## Compound Literals
Deﬁnition/Initialisation of Compound Literals

```
int *p = (int [2]){ 2, 4 };
```
p is initialized to the address of the ﬁrst element of an unnamed array of two ints.

P被初始化為未命名陣列的第一個元素的位址


* Compound literal with designators
```
  struct point {
    unsigned x;
    unsigned y;
  };
  extern void drawline(struct point, struct point);
 // used somewhere like this
 drawline((struct point){.x=1, .y=1}, (struct point){.x=3, .y=4});
```

A ﬁctive function drawline receives two arguments of type struct point.

(struct point){.x=1, .y=1} 未命名

* Compound literal without specifying array length
```
int *p = (int []){ 1, 2, 3};  
```
array is no speciﬁed then it will be determined by the length of the initializer.(example is length 3)

* Compound literal having length of initializer less than array size speciﬁed
```
int *p = (int [10]){1, 2, 3};
```
rest of the elements of compound literal will be initialized to 0 implicitly.

* Read-only compound literal

```
(const int[]){1,2}
```
* Compound literal containing arbitrary expressions
Inside a function, a compound literal, as for any initialization since C99, can have arbitrary expressions.
```
void foo()
{
    int *p;
    int i = 2; j = 5;
    /*...*/
    p = (int [2]){ i+j, i*j };
    /*...*/
}
```

## Bit-ﬁelds
Describe things that may have a speciﬁc number of bits involved.

Bit-ﬁeldsare often used when interfacing with hardware that outputs data associated with speciﬁc number of bits.

```
struct encoderPosition {
   unsigned int encoderCounts : 23;
   unsigned int encoderTurns  : 4;
   unsigned int _reserved     : 5;
};
```
an encoder with 23 bits of single precision and 4 bits to describe multi-turn

* Using bit-ﬁelds as small integers
```
#include <stdio.h>
int main(void)
{
    /* define a small bit-field that can hold values from 0 .. 7 */
    struct
    {
        unsigned int uint3: 3;
    } small;
    /* extract the right 3 bits from a value */
    unsigned int value = 255 - 2; /* Binary 11111101 */
    small.uint3 = value;          /* Binary      101 */
    printf("%d", small.uint3);
    /* This is in effect an infinite loop */
    for (small.uint3 = 0; small.uint3 < 8; small.uint3++)
    {
        printf("%d\n", small.uint3);
    }
    return 0;
}
```

## Bit-ﬁeld alignmentv 
Bit-ﬁelds 可以宣告比character更小的寬度(width)。
```
struct C
{
    short s;            /* 2 bytes */
    char  c;            /* 1 byte */
    int   bit1 : 1;     /* 1 bit */
    int   nib  : 4;     /* 4 bits padded up to boundary of 8 bits. Thus 3 bits are padded */
    int   sept : 7;     /* 7 Bits septet, padded up to boundary of 32 bits. */
};
```
An unnamed bit-ﬁeld may be of any size, but they can't be initialized or referenced.
```
struct Flags {
    unsigned int flag1 : 1;
    unsigned int : 0; // unnamed bit-field
    unsigned int flag2 : 1;
};
```

```
/*The size of structure 'A' is 1 byte.*/
struct A
{
    unsigned char c1 : 3;
    unsigned char c2 : 4;
    unsigned char c3 : 1;
};
```
In structure B, the ﬁrst unnamed bit-ﬁeld skips 2 bits; the zero width bit-ﬁeld after c2 causes c3 to start from the
char boundary (so 3 bits are skipped between c2 and c3. There are 3 padding bits after c4. Thus the size of the
structure is 2 bytes.
```
struct B
{
    unsigned char c1 : 1;
    unsigned char    : 2;    /* Skips 2 bits in the layout */
    unsigned char c2 : 2;
    unsigned char    : 0;    /* Causes padding up to next container boundary */
    unsigned char c3 : 4;
    unsigned char c4 : 1;
};
```
1. Arrays of bit-ﬁelds, pointers to bit-ﬁelds and functions returning bit-ﬁelds are not allowed.
2. The address operator (&) cannot be applied to bit-ﬁeld members.
3. The data type of a bit-ﬁeld must be wide enough to contain the size of the ﬁeld.
4. The sizeof() operator cannot be applied to a bit-ﬁeld.
5. There is no way to create a typedef for a bit-ﬁeld in isolation (though you can certainly create a typedef for a
structure containing bit-ﬁelds).
```
typedef struct mybitfield
{
    unsigned char c1 : 20;   /* incorrect, see point 3 */
    unsigned char c2 : 4;    /* correct */
    unsigned char c3 : 1;
    unsigned int x[10]: 5;   /* incorrect, see point 1 */
} A;
int SomeFunction(void)
{
    // Somewhere in the code
    A a = { … };
    printf("Address of a.c2 is %p\n", &a.c2);      /* incorrect, see point 2 */
    printf("Size of a.c2 is %zu\n", sizeof(a.c2)); /* incorrect, see point 4 */
}
```

## Array length
In particular, this can make code more ﬂexible when the array
length is determined automatically from an initializer:
```
int array[] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
/* size of `array` in bytes */
size_t size = sizeof(array);
/* number of elements in `array` */
size_t length = sizeof(array) / sizeof(array[0]);
```

However, in most contexts where an array appears in an expression, it is automatically converted to ("decays to") a
pointer to its ﬁrst element.
傳遞進function時是pointer，用上述方法計算length會是錯誤。

正確的使用傳遞:
```
/* array will decay to a pointer, so the length must be passed separately */
int last = get_last(array, length);
```
```
The function could be implemented like this:
int get_last(int input[], size_t length) {
    return input[length - 1];
}
```

It is a very common error to attempt to determine array size from a pointer, which cannot work. 

DO NOT DO THIS:
```
int BAD_get_last(int input[]) {
    /* INCORRECTLY COMPUTES THE LENGTH OF THE ARRAY INTO WHICH input POINTS: */

    size_t length = sizeof(input) / sizeof(input[0]));
    return input[length - 1];  
    /* Oops -- not the droid we are looking for */
}
```

## Clearing array contents (zeroing)
```
#include <stdlib.h> /* for EXIT_SUCCESS */
#define ARRLEN (10)
int main(void)
{
  int array[ARRLEN]; /* Allocated but not initialised, as not defined static or global. */
  size_t i;
  for(i = 0; i < ARRLEN; ++i)
  {
    array[i] = 0;
  }
  return EXIT_SUCCESS;
}
```
An common short cut to the above loop is to use memset() from <string.h>.
```
memset(array, 0, ARRLEN * sizeof (int)); /* Use size explicitly provided type (int here). */
```
or
```
memset(array, 0, ARRLEN * sizeof *array); /* Use size of type the pointer is pointing to. */
```
If it is an array and not just a pointer, can use
```
 memset(array, 0, sizeof array); /* Use size of the array itself. */
```

## Enumerations
If you use enum instead of int or string/ char*, you increase compile-time checking and avoid errors from passing
in invalid constants, and you document which values are legal to use.

```
enum color{ RED, GREEN, BLUE };
void printColor(enum color chosenColor)
{
    const char *color_name = "Invalid color";
    switch (chosenColor)
    {
       case RED:
         color_name = "RED";
         break;
       
       case GREEN:
        color_name = "GREEN";
        break;    
       case BLUE:
        color_name = "BLUE";
        break;
    }
    printf("%s\n", color_name);
}
int main(){
    enum color chosenColor;
    printf("Enter a number between 0 and 2");
    scanf("%d", (int*)&chosenColor);
    printColor(chosenColor);
    return 0;
}
```

```
enum week{ MON, TUE, WED, THU, FRI, SAT, SUN };
     
static const char* const dow[] = {
  [MON] = "Mon", [TUE] = "Tue", [WED] = "Wed",
  [THU] = "Thu", [FRI] = "Fri", [SAT] = "Sat", [SUN] = "Sun" };
   
void printDayOfWeek(enum week day)
{
   printf("%s\n", dow[day]);
}
```
```
enum week{ DOW_INVALID = -1,
  MON, TUE, WED, THU, FRI, SAT, SUN,
  DOW_MAX };
     
static const char* const dow[] = {
  [MON] = "Mon", [TUE] = "Tue", [WED] = "Wed",
  [THU] = "Thu", [FRI] = "Fri", [SAT] = "Sat", [SUN] = "Sun" };
   
void printDayOfWeek(enum week day)
{
   assert(day > DOW_INVALID && day < DOW_MAX);
   printf("%s\n", dow[day]);
}
```

enumeration without name 
```
  enum { buffersize = 256, };
  static unsigned char buffer [buffersize] = { 0 };
```
Enumeration with duplicate value

An enumerations value in no way needs to be unique:

列舉不一定需要獨一連續，可以有不同明的同數值。
```
#include <stdlib.h> /* for EXIT_SUCCESS */
#include <stdio.h> /* for printf() */

enum Dupes
{
   Base, /* Takes 0 */
   One, /* Takes Base + 1 */
   Two, /* Takes One + 1 */
   Negative = -1,
   AnotherZero /* Takes Negative + 1 == 0, sigh */
};
int main(void)
{
  printf("Base = %d\n", Base);
  printf("One = %d\n", One);
  printf("Two = %d\n", Two);
  printf("Negative = %d\n", Negative);
  printf("AnotherZero = %d\n", AnotherZero);
  return EXIT_SUCCESS;
}

output:
Base = 0
One = 1
Two = 2
Negative = -1
AnotherZero = 0
```

## Typedef enum

normal use
```
enum color
{
    RED,
    GREEN,
    BLUE
};
```
```
enum color chosenColor = RED;
```
simplify
But in this latter case we cannot use it as `enum color`
```
typedef enum
{
    RED,
    GREEN,
    BLUE
} color;

color chosenColor = RED;
```

use `enum color` and `color` at same time. (compatible with C++)
```
enum color                /* as in the first example */
{
    RED,
    GREEN,
    BLUE
};
typedef enum color color; /* also a typedef of same identifier */
color chosenColor  = RED;
enum color defaultColor = BLUE;
```


## Linked lists

* reserved
1. A doubly linked list
2. Reversing a linked list
3. Inserting a node at the nth position
4. Inserting a node at the beginning of a singly
linked list


## Structs
### Flexible Array Members
not exist prior to C99
```
struct ex1
{
    size_t foo;
    int flex[];
};
struct ex2_header
{
    int foo;
    char bar;
};
struct ex2
{
    struct ex2_header hdr;
    int flex[];
};
```
A ﬂexible array member is treated as having no size when calculating the size of a structure.
提供連續記憶體位置的彈性陣列宣告，與 char* 不連續位置不同。

不能使用初始化，需使用`malloc`, `calloc`, or `realloc`。
```
/* invalid: cannot initialize flexible array member */
struct ex1 e1 = {1, {2, 3}};
/* invalid: hdr={foo=1, bar=2} OK, but cannot initialize flexible array member */
struct ex2 e2 = {{1, 2}, {3}};
/* valid: initialize foo=1, bar=2 members */
struct ex3 e3 = {1, 2};
e1.flex[0] = 3; /* undefined behavior, in my case */
e3.flex[0] = 2; /* undefined behavior again */
e2.flex[0] = e3.flex[0]; /* undefined behavior */
```
```
/* valid: allocate an object of structure type `ex1` along with an array of 2 ints */
struct ex1 *pe1 = malloc(sizeof(*pe1) + 2 * sizeof(pe1->flex[0]));
/* valid: allocate an object of structure type ex2 along with an array of 4 ints */
struct ex2 *pe2 = malloc(sizeof(struct ex2) + sizeof(int[4]));
/* valid: allocate 5 structure type ex3 objects along with an array of 3 ints per object */
struct ex3 *pe3 = malloc(5 * (sizeof(*pe3) + sizeof(int[3])));
pe1->flex[0] = 3; /* valid */
pe3[0]->flex[0] = pe1->flex[0]; /* valid */
```

## Typedef Structs
Combining typedef with struct can make code clearer.
```
typedef struct
{
    int x, y;
} Point;

Point point;
```
as
```
struct Point
{
    int x, y;
};

struct Point point;
```
even better use:
```
typedef struct Point Point;
struct Point
{
    int x, y;
};
```
## Passing structs to functions
In C, all arguments are passed to functions by value, including structs. For small structs, this is a good thing as it
means there is no overhead from accessing the data through a pointer. However, it also makes it very easy to
accidentally pass a huge struct resulting in poor performance, particularly if the programmer is used to other
languages where arguments are passed by reference.

C是pass-by-value，對小型struct是好事，因有沒pointer查詢的開銷。但大型struct會導致效能下降。

fast pass
```
struct coordinates
{
    int x;
    int y;
    int z;
};
// Passing and returning a small struct by value, very fast
struct coordinates move(struct coordinates position, struct coordinates movement)
{
    position.x += movement.x;
    position.y += movement.y;
    position.z += movement.z;
    return position;
}
```
very slow
```
// A very big struct
struct lotsOfData
{
    int param1;
    char param2[80000];
};
// Passing and returning a large struct by value, very slow!
// Given the large size of the struct this could even cause stack overflow
struct lotsOfData doubleParam1(struct lotsOfData value)
{
    value.param1 *= 2;
    return value;
}
```
// Passing the large struct by pointer instead, fairly fast
```
void doubleParam1ByPtr(struct lotsOfData *value)
{
    value->param1 *= 2;
}
```
## Object-based programming using structs
類Object-based.

A struct is similar to a class, but is missing
the functions which normally also form part of a class.

we can add these as function pointer member variables.

```
/* coordinates.h */
typedef struct coordinate_s
{
    /* Pointers to method functions */
    void (*setx)(coordinate *this, int x);
    void (*sety)(coordinate *this, int y);
    void (*print)(coordinate *this);
    /* Data */
    int x;
    int y;
} coordinate;
/* Constructor */
coordinate *coordinate_create(void);
/* Destructor */
void coordinate_destroy(coordinate *this);
```
And now the implementing C ﬁle:
```
/* coordinates.c */
#include "coordinates.h"
#include <stdio.h>
#include <stdlib.h>
/* Constructor */
coordinate *coordinate_create(void)
{
    coordinate *c = malloc(sizeof(*c));
    if (c != 0)
    {
        c->setx = &coordinate_setx;
        c->sety = &coordinate_sety;
        c->print = &coordinate_print;
        c->x = 0;
        c->y = 0;
    }
    return c;
}
/* Destructor */
void coordinate_destroy(coordinate *this)
{
    if (this != NULL)
    {
        free(this);  
    }  
}
/* Methods */
static void coordinate_setx(coordinate *this, int x)
{
    if (this != NULL)
    {    
        this->x = x;
    }
}
static void coordinate_sety(coordinate *this, int y)
{
    if (this != NULL)
    {
        this->y = y;
    }
}
static void coordinate_print(coordinate *this)
{
    if (this != NULL)
    {
        printf("Coordinate: (%i, %i)\n", this->x, this->y);
    }
    else
    {
        printf("NULL pointer exception!\n");
    }
}
```
example usage:
```
/* main.c */
#include "coordinates.h"
#include <stddef.h>
int main(void)
{
    /* Create and initialize pointers to coordinate objects */
    coordinate *c1 = coordinate_create();
    coordinate *c2 = coordinate_create();
   
    /* Now we can use our objects using our methods and passing the object as parameter */
    c1->setx(c1, 1);
    c1->sety(c1, 2);
    c2->setx(c2, 3);
    c2->sety(c2, 4);
    c1->print(c1);
    c2->print(c2);
    /* After using our objects we destroy them using our "destructor" function */
    coordinate_destroy(c1);
    c1 = NULL;
    coordinate_destroy(c2);
    c2 = NULL;
    return 0;
}
```

## Simple data structures
```
struct point
{
    int x;
    int y;
};
```
Deﬁning and using structs:
```
struct point p;    // declare p as a point struct
p.x = 5;           // assign p member variables
p.y = 3;
```
Structs can be initialized at deﬁnition
```
struct point p = {5, 3};
```

# Structure Padding and Packing
## Packing structures

By default structures are padded in C. If you want to avoid this behaviour, you have to explicitly request it. Under
GCC it's \__attribute__((\__packed__)). Consider this example on a 64-bit machine:
```
struct foo {
    char *p;  /* 8 bytes */
    char c;   /* 1 byte  */
    long x;   /* 8 bytes */
};
```
The structure will be automatically padded to have8-byte alignment and will look like this:
```
struct foo {
    char *p;     /* 8 bytes */
    char c;      /* 1 byte  */
    char pad[7]; /* 7 bytes added by compiler */
    long x;      /* 8 bytes */
};
```
So sizeof(struct foo) will give us 24 instead of 17. 

## Structure packing
But if you add the attribute packed, the compiler will not add padding:
```
struct __attribute__((__packed__)) foo {
    char *p;  /* 8 bytes */
    char c;   /* 1 byte  */
    long x;   /* 8 bytes */
};
```
Now sizeof(struct foo) will return 17.

* To save space.
* To format a data structure to transmit over network without depending on each architecture alignment of
each node of the network.

## Structure padding

compiled with a 32 bit compiler
```
struct test_32 {
    int a;      // 4 byte
    short b;    // 2 byte
    int c;      // 4 byte    
} str_32;
```
We might expect this struct to occupy only 10 bytes of memory, but by printing sizeof(str_32) we see it uses 12
bytes.

This happened because the compiler aligns variables for fast access. A common pattern is that when the base type
occupies N bytes (where N is a power of 2 such as 1, 2, 4, 8, 16 — and seldom any bigger), the variable should be
aligned on an N-byte boundary (a multiple of N bytes).

For the structure shown with sizeof(int) == 4 and sizeof(short) == 2, a common layout is:
```
int a; stored at oﬀset 0; size 4.
short b; stored at oﬀset 4; size 2.
unnamed padding at oﬀset 6; size 2.
int c; stored at oﬀset 8; size 4.
```
__Thus struct test_32 occupies 12 bytes of memory. In this example, there is no trailing padding.__

## Chapter 15: Iteration Statements/Loops: for, while, do-while

### Section 15.2: Loop Unrolling and Du's Device
把switch的case標記在迴圈體內
```
switch (true) while (condition) {
case false: do_A(); /* FALL THROUGH */
default:    do_B(); /* FALL THROUGH */
}
```
with Duﬀ's Device, the code can follow this unrolling idiom that jumps into the right place in the middle of the
loop if n is not divisible by 4.
```
switch (n % 4) do {
case 0: *ptr++ ^= mask; /* FALL THROUGH */
case 3: *ptr++ ^= mask; /* FALL THROUGH */
case 2: *ptr++ ^= mask; /* FALL THROUGH */
case 1: *ptr++ ^= mask; /* FALL THROUGH */
} while ((n -= 4) > 0);
```
This kind of manual unrolling is rarely required with modern compilers, since the compiler's optimization engine
can unroll loops on the programmer's behalf.

## switch () Statements
```
int a = 1;
switch (a) {
case 1:
    puts("a is 1");
    break;
case 2:
    puts("a is 2");
    break;
default:
    puts("a is neither 1 nor 2");
    break;
}
```
case n: is used to describe where the execution ﬂow will jump in when the value passed to switch statement is n.
n must be compile-time constant and the same n can exist at most once in one switch statement.

default: is used to describe that when the value didn't match any of the choices for case n:. It is a good practice
to include a default case in every switch statement to catch unexpected behavior.

A break; statement is required to jump out of the switch block.

Note: If you accidentally forget to add a break after the end of a case, the compiler will assume that you intend to
"fall through" and all the subsequent case statements, if any, will be executed (unless a break statement is found in
any of the subsequent cases), regardless of whether the subsequent case statement(s) match or not.
Fall through，如果不加break會持續往下執，儘管條件不match。

這種特殊情會有用在: This particular
property is used to implement Duﬀ's Device. This behavior is often considered a ﬂaw in the C language
speciﬁcation.

* The best example is using a switch on an enum.
```
enum msg_type { ACK, PING, ERROR };
void f(enum msg_type t)
{
  switch (t) {
  case ACK:
    // do nothing
    break;
  case PING:
    // do something
    break;
  case ERROR:
    // do something else
    break;
  }
}
```
advantages: 
* most compilers will report a warning if you don't handle a value (this would not be reported if a default case
were present)
* for the same reason, if you add a new value to the enum, you will be notiﬁed of all the places where you forgot
to handle the new value (with a default case, you would need to manually explore your code searching for
such cases)
* The reader does not need to ﬁgure out "what is hidden by the default:", whether there other enum values or
whether it is a protection for "just in case". And if there are other enum values, did the coder intentionally use
the default case for them or is there a bug that was introduced when he added the value?
* handling each enum value makes the code self explanatory as you can't hide behind a wild card, you must
explicitly handle each of them.
```
void f(enum msg_type t)
{
   if (!is_msg_type_valid(t)) {
      // Handle this unlikely error
   }
    switch(t) {
    // Same code than before
   }
}
```
## Initializing :
* Character arrays
```
char chr_array[] = "hello";
```

is a shorthand for the longer but equivalent:

```
char chr_array[] = { 'h', 'e', 'l', 'l', 'o', '\0' };
```

* designated initializers

```
int array[] = { [4] = 29, [5] = 31, [17] = 101, [18] = 103, [19] = 107, [20] = 109 };
```
未指定為默認初始化為零。不一定需要順序。array最大值未指定則默認index最大值。上例為21。若指定array size但初始化entry超過則會報錯。

* Designated initializers for structures
```
struct Date
{
    int year;
    int month;
    int day;
};
```
```
struct Date us_independence_day = { .day = 4, .month = 7, .year = 1776 };
```
* Designated initializer for unions
```
struct discriminated_union
{
    enum { DU_INT, DU_DOUBLE } discriminant;
    union
    {
        int     du_int;
        double  du_double;
    } du;
};
```
```
struct discriminated_union du1 = { .discriminant = DU_INT, .du = { .du_int = 1 } };
struct discriminated_union du2 = { .discriminant = DU_DOUBLE, .du = { .du_double = 3.14159 } };
```
* if Version ≥ C11.
Note that C11 allows you to use anonymous union members inside a structure
```
struct discriminated_union
{
    enum { DU_INT, DU_DOUBLE } discriminant;
    union
    {
        int     du_int;
        double  du_double;
    };
};
```
```
struct discriminated_union du1 = { .discriminant = DU_INT, .du_int = 1 };
struct discriminated_union du2 = { .discriminant = DU_DOUBLE, .du_double = 3.14159 };
```
* Initializing structures and arrays of structures
```
struct Date
{
    int year;
    int month;
    int day;
};
struct Date us_independence_day = { 1776, 7, 4 };
struct Date uk_battles[] =
{
    { 1066, 10, 14 },    // Battle of Hastings
    { 1815,  6, 18 },    // Battle of Waterloo
    { 1805, 10, 21 },    // Battle of Trafalgar
};
```

## Command-line arguments
```
#include <stdlib.h>
#include <stdio.h>
#include <errno.h>
#include <limits.h>
int main(int argc, char* argv[]) {
    for (int i = 1; i < argc; i++) {
        printf("Argument %d is: %s\n", i, argv[i]);
        errno = 0;
        char *p;
        long argument_numValue = strtol(argv[i], &p, 10);
        if (p == argv[i]) {
            fprintf(stderr, "Argument %d is not a number.\n", i);
        }
        else if ((argument_numValue == LONG_MIN || argument_numValue == LONG_MAX) && errno ==
ERANGE) {
            fprintf(stderr, "Argument %d is out of range.\n", i);
        }
        else {
            printf("Argument %d is a number, and the value is: %ld\n",
                   i, argument_numValue);
        }
    }
    return 0;
}
```
* Printing the command line arguments
```
int main(int argc, char **argv)
{
    for (int i = 1; i < argc; i++)
    {
        printf("Argument %d: [%s]\n", i, argv[i]);
    }
}
```
* GNU getopt tools
```
/* getopt_long supports GNU-style full-word "long" options in addition
         * to the single-character "short" options which are supported by
         * getopt.
         * the third argument is a collection of supported short-form options.
         * these do not necessarily have to correlate to the long-form options.
         * one colon after an option indicates that it has an argument, two
         * indicates that the argument is optional. order is unimportant.
         */
        opt = getopt_long (argc, argv, "hf::m:", longopts, 0);
```

## Files and I/O streams
```
#include <stdio.h>   /* for perror(), fopen(), fputs() and fclose() */
#include <stdlib.h>  /* for the EXIT_* macros */
 
int main(int argc, char **argv)
{
    int e = EXIT_SUCCESS;
    /* Get path from argument to main else default to output.txt */
    char *path = (argc > 1) ? argv[1] : "output.txt";
    /* Open file for writing and obtain file pointer */
    FILE *file = fopen(path, "w");
   
    /* Print error message and exit if fopen() failed */
    if (!file)
    {
        perror(path);
        return EXIT_FAILURE;
    }
    /* Writes text to file. Unlike puts(), fputs() does not add a new-line. */
    if (fputs("Output in file.\n", file) == EOF)
    {
        perror(path);
        e = EXIT_FAILURE;
    }
    /* Close file */
    if (fclose(file))
    {
        perror(path);
        return EXIT_FAILURE;
    }
    return e;
}
```
## Run process
```
#include <stdio.h>
void print_all(FILE *stream)
{
    int c;
    while ((c = getc(stream)) != EOF)
        putchar(c);
}
int main(void)
{
    FILE *stream;
    /* call netstat command. netstat is available for Windows and Linux */
    if ((stream = popen("netstat", "r")) == NULL)
        return 1;
 
    print_all(stream);
    pclose(stream);
    return 0;
}
```
This program runs a process (netstat) via popen() and reads all the standard output from the process and echoes
that to standard output.

Note: popen() does not exist in the standard C library, but it is rather a part of POSIX C)

filo I/O

* fprintf
* getline()

Another option is getdelim(). This is the same as getline() except you specify the line ending character. This is
only necessary if the last character of the line for your ﬁle type is not '\n'. getline() works even with Windows text
ﬁles because with the multibyte line ending ("\r\n")'\n'` is still the last character on the line.

* fscanf()
* fgets()

fgets() function. This function reads a line from a stream and stores it in a
speciﬁed string.

The function stops reading text from the stream when either n - 1 characters are read, the
newline character ('\n') is read or the end of ﬁle (EOF) is reached.

Open and write to a binary ﬁle
```
#include <stdlib.h>
#include <stdio.h>

int main(void)
{
   result = EXIT_SUCCESS;
   char file_name[] = "outbut.bin";
   char str[] = "This is a binary file example";
   FILE * fp = fopen(file_name, "wb");
   
   if (fp == NULL)  /* If an error occurs during the file creation */
   {
     result = EXIT_FAILURE;
     fprintf(stderr, "fopen() failed for '%s'\n", file_name);
   }
   else
   {
     size_t element_size = sizeof *str;
     size_t elements_to_write = sizeof str;
     /* Writes str (_including_ the NUL-terminator) to the binary file. */
     size_t elements_written = fwrite(str, element_size, elements_to_write, fp);
     if (elements_written != elements_to_write)
     {
       result = EXIT_FAILURE;
       /* This works for >=c99 only, else the z length modifier is unknown. */
       fprintf(stderr, "fwrite() failed: wrote only %zu out of %zu elements.\n",
         elements_written, elements_to_write);
       /* Use this for <c99: *
       fprintf(stderr, "fwrite() failed: wrote only %lu out of %lu elements.\n",
         (unsigned long) elements_written, (unsigned long) elements_to_write);
        */
     }
     fclose(fp);
   }
   return result;
}
```
This program creates and writes text in the binary form through the fwrite function to the ﬁle output.bin.

To write integers portably, it must be known whether the ﬁle format expects them in big or little-endian format, and
the size (usually 16, 32 or 64 bits). Bit shifting and masking may then be used to write out the bytes in the correct
order. Integers in C are not guaranteed to have two's complement representation (though almost all
implementations do). Fortunately a conversion to unsigned is guaranteed to use twos complement. The code for
writing a signed integer to a binary ﬁle is therefore a little surprising.
```
/* write a 16-bit little endian integer */
int fput16le(int x, FILE *fp)
{
    unsigned int rep = x;
    int e1, e2;
    e1 = fputc(rep & 0xFF, fp);
    e2 = fputc((rep >> 8) & 0xFF, fp);
    if(e1 == EOF || e2 == EOF)
        return EOF;
    return 0;  
}
```

## Conversion Speciﬁers for printing
Speciﬁers for print format parameter 
|Conversion Speciﬁer |Type of Argument |Description |
|----- |:---------:|:------------|
|i, d |int |prints decimal|
|u |unsigned int| prints decimal|
|o |unsigned int| prints octal|
|x |unsigned int| prints hexadecimal, lower-case|
|X |unsigned int| prints hexadecimal, upper-case|
|f |double| prints ﬂoat with a default precision of 6, |if no precision is given (lower-case used for special numbers nan and inf or infinity)|
|F |double| prints ﬂoat with a default precision of 6, if no precision is given (upper-case used for special numbers NAN and INF or INFINITY)
|e |double| prints ﬂoat with a default precision of 6, if no precision is given, using scientiﬁc notation using mantissa/exponent; lower-case exponent and special numbers
|E |double| prints ﬂoat with a default precision of 6, if no precision is given, using scientiﬁc notation using mantissa/exponent; upper-case exponent and special numbers
|g |double| uses either f or e [see below]
|G |double| uses either F or E [see below]
|a |double| prints hexadecimal, lower-case
|A |double| prints hexadecimal, upper-case
|c |char| prints single character
|s| char*| prints string of characters up to a NUL terminator, or truncated to length given by precision, if speciﬁed
|p| void*| prints void-pointer value; a nonvoid-pointer should be explicitly converted ("cast") to void*; pointer to object only, not a function-pointer
|%| n/a| prints the % character
|n| int *| write the number of bytes printed so far into the int pointed at.

* reserved
## Printing the Value of a Pointer to an Object
To print the value of a pointer to an object (as opposed to a function pointer) use the p conversion speciﬁer. It is
deﬁned to print void-pointers only, so to print out the value of a non void-pointer it needs to be explicitly
converted ("casted*") to void*.
```
int main(void)
{
  int i;
  int * p = &i;
  printf("The address of i is %p.\n", (void*) p);
  return EXIT_SUCCESS;
}
```

Version ≥ C99

Using <inttypes.h> and uintptr_t

Another way to print pointers in C99 or later uses the uintptr_t type and the macros from <inttypes.h>:
```
#include <inttypes.h> /* for uintptr_t and PRIXPTR */
#include <stdio.h>    /* for printf() */
int main(void)
{
  int  i;
  int *p = &i;
  printf("The address of i is 0x%" PRIXPTR ".\n", (uintptr_t)p);
  return 0;
}
```

## Length modiﬁers
* reserved

## Pointers
```
int *pointer; 
/* inside a function, pointer is uninitialized and doesn't point to any valid object yet */
```

```
int value = 1;
pointer = &value;

printf("Value of pointed to integer: %d\n", *pointer);
/* Value of pointed to integer: 1 */
```
```
SomeStruct *s = &someObject;
s->someMember = 5; /* Equivalent to (*s).someMember = 5 */
```

*ptr refers to the ﬁrst double, *(ptr + 1) to the second, *(ptr + 2) to the third.
```
double point[3] = {0.0, 1.0, 2.0};
double *ptr = point;
/* prints x 0.0, y 1.0 z 2.0 */
printf("x %f y %f z %f\n", ptr[0], ptr[1], ptr[2]);
```


## ptr Common errors
Memory allocation is not guaranteed to succeed.
For example, unsafe way:
```
struct SomeStruct *s = malloc(sizeof *s);
s->someValue = 0; /* UNSAFE, because s might be a null pointer */
```
Safe way:
```
struct SomeStruct *s = malloc(sizeof *s);
if (s)
{
    s->someValue = 0; /* This is safe, we have checked that s is valid */
}
```

Non-portable allocation:
```
int *intPtr = malloc(4*1000);    /* allocating storage for 1000 int */
long *longPtr = malloc(8*1000);  /* allocating storage for 1000 long */
```
Portable allocation:
```
int *intPtr = malloc(sizeof(int)*1000);     /* allocating storage for 1000 int */
long *longPtr = malloc(sizeof(long)*1000);  /* allocating storage for 1000 long */
```
better still:
```
int *intPtr = malloc(sizeof(*intPtr)*1000);     /* allocating storage for 1000 int */
long *longPtr = malloc(sizeof(*longPtr)*1000);  /* allocating storage for 1000 long */
```


* Memory leaks:
Failure to de-allocate memory using free leads to a buildup of non-reusable memory, which is no longer used by
the program; this is called a memory leak. Memory leaks waste memory resources and can lead to allocation
failures.
Use free:
```
void f(int n)
{
  int* array = calloc(n, sizeof(int));
  do_some_work(array);
  free(array);
}
```

* Creating pointers to stack variables

Creating a pointer does not extend the life of the variable being pointed to. For example:
```
int* myFunction()
{
    int x = 10;
    return &x;
}
```
Here, x has automatic storage duration (commonly known as stack allocation). Because it is allocated on the stack, its
lifetime is only as long as myFunction is executing; after myFunction has exited, the variable x is destroyed. This
function gets the address of x (using &x), and returns it to the caller, leaving the caller with a pointer to a non-
existent variable. Attempting to access this variable will then invoke undeﬁned behavior.

x 的生命週期於myFunction結束而終結。回傳了被清掉內容的x pointer.於caller調用回傳的X pointer 造成undeﬁned behavior。某些編譯器未清掉可以會產生預期結果，但該位置(pointer)可能會被其他操作複寫。

resolve:
```
#include <stdlib.h>
#include <stdio.h>
int *solution1(void)
{
    int *x = malloc(sizeof *x);
    if (x == NULL)
    {
        /* Something went wrong */
        return NULL;
    }
    *x = 10;
    return x;
}
void solution2(int *x)
{
    /* NB: calling this function with an invalid or null pointer
       causes undefined behaviour. */
    *x = 10;
}
int main(void)
{
    {
        /* Use solution1() */
        int *foo = solution1();  
        if (foo == NULL)
        {
            /* Something went wrong */
            return 1;
        }
        printf("The value set by solution1() is %i\n", *foo);
        /* Will output: "The value set by solution1() is 10" */
        free(foo);    /* Tidy up */
    }
    {
        /* Use solution2() */
        int bar;
        solution2(&bar);
        printf("The value set by solution2() is %i\n", bar);
        /* Will output: "The value set by solution2() is 10" */
    }
    return 0;
}
```

* Incrementing / decrementing and dereferencing

If you write *p++ to increment what is pointed by p, you are wrong.

Post incrementing / decrementing is executed before dereferencing. Therefore, this expression will increment the pointer p itself and return what was pointed by p before incrementing without changing it.

You should write (*p)++ to increment what is pointed by p.

*p++ 不會如其他operationv先增加pointer位置再dereferencing。

* Dereferencing a Pointer to a struct
```
int a = 1;
int *a_pointer = &a;
```
```
struct MY_STRUCT
{
    int my_int;
    float my_float;
};
typedef struct MY_STRUCT MY_STRUCT;
MY_STRUCT *instance;

MY_STRUCT info = { 1, 3.141593F };
MY_STRUCT *instance = &info;

int a = (*instance).my_int;
float b = instance->my_float;
```
## Type Qualiﬁers
* Volatile variables

The volatile keyword tells the compiler that the value of the variable may change at any time as a result of
external conditions, not only as a result of program control ﬂow.
```
volatile int foo; /* Different ways to declare a volatile variable */
int volatile foo;
volatile uint8_t * pReg; /* Pointers to volatile variable */
uint8_t volatile * pReg;
```
* To interface with hardware that has memory-mapped I/O registers.
* When using variables that are modiﬁed outside the program control ﬂow (e.g., in an interrupt service routine)

example
```
int quit = false;
void main()
{
    ...
    while (!quit) {
      // Do something that does not modify the quit variable
    }
    ...
}
void interrupt_handler(void)
{
  quit = true;
}
```
---
The same problem happens when accessing hardware, as we see in this example:
```
uint8_t * pReg = (uint8_t *) 0x1717;
// Wait for register to become non-zero
while (*pReg == 0) { } // Do something else
```
The behavior of the optimizer is to read the variable's value once, there is no need to reread it, since the value will
always be the same. So we end up with an inﬁnite loop. To force the compiler to do what we want, we modify the
declaration to:
```
uint8_t volatile * pReg = (uint8_t volatile *) 0x1717;
```

## Unmodiﬁable (const) variables
```
const int a = 0; /* This variable is "unmodifiable", the compiler
                    should throw an error when this variable is changed */
int b = 0; /* This variable is modifiable */
b += 10; /* Changes the value of 'b' */
a += 10; /* Throws a compiler error */
```
The const qualiﬁcation only means that we don't have the right to change the data. It doesn't mean that the value
cannot change behind our back.

```
_Bool doIt(double const* a) {
   double rememberA = *a;
   // do something long and complicated that calls other functions
   return rememberA == *a;
}
```
During the execution of the other calls *a might have changed, and so this function may return either false or
true.

* Warning

Variables with const qualiﬁcation could still be changed using pointers:
```
const int a = 0;
int *a_ptr = (int*)&a; /* This conversion must be explicitly done with a cast */
*a_ptr += 10;          /* This has undefined behavior */
printf("a = %d\n", a); /* May print: "a = 10" */
```
But doing so is an error that leads to undeﬁned behavior. The diﬃculty here is that this may behave as expected in
simple examples as this, but then go wrong when the code grows.

## Typedef
```
typedef struct Person {
    char name[32];
    int age;
} Person;
Person person;
```
or
```
typedef struct Person {
    char name[32];
    int age;
    struct Person *next;
} Person;

//or

typedef struct Person Person;
struct Person {
    char name[32];
    int age;
    Person *next;
};
```

typedef for a union type is very similar.
```
typedef union Float Float;
union Float
{
    float f;
    char  b[sizeof(float)];
};
```

## Typedef for Function Pointers
```
#include<stdio.h>
void print_to_n(int n)
{
    for (int i = 1; i <= n; ++i)
        printf("%d\n", i);
}
void print_n(int n)
{
    printf("%d\n, n);
}
```
```
typedef void (*printer_t)(int);
```
This creates a type, named printer_t for a pointer to a function that takes a single int argument and returns
nothing, which matches the signature of the functions we have above. To use it we create a variable of the created
type and assign it a pointer to one of the functions in question:
```
printer_t p = &print_to_n;
void (*p)(int) = &print_to_n; // This would be required without the type
```
Then to call the function pointed to by the function pointer variable:
```
p(5);           // Prints 1 2 3 4 5 on separate lines
(*p)(5);        // So does this
```
---
* without function pointer type 
```
void foo (void (*printer)(int), int y){
    //code
    printer(y);
    //code
}
```
* with function pointer type 
```
void foo (printer_t printer, int y){
    //code
    printer(y);
    //code
}
```
---


## Storage Classes
---
## auto

This storage class denotes that an identiﬁer has automatic storage duration. This means once the scope in which
the identiﬁer was deﬁned ends, the object denoted by the identiﬁer is no longer valid.

Since all objects, not living in global scope or being declared static, have automatic storage duration by default
when deﬁned, this keyword is mostly of historical interest and should not be used:
```
int foo(void)
{
    /* An integer with automatic storage duration. */
    auto int i = 3;
    /* Same */
    int j = 5;
    return 0;
} /* The values of i and j are no longer able to be used. */
```
---
## register

Hints to the compiler that access to an object should be as fast as possible. Whether the compiler actually uses the
hint is implementation-deﬁned; it may simply treat it as equivalent to auto.

The only property that is deﬁnitively diﬀerent for all objects that are declared with register is that they cannot
have their address computed. Thereby register can be a good tool to ensure certain optimizations:

```
register size_t size = 467;
```
is an object that can never alias because no code can pass its address to another function where it might be
changed unexpectedly.
This property also implies that an array
```
register int array[5];
```
register 被能被計算地址，也代表不能使array衰退成pointer，或被取址送到function。

The register storage class is more appropriate for variables that are deﬁned inside a block and are accessed with
high frequency. For example,
```
/* prints the sum of the first 5 integers*/
/* code assumed to be part of a function body*/
{
    register int k, sum;
    for(k = 1, sum = 0; k < 6; sum += k, k++);
        printf("\t%d\n",sum);
}
```
The _Alignof operator is also allowed to be used with register arrays. 

In fact, the only legal usage of an array declared with a register storage class is the sizeof operator; any other
operator would require the address of the ﬁrst element of the array. For that reason, arrays generally should not be
declared with the register keyword since it makes them useless for anything other than size computation of the
entire array, which can be done just as easily without the register keyword.

In C, the register keyword is used to suggest to the compiler that a particular variable should be stored in a CPU register for better performance. It is not guaranteed that the variable will be stored in a register as the compiler can choose to ignore the request if it decides that storing the variable in memory would be more efficient.

It's worth noting that the register keyword is considered obsolete in modern compilers, as modern compilers are smart enough to automatically optimize the storage of variables in registers when it is appropriate to do so.

--- 
## static

The static storage class serves diﬀerent purposes, depending on the location of the declaration in the ﬁle:

1. To conﬁne the identiﬁer to that translation unit only (scope=ﬁle).
```
/* No other translation unit can use this variable. */
static int i;
/* Same; static is attached to the function type of f, not the return type int. */
static int f(int n);
```
translation unit : 

In C programming, a translation unit is the basic unit of compilation. It is the source file and all of its included header files after preprocessing. A translation unit can be compiled independently, and its compiled object code can be combined with other object code to form a program.

Each translation unit is processed by the compiler through a series of stages known as the translation phases, which include preprocessing, lexical analysis, parsing, semantic analysis, code generation, and optimization.

static 變數在不同的translation unit會獨立，例如在兩個file1.o file2.o的目標檔所連結成的執行檔。translation unit大致上相當於compile後的.o object file。在這兩個.o黨的.c原始檔中不需要(也不可)引用(include)彼此。
e.g. 
```
gcc -o myprogram main.o file1.o file2.o
```

2.  To save data for use with the next call of a function (scope=block):
```
void foo()
 {
     static int a = 0; /* has static storage duration and its lifetime is the
                        * entire execution of the program; initialized to 0 on
                        * first function call */
     int b = 0; /* b has block scope and has automatic storage duration and
                 * only "exists" within function */
     
     a += 10;
     b += 10;
     printf("static int a = %d, int b = %d\n", a, b);
 }
 int main(void)
 {
     int i;
     for (i = 0; i < 5; i++)
     {
         foo();
     }
     return 0;
 }
 ```
 output
 ```
 static int a = 10, int b = 10
 static int a = 20, int b = 10
 static int a = 30, int b = 10
 static int a = 40, int b = 10
 static int a = 50, int b = 10
 ```

 3. Used in function parameters to denote an array is expected to have a constant minimum number of
elements and a non-null parameter:
```
/* a is expected to have at least 512 elements. */
void printInts(int a[static 512])
{
    size_t i;
    for (i = 0; i < 512; ++i)
        printf("%d\n", a[i]);
}
```

The static keyword has a different meaning in C. When used as a storage class specifier, it indicates that a variable or function has static storage duration. This means that the variable or function is allocated once and retains its value throughout the program's execution, rather than being created and destroyed each time the variable or function is used.

In the context of the printInts function, the static keyword could be used to declare a local variable within the function that retains its value between function calls. However, this usage would not affect the behavior of the function parameter declaration.


## extern

Used to declare an object or function that is deﬁned elsewhere (and that has external linkage). In general, it is
used to declare an object or function to be used in a module that is not the one in which the corresponding object
or function is deﬁned:

```
/* file1.c */
int foo = 2;  /* Has external linkage since it is declared at file scope. */
/* file2.c */
#include <stdio.h>
int main(void)
{
    /* `extern` keyword refers to external definition of `foo`. */
    extern int foo;
    printf("%d\n", foo);
    return 0;
}
```

Things get slightly more interesting with the introduction of the inline keyword in C99:
```
/* Should usually be place in a header file such that all users see the definition */
/* Hints to the compiler that the function `bar` might be inlined */
/* and suppresses the generation of an external symbol, unless stated otherwise. */
inline void bar(int drink)
{
    printf("You ordered drink no.%d\n", drink);
}
/* To be found in just one .c file.
   Creates an external function definition of `bar` for use by other files.
   The compiler is allowed to choose between the inline version and the external
   definition when `bar` is called. Without this line, `bar` would only be an inline
   function, and other files would not be able to call it. */
extern void bar(int);
```
Note:
So, while #include allows you to use code from other files in the same translation unit, extern allows you to use code from other files in different translation units.


## _Thread_local
 
In C11 along with multi-threading. This isn't available in earlier C
standards.

Denotes thread storage duration. A variable declared with _Thread_local storage speciﬁer denotes that the object is
local to that thread and its lifetime is the entire execution of the thread in which it's created. It can also appear along
with static or extern.
```
#include <threads.h>
#include <stdio.h>
#define SIZE 5
int thread_func(void *id)
{
    /* thread local variable i. */
    static _Thread_local int i;
    /* Prints the ID passed from main() and the address of the i.
     * Running this program will print different addresses for i, showing
     * that they are all distinct objects. */
    printf("From thread:[%d], Address of i (thread local): %p\n", *(int*)id, (void*)&i);
    return 0;
}
int main(void)
{
    thrd_t id[SIZE];
    int arr[SIZE] = {1, 2, 3, 4, 5};
    /* create 5 threads. */
    for(int i = 0; i < SIZE; i++) {
        thrd_create(&id[i], thread_func, &arr[i]);
    }
    /* wait for threads to complete. */
    for(int i = 0; i < SIZE; i++) {
        thrd_join(id[i], NULL);
    }
}
```

---
## Const Pointers
* Pointer to a const int
```
int b;
const int* p;
p = &b;    /* OK */
*p = 100;  /* Compiler Error */
```
* const pointer to int
```
int a, b;
int* const p = &b; /* OK as initialisation, no assignment */
*p = 100;  /* OK */
p = &a;    /* Compiler Error */
```
* const pointer to const int
```
int a, b;
const int* const p = &b; /* OK as initialisation, no assignment */
p = &a;   /* Compiler Error */
*p = 100; /* Compiler Error */
```
Pointer to pointer
* Pointer to pointer to a const int
```
void f2(void)
{
  int b;
  const int *p1;
  const int **p;
  p = &p1; /* OK */
  *p = &b; /* OK */
  **p = 100; /* error: assignment of read-only location ‘**p’ */
}
```
* Pointer to const pointer to an int
```
void f3(void)
{
  int b;
  int *p1;
  int * const *p;
  p = &p1; /* OK */
  *p = &b; /* error: assignment of read-only location ‘*p’ */
  **p = 100; /* OK */
}
```
* const pointer to pointer to int
```
void f4(void)
{
  int b;
  int *p1;
  int ** const p = &p1; /* OK as initialisation, not assignment */
  p = &p1; /* error: assignment of read-only variable ‘p’ */
  *p = &b; /* OK */
  **p = 100; /* OK */
}
```
* Pointer to const pointer to const int
```
void f5(void)
{
  int b;
  const int *p1;
  const int * const *p;
  p = &p1; /* OK */
  *p = &b; /* error: assignment of read-only location ‘*p’ */
  **p = 100; /* error: assignment of read-only location ‘**p’ */
}
```
* const pointer to pointer to const int
```
void f6(void)
{
  int b;
  const int *p1;
  const int ** const p = &p1; /* OK as initialisation, not assignment */
  p = &p1; /* error: assignment of read-only variable ‘p’ */
  *p = &b; /* OK */
  **p = 100; /* error: assignment of read-only location ‘**p’ */
}
```
* const pointer to const pointer to int
```
void f7(void)
{
  int b;
  int *p1;
  int * const * const p = &p1; /* OK as initialisation, not assignment */
  p = &p1; /* error: assignment of read-only variable ‘p’  */
  *p = &b; /* error: assignment of read-only location ‘*p’ */
  **p = 100; /* OK */
}
```

## Function pointers
```
int my_function(int a, int b)
{
    return 2 * a + 3 * b;
}
```
pointer of that function's type:
```
int (*my_pointer)(int, int);

/*templete:
return_type_of_func (*my_func_pointer)(type_arg1, type_arg2, ...)*/
```

assign this pointer:
```
my_pointer = &my_function;
```
used to call the function:
```
/* Calling the pointed function */
int result = (*my_pointer)(4, 2);
...
/* Using the function pointer as an argument to another function */
void another_function(int (*another_pointer)(int, int))
{
    int a = 4;
    int b = 2;
    int result = (*another_pointer)(a, b);
    printf("%d\n", result);
}
```
```
/* Attribution without the & operator */
my_pointer = my_function;
/* Dereferencing without the * operator */
int result = my_pointer(4, 2);
```
To increase the readability of function pointers, typedefs may be used.
```
typedef void (*Callback)(int a);
void some_function(Callback callback)
{
    int a = 4;
    callback(a);
}
```
or we can use: 
```
void some_function(void callback(int))
{
    int a = 4;
    callback(a);
}
```
以上兩者相等，再function call的時候給定base function，有相同的function signature 即可。

Note: function signature not include function name. example: `void (*)(int)`

* Using typedef 
```
typedef returnType (*name)(parameters);
```

Without a typedef we would pass a function pointer as an argument to a function in the following manner:
```
void sort(int (*compare)(const void *elem1, const void *elem2)) {
    /* inside of this block, the function is named "compare" */
}
```
With a typedef:
```
typedef int (*compare_func)(const void *, const void *);

void sort(compare_func func) {
    /* In this block the function is named "func" */
}
```


function( sort would accept any function of the form) 
```
int compare(const void *arg1, const void *arg2) {
    /* Note that the variable names do not have to be "elem1" and "elem2" */
}
```


* Initializing Pointers
```
#include <stddef.h>
int main()
{
    int *p1 = NULL;
    char *p2 = NULL;
    float *p3 = NULL;
         /* NULL is a macro defined in stddef.h, stdio.h, stdlib.h, and string.h */
         /* Pointer initialization is a good way to avoid wild pointers. The initialization is simple and is no diﬀerent from initialization of a variable. */
         /* Usually NULL is deﬁned as (void *)0. But this does not imply that the assigned memory address is 0x0
         */
    ...
}    
```


## Returning Function Pointers from a Function
```
#include <stdio.h>
enum Op
{
    ADD = '+',
  SUB = '-',
};

/* add: add a and b, return result */    
int add(int a, int b)
{
    return a + b;
}
/* sub: subtract b from a, return result */
int sub(int a, int b)
{
    return a - b;
}
/* getmath: return the appropriate math function */
int (*getmath(enum Op op))(int,int)
{
    switch (op)
    {
        case ADD:
            return &add;
        case SUB:
            return &sub;
        default:
            return NULL;
    }
}
int main(void)
{
    int a, b, c;
    int (*fp)(int,int);
    fp = getmath(ADD);
    a = 1, b = 2;
    c = (*fp)(a, b);
    printf("%d + %d = %d\n", a, b, c);
    return 0;
}
```

## Assigning a Function Pointer
```
#include <stdio.h>
/* increment: take number, increment it by one, and return it */
int increment(int i)
{
    printf("increment %d by 1\n", i);
    return i + 1;
}
/* decrement: take number, decrement it by one, and return it */
int decrement(int i)
{
    printf("decrement %d by 1\n", i);
    return i - 1;
}
int main(void)
{
    int num = 0;          /* declare number to increment */
    int (*fp)(int);       /* declare a function pointer */
    fp = &increment;      /* set function pointer to increment function */
    num = (*fp)(num);     /* increment num */
    num = (*fp)(num);     /* increment num a second time */
    fp = &decrement;      /* set function pointer to decrement function */
    num = (*fp)(num);     /* decrement num */
    printf("num is now: %d\n", num);
    return 0;
}
```
```
int decrement(int i)
{
    printf("decrement %d by 1\n", i);
    return i - 1;
}

int (*fp)(int);       /* declare a function pointer */
fp = &increment;      /* set function pointer to increment 
```

## declaring and change function pointer
Declaring the pointer takes the return value of the function, the name of the function, and the type of
arguments/parameters it receives.

* You can declare and initialize a pointer to this function:
```
int addInt(int n, int m){
    return n+m;
}

int (*functionPtrAdd)(int, int) = addInt; // or &addInt - the & is optional
```
```
typedef int (*ptrInt)(int, int);
int Add(int i, int j){
    return i+j;
}
int Multiply(int i, int j){
    return i*j;
}
int main()
{
    ptrInt ptr1 = Add;
    ptrInt ptr2 = Multiply;
    printf("%d\n", (*ptr1)(2,3)); //will print 5
    printf("%d\n", (*ptr2)(2,3)); //will print 6
    return 0;
}
```

## Pass 2D-arrays to functions

* p.165
reserved. not understand all.

```
#include <stdio.h>
#include <stdlib.h>
/* ALL CHECKS OMMITTED!*/
void fun1(int rows, int cols, int **);
int main(int argc, char **argv)
{
  int rows, cols, i, j;

  if(argc != 3){
     fprintf(stderr,"Usage: %s rows cols\n",argv[0]);
     exit(EXIT_FAILURE);
  }
  rows = atoi(argv[1]);
  cols = atoi(argv[2]);
  int array_2D[rows][cols];
  printf("Make array with %d rows and %d columns\n", rows, cols);
  for (i = 0; i < rows; i++) {
    for (j = 0; j < cols; j++) {
      array_2D[i][j] = i * cols + j;
      printf("array[%d][%d]=%d\n", i, j, array_2D[i][j]);
    }
  }
  // a "rows" number of pointers to "int". Again a VLA
  int *a[rows];
  // initialize them to point to the individual rows
  for (i = 0; i < rows; i++) {
      a[i] = array_2D[i];
  }
  fun1(rows, cols, a);
  exit(EXIT_SUCCESS);
}
void fun1(int rows, int cols, int **a)
{
  int i, j;
  int n, m;
  n = rows;
  m = cols;
  printf("\nPrint array with %d rows and %d columns\n", rows, cols);
  for (i = 0; i < n; i++) {
    for (j = 0; j < m; j++) {
      printf("array[%d][%d]=%d\n", i, j, a[i][j]);
    }
  }
}
```

## Error handling
* errno
The C standard requires at least 3 values for errno be set:

EDOM Domain error

ERANGE Range error

EILSEQ Illegal multi-byte character sequence

* perror
print a user-readable error message to stderr, call perror from <stdio.h>.
```
int main(int argc, char *argv[])
{
   FILE *fout;
   if ((fout = fopen(argv[1], "w")) == NULL) {
      perror("fopen: Could not open file for writing");
      return EXIT_FAILURE;
   }
return EXIT_SUCCESS;
}
```
* strerror
f perror is not ﬂexible enough, you may obtain a user-readable error description by calling strerror from
<string.h>.
```
int main(int argc, char *argv[])
{
    FILE *fout;
    int last_error = 0;
    if ((fout = fopen(argv[1], "w")) == NULL) {
        last_error = errno;
         /* reset errno and continue */
         errno = 0;
    }
    /* do some processing and try opening the file differently, then */

    if (last_error) {
        fprintf(stderr, "fopen: Could not open %s for writing: %s",
                argv[1], strerror(last_error));
        fputs("Cross fingers and continue", stderr);
    }
    /* do some other processing */
    return EXIT_SUCCESS;
}
```

## race condiction (data race) & thread 

* reserved

## Random Number Generation

```
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
int main(void) {
    int i;
    srand(time(NULL));
    i = rand();
    printf("Random value between [0, %d]: %d\n", RAND_MAX, i);
    return 0;
}
```
The C Standard does not guarantee the quality of the random sequence produced. In the past, some
implementations of rand() had serious issues in distribution and randomness of the generated numbers. The
usage of rand() is not recommended for serious random number generation needs, like cryptography.


# Preprocessor and Macros

## Header Include Guards

my-header-ﬁle.h
```
#ifndef MY_HEADER_FILE_H
#define MY_HEADER_FILE_H
// Code body for header file
#endif
```
header-1.h
```
typedef struct {
    …
} MyStruct;
int myFunction(MyStruct *value);
```
header-2.h
```
#include "header-1.h"
int myFunction2(MyStruct *value);
```
main.c
```
#include "header-1.h"
#include "header-2.h"
int main() {
    // do something
}
```
This code has a serious problem: the detailed contents of MyStruct is deﬁned twice, which is not allowed. This
would **result in a compilation error** that can be diﬃcult to track down, since one header ﬁle includes another. If you
instead did it with header guards:

header-1.h
```
#ifndef HEADER_1_H
#define HEADER_1_H
typedef struct {
        …
} MyStruct;
int myFunction(MyStruct *value);
#endif
```
header-2.h
```
#ifndef HEADER_2_H
#define HEADER_2_H
#include "header-1.h"
int myFunction2(MyStruct *value);
#endif
```
main.c
```
#include "header-1.h"
#include "header-2.h"
int main() {
    // do something
}
```

## #if 0 to block out code sections
```
#if 0
/* #if 0 evaluates to false, so everything between here and the #endif are
 * removed by the preprocessor. */
int myUnusedFunction(void)
{
    int i = 5;
    return i;
}
#endif
```

## Function-like macros

Function-like macros are similar to inline functions, these are useful in some cases, such as temporary debug log:

```
#ifdef DEBUG
# define LOGFILENAME "/tmp/logfile.log"
# define LOG(str) do {                            \
  FILE *fp = fopen(LOGFILENAME, "a");            \
  if (fp) {                                       \
    fprintf(fp, "%s:%d %s\n", __FILE__, __LINE__, \
                 /* don't print null pointer */   \
                 str ?str :"<null>");             \
    fclose(fp);                                   \
  }                                               \
  else {                                          \
    perror("Opening '" LOGFILENAME "' failed");   \
  }                                               \
} while (0)
#else
  /* Make it a NOOP if DEBUG is not defined. */
# define LOG(LINE) (void)0
#endif

#include <stdio.h>
int main(int argc, char* argv[])
{
    if (argc > 1)
        LOG("There are command line arguments");
    else
        LOG("No command line arguments");
    return 0;
}
```

## Source ﬁle inclusion
```
#include <stdio.h>
#include "myheader.h"
```
#include replaces the statement with the contents of the ﬁle referred to. Angle brackets (<>) refer to header ﬁles
installed on the system, while quotation marks ("") are for user-supplied ﬁles.

```
#if VERSION == 1
    #define INCFILE  "vers1.h"
#elif VERSION == 2
    #define INCFILE  "vers2.h"
    /*  and so on */
#else
    #define INCFILE  "versN.h"
#endif
/* ... */
#include INCFILE
```

## Token pasting
Token pasting allows one to glue together two macro arguments. For example, front##back yields frontback. A
famous example is Win32's <TCHAR.H> header. In standard C, one can write L"string" to declare a wide character
string. However, Windows API allows one to convert between wide character strings and narrow character strings
simply by #defineing UNICODE. In order to implement the string literals, TCHAR.H uses this

```
#ifdef UNICODE
#define TEXT(x) L##x
#endif
```

TEXT("hello, world") -> L"hello, world"

## Predeﬁned Macros
A predeﬁned macro is a macro that is already understood by the C pre processor without the program needing to
deﬁne it. Examples include

Mandatory Pre-Deﬁned Macros:
* \_\_FILE__, which gives the ﬁle name of the current source ﬁle (a string literal),
* \_\_LINE__ for the current line number (an integer constant),
* \_\_DATE__ for the compilation date (a string literal),
* \_\_TIME__ for the compilation time (a string literal).

e.g:
```
fprintf(stderr, "%s: %s: %d: Denominator is 0", __FILE__, __func__, __LINE__);
```

## Variadic arguments macro
```
#define debug_print(msg) printf("%s:%d %s", __FILE__, __LINE__, msg)

#define debug_print(msg, ...) printf(msg, __VA_ARGS__) \
                               printf("\nError occurred in file:line (%s:%d)\n", __FILE__, __LINE)
```
```
#define debug_print(msg, ...) printf(msg, ##__VA_ARGS__) \
                               printf("\nError occurred in file:line (%s:%d)\n", __FILE__, __LINE)
```
__VA_ARGS__ 會替換成前面的paramater "..."

##\_\_VA_ARGS__ 若分號後面無paramater，分號會被刪除避免SYNTAX ERROR


## Signal handling

```
#include <stdio.h>  /* printf() */
#include <stdlib.h> /* abort()  */
#include <signal.h> /* signal() */
void handler_nonportable(int sig)
{
    /* undefined behavior, maybe fine on specific platform */
    printf("Catched: %d\n", sig);
   
    /* abort is safe to call */
    abort();
}
sig_atomic_t volatile finished = 0;
void handler(int sig)
{
    switch (sig) {
    /* hardware interrupts should not return */
    case SIGSEGV:
    case SIGFPE:
    case SIGILL:
/*Version ≥ C11
      /* quick_exit is safe to call */
      quick_exit(EXIT_FAILURE);*/
/*Version < C11*/
      /* use _Exit in pre-C11 */
      _Exit(EXIT_FAILURE);
    default:
       /* Reset the signal to the default handler,
          so we will not be called again if things go
          wrong on return. */
      signal(sig, SIG_DFL);
      /* let everybody know that we are finished */
      finished = sig;
      return;
   }
}
int main(void)
{
    /* Catch the SIGSEGV signal, raised on segmentation faults (i.e NULL ptr access */
    if (signal(SIGSEGV, &handler) == SIG_ERR) {
        perror("could not establish handler for SIGSEGV");
        return EXIT_FAILURE;
    }
    /* Catch the SIGTERM signal, termination request */
    if (signal(SIGTERM, &handler) == SIG_ERR) {
        perror("could not establish handler for SIGTERM");
        return EXIT_FAILURE;
    }
    /* Ignore the SIGINT signal, by setting the handler to `SIG_IGN`. */
    signal(SIGINT, SIG_IGN);

    /* Do something that takes some time here, and leaves
       the time to terminate the program from the keyboard. */
    /* Then: */
    if (finished) {
       fprintf(stderr, "we have been terminated by signal %d\n", (int)finished);
        return EXIT_FAILURE;
    }

    /* Try to force a segmentation fault, and raise a SIGSEGV */
    {
      char* ptr = 0;
      *ptr = 0;
    }
    /* This should never be executed */
    return EXIT_SUCCESS;
}
```
POSIX recommends the usage of sigaction() instead of signal(), due to its underspeciﬁed behavior and
signiﬁcant implementation variations.

## Variable arguments

可變長度引數。
Variable arguments are used by functions in the printf family (printf, fprintf, etc) and others to allow a function
to be called with a diﬀerent number of arguments each time, hence the name varargs.

* Using an explicit count argument to determine the length of the va_list
```
#include <stdio.h>
#include <stdarg.h>
/* first arg is the number of following int args to sum. */
int sum(int n, ...) {
    int sum = 0;
    va_list it; /* hold information about the variadic argument list. */
    va_start(it, n); /* start variadic argument processing */
    while (n--)
      sum += va_arg(it, int); /* get and sum the next variadic argument */
    va_end(it); /* end variadic argument processing */
    return sum;
}
int main(void)
{
    printf("%d\n", sum(5, 1, 2, 3, 4, 5)); /* prints 15 */
    printf("%d\n", sum(10, 5, 9, 2, 5, 111, 6666, 42, 1, 43, -6218)); /* prints 666 */
    return 0;
}
```

## Assertion
Most common are simple assertions, which are validated at execution time. However, static
assertions are checked at compile time.
* Simple Assertion
```
#include <stdio.h>
/* Uncomment to disable `assert()` */
/* #define NDEBUG */
#include <assert.h>
int main(void)
{
    int x = -1;
    assert(x >= 0);
    printf("x = %d\n", x);  
    return 0;
}
```
Possible output with NDEBUG undeﬁned:
```
a.out: main.c:9: main: Assertion `x >= 0' failed.
```
* Static Assertion

A static assertion is one that is checked at compile time, not run time. The condition must be a constant expression,
and if false will result in a compiler error. The ﬁrst argument, the condition that is checked, must be a constant
expression, and the second a string literal.
```
#include <assert.h>
enum {N = 5};
_Static_assert(N == 5, "N does not equal 5");
static_assert(N > 10, "N is not greater than 10");  /* compiler error */
```
Prior to C11, there was no direct support for static assertions. However, in C99, static assertions could be emulated
with macros that would trigger a compilation failure if the compile time condition was false.

* Assertion of Unreachable Code

During development, when certain code paths must be prevented from the reach of control ﬂow, you may use
assert(0) to indicate that such a condition is erroneous:

```
switch (color) {
    case COLOR_RED:
    case COLOR_GREEN:
    case COLOR_BLUE:
        break;
    default:
        assert(0);
}
```
Whenever the argument of the assert() macro evaluates false, the macro will write diagnostic information to the
standard error stream and then abort the program.

 This information includes the ﬁle and line number of the
assert() statement and can be very helpful in debugging. Asserts can be disabled by deﬁning the macro NDEBUG.

The primary advantage of assert() is that it automatically prints debugging information. Calling abort() has the
advantage that it cannot be disabled like an assert, but it may not cause any debugging information to be displayed.
In some situations, using both constructs together may be beneﬁcial:
```
if (color == COLOR_RED || color == COLOR_GREEN) {
   ...
} else if (color == COLOR_BLUE) {
   ...
} else {
   assert(0), abort();
}
```

## restrict qualiﬁcation
```
void fan(float*restrict e, float*restrict f) {
    float a = *f
    *e = 22;
    float b = *f;
    print("is %g equal to %g?\n", a, b);
}
```
The restrict keyword in C only affects performance and does not affect the value of a pointer.

The restrict keyword is a type qualifier that informs the compiler that a pointer is the only means of accessing a particular memory location. It allows the compiler to generate more optimized code by making more aggressive assumptions about how the memory is accessed. Specifically, the compiler can assume that the memory accessed through a restricted pointer is not accessed through any other pointer, and can therefore perform certain optimizations that might not be possible otherwise.

However, the restrict keyword does not affect the value of the pointer itself. A restricted pointer still points to the same memory location as an unrestricted pointer, and can still be dereferenced and used like any other pointer. The only difference is that the compiler is allowed to make certain assumptions about how the memory is accessed through the restricted pointer.

It is also worth noting that the restrict keyword does not guarantee improved performance in all cases. It is up to the compiler to determine whether a particular use of the restrict keyword can result in improved performance, and the effectiveness of the optimization can depend on various factors such as the size and complexity of the code, the specific optimization being performed, and the underlying hardware architecture.


## Compilation

## Inline assembly
* gcc Inline assembly in macros

We can put assembly instructions inside a macro and use the macro like you would call a function.
```
#define mov(x,y) \
{ \
    __asm__ ("l.cmov %0,%1,%2" : "=r" (x) : "r" (y), "r" (0x0000000F)); \
}
/// some definition and assignment
unsigned char sbox[size][size];
unsigned char sbox[size][size];
///Using
mov(state[0][1], sbox[si][sj]);
```

Using inline assembly instructions embedded in C code can improve the run time of a program. This is very helpful
in time critical situations like cryptographic algorithms such as AES. For example, for a simple shift operation that is
needed in the AES algorithm, we can substitute a direct Rotate Right assembly instruction with C shift operator >>.
In an implementation of 'AES256', in 'AddRoundKey()' function we have some statements like this:

In an implementation of 'AES256', in 'AddRoundKey()' function we have some statements like this:
```
unsigned int w;          // 32-bit
unsigned char subkey[4]; // 8-bit, 4*8 = 32
subkey[0] = w >> 24;     // hold 8 bit, MSB, leftmost group of 8-bits
subkey[1] = w >> 16;     // hold 8 bit, second group of 8-bit from left    
subkey[2] = w >> 8;      // hold 8 bit, second group of 8-bit from right
subkey[3] = w;           // hold 8 bit, LSB, rightmost group of 8-bits
/// subkey <- w
```
```
__asm__ ("l.ror  %0,%1,%2" : "=r" (* (unsigned int *) subkey)  : "r" (w), "r" (0x10));
```
The ﬁnal result is exactly same.

## gcc Extended asm support
* Inline Assembly 

```
asm [volatile] ( AssemblerTemplate
                  : OutputOperands
                  [ : InputOperands
                  [ : Clobbers ] ])
 
 asm [volatile] goto ( AssemblerTemplate
                       :
                       : InputOperands
                       : Clobbers
                       : GotoLabels)
```
* code 使用到的組合語言
* output 輸出的變數
* input 輸入的變數
* clobbered register 哪些 register 會被這段程式碼修改
```
static inline __attribute_const__ __u32 __arch_swab32(__u32 x)
{
    __asm__ ("rev %0, %1" : "=r" (x) : "r" (x));
    return x;
}
```
## Pointer Conversions in Function Calls

Pointer conversions to void* are implicit, but any other pointer conversion must be explicit. While the compiler
allows an explicit conversion from any pointer-to-data type to any other pointer-to-data type, accessing an object
through a wrongly typed pointer is erroneous and leads to undeﬁned behavior. The only case that these are
allowed are if the types are compatible or if the pointer with which your are looking at the object is a character type.

```
#include <stdio.h>
void func_voidp(void* voidp) {
    printf("%s Address of ptr is %p\n", __func__, voidp);
}
/* Structures have same shape, but not same type */
struct struct_a {
    int a;
    int b;
} data_a;
struct struct_b {
    int a;
    int b;
} data_b;
void func_struct_b(struct struct_b* bp) {
    printf("%s Address of ptr is %p\n", __func__, (void*) bp);
}
int main(void) {

    /* Implicit ptr conversion allowed for void* */
    func_voidp(&data_a);
    /*
     * Explicit ptr conversion for other types
     *
     * Note that here although the have identical definitions,
     * the types are not compatible, and that the this call is
     * erroneous and leads to undefined behavior on execution.
     */
    func_struct_b((struct struct_b*)&data_a);
    /* My output shows: */
    /* func_charp Address of ptr is 0x601030 */
    /* func_voidp Address of ptr is 0x601030 */
    /* func_struct_b Address of ptr is 0x601030 */
    return 0;
}
```


# Allocating Memory

The C dynamic memory allocation functions are deﬁned in the <stdlib.h> header. If one wishes to allocate
memory space for an object dynamically, the following code can be used:
```
int *p = malloc(10 * sizeof *p);
if (p == NULL)
{
    perror("malloc() failed");
    return -1;
}
```

It is good practice to use sizeof to compute the amount of memory to request since the result of sizeof is
implementation deﬁned

(except for character types, which are char, signed char and unsigned char, for which
sizeof is deﬁned to always give 1).

---
__Because malloc might not be able to service the request, it might return a null pointer.__

__It is important to
check for this to prevent later attempts to dereference the null pointer.__

---

## Zeroed Memory

The memory returned by malloc may not be initialized to a reasonable value. Use ```memset``` or to immediately copy a suitable value into it

or
use ```calloc```
```
int *p = calloc(10, sizeof *p);
if (p == NULL)
{
    perror("calloc() failed");
    return -1;
}
```

## Aligned Memory
C11 introduced a new function aligned_alloc()

It can be used if
the memory to be allocated is needed to be aligned at certain boundaries
```
/* Allocates 1024 bytes with 256 bytes alignment. */
char *ptr = aligned_alloc(256, 1024);
if (ptr) {
    perror("aligned_alloc()");
    return -1;
}
free(ptr);
```
The C11 standard imposes two restrictions:
1. the size (second argument) requested must be an integral multiple of
the alignment (ﬁrst argument) and 
2. the value of alignment should be a valid alignment supported by the
implementation. Failure to meet either of them results in undeﬁned behavior.

##Freeing Memory
It is possible to release dynamically allocated memory by calling free(). without free will cause __Memory leak__
```
int *p = malloc(10 * sizeof *p); /* allocation of memory */
if (p == NULL)
{
    perror("malloc failed");
    return -1;
}
free(p); /* release of memory */
/* note that after free(p), even using the *value* of the pointer p
   has undefined behavior, until a new value is stored into it. */
/* reusing/re-purposing the pointer itself */
int i = 42;
p = &i; /* This is valid, has defined behaviour */
```

Pointers that reference
memory elements that have been freed are commonly called dangling pointers,  and present a security risk.
Furthermore, the C standard states that even accessing the value of a dangling pointer has undeﬁned behavior.

## realloc(ptr, 0) is not equivalent to free(ptr)

realloc is conceptually equivalent to malloc + memcpy + free on the other pointer.

## Multidimensional arrays of variable size
```
double sumAll(size_t n, size_t m, double A[n][m]) {
    double ret = 0.0;
    for (size_t i = 0; i < n; ++i)
       for (size_t j = 0; j < m; ++j)
          ret += A[i][j]
    return ret;
}
int main(int argc, char *argv[argc+1]) {
   size_t n = argc*10;
   size_t m = argc*8;
   double (*matrix)[m] = malloc(sizeof(double[n][m]));
   // initialize matrix somehow
   double res = sumAll(n, m, matrix);
   printf("result is %g\n", res);
   free(matrix);
}
```

# Atomics
## atomics and operators

Atomic variables can be accessed concurrently between diﬀerent threads without creating race conditions. 

多執行緒中避免race conditions for variable.
```
/* a global static variable that is visible by all threads */
static unsigned _Atomic active = ATOMIC_VAR_INIT(0);

int myThread(void* a) {
  ++active;         // increment active race free
  // do something
  --active;         // decrement active race free
  return 0;
}
```
* Operations on atomic objects are generally orders of magnitude slower than normal arithmetic operations.
This also includes simple load or store operations. So you should only use them for critical tasks.
* Usual arithmetic operations and assignment such as a = a+1; are in fact three operations on a: ﬁrst a load,
then addition and ﬁnally a store. This is not race free. Only the operation a += 1; and a++; are. 

使用a += 1; and a++來替代 a = a+1 可以避免race conditions.
a += 1中的步驟(讀，操作，存)被認為是atomic.

## Notation and Miscellany

The C standard says that there is very little diﬀerence between the #include <header.h> and #include
"header.h" notations.

[#include <header.h>] searches a sequence of implementation-deﬁned places for a header identiﬁed.
* usually system path

[#include "header.h"] causes the replacement of that directive by the entire contents of the source ﬁle
identiﬁed by the speciﬁed sequence between the "…" delimiters.
* usually application root path/user defined path 

A number of projects use a notation such as:
```
#include <openssl/ssl.h>
#include <sys/stat.h>
#include <linux/kernel.h>
```

You should consider whether to use that namespace control in your project (it is quite probably a good idea). You
should steer clear of the names used by existing projects (in particular, both sys and linux would be bad choices).

If you use this, your code should be careful and consistent in the use of the notation.

Do not use ```#include "../include/header.h"``` notation.

Corollary: you will not declare global variables in a source ﬁle — a source ﬁle will only contain deﬁnitions.

---
Header ﬁles should seldom declare static functions, with the notable exception of static inline functions which
will be deﬁned in headers if the function is needed in more than one source ﬁle.

* Source ﬁles deﬁne global variables, and global functions.
* Source ﬁles do not declare the existence of global variables or functions; they include the header that
declares the variable or function.
* Header ﬁles declare global variable and functions (and types and other supporting material).
* Header ﬁles do not deﬁne variables or any functions except (static) inline functions.

# Inlining
For small functions that get called often, the overhead associated with the function call can be a signiﬁcant fraction
of the total execution time of that function. One way of improving performance, then, is to eliminate the overhead.

對於經常被調用的小型函數，函數調用的的開銷可能是總執行時間的大開銷
消除開銷是一種提高效能的方法。

We have four functions (plus main()) in three source files. Two of those functions (plusfive() and timestwo()) are called by the other two functions located in "source1.c" and "source2.c". We included the main() function to create a working example.

我们有三个源文件中的四个函数（加上 main()）。其中两个函数（plusfive() 和 timestwo()）由位于 "source1.c" 和 "source2.c" 中的另外两个函数调用。我们包含了 main() 函数来创建一个工作示例。

In C, when a function is inlined, its code is directly inserted at the place where the function is called instead of being executed by calling the function.

main.c:
```
#include <stdio.h>
#include <stdlib.h>
#include "headerfile.h"
int main(void) {
    int start = 3;
    int intermediate = complicated1(start);
    printf("First result is %d\n", intermediate);
    intermediate = complicated2(start);
    printf("Second result is %d\n", intermediate);
    return 0;
}
```
source1.c:
```
#include <stdio.h>
#include <stdlib.h>
#include "headerfile.h"
int complicated1(int input) {
    int tmp = timestwo(input);
    tmp = plusfive(tmp);
    return tmp;
}
```
source2.c:
```
#include <stdio.h>
#include <stdlib.h>
#include "headerfile.h"
int complicated2(int input) {
    int tmp = plusfive(input);
    tmp = timestwo(tmp);
    return tmp;
}
```
headerﬁle.h:
```
#ifndef HEADERFILE_H
#define HEADERFILE_H
int complicated1(int input);
int complicated2(int input);
inline int timestwo(int input) {
  return input * 2;
}
inline int plusfive(int input) {
 return input + 5;
}
#endif
```

Functions timestwo and plusfive get called by both complicated1 and complicated2, which are in diﬀerent
"translation units", or source ﬁles. In order to use them in this way, we have to deﬁne them in the header.

```
cc -O2 -std=c99 -c -o main.o main.c
cc -O2 -std=c99 -c -o source1.o source1.c
cc -O2 -std=c99 -c -o source2.o source2.c
cc main.o source1.o source2.o -o main
```

__We use the -O2 optimization option because some compilers don't inline without optimization turned on.__


The eﬀect of the inline keyword is that the function symbol in question is not emitted into the object ﬁle.
Otherwise an error would occur in the last line, where we are linking the object ﬁles to form the ﬁnal executable. If
we would not have inline, the same symbol would be deﬁned in both .o ﬁles, and a "multiply deﬁned symbol"
error would occur.

不加-O2會導致編譯錯誤 same symbol 會被重複定義的兩個.o

In situations where the symbol is actually needed, this has the disadvantage that the symbol is not produced at all.
There are two possibilities to deal with that. The ﬁrst is to add an extra extern declaration of the inlined functions
in exactly one of the .c ﬁles. So add the following to source1.c:

```
extern int timestwo(int input);
extern int plusfive(int input);
```

The other possibility is to deﬁne the function with static inline instead of inline. This method has the drawback
that eventually a copy of the function in question may be produced in every object ﬁle that is produced with this
header.
另一個辦法是static inline。但最終可能會導致每個頭文件副本都有一份函數。


# Threads (native)
In most cases all data that is accessed by several threads should be initialized before the threads are created. This
ensures that all threads start with a clear state and no race condition occurs.
If this is not possible once_flag and call_once can be used
```
#include <threads.h>
#include <stdlib.h>
// the user data for this example
double const* Big = 0;
// the flag to protect big, must be global and/or static
static once_flag onceBig = ONCE_INIT;
void destroyBig(void) {
   free((void*)Big);
}
void initBig(void) {
    // assign to temporary with no const qualification
    double* b = malloc(largeNum);
    if (!b) {
       perror("allocation failed for Big");
       exit(EXIT_FAILURE);
    }
    // now initialize and store Big
    initializeBigWithSophisticatedValues(largeNum, b);
    Big = b;
    // ensure that the space is freed on exit or quick_exit
    atexit(destroyBig);
    at_quick_exit(destroyBig);
}
// the user thread function that relies on Big
int myThreadFunc(void* a) {
   call_once(&onceBig, initBig);
   // only use Big from here on
   ...
   return 0;
}
```
The once_flag is used to coordinate diﬀerent threads that might want to initialize the same data Big. The call to
call_once guarantees that

* initBig is called exactly once
* call_once blocks until such a call to initBig has been made, either by the same or another thread.

Besides allocation, a typical thing to do in such a once-called function is a dynamic initialization of a thread control
data structures such as mtx_t or cnd_t that can't be initialized statically, using mtx_init or cnd_init, respectively.

## Start several threads
```
#include <stdio.h>
#include <threads.h>
#include <stdlib.h>
struct my_thread_data {
   double factor;
};
int my_thread_func(void* a) {
   struct my_thread_data* d = a;
   // do something with d
   printf("we found %g\n", d->factor);
   // return an success or error code
   return d->factor > 1.0;
}

int main(int argc, char* argv[argc+1]) {
    unsigned n = 4;
    if (argc > 1) n = strtoull(argv[1], 0, 0);
    // reserve space for the arguments for the threads
    struct my_thread_data D[n];     // can't be initialized
    for (unsigned i = 0; i < n; ++i) {
         D[i] = (struct my_thread_data){ .factor = 0.5*i, };
    }
    // reserve space for the ID's of the threads
    thrd_t id[4];
    // launch the threads
    for (unsigned i = 0; i < n; ++i) {
         thrd_create(&id[i], my_thread_func, &D[i]);
    }
    // Wait that all threads have finished, but throw away their
    // return values
    for (unsigned i = 0; i < n; ++i) {
         thrd_join(id[i], 0);
    }
    return EXIT_SUCCESS;
}
```

# Multithreading
In C11 there is a standard thread library, <threads.h>, but no known compiler that yet implements it. Thus, to use
multithreading in C you must use platform speciﬁc implementations such as the POSIX threads library (often
referred to as pthreads) using the pthread.h header.
```
#include <threads.h>
#include <stdio.h>
int run(void *arg)
{
    printf("Hello world of C11 threads.");
    return 0;
}
int main(int argc, const char *argv[])
{
    thrd_t thread;
    int result;
    thrd_create(&thread, run, NULL);
    thrd_join(&thread, &result);
    printf("Thread return %d at the end\n", result);
}
```

Note: ```pthread``` is an option.

# Interprocess Communication (IPC)
Inter-process communication (IPC) mechanisms allow diﬀerent independent processes to communicate with each
other.

all such mechanisms are deﬁned by the host
operating system. POSIX deﬁnes an extensive set of IPC mechanisms; Windows deﬁnes another set; and other
systems deﬁne their own variants.
根據各自作業系統定義IPC，標準中未定下。

## Semaphores

Semaphores are used to synchronize operations between two or more processes. POSIX deﬁnes two diﬀerent sets
of semaphore functions:

1. 'System V IPC' — semctl(), semop(), semget().
2. 'POSIX Semaphores' — sem_close(), sem_destroy(), sem_getvalue(), sem_init(), sem_open(), sem_post(),
sem_trywait(), sem_unlink().

## Example : Avoid Racing with Semaphores
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/types.h>
#include <sys/ipc.h>
#include <sys/sem.h>
#define KEY 0x1111
union semun {
    int val;
    struct semid_ds *buf;
    unsigned short  *array;
};
struct sembuf p = { 0, -1, SEM_UNDO};
struct sembuf v = { 0, +1, SEM_UNDO};
int main()
{
    int id = semget(KEY, 1, 0666 | IPC_CREAT);
    if(id < 0)
    {
        perror("semget"); exit(11);
    }
    union semun u;
    u.val = 1;
    if(semctl(id, 0, SETVAL, u) < 0)
    {
         perror("semctl"); exit(12);
    }
    int pid;
    pid =  fork();
    srand(pid);
    if(pid < 0)
    {
        perror("fork"); exit(1);
    }
    else if(pid)
    {
        char *s = "abcdefgh";
        int l = strlen(s);
        for(int i = 0; i < l; ++i)
        {
            if(semop(id, &p, 1) < 0)
            {
                perror("semop p"); exit(13);
            }
            putchar(s[i]);
            fflush(stdout);
            sleep(rand() % 2);
            putchar(s[i]);
            fflush(stdout);
            if(semop(id, &v, 1) < 0)
            {
                perror("semop p"); exit(14);
            }
            sleep(rand() % 2);
        }
    }
    else
    {
        char *s = "ABCDEFGH";
        int l = strlen(s);
        for(int i = 0; i < l; ++i)
        {
            if(semop(id, &p, 1) < 0)
            {
                perror("semop p"); exit(15);
            }
            putchar(s[i]);
            fflush(stdout);
            sleep(rand() % 2);
            putchar(s[i]);
            fflush(stdout);
            if(semop(id, &v, 1) < 0)
            {
                perror("semop p"); exit(16);
            }
            sleep(rand() % 2);
        }
    }
}
```

# Testing frameworks

CMocka

CMocka is an elegant unit testing framework for C with support for mock objects. It only requires the standard C
library, works on a range of computing platforms (including embedded) and with diﬀerent compilers. It has a
tutorial on testing with mocks, API documentation, and a variety of examples.

```
#include <stdarg.h>
#include <stddef.h>
#include <setjmp.h>
#include <cmocka.h>
void null_test_success (void ** state) {}
void null_test_fail (void ** state)
{
    assert_true (0);
}
/* These functions will be used to initialize
   and clean resources up after each test run */
int setup (void ** state)
{
    return 0;
}
int teardown (void ** state)
{
    return 0;
}

int main (void)
{
    const struct CMUnitTest tests [] =
    {
        cmocka_unit_test (null_test_success),
        cmocka_unit_test (null_test_fail),
    };
    /* If setup and teardown functions are not
       needed, then NULL may be passed instead */
    int count_fail_tests =
        cmocka_run_group_tests (tests, setup, teardown);
    return count_fail_tests;
}
```

CppUTest

CppUTest is an xUnit-style framework for unit testing C and C++. It is written in C++ and aims for portability and
simplicity in design. It has support for memory leak detection, building mocks, and running its tests along with the
Google Test. Comes with helper scripts and sample projects for Visual Studio and Eclipse CDT..


```
#include <CppUTest/CommandLineTestRunner.h>
#include <CppUTest/TestHarness.h>

TEST_GROUP(Foo_Group) {}
TEST(Foo_Group, Foo_TestOne) {}
/* Test runner may be provided options, such
   as to enable colored output, to run only a
   specific test or a group of tests, etc. This
   will return the number of failed tests. */
int main(int argc, char ** argv)
{
    RUN_ALL_TESTS(argc, argv);
}
```

A test group may have a setup() and a teardown() method. The setup method is called prior to each test and the teardown() method is called after. Both are optional and either may be omitted independently. Other methods and
variables may also be declared inside a group and will be available to all tests of that group.
```
TEST_GROUP(Foo_Group)
{
    size_t data_bytes = 128;
    void * data;
    void setup()
    {
        data = malloc(data_bytes);
    }
    void teardown()
    {
        free(data);
    }
    void clear()
    {
        memset(data, 0, data_bytes);
    }
}
```

## memory leak test tool
* Valgrind
```
#include <stdio.h>
#include <stdlib.h>
int main(int argc, char **argv)
{
    char *s;
    s = malloc(26); // the culprint
    return 0;
}
```
With no extra arguments, valgrind will not look for this error.
But if we turn on --leak-check=yes or --tool=memcheck, it will complain and display the lines responsible for those
memory leaks if the program was compiled in debug mode:
```
$ valgrind -q --leak-check=yes ./missing_free
==4776== 26 bytes in 1 blocks are definitely lost in loss record 1 of 1
==4776==    at 0x4024F20: malloc (vg_replace_malloc.c:236)
==4776==    by 0x80483F8: main (missing_free.c:9)
==4776==
```



#  Common C programming idioms and developer practices
```
if ( i  == 2) //Bad-way
{
    doSomething;
}
```
```
if( 2 == i) //Good-way
{
    doSomething;
}
```
The equal sign is accidentally left out, the compiler will complain about an “attempted assignment to literal.”


## Do not leave the parameter list of a function blank — use void

```
int foo(void);

// notlike: int foo();
```
```
int foo(void)
{
    ...
    <statements>
    ...
    return 1;
}
```

Note that even though a function deﬁned with an empty parameter list takes no arguments, it does not provide a
prototype for the function, so the compiler will not complain if the function is subsequently called with arguments.
For example:
沒有宣告void在參數。調用時給參數，編譯器可能會不會報錯。

__ERROR one__:
```
#include <stdio.h>
static void parameterless()
{
    printf("%s called\n", __func__);
}
int main(void)
{
    parameterless(3, "arguments", "provided");
    return 0;
}
```
無報錯:
```
$ gcc -O3 -g -std=c11 -Wall -Wextra -Werror -Wmissing-prototypes -pedantic proto79.c -o proto79
$
```

If you compile with more stringent options, you get errors:
更嚴格編譯參數
```
$ gcc -O3 -g -std=c11 -Wall -Wextra -Werror -Wmissing-prototypes -Wstrict-prototypes -Wold-style-
definition -pedantic proto79.c -o proto79
proto79.c:3:13: error: function declaration isn’t a prototype [-Werror=strict-prototypes]
 static void parameterless()
             ^~~~~~~~~~~~~
proto79.c: In function ‘parameterless’:
proto79.c:3:13: error: old-style function definition [-Werror=old-style-definition]
cc1: all warnings being treated as errors
$
```

## Forgetting to copy the return value of realloc into a temporary
```
char *buf, *tmp;
buf = malloc(...);
...
/* WRONG */
if ((buf = realloc(buf, 16)) == NULL)
    perror("realloc");
/* RIGHT */
if ((tmp = realloc(buf, 16)) != NULL)
    buf = tmp;
else
    perror("realloc");
```

## Forgetting to allocate one extra byte for \0
```
char *dest = malloc(strlen(src)); /* WRONG */
char *dest = malloc(strlen(src) + 1); /* RIGHT */
strcpy(dest, src);
```
This is because strlen does not include the trailing \0 in the length. If you take the WRONG (as shown above)
approach, upon calling strcpy, your program would invoke undeﬁned behaviour.
```
#define MAX_INPUT_LEN 42
char buffer[MAX_INPUT_LEN]; /* WRONG */
char buffer[MAX_INPUT_LEN + 1]; /* RIGHT */
scanf("%42s", buffer);  /* Ensure that the buffer is not overflowed */
```

## Misunderstanding array decay
Type** is not Type[M][N]
```
#include <stdio.h>
void print_strings(char **strings, size_t n)
{
    size_t i;
    for (i = 0; i < n; i++)
        puts(strings[i]);
}
int main(void)
{
    char s[4][20] = {"Example 1", "Example 2", "Example 3", "Example 4"};
    print_strings(s, 4);
    return 0;
}
```
```
file1.c: In function 'main':
file1.c:13:23: error: passing argument 1 of 'print_strings' from incompatible pointer type [-
Wincompatible-pointer-types]
         print_strings(strings, 4);
                       ^
file1.c:3:10: note: expected 'char **' but argument is of type 'char (*)[20]'
      void print_strings(char **strings, size_t n)
```

|Before Decay|After Decay|
|:-------|:-------|
|char [20] array of (20 chars)|char *  pointer to (1 char)|
|char [4][20] array of (4 arrays of 20 chars)|  char (*)[20] pointer to (1 array of 20 chars)
|char *[4] array of (4 pointers to 1 char) | char ** pointer to (1 pointer to 1 char)
|char [3][4][20] array of (3 arrays of 4 arrays of 20 chars) | char (*)[4][20] pointer to (1 array of 4 arrays of 20 chars)
|char (*[4])[20] array of (4 pointers to 1 array of 20 chars) | char (**)[20] pointer to (1 pointer to 1 array of 20 chars)

In other words, a pointer to an array (char (*)[20]) will never become a pointer to a pointer (char **). To ﬁx the
print_strings function, simply make it receive the correct type:
```
void print_strings(char (*strings)[20], size_t n)
/* OR */
void print_strings(char strings[][20], size_t n)
```

A problem arises when you want the print_strings function to be generic for any array of chars: what if there are
30 chars instead of 20? Or 50? The answer is to add another parameter before the array parameter:
```
#include <stdio.h>
/*
 * Note the rearranged parameters and the change in the parameter name
 * from the previous definitions:
 *      n (number of strings)
 *   => scount (string count)
 *
 * Of course, you could also use one of the following highly recommended forms
 * for the `strings` parameter instead:
 *
 *    char strings[scount][ccount]
 *    char strings[][ccount]
 */
void print_strings(size_t scount, size_t ccount, char (*strings)[ccount])
{
    size_t i;
    for (i = 0; i < scount; i++)
    puts(strings[i]);
}
int main(void)
{
    char s[4][20] = {"Example 1", "Example 2", "Example 3", "Example 4"};
    print_strings(4, 20, s);
    return 0;
}
```
output
```
Example 1
Example 2
Example 3
Example 4
```

## Forgetting to free memory (memory leaks)
* When programming, always free memory that you allocate directly or indirectly using functions like ```strdup()```.
* Failing to free memory can cause a memory leak, which can eventually lead to program crashes or undefined behavior.
* Memory leaks are especially problematic when they occur in a loop or recursive function.
* The longer a leaking program runs, the higher the risk of program failure due to memory exhaustion.

An example of a memory leak is an infinite loop that allocates memory using a function like getline() without freeing it.
```
#include <stdlib.h>
#include <stdio.h>
int main(void)
{
    char *line = NULL;
    size_t size = 0;
    /* The loop below leaks memory as fast as it can */
    for(;;) {
        getline(&line, &size, stdin); /* New memory implicitly allocated */
        /* <do whatever> */
        line = NULL;
    }
    return 0;
}
```
n contrast, the code below also uses the getline() function, but this time, the allocated memory is correctly freed,
avoiding a leak.
```
int main(void)
{
    char *line = NULL;
    size_t size = 0;
    for(;;) {
        if (getline(&line, &size, stdin) < 0) {
            free(line);
            line = NULL;
            /* Handle failure such as setting flag, breaking out of loop and/or exiting */
        }
        /* <do whatever> */
        free(line);
        line = NULL;
    }
    return 0;
}
```
* It's generally a good practice to free memory at strategic points to reduce memory usage and lower the risk of memory exhaustion.
* However, in some cases, it may be acceptable to bypass explicit deallocation if the program is bounded in duration and scope, and the risk of allocation failure is low.
* Allocation can fail if there's insufficient memory available, and appropriate error handling should be implemented.
* When using C API functions, it's important to read the documentation and pay attention to error conditions and memory usage.
* Setting memory pointers to NULL after freeing the memory can prevent accessing freed memory, which can cause problems like getting garbage data or program crashes.
* Freeing memory location 0 (NULL) is harmless, and double-freeing memory should be avoided, as it can lead to time-consuming and difficult-to-diagnose failures.


## Recursive function — missing out the base condition
```
#include <stdio.h>
int factorial(int n)
{
       return n * factorial(n - 1);
}
int main()
{
    printf("Factorial %d = %d\n", 3, factorial(3));
    return 0;
}
```
Typical output: Segmentation fault: 11

```
#include <stdio.h>
int factorial(int n)
{
    if (n == 1) // Base Condition, very crucial in designing the recursive functions.
    {
       return 1;
    }
    else
    {
       return n * factorial(n - 1);
    }
}
```
Rules to be followed:
1. Initialize the algorithm. Recursive programs often need a seed value to start with. This is accomplished either
by using a parameter passed to the function or by providing a gateway function that is non-recursive but that
sets up the seed values for the recursive calculation.
2. Check to see whether the current value(s) being processed match the base case. If so, process and return the
value.
3. Redeﬁne the answer in terms of a smaller or simpler sub-problem or sub-problems.
4. Run the algorithm on the sub-problem.
5. Combine the results in the formulation of the answer.
6. Return the results.






