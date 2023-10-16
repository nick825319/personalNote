# C++
|ver|date|
|:--|:--|
|C++98 ISO/IEC 14882|1998 1998-09-01|
|C++03 ISO/IEC 14882|2003 2003-10-16|
|C++11 ISO/IEC 14882|2011 2011-09-01|
|C++14 ISO/IEC 14882|2014 2014-12-15|
|C++17 TBD | 2017-01-01|
|C++20 TBD | 2020-01-01|


# #2 Literals
## this
Within a member function of a class, the keyword this is a pointer to the instance of the class on which the
function was called. this cannot be used in a static member function.

this 不能用於靜態(static) member. 靜態物件沒有具體的instant(particular object). this 用於呼叫具體物件的pointer.而static class是呼叫class本身.
```
struct S {
    int x;
    S& operator=(const S& other) {
        x = other.x;
        // return a reference to the object being assigned to
        return *this;
    }
};
```
By overloading the assignment operator, you have the flexibility to customize the assignment behavior based on your specific requirements. You can perform additional operations, handle resource management, or implement a different assignment logic according to your needs. Overloading the assignment operator allows you to define how objects of the structure type are assigned to each other.
\
The above example have not special assignment operator behavior same as default.


The type of this depends on the cv-qualiﬁcation of the member function: if X::f is const, then the type of this
within f is const X*, so this cannot be used to modify non-static data members from within a const member
function. Likewise, this inherits volatile qualiﬁcation from the function it appears in.

note: this 回傳值根據class(struct)所宣告的型態

---
this can also be used in a brace-or-equal-initializer for a non-static data member.
```
struct S;
struct T {
    T(const S* s);
    // ...
};
struct S {
    // ...
    T t{this};
};
```
"brace-or-equal-initializer" 使用"brace"或"equal"的初始化成員變數方式。
```
class MyClass {
public:
    int x = 0;
    int y{42};
};
```
* x is initialized using the equal sign = 
* y is initialized using the brace-initializer syntax {}

## Integer literal
* decimal-literal ```int d = 42;```
* octal-literal ```int o = 052;```
* hex-literal ```int x = 0x2a; int X = 0X2A;```
* binary-literal (since C++14) ```int b = 0b101010;```
* unsigned-suﬃx ```unsigned int u_1 = 42u;``` (the character u or the character U)
---
* long-suﬃx (the character l or the character L) or the long-long-suﬃx (the character sequence ll or the
character sequence LL) (since C++11)

The following variables are also initialized to the same value:
```
unsigned long long l1 = 18446744073709550592ull; // C++11
unsigned long long l2 = 18'446'744'073'709'550'592llu; // C++14
unsigned long long l3 = 1844'6744'0737'0955'0592uLL; // C++14
unsigned long long l4 = 184467'440737'0'95505'92LLU; // C++14
```

## nullptr
A keyword denoting a null pointer constant. It can be converted to any pointer or pointer-to-member type, yielding
a null pointer of the resulting type.
```
Widget* p = new Widget();
delete p;
p = nullptr; // set the pointer to null after deletion
```
Note that nullptr is not itself a pointer. The type of nullptr is a fundamental type known as std::nullptr_t.
```
void f(int* p);
template <class T>
void g(T* p);
void h(std::nullptr_t p);
int main() {
    f(nullptr); // ok
    g(nullptr); // error
    h(nullptr); // ok
}
```

# #3 Bit Manipulation
C-style bit-manipulation
```
x = -1; // -1 == 1111 1111 ... 1111b
```
Using std::bitset
```
std::bitset<10> x;
x.set(); // Sets all bits to '1'
```
C-style

A bit can be toggled using the XOR operator (^).
```
// Bit x in "number" will be the opposite value of what it is currently
number ^= 1LL << x;
```
Using std::bitset
```
std::bitset<4> num(std::string("0100"));
num.flip(2); // num is now 0000
num.flip(0); // num is now 0001
num.flip();  // num is now 1110 (flips all bits)
```
### Checking a bit

Using std::bitset
```
std::bitset<4> num(std::string("0010"));
bool bit_val = num.test(1);  // bit_val value is set to true;
```

### Using std::bitset
set(x) or set(x,true) - sets bit at position x to 1.
```
std::bitset<5> num(std::string("01100"));
num.set(0);      // num is now 01101
num.set(2);      // num is still 01101
num.set(4,true); // num is now 11110
```
```
std::bitset<5> num(std::string("00100"));
num.set(0,true);  // num is now 00101
num.set(2,false); // num is now 00001
```

Clearing a bit
```
std::bitset<5> num(std::string("01100"));
num.reset(2);     // num is now 01000
num.reset(0);     // num is still 01000
num.set(3,false); // num is now 00000
```
# Value Categories
* identity :  it has a variable name and represents a specific memory location. something like '5+2' expression that is temporary or intermediate values without identity.
C++ deﬁnes 3 value categories:  
* lvalue (expressions with identity but not movable from)
* xvalue (expressions with identity that are moveable from)
* prvalue (expressions without identity that are moveable from)
---
* std::string&& is an rvalue reference
* std::string& is an lvalue reference 
---
```
 int a = 42; // 'a' is an lvalue
 int b = 10; // '10' is a prvalue
 int&& d = 3 * a; // '3 * a' is an xvalue
```
* glvalue (expressions with identity)
* rvalue (expressions that can be moved from)
```
int a = 42;  // 'a' is an object with identity

// Expressions with identity:
int& ref = a;  // 'ref' refers to the object 'a'
int* ptr = &a; // 'ptr' points to the memory location of 'a'

// Expressions without identity:
int result = 2 + 3;
// '2 + 3' is a temporary value without identity
int value = a * (result + 1); 
// '(result + 1)' is an intermediate value without identity
```
```
       expression
        /      \
     glvalue rvalue
    /    \   /    \
lvalue   xvalue  prvalue
```
##  rvalue
A rvalue (right value)  
rvalue expression 是指任何可以(implicitly moved)的expression(表達式)，無論其是否有identity。
更確切的說，ravlue expression 可以作為一個function的parameter。(這個function接收的parameter type是`T &&` rvalue reference(用來接收rvalue類型))。
此時只有 rvalue expression可以被當作此function的parameter。
如果使用non-rvalue expression。overload resolution(function overload 程序)就會選擇其他function parameter 不使用rvalue reference的項目。如果沒有就會報錯。
* rvalue expressions includes: xvalue and prvalue expressions.

std::move 用於將non-rvalue expression轉換成rvalue (explicitly transform)。
他將expression轉換成xvalue。即使expression是沒有identity的prvalue。通過將prvalue傳進std::move，prvalue就可以獲得identity並成為xvalue。
```
std::string str("init");                       //1
std::string test1(str);                        //2
std::string test2(std::move(str));             //3

str = std::string("new value");                //4
std::string &&str_ref = std::move(str);        //5
std::string test3(str_ref);                    //6
```
Line 1: std::string對於一邊的數值rvalue會使用其內的move
constructor，搬移temporary or intermediate values形成string object。
但此處是lvalue(`test1(str)`)，所以constructor進行overload。作為替換進行const std::string& overload (copy constructor)。

Line 3: std::move 的return value type是`T&&`。當rvalue reference是作為某function的return type時，回傳會變成rvalue expression (speciﬁcally an xvalue)。std::string此時會使用move constructor

Line 4: temporary rvalue. Not call overload. create std::string("new value");  
"str =", here is the str call the overload on "assignment operator overload" (std::string::operator=(std::string&&)). This allows for efficient move assignment, where the resources of the temporary object can be transferred to the target object without unnecessary copying.

Line 5 creates a named rvalue reference called str_ref that refers to str. This is where value categories get confusing.

## xvalue
An xvalue (eXpiring value)  
It has identity and represents an object which can be implicitly moved from.

Object they represent is going to be destroyed soon (hence the "eXpiring" part), and therefore implicitly moving from them is ﬁne.
```
struct X { int n; };
extern X x;
4;                   // prvalue: does not have an identity
x;                   // lvalue
x.n;                 // lvalue
std::move(x);        // xvalue
std::forward<X&>(x); // lvalue
X{4};                // prvalue: does not have an identity
X{4}.n;              // xvalue: does have an identity and denotes resources
                     // that can be reused
```

## prvalue
A prvalue (pure-rvalue)  
This lacks identity, whose evaluation is typically used to
initialize an object, and which can be implicitly moved from. These include, but are not limited to:
* Expressions that represent temporary objects, such as std::string("123").
```
std::string str = std::string("Hello");  // Temporary object
int result = 10 + 5;  // Temporary object
```
* A function call expression that does not return a reference
```
std::string createString() {
    return "Hello, World!";  // Function returning a prvalue string
}
int main() {
    std::string message = createString();
    std::cout << message << std::endl;  // Output: Hello, World!
    return 0;
}
```

* A literal (except a string literal - those are lvalues), such has 1, true, 0.5f, or 'a'
```
int number = 42;  // prvalue literal
bool condition = true;  // prvalue literal
float floatValue = 3.14f;  // prvalue literal
char character = 'a';  // prvalue literal
```
* A lambda expression
```
auto sum = [](int a, int b) { return a + b; };  // prvalue lambda expression
int result = sum(5, 10);  // Invoking the lambda
```

## lvalue
An lvalue expression is an expression which has identity, but cannot be implicitly moved from.  
Consist of a `variable name`, `function name`, expressions that are built-in dereference operator uses (`int*`) and expressions that refer to lvalue references (`int&`).
```
struct X { ... };
X x;         // x is an lvalue
X* px = &x;  // px is an lvalue
*px = X{};   // *px is also an lvalue, X{} is a prvalue
X* foo_ptr();  // foo_ptr() is a prvalue
X& foo_ref();  // foo_ref() is an lvalue
```

## glvalue
A glvalue (a "generalized lvalue").  
It is any expression which has identity, regardless of whether it can be
moved from or not.
Includes lvalues and xvalues.

If an expression has a name, it's a glvalue:
```
struct X { int n; };
X foo();
X x;
x; // has a name, so it's a glvalue
std::move(x); // has a name (we're moving from "x"), so it's a glvalue
              // can be moved from, so it's an xvalue not an lvalue
foo(); // has no name, so is a prvalue, not a glvalue
X{};   // temporary has no name, so is a prvalue, not a glvalue
X{}.n; // HAS a name, so is a glvalue. can be moved from, so it's an xvalue
```

# Array

## Fxed size raw array matrix

```
// A fixed size raw array matrix (that is, a 2D raw array).
#include <iostream>
#include <iomanip>
using namespace std;
auto main() -> int
{
    int const   n_rows  = 3;
    int const   n_cols  = 7;
    int const   m[n_rows][n_cols] =             // A raw array matrix.
    {
        {  1,  2,  3,  4,  5,  6,  7 },
        {  8,  9, 10, 11, 12, 13, 14 },
        { 15, 16, 17, 18, 19, 20, 21 }
    };
   
    for( int y = 0; y < n_rows; ++y )
    {
        for( int x = 0; x < n_cols; ++x )
        {
            cout << setw( 4 ) << m[y][x];       // Note: do NOT use m[y,x]!
        }
        cout << '\n';
    }
}
```
```
auto

In C++, the auto keyword is used for type inference, allowing the compiler to automatically deduce the type of a variable based on its initializer. It was introduced in C++11 as part of the language's ongoing support for type safety and improved code readability.


In C, the auto keyword has a different meaning. In C, auto is a storage class specifier that is used to define local variables with automatic storage duration. However, in modern C, the auto keyword is optional and is rarely used since local variables are automatic by default.
```


## Dynamically sized raw array


Example of raw dynamic size array. It's generally better to use std::vector.
```
// Example of raw dynamic size array. It's generally better to use std::vector.
#include <algorithm>            // std::sort
#include <iostream>
using namespace std;
auto int_from( istream& in ) -> int { int x; in >> x; return x; }
auto main()
    -> int
{
    cout << "Sorting n integers provided by you.\\n";
    cout << "n? ";
    int const   n   = int_from( cin );
    int*        a   = new int[n];       // ← Allocation of array of n items.
   
    for( int i = 1; i <= n; ++i )
    {
        cout << "The #" << i << " number, please: ";
        a[i-1] = int_from( cin );
    }
    sort( a, a + n );
    for( int i = 0; i < n; ++i ) { cout << a[i] << ' '; }
    cout << '\\n';
   
    delete[] a;
}
```

## array size for c++ 
For C++11, 
using C++11 you can do:
```
std::extent<decltype(MyArray)>::value;
```
```
char MyArray[] = { 'X','o','c','e' };
const auto n = std::extent<decltype(MyArray)>::value;
std::cout << n << "\n"; // Prints 4
```
Up till C++17 (forthcoming as of this writing) C++ had no built-in core language or standard library utility to obtain
the size of an array, but this can be implemented by passing the array by reference to a function template, as shown
above. Fine but important point: the template size parameter is a size_t, somewhat inconsistent with the signed
Size function result type, in order to accommodate the g++ compiler which sometimes insists on size_t for
template matching.
With C++17 and later one may instead use ```std::size```, which is specialized for arrays.

## Expanding dynamic size array by using std::vector

```
// Example of std::vector as an expanding dynamic size array.
#include <algorithm>            // std::sort
#include <iostream>
#include <vector>               // std::vector
using namespace std;
int int_from( std::istream& in ) { int x = 0; in >> x; return x; }
int main()
{
    cout << "Sorting integers provided by you.\n";
    cout << "You can indicate EOF via F6 in Windows or Ctrl+D in Unix-land.\n";
    vector<int> a;      // ← Zero size by default.
    while( cin )
    {
        cout << "One number, please, or indicate EOF: ";
        int const x = int_from( cin ); // Pass cin as a reference
        if( !cin.fail() ) { a.push_back( x ); }  // Expands as necessary.
    }
    sort( a.begin(), a.end() );
    int const n = a.size();
    for( int i = 0; i < n; ++i ) { cout << a[i] << ' '; }
    cout << '\n';
}
```

## A dynamic size matrix using std::vector
Unfortunately as of C++14 there's no dynamic size matrix class in the C++ standard library.

If you don't want a dependency on Boost or some other library, then one poor man's dynamic size matrix in C++ is
just like
```
vector<vector<int>> m( 3, vector<int>( 7 ) );
```
```
// A dynamic size matrix using std::vector for storage.
//--------------------------------------------- Machinery:
#include <algorithm> // std::copy
#include <cassert>   // assert
#include <initializer_list>// std::initializer_list
#include <vector>          // std::vector
#include <cstddef>         // ptrdiff_t
namespace my
{
    using Size = ptrdiff_t;
    using std::initializer_list;
    using std::vector;
    template <class Item>
    class Matrix
    {
    private:
        vector items_;
        Size n_cols_;
        auto index_for(Size const x, Size const y) const
            -> Size
        {
            return y * n_cols_ + x;
        }

    public:
        auto n_rows() const -> Size { return items_.size() / n_cols_; }
        auto n_cols() const -> Size { return n_cols_; }
        auto item(Size const x, Size const y)
            -> Item &
        {
            return items_[index_for(x, y)];
        }
        auto item(Size const x, Size const y) const
            -> Item const &
        {
            return items_[index_for(x, y)];
        }
        Matrix() : n_cols_(0) {}
        Matrix(Size const n_cols, Size const n_rows)
            : items_(n_cols * n_rows), n_cols_(n_cols)
        {
        }
        Matrix(initializer_list<initializer_list> const &values)
            : items_(), n_cols_(values.size() == 0 ? 0 : values.begin()->size())
        {
            for (auto const &row : values)
            {
                assert(Size(row.size()) == n_cols_);
                items_.insert(items_.end(), row.begin(), row.end());
            }
        }
    };
} // namespace my
//--------------------------------------------- Usage:
using my::Matrix;
auto some_matrix()
    -> Matrix
{
    return {
        {1, 2, 3, 4, 5, 6, 7},
        {8, 9, 10, 11, 12, 13, 14},
        {15, 16, 17, 18, 19, 20, 21}};
}
#include
#include
using namespace std;
auto main() -> int
{
    Matrix const m = some_matrix();
    assert(m.n_cols() == 7);
    assert(m.n_rows() == 3);
    for (int y = 0, y_end = m.n_rows(); y < y_end; ++y)
    {
        for (int x = 0, x_end = m.n_cols(); x < x_end; ++x)
        {
            cout <← Note : not `m[y][x]`!
        }
        cout <
    }
}
```

# Basic input/output in c++
```
#include <iostream>
int main()
{
    int value;
    std::cout << "Enter a value: " << std::endl;
    std::cin >> value;
    std::cout << "The square of entered value is: " << value * value << std::endl;
    return 0;
}
```


# Iterators

Iterators are a means of navigating and operating on a sequence of elements and are a generalized extension of
pointers. Conceptually it is important to remember that iterators are positions, not elements.
```
+---+---+---+---+
| A | B | C |   |
+---+---+---+---+
```
To convert from a position to a value, an iterator is dereferenced:
```
auto my_iterator = my_vector.begin(); // position
auto my_value = *my_iterator; // value
```
One can think of an iterator as dereferencing to the value it refers to in the sequence. This is especially useful in
understanding why you should never dereference the end() iterator in a sequence:
```
+---+---+---+---+
| A | B | C |   |
+---+---+---+---+
↑           ↑
|           +-- An iterator here has no value. Do not dereference it!
+-------------- An iterator here dereferences to the value A.
```

The C++ standard describes several diﬀerent iterator concepts. These are grouped according to how they behave in
the sequences they refer to. If you know the concept an iterator models (behaves like), you can be assured of the
behavior of that iterator regardless of the sequence to which it belongs.

1. Input Iterators : Can be dereferenced only once per position. Can only advance, and only one position at a
time.
1. Forward Iterators : An input iterator that can be dereferenced any number of times.
1. Bidirectional Iterators : A forward iterator that can also advance backwards one position at a time.
1. Random Access Iterators : A bidirectional iterator that can advance forwards or backwards any number of
positions at a time.
1. Contiguous Iterators (since C++17) : A random access iterator that guaranties that underlying data is
contiguous in memory.


## Explain why use iterator

The std::vector<int>::iterator type is a nested type defined within the std::vector<int> class. It is implemented as a separate class specifically designed to work with the std::vector container.

You can think of the iterator as an abstraction that encapsulates a position within the vector. It provides methods and operators to navigate through the elements, such as operator++ to move to the next element, operator* to dereference and access the current element, and so on.

Generalization: The iterator concept is not limited to std::vector. It is a fundamental concept in C++ that is shared across various standard containers, such as std::list, std::map, and std::set. By using iterators, you can write code that works with different container types without having to change the implementation.

This concept of generalization through iterators allows you to write code that is more generic, reusable, and flexible, as it can work with different container types without modification.

In the context of iterators, generalization refers to the concept that iterators are not specific to a particular container type but can be used across different types of containers in C++. It allows you to write code that is independent of the specific container being used and promotes code reuse and modularity.

C++ provides a standard set of container classes, such as std::vector, std::list, std::map, std::set, and more. Each container class has its own implementation and characteristics, but they all share a common interface through the use of iterators. This allows you to write generic algorithms that operate on containers without being tightly coupled to a specific container type.

For example, you can write a function that sorts the elements of any container using iterators. Here's a simplified example:

```
template <typename Container>
void sortContainer(Container& container) {
    // Obtain iterators for the container
    typename Container::iterator begin = container.begin();
    typename Container::iterator end = container.end();

    // Sort the container using the iterators
    std::sort(begin, end);
}
```
In the above code, the sortContainer function is templated on the container type Container. It obtains iterators using the begin() and end() member functions of the container and then uses those iterators to sort the elements using std::sort. This function can be used with any container that provides the necessary iterator interface, regardless of whether it's a std::vector, std::list, or any other container that meets the requirements.

Containers (std::vector, std::list, std::map/std::unordered_map, std::set)

1. std::vector: A dynamic array that allows efficient random access and resizing.
1. std::list: A doubly-linked list that allows efficient insertion and removal of elements.
1. std::map/std::unordered_map: Associative containers that store key-value pairs, with std::map providing ordered access and std::unordered_map providing fast lookup based on hashed keys.
1. std::set/std::unordered_set: Associative containers that store unique elements, with std::set maintaining ordered elements and std::unordered_set offering fast element lookup based on hashing.

By utilizing iterators and generic algorithms, you can write code that works with different container types.

### Vector Iterator
begin returns an iterator to the ﬁrst element in the sequence container.end returns an iterator to the ﬁrst element past the end.

If the vector object is const, both begin and end return a const_iterator. If you want a const_iterator to be
returned even if your vector is not const, you can use cbegin and cend.

```
#include <vector>
#include <iostream>
int main() {
    std::vector<int> v = { 1, 2, 3, 4, 5 };  //intialize vector using an initializer_list
    for (std::vector<int>::iterator it = v.begin(); it != v.end(); ++it) {
        std::cout << *it << " ";
    }
    return 0;
}
```

### Map Iterator
```
// Create a map and insert some values
std::map<char,int> mymap;
mymap['b'] = 100;
mymap['a'] = 200;
mymap['c'] = 300;
// Iterate over all tuples
for (std::map<char,int>::iterator it = mymap.begin(); it != mymap.end(); ++it)
    std::cout << it->first << " => " << it->second << '\n';
```
```
a => 200
b => 100
c => 300
```

## Reverse Iterators
If we want to iterate backwards through a list or vector we can use a reverse_iterator. A reverse iterator is made
from a bidirectional, or random access iterator which it keeps as a member which can be accessed through base().

```
std::vector<int> v{1, 2, 3, 4, 5};
for (std::vector<int>::reverse_iterator it = v.rbegin(); it != v.rend(); ++it)
{
    cout << *it;
} // prints 54321
```

A reverse iterator can be converted to a forward iterator via the base() member function. The relationship is that
the reverse iterator references one element past the base() iterator:

```
std::vector<int>::reverse_iterator r = v.rbegin();
std::vector<int>::iterator i = r.base();
assert(&*r == &*(i-1)); // always true if r, (i-1) are dereferenceable
                        // and are not proxy iterators
 +---+---+---+---+---+---+---+
 |   | 1 | 2 | 3 | 4 | 5 |   |
 +---+---+---+---+---+---+---+
   ↑   ↑               ↑   ↑
   |   |               |   |
rend() |         rbegin()  end()
       |                   rbegin().base()
     begin()
     rend().base()
```


對存在reverse_iterator (取自rbegin)的it 做 ++ 即可向後讀取vector內部值。若變回forward iterator。可以使用.base()，取得後一位的位址並將 "++" "--"的操作方向相反。
```
rbegin "++" 5 4 3 2 1
rbegin "--" 0 0 0 0 0 (or 未知的記憶體位置內值)

forward iterator
r.base() "++" 0 0 0 0 0 (or 未知的記憶體位置內值)
r.base() "--" 0 5 4 3 2 1
```
e.g.
```
#include <iostream>
#include <vector>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5};

    std::vector<int>::reverse_iterator r = numbers.rbegin();
    std::vector<int>::iterator i = r.base();
    // Print elements in reverse order using the reverse iterator
    while (i != numbers.begin()) {
        std::cout << *i << ' ';
        i++;  // Move the reverse iterator to the next element
    }
    std::cout << *i << ' ';
    std::cout << '\n';

    return 0;
}
```

## Stream Iterators
可以讀取串流狀的資料

Stream iterators are useful when we need to read a sequence or print formatted data from a container:

```
#include <iostream>
#include <vector>
#include <sstream>
#include <string>
#include <iterator>

int main() {
    std::string data = "42 3.14 Hello World";

    std::istringstream istr(data);
    std::vector<int> v;
    // Constructing stream iterators and copying data from stream into vector.
    std::copy(
        // Iterator which will read stream data as integers.
        std::istream_iterator<int>(istr),
        // Default constructor produces end-of-stream iterator.
        std::istream_iterator<int>(),
        std::back_inserter(v));
    // Print vector contents.
    std::copy(v.begin(), v.end(),
              // Will print values to standard output as integers delimeted by " -- ".
              std::ostream_iterator<int>(std::cout, " -- "));
    return 0;
}
```
```
42 -- 3 --
```
```
#include <iostream>
#include <vector>
#include <sstream>
#include <string>
#include <iterator>

int main() {
    std::string data = "1\t 2     3 4";

    std::istringstream istr(data);
    std::vector<int> v;
    std::copy(std::istream_iterator<int>(istr),
              std::istream_iterator<int>(),
              std::back_inserter(v));

    std::copy(v.begin(),
              v.end(),
              std::ostream_iterator<int>(std::cout, " -- "));
    return 0;
}

```
```
1 -- 2 -- 3 -- 4 --
```
### std::copy
```
std::copy(source_begin, source_end, destination_begin);
```

### std::back_inserter
It returns a special iterator called a "back insert iterator" that can be used to insert elements at the end of a container.

The std::back_inserter function takes a container as its argument and returns an iterator that, when assigned a value using the assignment operator (=) or used with the ++ operator, inserts the value at the end of the container.

```
#include <iostream>
#include <iterator>
#include <vector>
 
int main() {
    std::vector<int> numbers;
 
    // Use std::back_inserter to insert values at the end of the vector
    std::back_insert_iterator<std::vector<int>> inserter(numbers);
 
    // Insert values using the back insert iterator
    *inserter = 1;
    ++inserter;
    *inserter = 2;
    ++inserter;
    *inserter = 3;
 
    // Print the vector
    for (int num : numbers) {
        std::cout << num << ' ';
    }
    std::cout << '\n';
 
    return 0;
}
```
```
1 2 3
```
Using std::back_inserter provides a convenient way to dynamically add elements to a container without explicitly managing the resizing or reallocation of the container. It abstracts away the complexity of container resizing and provides a simple interface for appending elements at the end.

## Other way to appent container end
```
std::vector<int> numbers;
numbers.push_back(1);  // Appends element 1 to the end of the vector
numbers.push_back(2);  // Appends element 2 to the end of the vector
```
```
std::vector<int> numbers;
numbers.insert(numbers.end(), 1);  // Appends element 1 to the end of the vector
numbers.insert(numbers.end(), 2);  // Appends element 2 to the end of the vector
```
```
std::vector<int> source = {1, 2};
std::vector<int> destination;

destination.insert(destination.end(), source.begin(), source.end());
```


# Loops

## Range-Based For
說明for在c++下容器遞移的實現。
```
ector<float> v = {0.4f, 12.5f, 16.234f};
for(auto val: v)
{
    std::cout << val << " ";
}
std::cout << std::endl;

//for-range-declaration: auto val, for-range-initializer: v
```

This will iterate over every element in v, with val getting the value of the current element. The following statement:
```
for (for-range-declaration : for-range-initializer ) statement
```

is equivalent to:
```
{
    auto&& __range = for-range-initializer;
    auto __begin = begin-expr, __end = end-expr;
    for (; __begin != __end; ++__begin) {
        for-range-declaration = *__begin;
        statement
    }
}
```
Version ≥ C++17
```
{
    auto&& __range = for-range-initializer;
    auto __begin = begin-expr;
    auto __end = end-expr; // end is allowed to be a different type than begin in C++17
    for (; __begin != __end; ++__begin) {
        for-range-declaration = *__begin;
        statement
    }
}
```
This change was introduced for the planned support of Ranges TS in C++20.
In this case, our loop is equivalent to:
```
{
    auto&& __range = v;
    auto __begin = v.begin(), __end = v.end();
    for (; __begin != __end; ++__begin) {
        auto val = *__begin;
        std::cout << val << " ";
    }
}
```

說明:
'auto' is a placeholder type specifier that instructs the compiler to deduce the type of the variable automatically based on the initializer.\
'auto' 是會由c++編譯期自動推斷型別的specifier。

'&&' is an rvalue reference qualifier that is used to bind to rvalues and forward them efficiently.\
'&&' 是右值引用指示詞(rvalue reference qualifier)用來連接rvalues與遞移傳送。


If the range expression (for-range-initializer) is an lvalue, auto&& deduces the range variable to an lvalue reference. This allows the loop to iterate over the elements of the range by reference, enabling modification of the original elements if necessary.\
如果range expression是左值(lvalue)，auto&&推倒為(lvalue reference)這允許循環通過引用遍歷範圍的元素，從而在必要時修改原始元素。
example
```
std::vector<int> vec = {1, 2, 3};
for (auto&& elem : vec) {
    elem *= 2;  // Modify the elements of the vector
}
```
vec 即為 lvalue 的類型，一般我們直接assign的那種樣子。
for內的值可以修改。


If the range expression is an rvalue, auto&& deduces the range variable to an rvalue reference. This allows the loop to efficiently move or transfer the elements of the range, avoiding unnecessary copying.\
如果range expression是右值(rvalue)，auto&&推倒為(rvalue reference)這允許循環有效地移動或傳輸範圍的元素，避免不必要的複制。
```
std::vector<int> getVector();  // Function returning an rvalue vector
for (auto&& elem : getVector()) {
    // Process the elements of the temporary vector
}
```
getVector() 即為 rvalue 的類型。

auto&& 將 elem 推導為右值引用，從而能夠高效處理臨時向量的元素，而無需進行不必要的複制。這是auto&&被設計出來的目的，高效處理forword元素(遞移iterator)。雖然auto&可以做到相同的目標，但語意上與底層能力有所差別。

### Lvalue Reference (T&):
(T&)的左值引用:可以修改內容(如果無const)，生命週期延長。
1. An lvalue reference binds to an lvalue, which refers to an object that has a persistent identity and can be referenced.
1. It is declared using the & symbol and requires an object with a valid storage location.
1. An lvalue reference extends the lifetime of the object it references to match the lifetime of the reference itself.
Example:
```
int x = 42;

// Lvalue reference to an int
int& lvalueRef = x;
lvalueRef = 10; // Modifying the referenced object

const int& constLvalueRef = x;
int y = constLvalueRef; // Accessing the value of the referenced object
```

### Rvalue Reference (T&&):
T&& 可以接受 rvalue 或是 Lvalue (T&只能是Lvalue)。T&&在移動iterator避免複製並專門設計為perfect forwarding(efficient transfer)來遞送container的內容(e.g. vector 裡的 int note:vector稱為container 裡面int稱為range-based的range's value type)
1. An rvalue reference binds to an rvalue, which refers to a temporary object or an object that is about to be destroyed.
1. It is declared using the && symbol and is typically used for move semantics and perfect forwarding.
1. An rvalue reference allows for the efficient transfer of resources from one object to another, such as in move constructors and move assignment operators.
Example:
```
int getX() {
    return 42;
}

// Rvalue reference to an int
int&& rvalueRef = getX();
int z = std::move(rvalueRef); // Moving the value of the referenced object
```

差異說明\
The difference between T& (lvalue reference) and T&& (rvalue reference) goes beyond just performance. While performance is one aspect, there are also semantic and behavioral differences between the two reference types.

Here are some key differences between T& and T&&:

1. Lifetime Extension: Lvalue references (T&) extend the lifetime of the object they reference. This ensures that the object remains valid as long as the reference is in scope. Rvalue references (T&&), on the other hand, do not extend the lifetime of the temporary objects they bind to. They are commonly used for efficient resource transfer or when binding to temporary objects.

1. Move Semantics: Rvalue references (T&&) are crucial for move semantics in C++. They enable the efficient transfer of resources from one object to another, such as in move constructors and move assignment operators. Move semantics improve performance by avoiding unnecessary copies and enabling the reuse of existing resources.\
Move semantics, which involve transferring resources from one object to another, are primarily enabled by rvalue references (T&&). By using move constructors and move assignment operators, T&& allows for efficient resource transfer, such as moving dynamically allocated memory or expensive-to-copy objects. This can significantly improve performance by avoiding unnecessary copying. Lvalue references (T&) do not provide the same semantics and are not designed for resource transfer.

* Forwarding reference:  
a forwarding reference is a type declaration using && that allows a parameter to accept both lvalue and rvalue references

1. Perfect Forwarding: Rvalue references (T&&), in combination with forwarding references (T&& in a template context), allow for perfect forwarding. This preserves the value category and constness of an argument when forwarding it to another function, enabling precise argument forwarding in generic code.\
Perfect forwarding, achieved through forwarding references (T&& in a template context), allows for precise argument forwarding in generic code. It preserves the value category and constness of an argument when forwarding it to another function. This is essential for functions that need to pass arguments to other functions without losing the original value category. Lvalue references (T&) alone cannot achieve perfect forwarding.

1. Overload Resolution: Having both T& and T&& allows for more precise overload resolution. Functions can be overloaded to accept either lvalues or rvalues explicitly, enabling different behavior based on the value category of the argument.

While there may be performance implications associated with move semantics and the avoidance of unnecessary copies, the differences between T& and T&& extend beyond performance considerations. They enable different programming patterns, optimizations, and behaviors in C++.


使用範例\
```
vector<float> v = {0.4f, 12.5f, 16.234f};
for(float &val: v)
{
    std::cout << val << " ";
}
```
使用const確保唯讀
```
const vector<float> v = {0.4f, 12.5f, 16.234f};
for(const float &val: v)
{
    std::cout << val << " ";
}
```
One would use forwarding references when the sequence iterator returns a proxy object and you need to operate on that object in a non-const way。\
'auto&&' 提供最佳的forwarding能力
```
vector<bool> v(10);
for(auto&& val: v)
{
    val = true;
}
```

Any type which has member functions begin() and end(), which return iterators to the elements of the type.
The standard library containers qualify, but user-deﬁned types can be used as well:\
任何class struct 擁有begin() and end() 就可以使用iterate.\
std::vector預設容器:
```
std::vector<int> numbers = {1, 2, 3, 4, 5};
for(auto num : numbers) {
    std::cout << num << " ";
}
```
customize
```
class MyClass {
private:
    int data[3] = {10, 20, 30};
public:
    int* begin() { return std::begin(data); }
    int* end() { return std::end(data); }
};

MyClass obj;
for(auto val : obj) {
    std::cout << val << " ";
}
```
```
struct Rng
{
    float arr[3];
    // pointers are iterators
    const float* begin() const {return &arr[0];}
    const float* end() const   {return &arr[3];}
    float* begin() {return &arr[0];}
    float* end()   {return &arr[3];}
};
int main()
{
    Rng rng = {{0.4f, 12.5f, 16.234f}};
    for(auto val: rng)
    {
        std::cout << val << " ";
    }
}
```
Any type which has non-member begin(type) and end(type) functions which can found via argument
dependent lookup, based on type.
```
namespace Mine
{
    struct Rng {float arr[3];};
    // pointers are iterators
    const float* begin(const Rng &rng) {return &rng.arr[0];}
    const float* end(const Rng &rng) {return &rng.arr[3];}
    float* begin(Rng &rng) {return &rng.arr[0];}
    float* end(Rng &rng) {return &rng.arr[3];}
}
int main()
{
    Mine::Rng rng = {{0.4f, 12.5f, 16.234f}};
    for(auto val: rng)
    {
        std::cout << val << " ";
    }
}
```

Note that allocating a dynamic array does not count (iterator fail):
```
float *arr = new float[3]{0.4f, 12.5f, 16.234f};
for(auto val: arr) //Compile error.
{
    std::cout << val << " ";
}
```

# for loop
```
for (int i = 0; i < 5; ++i) {
    do_something(i);
}
// i is no longer in scope.
for (auto& a : some_container) {
    a.do_something();
}
// a is no longer in scope.
while(std::shared_ptr<Object> p = get_object()) {
   p->do_something();
}
// p is no longer in scope.
```

# File I/O

std::istream for reading text.

std::ostream for writing text.

std::streambuf for reading or writing characters

Formatted input uses operator>>.

Formatted output uses operator<<.

## Writing to a ﬁle
```
std::ofstream os("foo.txt");
if(os.is_open()){
    os << "Hello World!";
}
```
```
std::ofstream os("foo.txt");
if(os.is_open()){
    char data[] = "Foo";
    // Writes 3 characters from data -> "Foo".
    os.write(data, 3);
}
```
## Opening a ﬁle
```
// ifstream: Opens file "foo.txt" for reading only.
std::ifstream ifs("foo.txt");  

// ofstream: Opens file "foo.txt" for writing only.
std::ofstream ofs("foo.txt");  

// fstream:  Opens file "foo.txt" for reading and writing.
std::fstream iofs("foo.txt");  

//or Alternatively,
std::ifstream ifs;
ifs.open("bar.txt"); 

// Check if the file has been opened successfully.
if (!ifs.is_open()) {
    // The file hasn't been opened; take appropriate actions here.
    throw CustomException(ifs, "File could not be opened");
}
```
When ﬁle path contains backslashes (for example, on Windows system) you should properly escape them:
```
// using escaped backslashes
std::ifstream ifs("c:\\\\folder\\\\foo.txt"); 

// or use raw literal:
std::ifstream ifs(R"(c:\\folder\\foo.txt)"); 

//or use forward slashes instead:
std::ifstream ifs("c:/folder/foo.txt"); //c++11


//Open the file 'пример\\foo.txt' on Windows. wide characters and raw literal
std::ifstream ifs(LR"(пример\\foo.txt)");
```

txt file:
```
John Doe 25 4 6 1987
Jane Doe 15 5 24 1976
```
// Extract firstname, lastname, age, bday month, bday day, and bday year in that order.\
// Note: '>>' returns false if it reached EOF (end of file) or if the input data doesn't\
// correspond to the type of the input variable (for example, the string "foo" can't be\
// extracted into an 'int' variable).
```
// Define variables.
std::ifstream is("foo.txt");
std::string firstname, lastname;
int age, bmonth, bday, byear;

while (is >> firstname >> lastname >> age >> bmonth >> bday >> byear)
    // Process the data that has been read.
```
1. For string types, the operator stops at a whitespace () or at a newline (\n).
1. For numbers, the operator stops at a non-number character.

Read all file to string:
```
// Opens 'foo.txt'.
std::ifstream is("foo.txt");
std::string whole_file;
// Sets position to the end of the file.
is.seekg(0, std::ios::end);
// Reserves memory for the file.
whole_file.reserve(is.tellg());
// Sets position to the start of the file.
is.seekg(0, std::ios::beg);
// Sets contents of 'whole_file' to all characters in the file.
whole_file.assign(std::istreambuf_iterator<char>(is),
  std::istreambuf_iterator<char>());
```
Read line by line
```
std::ifstream is("foo.txt");  
// The function getline returns false if there are no more lines.
for (std::string str; std::getline(is, str);) {
    // Process the line that has been read.
}
```
Read by one char
```
char ch;
while (inputFile.get(ch)) {
    std::cout << ch;
}
```

Read a ﬁxed number of characters
```
std::ifstream is("foo.txt");
char str[4];
// Read 4 characters from the file.
is.read(str, 4);

// always check if the error state ﬂag
if (is.fail())
    // Failed to read!
```

### Opening modes
```
std::ofstream os("foo.txt", std::ios::out | std::ios::trunc);

std::ifstream is;
is.open("foo.txt", std::ios::in | std::ios::binary);
```
(All modes can be found in the std::ios namespace.)\
 following default modes are used
* ifstream - in
* ofstream - out
* fstream - in and out


|Mode|Meaning|For|Description|
|-|-|-|-|
|app |append| Output |Appends data at the end of the ﬁle.|
|binary| binary| Input/Output| Input and output is done in binary.|
|in| input| Input| Opens the ﬁle for reading.|
|out| output| Output| Opens the ﬁle for writing.|
|trunc| truncate| Input/Output| Removes contents of the ﬁle when opening.|
|ate| at end| Input| Goes to the end of the ﬁle when opening.|

Note: Setting the binary mode lets the data be read/written exactly as-is; not setting it enables the translation of
the newline '\n' character to/from a platform speciﬁc end of line sequence.

For example on Windows the end of line sequence is CRLF ("\r\n").\
Write: "\n" => "\r\n"\
Read: "\r\n" => "\n"\
(in fstream)


## Reading an ASCII ﬁle into a std::string
```
std::ifstream f("file.txt");
if (f)
{
  std::stringstream buffer;
  buffer << f.rdbuf();
  f.close();
  // The content of "file.txt" is available in the string `buffer.str()`
}
```
The rdbuf() method returns a pointer to a streambuf


Another possibility (popularized in Eﬀective STL by Scott Meyers) is:
```
std::ifstream f("file.txt");
if (f)
{
  std::string str((std::istreambuf_iterator<char>(f)),
                  std::istreambuf_iterator<char>());
  // Operations on `str`...
}
```  


Last but not least:
```
std::ifstream f("file.txt");
if (f)
{
  f.seekg(0, std::ios::end);
  const auto size = f.tellg();
  std::string str(size, ' ');
  f.seekg(0);
  f.read(&str[0], size);
  f.close();
  // Operations on `str`...
}
```

## Flushing a stream
You can do this either directly by invoking the flush() method or through the std::flush
stream manipulator:
```
std::ofstream os("foo.txt");
os << "Hello World!" << std::flush;
char data[3] = "Foo";
os.write(data, 3);
os.flush();
```
or

```
// Both following lines do the same thing
os << "Hello World!\n" << std::flush;
os << "Hello world!" << std::endl;
```

## Reading a ﬁle into a container
```
std::ifstream file("file3.txt");
std::vector<std::string>  v;
std::string s;
while(file >> s) // keep reading until we run out
{
    v.push_back(s);
}
```

read line to vector
```
std::vector<std::string> lines;
std::string line;
while (std::getline(inputFile, line)) {
    lines.push_back(line);
}
```

get by char
```
char ch;
while (inputFile.get(ch)) {
    std::cout << ch;
}
```

get by word
```
std::vector<std::string> words;
std::string word;
while (inputFile >> word) {
    words.push_back(word);
}
```
get line separate by word
```
std::vector<std::string> words;
std::string line;
while (std::getline(inputFile, line)) {
    std::istringstream iss(line);
    std::string word;
    while (iss >> word) {
        words.push_back(word);
    }
}
```
read file to vector
```
std::ifstream file("file3.txt");
std::vector<std::string>  v(std::istream_iterator<std::string>{file},
                            std::istream_iterator<std::string>{});
```

the way the inital vector
```
std::vector<int> v; 

std::vector<int> v(5);

std::vector<int> v(5, 10);

std::vector<int> v(begin_iterator, end_iterator); 

std::vector<int> v = {1, 2, 3, 4, 5};
```
An example that shows how to use a custom end iterator to create a std::vector<int> with a special end range:
```
int main() {
    std::vector<int> source = {1, 2, 3, 4, 5};

    // Create a vector with a special end range
    auto beginIterator = source.begin();
    auto endIterator = source.begin() + 3;  // Custom end iterator

    std::vector<int> v(beginIterator, endIterator);
```

## Copying a ﬁle
```
std::ifstream  src("source_filename", std::ios::binary);
std::ofstream  dst("dest_filename",   std::ios::binary);
dst << src.rdbuf();
```


## Closing a ﬁle
Explicitly closing a ﬁle is rarely necessary in C++.
ile stream will automatically close its associated ﬁle in its
destructor.\
However, you should try to limit the lifetime of a ﬁle stream object, so that it does not keep the ﬁle
handle open longer than necessary.\
Control lifetime(scope):
```
std::string const prepared_data = prepare_data();
{
    // Open a file for writing.
    std::ofstream output("foo.txt");
    // Write data.
    output << prepared_data;
}
```

Calling close() explicitly is only necessary if you want to reuse the same fstream object later, but don't want to
keep the ﬁle open in between:
```
std::ofstream output("foo.txt");

std::string const prepared_data = prepare_data();

output << prepared_data;

output.close();
// Preparing data might take a long time. Therefore, we don't open the output file stream
// before we actually can write some data to it.
std::string const more_prepared_data = prepare_complex_data();
// Open the file "foo.txt" for the second time once we are ready for writing.
output.open("foo.txt");

output << more_prepared_data;

output.close();
```

## Reading a `struct` from a formatted text ﬁle
```
struct info_type
{
    std::string name;
    int age;
    float height;
   
    // we define an overload of operator>> as a friend function which
    // gives in privileged access to private data members
    friend std::istream& operator>>(std::istream& is, info_type& info)
    {
        // skip whitespace
        is >> std::ws;
        std::getline(is, info.name);
        is >> info.age;
        is >> info.height;
        return is;
    }
};
void func4()
{
    auto file = std::ifstream("file4.txt");
    std::vector<info_type> v;
    for(info_type info; file >> info;) // keep reading until we run out
    {
        // we only get here if the read succeeded
        v.push_back(info);
    }
    for(auto const& info: v)
    {
        std::cout << "  name: " << info.name << '\n';
        std::cout << "   age: " << info.age << " years" << '\n';
        std::cout << "height: " << info.height << "lbs" << '\n';
        std::cout << '\n';
    }
}
```
When you write is >> info, it is equivalent to calling the operator>> function with the arguments is and info.\
The first argument is represents the left-hand side of the operator, and the second argument info represents the right-hand side.

# C++ Streams

```
#include <sstream>
#include <string>                                                                                  
                                       
using namespace std;
int main()
{
    ostringstream ss;
    ss << "the answer to everything is " << 42;
    const string result = ss.str();
}  
```
```
(the string result will be equal to "the answer to everything is 42").
```

## Printing collections with iostream

```
std::vector<int> v = {1,2,3,4};
std::copy(v.begin(), v.end(), std::ostream_iterator<int>(std::cout, " ! "));
```
```
1 ! 2 ! 3 ! 4 !
```
Implicit type cast:\
std::ostream_iterator.\
despite std::vector holds ints.\
int -> float 
```
std::cout << std::setprecision(3);
std::fixed(std::cout);

std::vector<int> v = {1,2,3,4};
std::copy(v.begin(), v.end(), std::ostream_iterator<float>(std::cout, " ! "));
```
```
output: 
1.000 ! 2.000 ! 3.000 ! 4.000 !
```
we can easily print boolean value of "x is even" statement for each element:
```
std::boolalpha(std::cout); // print booleans alphabetically
std::transform(v.begin(), v.end(), std::ostream_iterator<bool>(std::cout, " "),
[](int val) {
    return (val % 2) == 0;
});
```

These function calls modify the behavior of the std::cout stream object
```
std::setprecision(3)
std::fixed(std::cout)
std::boolalpha(std::cout)
```
revert the modifications :
```
std::cout << std::setprecision(6);
std::cout << std::defaultfloat;
std::cout << std::noboolalpha;
...
```

Printing N space-delimited random numbers:
```
const int N = 10;
std::generate_n(std::ostream_iterator<int>(std::cout, " "), N, std::rand);
```

print squared values from a native array:
```
int v[] = {1,2,3,4,8,16};
std::transform(v, std::end(v), std::ostream_iterator<int>(std::cout, " "),
[](int val) {
    return val * val;
});
```

std::transform :  
```
template <class InputIt, class OutputIt, class UnaryOperation>
OutputIt transform(InputIt first1, InputIt last1, OutputIt d_first, UnaryOperation unary_op);
```
In this example, the lambda function [](int num) { return num * 2; } is used as the `unary_op`
```
std::transform(input.begin(), input.end(), output.begin(), [](int num) {
        return num * 2;
    });
```

The syntax for a lambda function is as follows:
```
[capture](parameters) -> return_type {
    // function body
}

// [](int num) :  [capture](parameters)

-> return_type :
In the previous example, the return type of the lambda function (int num) { return num * num; } can be deduced as int because the body consists of a single expression that returns an int value. Therefore, the -> part is not required.
```
a example of use capture list
```
int main() {
    int x = 5;
    int y = 10;

    // Capture x and y by value, and perform a calculation using them
    auto sum = [x, y]() {
        return x + y;
    };

    std::cout << "Sum: " << sum() << std::endl;

    // Update the values of x and y
    x = 2;
    y = 3;

    // The captured values in the lambda function remain unchanged
    std::cout << "Sum: " << sum() << std::endl;

    return 0;
}
```
# Templates
A template is a piece of code with some free parameters that will become a concrete class, function, or variable when all parameters are speciﬁed.

## Class Template
template parameter gets substituted by a type at compile time.  
Same class can be reused for multiple types
```
#include <iostream>
using std::cout;
template <typename T>         // A simple class to hold one number of any type
class Number {
public:
    void setNum(T n);         // Sets the class field to the given number
    T plus1() const;          // returns class field's "follower"
private:
    T num;                    // Class field
};
template <typename T>         // Set the class field to the given number
void Number<T>::setNum(T n) {
    num = n;
}
template <typename T>         // returns class field's "follower"
T Number<T>::plus1() const {
    return num + 1;
}
int main() {
    Number<int> anInt;        // Test with an integer (int replaces T in the class)
    anInt.setNum(1);
    cout << "My integer + 1 is " << anInt.plus1() << "\n";     // Prints 2
    Number<double> aDouble;   // Test with a double
    aDouble.setNum(3.1415926535897);
    cout << "My double + 1 is " << aDouble.plus1() << "\n";    // Prints 4.14159
    Number<float> aFloat;     // Test with a float
    aFloat.setNum(1.4);
    cout << "My float + 1 is " << aFloat.plus1() << "\n";      // Prints 2.4
    return 0;  // Successful completion
}
```
## Function Templates
Templating can also be applied to functions with the same eﬀect.
```
template<typename T>
void printSum(T add1, T add2)
{
    std::cout << (add1 + add2) << std::endl;
}

printSum<int>(4, 5);
printSum<float>(4.5f, 8.9f);
```
One additional property of template functions (unlike template classes) is that the compiler can infer the template
parameters based on the parameters passed to the function.
```
printSum(4, 5);     // Both parameters are int.
                    // This allows the compiler deduce that the type
                    // T is also int.
printSum(5.0, 4);   // error
                    // In this case the parameters are two different types.
                    // The compiler is unable to deduce the type of T
                    // because there are contradictions. As a result
                    // this is a compile time error.
```
explicitly specified: Pair<int, double> pair1(10, 3.14)  
implicitly specified (deduced by the compiler): Pair pair1(10, 3.14)

template functions 對於這種推導型態更為嚴格。template classes則是可以藉由constructor來幫助推導。

###  helper function for template classes
```
template<typename T1, typename T2>
struct MyPair
{
    T1      first;
    T2      second;
};
// 2) A make function that has a parameter type for
//    each template parameter in the template structure.
template<typename T1, typename T2>
MyPair<T1, T2> make_MyPair(T1 t1, T2 t2)
{
    return MyPair<T1, T2>{t1, t2};
}

auto val1 = MyPair<int, float>{5, 8.7};     // Create object explicitly defining the types
auto val2 = make_MyPair(5, 8.7);            // Create object using the types of the parameters.
                                            // In this code both val1 and val2 are the same
                                            // type.
```


## specoal: Variadic template data structures
(recursive variadic template class)  
使用variable number(`...`)定義可變數量和類型的data members。典型上是使用std::tuple。但有時候需要自定義訂製結構。這是一個不使用繼承的複合結構。  
典型: (std::tuple<int, double, std::string> myTuple(42, 3.14, "Hello");)
```
template<typename ... T>
struct DataStructure {};

template<typename T, typename ... Rest>
struct DataStructure<T, Rest ...>
{
    DataStructure(const T& first, const Rest& ... rest)
        : first(first)
        , rest(rest...)
    {}
   
    T first;                                
    DataStructure<Rest ... > rest;
};
```
You can visualise this as follows:
```
DataStructure<int, float>
   -> int first
   -> DataStructure<float> rest
         -> float first
         -> DataStructure<> rest
              -> (empty)
```
(for example to access the last member of DataStructure<int, float, std::string> data we would have to use data.rest.rest.first)  
other way:
```
template<typename T, typename ... Rest>
struct DataStructure<T, Rest ...>
{
    ...
    template<size_t idx>
    auto get()
    {
        return GetHelper<idx, DataStructure<T,Rest...>>::get(*this);
    }
    ...
};
```
(so usage can be things like data.get<1>(), similar to std::tuple)  
The detail is in original text.

## Argument forwarding
Template may accept lvalue and rvalue references using `forwarding reference`:
```
template <typename T>
void f(T &&t);
```
```
struct X {};

template <typename T>
void f(T&& t) {
    // Function implementation goes here.
}

int main() {
    X x;
    f(x);    // Calls f<X&>(x) - t has type X& (lvalue reference)
    f(X());  // Calls f<X>(X()) - t has type X&& (rvalue reference)

    return 0;
}
```
In the ﬁrst case, the type T is deduced as reference to X (X&), and the type of t is lvalue reference to X.  
In the second case the type of T is deduced as X and the type of t as rvalue reference to X (X&&).

In the first case, when f(x) is called, the type of T is deduced as X& because x is an lvalue, and the forwarding reference (T&&) with x as the argument results in deducing T as X&. Consequently, t becomes an lvalue reference to X.

In the second case, when f(X()) is called, X() is a temporary (rvalue), and the forwarding reference (T&&) with X() as the argument results in deducing T as X. Consequently, t becomes an rvalue reference to X.

##  Partial template specialization
only available for template class/structs  
The partial template specialization allows to introduce template with some of
the arguments of existing template ﬁxed
```
// Primary template with two template parameters
template <typename T, int N>
class MyClass {
    // Implementation details for the generic case
};

// Partial specialization for MyClass with N fixed to 0
template <typename T>
class MyClass<T, 0> {
    // Implementation details for N = 0
};

// Partial specialization for MyClass with N fixed to 1
template <typename T>
class MyClass<T, 1> {
    // Implementation details for N = 1
};
```
Also, it is not necessary to set all template parameters to 'typename'; you can use non-type template parameters like 'int' in partial specializations for type templates.
```
// Common case:
template<typename T, typename U>
struct S {
    T t_val;
    U u_val;
};
// Special case when the first template argument is fixed to int
template<typename V>
struct S<int, V> {
    double another_value;
    int foo(double arg) {// Do something}
};
```

## Template Specialization
for function template
```
#include <iostream>

template <typename T>
T my_sqrt(T t) {
    /* Some generic implementation */
    return t;
}

template <>
int my_sqrt<int>(int i) {
    /* Highly optimized integer implementation */
    return i * i;
}

int main() {
    double d = 4.0;
    std::cout << "Square root of " << d << " is " << my_sqrt(d) << std::endl; // Output: Square root of 4 is 4

    int i = 5;
    std::cout << "Square of " << i << " is " << my_sqrt(i) << std::endl; // Output: Square of 5 is 25

    return 0;
}
```

## Alias template
An alias template is used to create an alternative name (alias) for an existing template
```
template<typename T, typename U>
using MyPairVector = std::vector<std::pair<T, U>>;

// Usage:
MyPairVector<int, std::string> myPairs; // Equivalent to std::vector<std::pair<int, std::string>>
```

## Explicit instantiation
An explicit instantiation deﬁnition creates and declares a concrete class, function, or variable from a template,
without using it just yet. An explicit instantiation can be referenced from other translation units.


In C++, templates provide a powerful way to write generic code that can work with different types without the need to rewrite the same code multiple times for each type. However, templates are not actual code; they are like blueprints for generating code when they are used with specific types.

Templates are typically defined in header files, as the C++ compilation model requires the template code to be visible at the point of instantiation (when the template is used with specific types). Placing template definitions in header files ensures that the template code is available wherever the header is included. This allows the compiler to perform the necessary type deduction and code generation for the templates.

Explicit instantiation is a technique in C++ where you explicitly instruct the compiler to generate the code for a specific instantiation of a template, even if the template definition is not available at the point of use. This technique is often used to improve code separation and build times when templates are defined in source files instead of header files.

When templates are defined in source files, they require explicit instantiation declarations to generate the code for specific template instantiations. Instead of providing the template definition in the header, explicit instantiation declarations declare the template's existence in the header and then explicitly tell the compiler to generate the code for specific types in a separate source file.

The benefits of explicit instantiation include:

1. Reduced Compilation Times: By providing explicit instantiation declarations for specific template instantiations in a separate source file, the compiler generates the necessary code only once during the linking phase. This avoids redundant code generation and recompilation in multiple translation units, leading to faster compilation.
2. Code Separation: Explicit instantiation allows you to separate the template's definition from its use in a specific translation unit. This improves code organization and maintainability by keeping the template implementation separate from other parts of the code.
3. Code Size Reduction: Explicit instantiation can help reduce code bloat. Without explicit instantiation, the compiler generates the template's code in every translation unit where the template is used with specific types. With explicit instantiation, the code is generated only once and shared across multiple translation units, potentially reducing the overall size of the binary.

potential drawbacks:

1. Increased Linking Time: While explicit instantiation can reduce compilation times, it might increase the linking time. The linker needs to resolve references to the generated code in multiple translation units, adding some overhead during the linking phase.
2. Lack of Inlining and Optimization Opportunities: Explicit instantiation can limit certain compiler optimizations. In the absence of the template definition in the current translation unit, some optimizations like inlining may not be possible, affecting performance.
3. Template Visibility: Explicit instantiation declarations must be placed in a source file, making the instantiation less visible to other parts of the codebase. This can lead to issues if the explicit instantiations are scattered across multiple source files, making it harder to track template usage.
4. Risk of ODR Violations: One Definition Rule (ODR) violations can occur if explicit instantiations are not consistent across different translation units. Providing different explicit instantiations for the same template in different source files can lead to undefined behavior during linking.

In summary, explicit instantiation is a technique used to generate code for specific template instantiations separately from the header file. By providing explicit instantiation declarations in a source file, you allow the compiler to generate the necessary code when it is needed, improving build times and avoiding code duplication. However, explicit instantiation also has trade-offs, including potential impacts on linking time and optimizations. Careful management of explicit instantiation declarations is essential to ensure code separation and improve build efficiency while avoiding potential pitfalls. As with any optimization technique, it's important to measure its impact on your specific codebase to determine whether it provides significant benefits for your particular use case.

MyContainer.h:
```
#ifndef MY_CONTAINER_H
#define MY_CONTAINER_H

template <typename T>
class MyContainer {
public:
    MyContainer(T value) : data(value) {}
    T getValue() const { return data; }

private:
    T data;
};

#endif // MY_CONTAINER_H
```
instead of defining the template implementation in the header file, we will use explicit instantiation in a separate source file.

MyContainer.cpp:
```
#include "MyContainer.h"

// Explicit instantiation for int and double types
template class MyContainer<int>;
template class MyContainer<double>;
```
main.cpp:
```
#include <iostream>
#include "MyContainer.h"

int main() {
    // Using MyContainer with int
    MyContainer<int> intContainer(42);
    std::cout << "Value in intContainer: " << intContainer.getValue() << std::endl; // Output: Value in intContainer: 42

    // Using MyContainer with double
    MyContainer<double> doubleContainer(3.14);
    std::cout << "Value in doubleContainer: " << doubleContainer.getValue() << std::endl; // Output: Value in doubleContainer: 3.14

    return 0;
}
```
In this example, the MyContainer template class is defined in the header file MyContainer.h, but the template implementation is provided in the source file MyContainer.cpp. To ensure that the template's code is only generated once and shared across multiple translation units, we use explicit instantiation in MyContainer.cpp for specific types (int and double in this case).

By using explicit instantiation, we achieve code separation and avoid duplicating the template's implementation in multiple translation units. The main.cpp file can use MyContainer with specific types without having to recompile the entire template's implementation.

Note: Explicit instantiation is generally used when there is a performance benefit or to reduce build times, especially for large projects with complex templates. For small projects or simple templates, the traditional approach of defining the template entirely in the header file is often sufficient. The decision to use explicit instantiation should be based on your specific use case and performance requirements.

## Non-type template parameter
```
#include <iostream>
template<typename T, std::size_t size>
std::size_t size_of(T (&anArray)[size])  // Pass array by reference. Requires.
{                                        // an exact size. We allow all sizes
    return size;                         // by using a template "size".
}
int main()
{
    char anArrayOfChar[15];
    std::cout << "anArrayOfChar: " << size_of(anArrayOfChar) << "\n";
    int  anArrayOfData[] = {1,2,3,4,5,6,7,8,9};
    std::cout << "anArrayOfData: " << size_of(anArrayOfData) << "\n";
}
```
## Declaring non-type template arguments with auto
Prior to C++17
```
template <auto N>
struct integral_constant {
    using type = decltype(N);
    static constexpr type value = N;
};
using five = integral_constant<5>;
```

## Template template parameters
```
template <class T>
struct Tag1 { };
template <class T>
struct Tag2 { };
template <template <class> class Tag>
struct IntTag {
   typedef Tag<int> type;
};
int main() {
   IntTag<Tag1>::type t;
}
```
Version ≥ C++11
```
#include <vector>
#include <iostream>
template <class T, template <class...> class C, class U>
C<T> cast_all(const C<U> &c) {
   C<T> result(c.begin(), c.end());
   return result;
}
int main() {
   std::vector<float> vf = {1.2, 2.6, 3.7};
   auto vi = cast_all<int>(vf);
   for(auto &&i: vi) {
      std::cout << i << std::endl;
   }
}
```

## Default template parameter value
```
template <class T, size_t N = 10>
struct my_array {
    T arr[N];
};
int main() {
    /* Default parameter is ignored, N = 5 */
    my_array<int, 5> a;
    /* Print the length of a.arr: 5 */
    std::cout << sizeof(a.arr) / sizeof(int) << std::endl;
    /* Last parameter is omitted, N = 10 */
    my_array<int> b;
    /* Print the length of a.arr: 10 */
    std::cout << sizeof(b.arr) / sizeof(int) << std::endl;
}
```

## Expression templates
```
// Expression template for a simple addition
template <typename T>
struct AddExpression {
    const T& lhs;
    const T& rhs;

    AddExpression(const T& left, const T& right) : lhs(left), rhs(right) {}

    // The operator+ overload for AddExpression objects
    T operator()() const {
        return lhs + rhs; // The actual addition is performed here
    }
};

// Overloaded '+' operator for any type that supports addition
template <typename T>
AddExpression<T> operator+(const T& left, const T& right) {
    return AddExpression<T>(left, right);
}

int main() {
    int a = 5, b = 10, c = 20;

    // Fused operation without creating intermediate temporary variables
    int result = a + b + c;

    // The expression 'a + b + c' is represented using AddExpression objects
    // and the actual addition is performed directly in the 'result' variable.
    // No temporary variables are created during the calculation.

    return 0;
}
```
```
template <typename LHS, typename RHS>
class MatrixSum
{
public:
    using value_type = typename LHS::value_type;
     MatrixSum(const LHS& lhs, const RHS& rhs) : rhs(rhs), lhs(lhs) {}
   
    value_type operator() (int x, int y) const  {
        return lhs(x, y) + rhs(x, y);
    }
private:
    const LHS& lhs;
    const RHS& rhs;
};

template <typename LHS, typename RHS>
MatrixSum<LHS, RHS> operator+(const LHS& lhs, const LHS& rhs) {
    return MatrixSum<LHS, RHS>(lhs, rhs);
}
```
1. `value_type operator()() const`: This is the function call operator of the `MatrixSum` class. It allows instances of `MatrixSum` to behave like function objects that can be called like functions. When an instance of `MatrixSum` is called with the function call operator (e.g., `matrixSum(x, y)`), it will evaluate the element-wise sum of the `LHS` and `RHS` expressions at the specified `(x, y)` position.
2. `MatrixSum<LHS, RHS> operator+(const LHS& lhs, const RHS& rhs)`: This is the overloaded `+` operator for the `MatrixSum` class. It allows instances of `MatrixSum` to be created using the `+` operator, making the element-wise sum syntax look similar to standard arithmetic operations. The operator takes two references to the left-hand side (`lhs`) and right-hand side (`rhs`) expressions and returns a `MatrixSum` object representing the sum operation.

## Curiously Recurring Template Pattern (CRTP)

# Metaprogramming
In C++ Metaprogramming refers to the use of macros or templates to generate code at compile-time.  
In general, macros are frowned upon in this role and `templates are preferred`, although they are not as generic.

## Calculating Factorials
Factorials can be computed at compile-time using template metaprogramming techniques.
```
#include <iostream>
template<unsigned int n>
struct factorial
{
    enum
    {
        value = n * factorial<n - 1>::value
    };
};
template<>
struct factorial<0>
{
    enum { value = 1 };
};
int main()
{
    std::cout << factorial<7>::value << std::endl;    // prints "5040"
}
```

template metaprogramming  treat  factorial struct as a template metafunction

Limitation: Most of the compilers won't allow recursion depth beyond a limit. For example, g++ compiler by default
limits recursion depeth to 256 levels. In case of g++, programmer can set recursion depth using -ftemplate-depth-
X option.

Since C++11, the std::integral_constant template can be used for this kind of template computation:
```
#include <iostream>
#include <type_traits>
template<long long n>
struct factorial :
  std::integral_constant<long long, n * factorial<n - 1>::value> {};
template<>
struct factorial<0> :
  std::integral_constant<long long, 1> {};
int main()
{
    std::cout << factorial<7>::value << std::endl;    // prints "5040"
}
```
Additionally, constexpr functions become a cleaner alternative.
```
#include <iostream>
constexpr long long factorial(long long n)
{
  return (n == 0) ? 1 : n * factorial(n - 1);
}
int main()
{
  char test[factorial(3)];
  std::cout << factorial(7) << '\n';
}
```

## Iterating over a parameter pack
variadic template parameter \
 Suppose we simply want to print every element in a pack. The simplest solution is to recurse:
 ```
 void print_all(std::ostream& os) {
    // base case
}
template <class T, class... Ts>
void print_all(std::ostream& os, T const& first, Ts const&... rest) {
    os << first;
   
    print_all(os, rest...);
}
 ```

"class... Ts" is a variadic template parameter pack  
The ellipsis ... indicates that Ts can accept zero or more template arguments. Each argument in the pack is treated as a separate type.

We could instead use the expander trick, to perform all the streaming in a single function. This has the advantage of
not needing a second overload, but has the disadvantage of less than stellar readability:
以下作法只需一個function, 不須overload (print_all in above example), 用一個dummy array 接住.
## parameter pack expansion
```
Version ≥ C++11
template <class... Ts>
void print_all(std::ostream& os, Ts const&... args) {
    using expander = int[];
    (void)expander{0,
        (void(os << args), 0)...
    };
}
```
```
template<typename... Args>
static void foo2(Args &&... args)
{
    int dummy[] = { 0, ( (void) bar(std::forward<Args>(args)), 0) ... };
}
```
```
{ 0, ( (void) bar(std::forward<Args>(args)), 0) ... };
  |       |       |                        |     |
  |       |       |                        |     --- pack expand the whole thing 
  |       |       |                        |   
  |       |       --perfect forwarding     --- comma operator
  |       |
  |       -- cast to void to ensure that regardless of bar()'s return type
  |          the built-in comma operator is used rather than an overloaded one
  |
  ---ensure that the array has at least one element so that we don't try to make an
     illegal 0-length array when args is empty
```

With C++17,we get two powerful new tools. fold-expression:\
don't need 0, and ellipsis "..." after comma ","
```
((void) bar(std::forward<Args>(args)), ...);
```
```
template <class... Ts>
void print_all(std::ostream& os, Ts const&... args) {
    ((os << args), ...);
}
```

And the second way is if constexpr, which allows us to write our original recursive solution in a single function:
```
template <class T, class... Ts>
void print_all(std::ostream& os, T const& first, Ts const&... rest) {
    os << first;
    if constexpr (sizeof...(rest) > 0) {        
        // this line will only be instantiated if there are further
        // arguments. if rest... is empty, there will be no call to
        // print_all(os).
        print_all(os, rest...);
    }
}
```
`constexpr` :  
In C++, if constexpr is a feature introduced in C++17 that allows compile-time branching based on a constant expression. It differs from the regular if statement in that the condition is evaluated at compile-time, and only the branch that satisfies the condition will be compiled. The branch that does not satisfy the condition is discarded during compilation.

In the given example, the if constexpr statement is used to conditionally call the print_all function recursively only if there are more arguments (sizeof...(rest) > 0).  
If there are no more arguments, the if constexpr branch will not be instantiated and there will be no further recursive calls to print_all.

##  Iterating with std::integer_sequence
```
namespace detail {
    template <class F, class Tuple, std::size_t... Is>
    decltype(auto) apply_impl(F&& f, Tuple&& tpl, std::index_sequence<Is...> ) {
        return std::forward<F>(f)(std::get<Is>(std::forward<Tuple>(tpl))...);
    }
}
template <class F, class Tuple>
decltype(auto) apply(F&& f, Tuple&& tpl) {
    return detail::apply_impl(std::forward<F>(f),
        std::forward<Tuple>(tpl),
        std::make_index_sequence<std::tuple_size<std::decay_t<Tuple>>::value>{});
}

// this will print 3
int f(int, char, double);
auto some_args = std::make_tuple(42, 'x', 3.14);
int r = apply(f, some_args); // calls f(42, 'x', 3.14)
```

## Tag Dispatching
```
namespace details {
    template <class RAIter, class Distance>
    void advance(RAIter& it, Distance n, std::random_access_iterator_tag) {
        it += n;
    }
    template <class BidirIter, class Distance>
    void advance(BidirIter& it, Distance n, std::bidirectional_iterator_tag) {
        if (n > 0) {
            while (n--) ++it;
        }
        else {
            while (n++) --it;
        }
    }
    template <class InputIter, class Distance>
    void advance(InputIter& it, Distance n, std::input_iterator_tag) {
        while (n--) {
            ++it;
        }
    }    
}
template <class Iter, class Distance>
void advance(Iter& it, Distance n) {
    details::advance(it, n,
            typename std::iterator_traits<Iter>::iterator_category{} );
}
```

## Manual distinction of types when given any type
When implementing SFINAE using std::enable_if 

The following example show how to detect if a type T is a pointer or not, the is_pointer template mimic the
behavior of the standard std::is_pointer helper:
```
template <typename T>
struct is_pointer_: std::false_type {};
template <typename T>
struct is_pointer_<T*>: std::true_type {};
template <typename T>
struct is_pointer: is_pointer_<typename std::remove_cv<T>::type> { }
```

1. he ﬁrst declaration of is_pointer_ is the default case, and inherits from std::false_type. The default case
should always inherit from std::false_type since it is analogous to a "false condition".
2. The second declaration specialize the is_pointer_ template for pointer T* without caring about what T is
really. This version inherits from std::true_type.
3. The third declaration (the real one) simply remove any unnecessary information from T (in this case we
remove const and volatile qualiﬁers) and then fall backs to one of the two previous declarations.

Since is_pointer<T> is a class, to access its value you need to either:
1. Use ::value, e.g. is_pointer<int>::value – value is a static class member of type bool inherited from
std::true_type or std::false_type;
2. onstruct an object of this type, e.g. is_pointer<int>{} – This works because std::is_pointer inherits its
default constructor from std::true_type or std::false_type (which have constexpr constructors) and both
std::true_type and std::false_type have constexpr conversion operators to bool.
```
template <typename T>
constexpr bool is_pointer_v = is_pointer<T>::value;
```


# const keyword

```
class Foo
{
public:
    Bar& GetBar(/* some arguments */)
    {
        /* some calculations */
        return bar;
    }
   
    const Bar& GetBar(/* some arguments */) const
    {
        /* some calculations */
        return bar;
    }
    // ...
};
```
he only diﬀerence between them is that one method is non-const and return a non-const reference (that can be
use to modify object) and the second is const and returns const reference.


To avoid the code duplication, there is a temptation to call one method from another.
```
struct Foo
{
    Bar& GetBar(/*arguments*/)
    {
        return const_cast<Bar&>(const_cast<const Foo*>(this)->GetBar(/*arguments*/));
    }
   
    const Bar& GetBar(/*arguments*/) const
    {
        /* some calculations */
        return foo;
    }
};
```

In the example, const_cast is used to remove the const-qualification from this pointer(const Bar& GetBar) inside the non-const member function GetBar(). By doing so, you can call the const member function GetBar() from within the non-const member function.


1. Non-const Foo object:

* You can call both const and non-const member functions of Foo.
* Inside a non-const member function, you can call both const and non-const member functions of Foo directly.
2. Const Foo object:
* You can only call the const-qualified member functions of Foo.
* Inside a const member function, you can call both const and non-const member functions of Foo directly.
3. Calling const member function from non-const member function:
* Inside a non-const member function, you can call the const member function of Foo directly without the need for const_cast. The compiler automatically resolves the call to the appropriate version based on the const-qualification of the object.
4. Calling non-const member function from const member function:
* Inside a const member function, you cannot call the non-const member functions of Foo directly. If you need to call a non-const member function from a const member function, you would need to use const_cast to remove the const-qualification. However, modifying a non-const object from a const member function is generally considered bad practice and should be avoided.

# Const member functions
Member functions of a class can be declared const, which tells the compiler and future readers that this function
will not modify the object:  
`int myInt() const`
```
class MyClass
{
private:
    int myInt_;
public:
    int myInt() const { return myInt_; }
    void setMyInt(int myInt) { myInt_ = myInt; }
};
```

# mutable keyword

## lambda in c++ basic example
```
//Example 1: Lambda with no parameters 
//Lambda with no parameters
auto func = []() {
    // Function body
};


//The implicit operator() of this lambda takes no parameters.
//Example 2: Lambda with parameters
auto add = [](int a, int b) {
    return a + b;
};

//The implicit operator() of this lambda takes two int parameters a and b.
//Example 3: Lambda with capture
int x = 5;
auto multiply = [x](int y) {
    return x * y;
};

//The implicit operator() of this lambda takes one int parameter y and captures the variable x by value.
//Example 4: Lambda with variadic parameters
auto print = [](const auto&... values) {
    ((std::cout << values << " "), ...);
};
```

By default, the implicit operator() of a lambda is const. This disallows performing non-const operations on the
lambda. In order to allow modifying members, a lambda may be marked mutable, which makes the implicit
operator() non-const:
```
int a = 0;
auto bad_counter = [a] {
    return a++;   // error: operator() is const
                  // cannot modify members
};
auto good_counter = [a]() mutable {
    return a++;  // OK
}
good_counter(); // 0
good_counter(); // 1
good_counter(); // 2
```

%Note:  [a] is the capture list, () is the parameter list for the lambda function call. If the parameter list is empty, the operator() can be omitted.

# non-static class member modiﬁer

In the given example, pi_calculated is marked as mutable because its value needs to be modified inside the get_pi() const member function. Without the mutable keyword, attempting to modify pi_calculated would result in a compilation error.

You should not use this keyword to break logical const-ness of an object.

```
class pi_calculator {
 public:
     double get_pi() const {
         if (pi_calculated) {
             return pi;
         } else {
             double new_pi = 0;
             for (int i = 0; i < 1000000000; ++i) {
                 // some calculation to refine new_pi
             }
             // note: if pi and pi_calculated were not mutable, we would get an error from a
compiler
             // because in a const method we can not change a non-mutable field
             pi = new_pi;
             pi_calculated = true;
             return pi;
         }
     }
 private:
     mutable bool pi_calculated = false;
     mutable double pi = 0;
};
```

# Friend keyword

The friend keyword in C++ is used to grant access to private or protected members of a class to functions or other classes that are declared as friends.

When a function is declared as a friend inside a class, it is not a member function of the class, but rather a standalone function that is granted access to the private and protected members of the class.

```
// Forward declaration of functions.
void friend_function();
void non_friend_function();
class PrivateHolder {
public:
    PrivateHolder(int val) : private_value(val) {}
private:
    int private_value;
    // Declare one of the function as a friend.
    friend void friend_function();
};
void non_friend_function() {
    PrivateHolder ph(10);
    // Compilation error: private_value is private.
    std::cout << ph.private_value << std::endl;
}
void friend_function() {
    // OK: friends may access private values.
    PrivateHolder ph(10);
    std::cout << ph.private_value << std::endl;
}
```

# 19.2 Friend method
```
class Accesser {
public:
    void private_accesser();
};
class PrivateHolder {
public:
    PrivateHolder(int val) : private_value(val) {}
    friend void Accesser::private_accesser();
private:
    int private_value;
};
void Accesser::private_accesser() {
    PrivateHolder ph(10);
    // OK: this method is declares as friend.
    std::cout << ph.private_value << std::endl;
}
```


# 20 Type Keywords

## class
```
class foo; // elaborated type specifier -> forward declaration
class bar {
  public:
    bar(foo& f);
};
void baz();
class baz; // another elaborated type specifer; another forward declaration
           // note: the class has the same name as the function void baz()
class foo {
    bar b;
    friend class baz; // elaborated type specifier refers to the class,
                      // not the function of the same name
  public:
    foo();
};
```
## enum
```
enum Direction {
    UP,
    LEFT,
    DOWN,
    RIGHT
};
Direction d = UP;
```
```
enum class Format : char {
       TEXT,
       PDF,
       OTHER
   };
   Format f = Format::TEXT;
   enum Language : int {
       ENGLISH,
       FRENCH,
       OTHER
   };
```
%Note: enum class與class無關，是種強行別的enum，使用Format::TEXT之類的方法避免重複。

## struct

Interchangeable with class, except for the following diﬀerences:
* If a class type is deﬁned using the keyword struct, then the default accessibility of bases and members is
public rather than private.
* struct cannot be used to declare a template type parameter or template template parameter; only class can.
* So, in summary, both struct and class can define methods, but the default access level of members is different.


# decltype
pronounced as "dee-kl-type" 

Yields the type of its operand, which is not evaluated.

1. If e is an lvalue of type T, then decltype(e) is T& (lvalue reference to T).
Example:
```
int x = 10;
decltype(x) y = x;  // y is declared as int&
```
2. If e is an xvalue of type T, then decltype(e) is T&& (rvalue reference to T).
Example:
```
int x = 10;
decltype(std::move(x)) y = std::move(x);  // y is declared as int&&
```
 
3. If e is a prvalue of type T, then decltype(e) is T (the type itself).
Example:
```
decltype(42) x = 42;  // x is declared as int
```

# const 
```
const int x = 123;
x = 456;    // error
int& r = x; // error, this try to bind a 
            //non-const lvalue reference r to a 
            //const variable x
struct S {
    void f();
    void g() const;
};
const S s;
s.f(); // error
s.g(); // OK
```

# volatile
In the example below, if memory_mapped_port were not volatile, the compiler could optimize the function so that it
performs only the ﬁnal write, which would be incorrect if sizeof(int) is greater than 1. 

The volatile qualiﬁcation
forces it to treat all sizeof(int) writes as diﬀerent side eﬀects and hence perform all of them (in order).

```
extern volatile char memory_mapped_port;
void write_to_device(int x) {
    const char* p = reinterpret_cast<const char*>(&x);
    for (int i = 0; i < sizeof(int); i++) {
        memory_mapped_port = p[i];
    }
}
```

# typename

When followed by a qualiﬁed name, typename speciﬁes that it is the name of a type.  
This is often required in templates, in particular, when the nested name speciﬁer is a dependent type other than the current
instantiation.  
In this example, std::decay<T> depends on the template parameter T, so in order to name the
nested type type, we need to preﬁx the entire qualiﬁed name with typename. 

---

It can't be looked up until the actual template arguments are known - then it's called a dependent name 

在C++中，編譯器需要知道目標是類型與否(types or not)
```
t * f;
```
上例中t的意思就會有多種，* 可能成會乘法運算，若t是某種type(類型定義)、f 就會被解釋為pointer。
```
t::x
```
如果上述t是一個template type? C++會如何解釋? `x`可能是靜態變數(static int data member)可以被乘、或是等同於巢狀class(nested class)、或是可以產生declaration的typedef。編譯器無法知道這件事直到實例化。所以需要使用typename。
```
template <typename T>
struct MyTemplate {
    using Nested = typename T::Type;
};

struct MyClass {
    using Type = int;
};

int main() {
    MyTemplate<MyClass>::Nested obj;  // Need typename to indicate Nested is a type
    return 0;
}
```
在上面的範例中struct MyTemplate中的typename若沒有寫，會報編譯錯誤。`typename`這邊通知了編譯器，這東西是個type。
`Nested`就是個 dependent type name 在MyTemplate裡。  
Without typename, the compiler would interpret Nested as a variable or member。
若Nested被當成variable則，`MyTemplate<MyClass>::Nested obj;`這句就不成立。

---


```
template <class T>
auto decay_copy(T&& r) -> typename std::decay<T>::type;
```
Introduces a type parameter in the declaration of a template. In this context, it is interchangeable with class.  
template \<typename T\>當中的typename，則是假設T是nested class的type類型
```
template <typename T>
const T& min(const T& x, const T& y) {
    return b < a ? b : a;
}
```


Note: template \<class T\> 裡面的 class 是可以跟 typename 互換。在現代c++中，typename只是語法直覺上比較合適。但兩者一般上可以互換。
另外:  
however, have to use class (and not typename) when declaring a template template parameter:
```
template <template <typename> class    T> class C { }; // valid!
template <template <typename> typename T> class C { }; // invalid!  o noez!
```

# explicit
避免被隱式轉換

When applied to a single-argument constructor, prevents that constructor from being used to perform
implicit conversions.
```
class MyVector {
  public:
    explicit MyVector(uint64_t size);
};
MyVector v1(100);  // ok
uint64_t len1 = 100;
MyVector v2{len1}; // ok, len1 is uint64_t
int len2 = 100;
MyVector v3{len2}; // ill-formed, implicit conversion from int to uint64_t
```
```
class C {
    const int x;
  public:
    C(int x) : x(x) {}
    explicit operator int() { return x; }
};
C c(42);
int x = c;                   // ill-formed
int y = static_cast<int>(c); // ok; explicit conversion
```
Note:
```
int y = static_cast<int>(c);
int y = (int)c;

//they do the same thing. (int)c is by C-style cast syntax 
//C-style casts are more lenient and allow various types of conversions, including implicit conversions. They attempt different casting methods until a suitable conversion is found. However, this flexibility comes at the cost of reduced type safety, as it may allow potentially unsafe or unintended conversions.

// recommended to use `static_cast` or other specific cast operators.
// They provide clearer intent, better compile-time checks, and are less prone to unexpected behavior.
```

# 24 Returning several values from a function

* Using std::tuple  

* Structured Bindings    
C++17 that allows you to conveniently extract and assign multiple values from a single expression
```auto [iterator, success] = m.insert({"Hello", 42});```  
Structured bindings can be used by default with std::pair, std::tuple,
```
#include <tuple>
#include <iostream>

int main() {
    std::tuple<int, double, std::string> myTuple{42, 3.14, "Hello"};

    auto [val1, val2, val3] = myTuple;

    std::cout << val1 << ", " << val2 << ", " << val3 << std::endl;

    return 0;
}
```
```
std::map<std::string, int> m;
// insert an element into the map and check if insertion succeeded
auto [iterator, success] = m.insert({"Hello", 42});
if (success) {
    // your code goes here
}
// iterate over all elements without having to use the cryptic 'first' and 'second' names
for (auto const& [key, value] : m) {
    std::cout << "The value for " << key << " is " << value << '\n';
}
```

* struct

* Using Output Parameters
```
void calculate(int a, int b, int& c, int& d, int& e, int& f) {
    c = a + b;
    d = a - b;
    e = a * b;
    f = a / b;
}

void calculate(int a, int b, int* c, int* d, int* e, int* f) {
    *c = a + b;
    *d = a - b;
    *e = a * b;
    *f = a / b;
}
```

* Using a Function Object Consumer
```
template <class F>
void foo(int a, int b, F consumer) {
    consumer(a + b, a - b, a * b, a / b);
}
// use is simple... ignoring some results is possible as well
foo(5, 12, [](int sum, int , int , int ){
    std::cout << "sum is " << sum << '\n';
});
```
```
template<class Tuple>
struct continuation {
  Tuple t;
  template<class F>
  decltype(auto) operator->*(F&& f)&&{
    return std::apply( std::forward<F>(f), std::move(t) );
  }
};
std::tuple<int,int,int,int> foo(int a, int b);
continuation(foo(5,12))->*[](int sum, auto&&...) {
  std::cout << "sum is " << sum << '\n';
};
```
* Using std::pair
* Using std::array
* Using std::vector
* Using Output Iterator
```
template<typename Incrementable, typename OutputIterator>
void generate_sequence(Incrementable from, Incrementable to, OutputIterator output) {
    for (Incrementable k = from; k != to; ++k)
        *output++ = k;
}
```

# Polymorphism
deﬁne polymorphic ->   keyword `virtual`
```
class Shape {
public:
    virtual ~Shape() = default;
    virtual double get_surface() const = 0;
    virtual void describe_object() const { std::cout << "this is a shape" << std::endl; }  
    double get_doubled_surface() const { return 2 * get_surface(); }
};
```

* You can deﬁne polymorphic behavior by introduced member functions with the keyword virtual.  

Here get_surface() and describe_object() will obviously be implemented diﬀerently for a square than for a circle. When the function is invoked on an object, function corresponding to the real class of the object will be determined at runtime.


It makes no sense to deﬁne get_surface() for an abstract shape. This is why the function is followed by = 0.
* This means that the function is pure virtual function.

* A class that contains at least one pure virtual function is an abstract class  
Abstract classes cannot be instantiated. You may only have pointers or references of an abstract class type.

* A polymorphic class should always deﬁne a virtual destructor.

* non-virtual function -> If you have a derived class called "Circle" that inherits from the Shape class, and you call the get_doubled_surface() function on an object of type Circle (even if accessed through a pointer or reference of the base class type Shape), the specific implementation of get_doubled_surface() in the Circle class will be invoked.

Derived classes
```
class Square : public Shape {
    Point top_left;
    double side_length;
public:
    Square (const Point& top_left, double side)
       : top_left(top_left), side_length(side_length) {}
    double get_surface() override { return side_length * side_length; }  
    void describe_object() override {
        std::cout << "this is a square starting at " << top_left.x << ", " << top_left.y
                  << " with a length of " << side_length << std::endl;
    }  
};
```

* No need to tell the compiler the keyword virtual again.  
 But it's recommended to add the keyword `override` at the end of the function declaration, in order to
prevent subtle bugs caused by unnoticed variations in the function signature.
* If all the `pure virtual functions` of the parent class are deﬁned you can instantiate objects for this class, else it will also become an `abstract class`.
* You are not obliged to override all the virtual functions. You can keep the version of the parent if it suits your
need.

Example of instantiation
```
int main() {
    Square square(Point(10.0, 0.0), 6); // we know it's a square, the compiler also
    square.describe_object();
    std::cout << "Surface: " << square.get_surface() << std::endl;
    Circle circle(Point(0.0, 0.0), 5);
    Shape *ps = nullptr;  // we don't know yet the real type of the object
    ps = &circle;         // it's a circle, but it could as well be a square
    ps->describe_object();
    std::cout << "Surface: " << ps->get_surface() << std::endl;
}
```

# Safe downcasting

```
Shape *ps;                       // see example on defining a polymorphic class
ps =  get_a_new_random_shape();  // if you don't have such a function yet, you
                                 // could just write ps = new Square(0.0,0.0, 5);
```

However, you may need sometimes to downcast. A typical example is when you want to invoke a non virtual
function that exist only for the child class.
* 若需要的函數是只位於子類的非虛擬函數
```
class Circle: public Shape { // for Shape, see example on defining a polymorphic class
    Point center;
    double radius;
public:
    Circle (const Point& center, double radius)
       : center(center), radius(radius) {}
    double get_surface() const override { return r * r * M_PI; }  
    // this is only for circles. Makes no sense for other shapes
    double get_diameter() const { return 2 * r; }
};
```
The get_diameter() member function only exist for circles. It was not deﬁned for a Shape object:

ERROR!:
```
Shape* ps = get_any_shape();
ps->get_diameter(); // OUCH !!! Compilation `error`
```

* How to downcast ?

If you'd know `for sure` that ps points to a circle you could opt for a static_cast:
* static_cast
```
std::cout << "Diameter: " << static_cast<Circle*>(ps)->get_diameter() << std::endl;
```
But it's very `risky`: if ps appears to by anything else than a Circle the behavior of your code
will be undeﬁned.


* dynamic_cast
```
int main() {
    Circle circle(Point(0.0, 0.0), 10);
    Shape &shape = circle;
    std::cout << "The shape has a surface of " << shape.get_surface() << std::endl;
    //shape.get_diameter();   // OUCH !!! Compilation error
    Circle *pc = dynamic_cast<Circle*>(&shape); // will be nullptr if ps wasn't a circle
    if (pc)
        std::cout << "The shape is a circle of diameter " << pc->get_diameter() << std::endl;
    else
        std::cout << "The shape isn't a circle !" << std::endl;
}        
```
Note that `dynamic_cast` is not possible on a class that is not polymorphic. You'd need `at least one virtual function` in
the class or its parents to be able to use it.


# Polymorphism & Destructors
If a class is intended to be used polymorphically, with derived instances being stored as base pointers/references, its base class' destructor should be either virtual or protected. 

In the former case, this will cause object destruction to check the vtable, automatically calling the correct destructor based on the dynamic type. 

In the latter case, destroying the object through a base class pointer/reference is disabled, and the object can only be
deleted when explicitly treated as its actual type.
* 若Destructors是protected則需要指定type轉型否則會編譯錯誤。若為virtual為自動檢查vtable不須轉換。
```
struct VirtualDestructor {
    virtual ~VirtualDestructor() = default;
};
struct VirtualDerived : VirtualDestructor {};
struct ProtectedDestructor {
  protected:
    ~ProtectedDestructor() = default;
};
struct ProtectedDerived : ProtectedDestructor {
    ~ProtectedDerived() = default;
};
// ...
VirtualDestructor* vd = new VirtualDerived;
delete vd; // Looks up VirtualDestructor::~VirtualDestructor() in vtable, sees it's
           // VirtualDerived::~VirtualDerived(), calls that.
ProtectedDestructor* pd = new ProtectedDerived;
delete pd; // Error: ProtectedDestructor::~ProtectedDestructor() is protected.
delete static_cast<ProtectedDerived*>(pd); // Good.
```


#  Deep copying
If a type wishes to have value semantics, and it needs to store objects that are dynamically allocated, then on copy
operations, the type will need to allocate new copies of those objects. It must also do this for copy assignment.  
This kind of copying is called a "deep copy". It eﬀectively takes what would have otherwise been reference
semantics and turns it into value semantics:

In languages like C++ and Java, deep copying is typically done by implementing a copy constructor and an assignment operator overload that perform a deep copy of the data.  
\#Note: deep copying會使用constructor和assignment operator完成。
```
struct Inner {int i;};
const int NUM_INNER = 5;
class Value
{
private:
  Inner *array_; //Normally has reference semantics.
public:
  Value() : array_(new Inner[NUM_INNER]){}
  ~Value() {delete[] array_;}
  Value(const Value &val) : array_(new Inner[NUM_INNER])
  {
    for(int i = 0; i < NUM_INNER; ++i)
      array_[i] = val.array_[i];
  }
  Value &operator=(const Value &val)
  {
    for(int i = 0; i < NUM_INNER; ++i)
      array_[i] = val.array_[i];
    return *this;
  }
};
```
"value semantics", "reference semantics", "Move semantics"

## "reference semantics" different:
\# Note: 
Fundamental types in C++ have value semantics, which means that when you assign a new value to a variable of a fundamental type, you are replacing the existing value with a new value. Modifying a fundamental type variable does not affect other variables that have been assigned the same value.

On the other hand, class types, including user-defined classes and standard library classes, can have different semantics depending on their implementation. They may have reference semantics, where multiple variables can refer to the same underlying object, and modifications to one variable will affect all variables that refer to the same object.

基本型態是value semantics。class型態是reference semantics。有情況是在做reference時class類型會因為constructor產生副本導致不引響原本的數值參數。但如果是基本型態的reference不會產生副本。故在對這些資料做修改時，基本型態會一同改變，但class型態不會因為class改到的是副本。

## 基本型態 Fundamental types - reference semantics
```
#include <iostream>

class MyClass {
public:
    MyClass(int& value) : data_(value) {}
    
    void modifyData() {
        data_ *= 2; // Modify the original data
    }
    
    int getData() const {
        return data_;
    }

private:
    int& data_;
};

int main() {
    int value = 5;
    MyClass obj(value); // Pass value by reference
    
    obj.modifyData(); // Modify the data through the object
    
    std::cout << value << std::endl; // Output: 10
    std::cout << obj.getData() << std::endl; // Output: 10
    
    return 0;
}
```

## class types - reference semantics
```
class MyClass {
public:
    MyClass(const std::string& str) : data_(str) {}
    const std::string& getData() const {
        return data_;
    }
private:
    std::string data_;
};

int main() {
    std::string str = "Hello, world!";
    MyClass obj(str); // Copying str using reference semantics
    str = "Updated data"; // Modifying str

    std::cout << obj.getData() << std::endl; // Output: Hello, world!
    return 0;
}
```

## this Pointer CV-Qualiﬁers
```
struct ThisCVQ {
    void no_qualifier()                {} // "this" is: ThisCVQ*
    void  c_qualifier() const          {} // "this" is: const ThisCVQ*
    void  v_qualifier() volatile       {} // "this" is: volatile ThisCVQ*
    void cv_qualifier() const volatile {} // "this" is: const volatile ThisCVQ*
};
```

## this Pointer Ref-Qualiﬁers
The power of ref-qualifiers
https://andreasfertig.blog/2022/07/the-power-of-ref-qualifiers/
```
struct RefQualifiers {
    std::string s;
    RefQualifiers(const std::string& ss = "The nameless one.") : s(ss) {}
    // Normal version.
    void func() &  { std::cout << "Accessed on normal instance "    << s << std::endl; }
    // Rvalue version.
    void func() && { std::cout << "Accessed on temporary instance " << s << std::endl; }
    const std::string& still_a_pointer() &  { return this->s; }
    const std::string& still_a_pointer() && { this->s = "Bob"; return this->s; }
};

RefQualifiers rf("Fred");
rf.func();              // Output:  Accessed on normal instance Fred
RefQualifiers{}.func(); // Output:  Accessed on temporary instance The nameless one
```

# 33 Smart Pointers

Unique ownership

std::unique_ptr

You can transfer ownership of the contents of a smart pointer to another pointer by using std::move, which will
cause the original smart pointer to point to nullptr.
```
// 1. std::unique_ptr
std::unique_ptr<int> ptr = std::make_unique<int>();
// Change value to 1
*ptr = 1;
// 2. std::unique_ptr (by moving 'ptr' to 'ptr2', 'ptr' doesn't own the object anymore)
std::unique_ptr<int> ptr2 = std::move(ptr);
int a = *ptr2; // 'a' is 1
int b = *ptr;  // undefined behavior! 'ptr' is 'nullptr'
               // (because of the move command above)
```


## 33.2 Sharing ownership
std::shared_ptr 

## 33.3: Sharing with temporary ownership
std::weak_ptr


## 34 Classes/Structures

Final classes and structs

## 34.3: Access speciﬁers
public: Everyone has access  
protected: Only the class itself, derived classes and friends have access  
private: Only the class itself and friends have access  

Inheritance
```
struct A
{
public:
    int p1;
protected:
    int p2;
private:
    int p3;
};
//Make B inherit publicly (default) from A
struct B : A
{
};
```
```
struct B : public A // or just `struct B : A`
{
    void foo()
    {
        p1 = 0; //well formed, p1 is public in B
        p2 = 0; //well formed, p2 is protected in B
        p3 = 0; //ill formed, p3 is private in A
    }
};
B b;
b.p1 = 1; //well formed, p1 is public
b.p2 = 1; //ill formed, p2 is protected
b.p3 = 1; //ill formed, p3 is inaccessible
```
```
struct B : private A
{
    void foo()
    {
        p1 = 0; //well formed, p1 is private in B
        p2 = 0; //well formed, p2 is private in B
        p3 = 0; //ill formed, p3 is private in A
    }
};
B b;
b.p1 = 1; //ill formed, p1 is private
b.p2 = 1; //ill formed, p2 is private
b.p3 = 1; //ill formed, p3 is inaccessible
```
```
struct B : protected A
{
    void foo()
    {
        p1 = 0; //well formed, p1 is protected in B
        p2 = 0; //well formed, p2 is protected in B
        p3 = 0; //ill formed, p3 is private in A
    }
};
B b;
b.p1 = 1; //ill formed, p1 is protected
b.p2 = 1; //ill formed, p2 is protected
b.p3 = 1; //ill formed, p3 is inaccessible
```

## 34.5: Friendship
```
class Animal{
private:
    double weight;
    double height;
public:
    friend void printWeight(Animal animal);
    friend class AnimalPrinter;
    // A common use for a friend function is to overload the operator<< for streaming.
    friend std::ostream& operator<<(std::ostream& os, Animal animal);
};
void printWeight(Animal animal)
{
    std::cout << animal.weight << "\n";
}
class AnimalPrinter
{
public:
    void print(const Animal& animal)
    {
        // Because of the `friend class AnimalPrinter;" declaration, we are
        // allowed to access private members here.
        std::cout << animal.weight << ", " << animal.height << std::endl;
    }
}
std::ostream& operator<<(std::ostream& os, Animal animal)
{
    os << "Animal height: " << animal.height << "\n";
    return os;
}
int main() {
    Animal animal = {10, 5};
    printWeight(animal);
    AnimalPrinter aPrinter;
    aPrinter.print(animal);
    std::cout << animal;
}
```

# Virtual Inheritance
```
class A { ... };
class B : public virtual A { ... };
class C : public virtual A { ... };
class D : public B, public C { ... };
```
A will reside in most derived class  
\# "most" refers to the immediate derived class that directly inherits from the virtual base class.  
It is D, the most derived class is the one that is at the top of the inheritance hierarchy and directly inherits from multiple virtual base classes, which in this case is class D.

### diamond problem

To prevent ambiguity from inheritance of A from B and C.  
The compiler may add a hidden pointer or offset in D to access the shared instance of A. This ensures that when an object of D is created, there is a single instance of A that is shared among both B and C.

Since virtual base resides only in most derived object, there will be only one instance of A in D.
### 只會有一個A的實體存在於實例化的D(the most derived class)

# 34.7: Private inheritance
Private inheritance is useful when it is required to restrict the public interface of the class:  
Note: 限制public interface繼承
```
class A {
public:
    int move();
    int turn();
};
class B : private A {
public:
    using A::turn;
};
B b;
b.move();  // compile error
b.turn();  // OK
```
`using A::turn; ` declaration using A::turn; inside B brings the turn() function from A into the scope of B as a public member. This allows instances of B to call turn() directly.

在Private inheritance中，其他的protected, public method都會被繼承成private

## static class members Accessing
When accessing static class members, the :: operator is used, but on the name of the class instead of an instance of it.
### 宣告於Class外部(type class_name::static_member)。調用如其他成員。
```
struct SomeStruct {
  int a;
  int b;
  void foo() {}
  static int c;
  static void bar() {}
};
int SomeStruct::c;
SomeStruct var;
SomeStruct* p = &var;

SomeStruct::c = 5;
// Accessing static member variable c in struct SomeStruct, through var and p.
var.a = var.c;
var.b = p->c;
// Calling a static member function.
SomeStruct::bar();
var.bar();
p->bar();
```

## Unnamed struct/class
unnamed struct is allowed (type has no name)
```
void foo()
{
    struct /* No name */ {
        float x;
        float y;
    } point;
   
    point.x = 42;
}

struct Circle
{
    struct /* No name */ {
        float x;
        float y;
    } center; // but a member name
    float radius;
};


Circle circle;
circle.center.x = 42.f;
```

* lamdba can be seen as a special unnamed struct.
* decltype allows to retrieve the type of unnamed struct:
```
decltype(circle.point) otherPoint;
```
# 34.12: Static class members

```
class Example {
    static int num_instances;      // Static data member (static member variable).
    int i;                         // Non-static member variable.
  public:
    static std::string static_str; // Static data member (static member variable).
    static int static_func();      // Static member function.
    // Non-static member functions can modify static member variables.
    Example() { ++num_instances; }
    void set_str(const std::string& str);
};
int         Example::num_instances;
std::string Example::static_str = "Hello.";
// ...
Example one, two, three;
// Each Example has its own "i", such that:
//  (&one.i != &two.i)
//  (&one.i != &three.i)
//  (&two.i != &three.i).
// All three Examples share "num_instances", such that:


GoalKicker.com – C++ Notes for Professionals 188

//  (&one.num_instances == &two.num_instances)
//  (&one.num_instances == &three.num_instances)
//  (&two.num_instances == &three.num_instances)
```


# 34.13: Multiple Inheritance

Worng in main:
```
class base1
{
  public:
     void funtion( )
     { //code for base1 function }  
};
class base2
{
    void function( )
     { // code for base2 function }
};
class derived : public base1, public base2
{
   
};
int main()
{
    derived obj;
   
  // Error because compiler can't figure out which function to call
  //either function( ) of base1 or base2 .  
    obj.function( )  
}
```
fixed:
```
int main()
{
    obj.base1::function( );  // Function of class base1 is called.
    obj.base2::function( );  // Function of class base2 is called.
}
```

## 34.14: Non-static member functions
call cv-qualified member method 
```
struct CVQualifiers {
    void func()                   {} // 1: Instance is non-cv-qualified.
    void func() const             {} // 2: Instance is const.
    void cv_only() const volatile {}
};
CVQualifiers       non_cv_instance;
const CVQualifiers      c_instance;
non_cv_instance.func(); // Calls #1.
c_instance.func();      // Calls #2.
non_cv_instance.cv_only(); // Calls const volatile version.
c_instance.cv_only();      // Calls const volatile version.
```


```
#include <iostream>

class MyClass {
public:
    void process() & {
        std::cout << "Called on lvalue object" << std::endl;
    }

    void process() && {
        std::cout << "Called on rvalue object" << std::endl;
    }
};

int main() {
    MyClass obj1;
    obj1.process();             // Calls the lvalue version

    MyClass{}.process();        // Calls the rvalue version, `don't need instance`
    
    // MyClass obj2;
    // std::move(obj2).processData();  

    return 0;
}
```
In "&& finction in class", both methods will correctly call the process() function.
```
MyClass{}.process();
```
The first line creates an object obj2 of type MyClass. Then, in the second line, std::move(obj2) is used to cast obj2 to an rvalue reference, indicating that you are willing to move its resources, if any. This is followed by calling the process() member function on the result of std::move(obj2). The exact behavior of process() will depend on its implementation.

```
// MyClass obj2;
// std::move(obj2).processData();  
```
Second, a temporary object of type MyClass is created using the syntax MyClass{}, which creates a prvalue (pure rvalue). Then, the process() member function is called directly on this temporary object. The temporary object is constructed on-the-fly and will be destroyed at the end of the expression.


## Function Overloading
---

---
## Return Type in Function Overloading
You `cannot overload` a function based on its `return type`.
```
// WRONG CODE
std::string getValue()
{
  return "hello";
}
int getValue()
{
  return 0;
}
int x = getValue();
```

## Member Function cv-qualiﬁer Overloading
```
void print(int value) {
    std::cout << "Non-const version: " << value << std::endl;
}

void print(const int value) {
    std::cout << "Const version: " << value << std::endl;
}

int main() {
    int x = 5;
    const int y = 10;
    
    print(x); // Calls the non-const version.
    print(y); // Calls the const version.
    
    return 0;
}
```
```
void print(int value) {
    std::cout << "Non-const version: " << value << std::endl;
}

void print(const int value) {
    std::cout << "Const version: " << value << std::endl;
}

int main() {
    int x = 5;
    const int y = 10;
    
    print(x); // Calls the non-const version.
    print(y); // Calls the const version.
    
    return 0;
}
```


# Operator Overloading

Overloading outside of class/struct:
```
//operator+ should be implemented in terms of operator+=
T operator+(T lhs, const T& rhs)
{
    lhs += rhs;
    return lhs;
}
T& operator+=(T& lhs, const T& rhs)
{
    //Perform addition
    return lhs;
}
```

Overloading inside of class/struct:
```
//operator+ should be implemented in terms of operator+=
T operator+(const T& rhs)
{
    *this += rhs;
    return *this;
}
T& operator+=(const T& rhs)
{
    //Perform addition
    return *this;
}
```

## 36.3: Conversion operators

## 36.4: Complex Numbers Revisited

## 36.5: Named operators
You can extend C++ with named operators that are "quoted" by standard C++ operators.

## 36.6: Unary operators
You can overload the 2 unary operators:
* ++foo and foo++
* --foo and foo--
## 36.7: Comparison operators
You can overload all comparison operators:
* == and !=
* > and <
* >= and <=


## 36.8: Assignment operator
If you do not overload the assignment operator for your class/struct, it is automatically generated by the
compiler: the automatically-generated assignment operator performs a "memberwise assignment"

The assignment operator should be overloaded when the simple memberwise assignment is not suitable for your class/struct.  
For example if you need to perform a deep copy of an object.

follow some simple steps.  
* Test for self-assignment.
  * a self-assignment is a needless copy, so it does not make sense to perform it;
  * the next step will not work in the case of a self-assignment.
* Clean the old data.  
  * The old data must be replaced with new ones
  * if the content of the object was destroyed, a self-assignment will fail to perform the copy.
* Copy all members
  * If you overload the assignment operator for your class or your struct, it is not automatically generated by the compiler
* Return `*this`.
  * The operator returns by itself by reference, because it allows chaining (i.e. int b = (a = 6) + 4; //b == 10).
```
//T is some type
T& operator=(const T& other)
{
    //Do something (like copying values)
    return *this;
}
```
Note: other is passed by const&, because the object being assigned should not be changed, and passing by
reference is faster than by value, and to make sure than operator= doesn't modify it accidentally, it is const.


## 36.9:　Function call operator

You can overload the function call operator ():
```
//R -> Return type
//Types -> any different type
R operator()(Type name, Type2 name2, ...)
{
    //Do something
    //return something
}
//Use it like this (R is return type, a and b are variables)
R foo = object(a, b, ...);
```
```
struct Sum
{
    int operator()(int a, int b)
    {
        return a + b;
    }
};
//Create instance of struct
Sum sum;
int result = sum(1, 1); //result == 2
```

# 37: Function Template overloading

* The return type is diﬀerent, or  
\#Note : this `only` work on Function Template, `not work` on normal function.
* The template parameter list is diﬀerent, except for the naming of parameters and the presence of default
arguments (they are not part of the signature)

example 1 of describe 1
```
template <typename T>
T add(T a, T b) {
    return a + b;
}

template <typename T>
bool compare(T a, T b) {
    return a == b;
}

int main() {
    int result = add(5, 10); // Calls the add function template with return type int
    bool isEqual = compare(5, 10); // Calls the compare function template with return type bool
    return 0;
}
```
example 1 of describe 2
```
template<typename T>
void f(T*) { }
template<typename T>
void f(T) { }
```
or
```
template<typename T>
void f(T (*x)[sizeof(T) + sizeof(T)]) { }
template<typename T>
void f(T (*x)[2 * sizeof(T)]) { }

int main() {
    int arr1[8];
    char arr2[16];

    f(&arr1);  // Calls f<T>(T (*x)[sizeof(T) + sizeof(T)])
    f(&arr2);  // Calls f<T>(T (*x)[2 * sizeof(T)])

    return 0;
}
//if array size (like arr1[8]) is not match any function template
//compiler will not find a matching function template
```

## 38: Virtual Member Functions
final speciﬁer which forbids method overriding if appeared in method signature:
```
class Base {
public:
    virtual void foo() {
        std::cout << "Base::Foo\n";
    }
};
class Derived1 : public Base {
public:
    // Overriding Base::foo
    void foo() final {
        std::cout << "Derived1::Foo\n";
    }
};
class Derived2 : public Derived1 {
public:
    // Compilation error: cannot override final method
    virtual void foo() {
        std::cout << "Derived2::Foo\n";
    }
};
```

## 38.2: Using override keyword

Note that override is not a keyword, but a special identiﬁer which only may appear in function signatures. In all
other contexts override still may be used as an identiﬁer:

Without override:
```
#include <iostream>
struct X {
    virtual void f() { std::cout << "X::f()\n"; }
};
struct Y : X {
    // Y::f() will not override X::f() because it has a different signature,
    // but the compiler will accept the code (and silently ignore Y::f()).
    virtual void f(int a) { std::cout << a << "\n"; }
};
```
With override:
```
#include <iostream>
struct X {
    virtual void f() { std::cout << "X::f()\n"; }
};
struct Y : X {
    // The compiler will alert you to the fact that Y::f() does not
    // actually override anything.
    virtual void f(int a) override { std::cout << a << "\n"; }
};
```

## 38.3: Virtual vs non-virtual member functions
with virtual
```
#include <iostream>
struct X {
    virtual void f() { std::cout << "X::f()\n"; }
};
struct Y : X {
    // Specifying virtual again here is optional
    // because it can be inferred from X::f().
    virtual void f() { std::cout << "Y::f()\n"; }
};
void call(X& a) {
    a.f();
}
int main() {
    X x;
    Y y;
    call(x); // outputs "X::f()"
    call(y); // outputs "Y::f()"
}  
```
without virtual(it call function from parent instance)
```
#include <iostream>
struct X {
   void f() { std::cout << "X::f()\n"; }
};
struct Y : X {
   void f() { std::cout << "Y::f()\n"; }
};
void call(X& a) {
    a.f();
}
int main() {
    X x;
    Y y;
    call(x); // outputs "X::f()"
    call(y); // outputs "X::f()"
}
```


## 38.4 Behaviour of virtual functions in constructors and destructors
```
#include <iostream>
using namespace std;
class base {
public:
    base() { f("base constructor"); }
    ~base() { f("base destructor"); }
    virtual const char* v() { return "base::v()"; }
    void f(const char* caller) {
        cout << "When called from " << caller << ", "  << v() << " gets called.\n";
    }        
};
class derived : public base {
public:
    derived() { f("derived constructor"); }
    ~derived() { f("derived destructor"); }
    const char* v() override { return "derived::v()"; }
};
int main() {
     derived d;
}
```
Output:
```
When called from base constructor, base::v() gets called.
When called from derived constructor, derived::v() gets called.
When called from derived destructor, derived::v() gets called.
When called from base destructor, base::v() gets called.
```

## 38.5: Pure virtual functions
We can also specify that a virtual function is pure virtual (abstract), by appending = 0 to the declaration.  
"Have Pure virtual functions" = "abstract object"
```
struct Abstract {
    virtual void f() = 0;
};
struct Concrete {
    void f() override {}
};
Abstract a; // Error.
Concrete c; // Good.
```


Abstract 可以在外部被define. 但Abstract object 仍舊為Abstract不可實例化，該definition 會作為defualt，由derive決定是否要使用defualt或是override。
```
struct DefaultAbstract {
    virtual void f() = 0;
};
void DefaultAbstract::f() {}
struct WhyWouldWeDoThis : DefaultAbstract {
    void f() override { DefaultAbstract::f(); }
};
```

一般來說，default的constructor destructor會自動處理所有member創見與刪除
```
struct Interface {
      virtual ~Interface() = 0;
  };
  Interface::~Interface() = default;
  struct Implementation : Interface {};
  // ~Implementation() is automatically defined by the compiler if not explicitly
  //  specified, meeting the "must be defined before instantiation" requirement.
```

# Inline functions
An inline function can be multiply deﬁned without
violating the One Deﬁnition Rule, and can therefore be deﬁned in a header with external linkage

```
inline int add(int x, int y)
{
    return x + y;
}
```

## 39.2: Member inline functions
預設C++編譯會將class/struct內的function為inline但不絕對保證會轉換成inline
```
// header (.hpp)    
struct A
{
    void i_am_inlined()
    {
    }
};
struct B
{
    void i_am_NOT_inlined();
};
// source (.cpp)    
void B::i_am_NOT_inlined()
{
}
```

## 39.3: What is function inlining?
```
inline int add(int x, int y)
{
    return x + y;
}
int main()
{
    int a = 1, b = 2;
    int c = add(a, b);
}
```
will be compiled to like:
```
int main()
{
    int a = 1, b = 2;
    int c = a + b;
}
```
### 39.4: Non-member inline function declaration
```
inline int add(int x, int y);
```

# 40: Special Member Functions
## Default Constructor
A default constructor is a type of constructor that requires no parameters
```
class C{
    int i;
public:
    // the default constructor definition
    C()
    : i(0){ // member initializer list -- initialize i to 0
        // constructor function body -- can do more complex things here
    }
};
C c1; // calls default constructor of C to create object c1
C c2 = C(); // calls default constructor explicitly
C c3(); // ERROR: this intuitive version is not possible due to "most vexing parse"
C c4{}; // but in C++11 {} CAN be used in a similar way
C c5[2]; // calls default constructor for both array elements
C* c6 = new C[2]; // calls default constructor for both array elements
```

Or, Another way to satisfy the "no parameters" requirement is for the developer to provide default values for all
parameters:
```
class D{
    int i;
    int j;
public:
    // also a default constructor (can be called with no parameters)
    D( int i = 0, int j = 42 )
    : i(i), j(j){
    }
};
D d; // calls constructor of D with the provided default values for the parameters
```
### implicitly default constructor
Under some circumstances (i.e., the developer provides no constructors and there are no other disqualifying
conditions), the compiler implicitly provides an empty default constructor:
```
class C{
    std::string s; // note: members need to be default constructible themselves
};
C c1; // will succeed -- C has an implicitly defined default constructor
```


## If default value not provide to default constructor, that cause error
```
class C{
    int i;
public:
    C( int i ) : i(i){}
};
C c1; // Compile ERROR: C has no (implicitly defined) default constructor
```
Two way to fix it:
* add `member initialization list`
```
class C{
    int i;
public:
    C( int i ) : i(i){}
};
C c1(0); // Compile ERROR: C has no (implicitly defined) default constructor
```
* add `constructor's parameter list`
```
class C{
    int i;
public:
    C( int i =0 ) : i(i){}
};
C c1; // Compile ERROR: C has no (implicitly defined) default constructor
```

## delete 
a developer can also use the delete keyword to prevent the compiler from providing a default
constructor
```
class C{
    int i;
public:
    // default constructor is explicitly deleted
    C() = delete;
};
C c1; // Compile ERROR: C has its default constructor deleted
```
## default
developer ask compiler to explicitly provide a default constructor
```
class C{
    int i;
public:
    // does have automatically generated default constructor (same as implicit one)
    C() = default;
    C( int i ) : i(i){}
};
C c1; // default constructed, not value in i, maybe random value in memory space.
C c2( 1 ); // constructed with the int taking constructor
```

## 40.2: Destructor
use delete to call "Destructor"
```
class C{
    int* is;
    string s;
public:
    C()
    : is( new int[10] ){
    }
    ~C(){  // destructor definition
        delete[] is;
    }
};
class C_child : public C{
    string s_ch;
public:
    C_child(){}
    ~C_child(){} // child destructor
};
void f(){
    C c1; // calls default constructor
    C c2[2]; // calls default constructor for both elements
    C* c3 = new C[2]; // calls default constructor for both array elements
    C_child c_ch;  // when destructed calls destructor of s_ch and of C base (and in turn s)
    delete[] c3; // calls destructors on c3[0] and c3[1]
} // automatic variables are destroyed here -- i.e. c1, c2 and c_ch
```

## Copy and swap
If you're writing a class that manages resources (memory space(new []), dynamic allocation), you need to implement all the special member functions.   
(special member functions: destructor, copy constructor, copy assignment operator, move constructor, and move assignment operator).

```
person(const person &other)
    : name(new char[std::strlen(other.name) + 1])
    , age(other.age)
{
    std::strcpy(name, other.name);
}
person& operator=(person const& rhs) {
    if (this != &other) {
        delete [] name;
        name = new char[std::strlen(other.name) + 1];
        std::strcpy(name, other.name);
        age = other.age;
    }
    return *this;
}
```
if there is no cutomize the special member functions. The dynamic allocation (new char[]) in copy assignment will simply copy the values of the name and age members using a shallow copy.  
This may cause double deletion or memory leaks if the same object is assigned multiple times or object destructed.

## 40.4: Implicit Move and Copy
Bear in mind that declaring a destructor inhibits the compiler from generating implicit move constructors and move
assignment operators.

When a class has a user-defined destructor, it indicates that the class may have ownership over resources that need to be properly released. In such cases, the compiler cannot generate default move operations because it doesn't know how to transfer the ownership of these resources correctly.

```
class Movable {
public:
    virtual ~Movable() noexcept = default;
    //    compiler won't generate these unless we tell it to
    //    because we declared a destructor
    Movable(Movable&&) noexcept = default;
    Movable& operator=(Movable&&) noexcept = default;
    //    declaring move operations will suppress generation
    //    of copy operations unless we explicitly re-enable them
    Movable(const Movable&) = default;
    Movable& operator=(const Movable&) = default;
};
```

if write `~ClassName() = default;` there is no inhibits.  
if write `~ClassName(){}` there will have inhibits.


## Overload Name Hiding & Importing
term: `"name hiding"` or `"function hiding"`  
When a base class provides a set of overloaded functions, and a derived class adds another overload to the set, this
hides all of the overloads provided by the base class.

```
struct HiddenBase {
    void f(int) { std::cout << "int" << std::endl; }
    void f(bool) { std::cout << "bool" << std::endl; }
    void f(std::string) { std::cout << "std::string" << std::endl; }
};
struct HidingDerived : HiddenBase {
    void f(float) { std::cout << "float" << std::endl; }
};
// ...
HiddenBase hb;
HidingDerived hd;
std::string s;
hb.f(1);    // Output:  int
hb.f(true); // Output:  bool
hb.f(s);    // Output:  std::string;
hd.f(1.f);  // Output:  float
hd.f(3);    // Output:  float
hd.f(true); // Output:  float
hd.f(s);    // Error: Can't convert from std::string to float.
```
Use `using` to solve.
```
struct HiddenBase {
    void f(int) { std::cout << "int" << std::endl; }
    void f(bool) { std::cout << "bool" << std::endl; }
    void f(std::string) { std::cout << "std::string" << std::endl; }
};

struct HidingDerived : HiddenBase {
    using HiddenBase::f; // Bring base class overloads into scope

    void f(float) { std::cout << "float" << std::endl; }
};

int main() {
    HidingDerived obj;
    obj.f(42);             // Calls HiddenBase::f(int)
    obj.f(true);           // Calls HiddenBase::f(bool)
    obj.f("Hello");        // Calls HiddenBase::f(std::string)
    obj.f(3.14f);          // Calls HidingDerived::f(float)
    
    return 0;
}
```

## 41.5:Const Correctness
const class can only call the const member function.  
Non-const class call both.

Also class need to declare a const member function not only in return type.
```
class ConstIncorrect {
    Field fld;
  public:
    ConstIncorrect(const Field& f) : fld(f) {}     // Modifies.
    const Field& get_field()       { return fld; } // Doesn't modify; should be const.
    void set_field(const Field& f) { fld = f; }    // Modifies.
    void do_something(int i) {                     // Modifies.
        fld.insert_value(i);
    }
    void do_nothing()        { }                   // Doesn't modify; should be const.
};
class ConstCorrect {
    Field fld;
  public:
    ConstCorrect(const Field& f) : fld(f) {}       // Not const: Modifies.
    const Field& get_field() const { return fld; } // const: Doesn't modify.
    void set_field(const Field& f) { fld = f; }    // Not const: Modifies.
    void do_something(int i) {                     // Not const: Modifies.
        fld.insert_value(i);
    }
    void do_nothing() const  { }                   // const: Doesn't modify.
};
// ...
const ConstIncorrect i_cant_do_anything(make_me_a_field());
// Now, let's read it...
Field f = i_cant_do_anything.get_field();
  // Error: Loses cv-qualifiers, get_field() isn't const.
i_cant_do_anything.do_nothing();
  // Error: Same as above.
// Oops.
const ConstCorrect but_i_can(make_me_a_field());
// Now, let's read it...
Field f = but_i_can.get_field(); // Good.
but_i_can.do_nothing();          // Good.
```
# constant member function
```
#include <iostream>
#include <map>
#include <string>
using namespace std;
class A {
public:
    map<string, string> * mapOfStrings;
public:
    A() {
        mapOfStrings = new map<string, string>();
    }
    void insertEntry(string const & key, string const & value) const {
        (*mapOfStrings)[key] = value;             // This works? Yes it does.
        delete mapOfStrings;                      // This also works
        mapOfStrings = new map<string, string>(); // This * does * not work
    }
    void refresh() {
        delete mapOfStrings;
        mapOfStrings = new map<string, string>(); // Works as refresh is non const function
    }
    void getEntry(string const & key) const {
        cout << mapOfStrings->at(key);
    }
};
int main(int argc, char* argv[]) {
    A var;
    var.insertEntry("abc", "abcValue");
    var.getEntry("abc");
    getchar();
    return 0;
}
```
## Note: 
In a const member function (`void insertEntry(string const & key, string const & value) const`), the this pointer is const, meaning you cannot modify the object itself through the this pointer ("this" pointer to object itself).

The mapOfStrings pointer is a member variable of the class, and it exists within each instance of the class object. The constness of a member function affects the accessibility of the this pointer, which points to the object on which the member function is invoked.

const member function 會影響本身class的const。也就是this是指向本身object的pointer，而const會使所有需要通過this改變的東西都不能改變。
例如void insertEntry中mapOfStrings前兩行可以work的修改是因為透過mapOfStrings本身的pointer完成的，但是new mapOfStrings這個行為會需要透過"this"來修改，所以被const禁止了。


## const is `not` affect Static member
Static member variables are associated with the class itself rather than individual objects of the class. They exist independently of any specific object and are shared among all instances of the class.

In the context of const member functions, static member variables can be accessed and modified even within const member functions because they are not tied to a particular object. 


# C++ 43 Containers

## C++ Containers Flowchart
The flowchart determine which container is proper to use:  

https://stackoverflow.com/questions/471432/in-which-scenario-do-i-use-a-particular-stl-container/22671607#22671607


# 44: Namespaces

## 44.1: What are namespaces?
A C++ namespace is a collection of C++ entities (functions, classes, variables), whose names are preﬁxed by the
name of the namespace. When writing code within a namespace, named entities belonging to that namespace
need not be preﬁxed with the namespace name, but entities outside of it must use the fully qualiﬁed name. The
fully qualiﬁed name has the format <namespace>::<entity>. Example:
```
namespace Example
{
  const int test = 5;
  const int test2 = test + 12; //Works within `Example` namespace
}
const int test3 = test + 3; //Fails; `test` not found outside of namespace.
const int test3 = Example::test + 3; //Works; fully qualified name used.
```
---
不使用namespace也是可以接受的，但:  
it is acceptable to organize your code primarily within classes in C++. In fact, C++ is an object-oriented programming language, and classes are an essential component of its structure.

However, it's worth noting that even if you organize your code within classes, you may still need to use namespaces to avoid naming conflicts and provide better code organization. Namespaces help prevent `naming collisions` between different parts of your code and provide a way to group related code elements together. It is generally a good practice to use namespaces to organize your code, even within classes.

## Extending namespaces
```
namespace Foo
{
    void bar() {}
}
//some other stuff
namespace Foo
{
    void bar2() {}
}
```
## Using directive
```
namespace Foo
{
    void bar() {}
    void baz() {}
}
//Have to use Foo::bar()
Foo::bar();
//Import Foo
using namespace Foo;
bar(); //OK
baz(); //OK
```
---
## Bad example
A word of caution: `using namespaces in header ﬁles is seen as bad style` in most cases. If this is done, the
namespace is imported in every ﬁle that includes the header. Since there is no way of "un-using" a namespace, this
can lead to namespace pollution (more or unexpected symbols in the global namespace) or, worse, conﬂicts. See
this example for an illustration of the problem:
```
namespace Foo
{
    class C;
}
/***** bar.h *****/
namespace Bar
{
    class C;
}
/***** baz.h *****/
#include "foo.h"
using namespace Foo;
/***** main.cpp *****/
#include "bar.h"
#include "baz.h"
using namespace Bar;
C c; // error: Ambiguity between Bar::C and Foo::C
```
---

## Making namespaces.
```
//Creates namespace foo
namespace Foo
{
    //Declares function bar in namespace foo
    void bar() {}
}
```
this can work
```
namespace A
{
    namespace B
    {
        namespace C
        {
            void bar() {}
        }
    }
}
```
same as:
```
namespace A::B::C
{
    void bar() {}
}
```

## Section 44.6: Unnamed/anonymous namespaces
An unnamed namespace can be used to ensure names have internal linkage (can only be referred to by the current
translation unit). Such a namespace is deﬁned in the same way as any other namespace, but without the name:
```
namespace {
    int foo = 42;
}
```
foo is only visible in the translation unit in which it appears.

It is recommended to never use unnamed namespaces in header ﬁles as this gives a version of the content for
every translation unit it is included in. This is especially important if you deﬁne non-const globals.
```
// foo.h
namespace {
    std::string globalString;
}
// 1.cpp
#include "foo.h" //< Generates unnamed_namespace{1.cpp}::globalString ...
globalString = "Initialize";
// 2.cpp
#include "foo.h" //< Generates unnamed_namespace{2.cpp}::globalString ...
std::cout << globalString; //< Will always print the empty string
````

## Namespace alias
```
namespace AReallyLongName {
    namespace AnotherReallyLongName {
        int foo();
        int bar();
        void baz(int x, int y);
    }
}
void qux() {
    namespace N = AReallyLongName::AnotherReallyLongName;
    N::baz(N::foo(), N::bar());
}
```

## Section 46.2: Redeclaring, avoid name hiding
```
class Base {
public:
    void func() {
        std::cout << "Base::func()" << std::endl;
    }
};

class Derived : public Base {
public:
    void func() {
        std::cout << "Derived::func()" << std::endl;
    }

    // Bring the base class member into the derived class's scope
    using Base::func;
};
```
## Inheriting constructors
a derived class can inherit the constructors of its base class using a using-declaration
```
struct Base {
    Base(int x, const char* s);
};
struct Derived1 : Base {
    Derived1(int x, const char* s) : Base(x, s) {}
};
struct Derived2 : Base {
    using Base::Base;
};
int main() {
    Derived1 d1(42, "Hello, world");
    Derived2 d2(42, "Hello, world");
}
```

## std library
std::string

std::string_view

std::wstring

str.substr

std::mismatch which returns the ﬁrst mismatching pair

std::array
|Parameter|Deﬁnition|
|-|-|
|class T |Speciﬁes the data type of array members|
|std::size_t N | Speciﬁes the number of members in the array|

std::array<T, N>   
`T` using `scalar type` (fundamental data type int, float ...etc),  
`non-scalar` for element type in container

```
// 1) Using aggregate-initialization
std::array<int, 3> a{ 0, 1, 2 };

std::array<int, 3> a = { 0, 1, 2 };
// 2) Using the copy constructor
std::array<int, 3> a{ 0, 1, 2 };
std::array<int, 3> a2(a);

std::array<int, 3> a2 = a;
// 3) Using the move constructor
std::array<int, 3> a = std::array<int, 3>{ 0, 1, 2 };
```

non-scalar: 
```
struct A { int values[3]; }; // An aggregate type
// 1) Using aggregate initialization with brace elision
// It works only if T is an aggregate type!
std::array<A, 2> a{ 0, 1, 2, 3, 4, 5 };
std::array<A, 2> a = { 0, 1, 2, 3, 4, 5 };
// 2) Using aggregate initialization with brace initialization of sub-elements
std::array<A, 2> a{ A{ 0, 1, 2 }, A{ 3, 4, 5 } };
std::array<A, 2> a = { A{ 0, 1, 2 }, A{ 3, 4, 5 } };
// 3)
std::array<A, 2> a{{ { 0, 1, 2 }, { 3, 4, 5 } }};
std::array<A, 2> a = {{ { 0, 1, 2 }, { 3, 4, 5 } }};
// 4) Using the copy constructor
std::array<A, 2> a{ 1, 2, 3 };
std::array<A, 2> a2(a);
std::array<A, 2> a2 = a;
// 5) Using the move constructor
std::array<A, 2> a = std::array<A, 2>{ 0, 1, 2, 3, 4, 5 };
```
on complex struct
```
struct A {
  int values[3];
  float floatValue;
  std::string stringValue;
};

std::array<A, 2> a{
  { {1, 2, 3}, 1.23f, "Hello" },
  { {4, 5, 6}, 4.56f, "World" }
};
```
## Iterating through the Array
```
int main() {
     std::array<int, 3> arr = { 1, 2, 3 };
     for (auto i : arr)
         cout << i << '\n';
}
```

## std::vector

iterator invalidation rules 

Traditional `std::vector<char>` storing 8 Boolean values:  
```
std::vector<char> trad_vect = {true, false, false, false, true, false, true, true};
```
```
[0,0,0,0,0,0,0,1], [0,0,0,0,0,0,0,0], [0,0,0,0,0,0,0,0], [0,0,0,0,0,0,0,0],
[0,0,0,0,0,0,0,1], [0,0,0,0,0,0,0,0], [0,0,0,0,0,0,0,1], [0,0,0,0,0,0,0,1]
```
Specialized `std::vector<bool>` storing 8 Boolean values:
```
std::vector<bool> optimized_vect = {true, false, false, false, true, false, true, true};
```
```
[1,0,0,0,1,0,1,1]
```
```
std::vector<bool> boolVector(5, true);
std::vector<bool>::reference ref = boolVector[2];
ref = false;  // Modify the value at index 2
bool value = boolVector[2];  // Access the value at index 2
```
```
std::vector<bool> boolVector(5, true);

for (size_t i = 0; i < boolVector.size(); ++i) {
  std::vector<bool>::reference ref = boolVector[i];
  // Use the reference like a boolean variable
  if (ref) {
    std::cout << "Element at index " << i << " is true\n";
  } else {
    std::cout << "Element at index " << i << " is false\n";
  }
}
```


50: std::map

51: std::optional

52: std::function: To wrap any
element that is callable
# std::function
### callback function: 
```
#include <iostream>

// Define a function type for the callback
typedef void (*CallbackFunction)(int);

// Function that takes a callback function pointer as an argument
void performCallback(CallbackFunction callback) {
  // Perform some task
  std::cout << "Performing task..." << std::endl;

  // Call the callback function
  callback(42);
}

// Callback function to be passed as an argument
void myCallback(int value) {
  std::cout << "Callback called with value: " << value << std::endl;
}

int main() {
  // Pass the callback function to performCallback
  performCallback(myCallback);

  return 0;
}
```

using std::function
```
#include <iostream>
#include <functional>
std::function<void(int , const std::string&)> myFuncObj;
void theFunc(int i, const std::string& s)
{
    std::cout << s << ": " << i << std::endl;
}
int main(int argc, char *argv[])
{
    myFuncObj = theFunc;
    myFuncObj(10, "hello world");
}
```
```
#include <iostream>
#include <functional>

// Function with a function pointer argument
void printNumber(int number) {
    std::cout << "Number: " << number << std::endl;
}

int main() {
    // Declare a std::function object that can hold a function pointer
    std::function<void(int)> func;

    // Assign the function pointer to the std::function object
    func = &printNumber;

    // Call the function through the std::function object
    func(42);

    return 0;
}
```

## Section 52.6: `function` overhead
std::function can cause signiﬁcant overhead. Because std::function has [value semantics][1], it must copy or
move the given callable into itself. But since it can take callables of an arbitrary type, it will frequently have to
allocate memory dynamically to do this.

53: std::forward_list

54: std::pair

55: std::atomics

56: std::variant 

56: std::variant 

`std::variant` is a type-safe union-like class introduced in C++17 that can store values of different types. It provides a way to represent a value that can be one of several alternative types at runtime. The actual stored type is determined at runtime, and you can safely access and manipulate the value stored in the variant using type-safe operations.

Here's a simple explanation of how to use std::variant:

Include the <variant> header to use the std::variant class.

Define a std::variant object with the desired alternative types. For example:


```
std::variant<int, float, std::string> myVariant;
```
Assign a value to the variant using the std::variant constructor or the std::variant::operator= assignment operator. The type of the assigned value must match one of the alternative types defined in the variant. For example:

```
myVariant = 42;  // Assign an int value
```
Use std::visit to perform operations on the variant. std::visit allows you to handle each alternative type with a separate function or lambda expression. For example:

```
std::visit([](const auto& value) {
    std::cout << "Variant value: " << value << std::endl;
}, myVariant);
```
This will invoke the lambda expression with the value stored in the variant.

You can also use std::get to extract the value stored in the variant if you know its type. However, be cautious as accessing the value using the wrong type can result in undefined behavior. To check the type of the variant, you can use std::holds_alternative function.

std::variant provides various member functions and utilities to work with the variant, such as std::variant::index, std::variant::valueless_by_exception, and others.

57: std::iomanip

58: std::any 

59: std::set and std::multiset 

60: std::integer_sequence

Using std::unordered_map  

62: Standard Library Algorithms 


# 64: Inline variables
## Deﬁning a static data member in the class
deﬁnition

Before C++17, if you had multiple definitions of an inline static variable in different translation units, each translation unit would have its own independent copy of the variable.
```
// warning: not thread-safe...
class Foo {
  public:
    Foo() { ++num_instances; }
    ~Foo() { --num_instances; }
    inline static int num_instances = 0;
};
```


# 65: Random number generation
## cryptography std::random_device
```
#include <iostream>
#include <random>
int main()
{
   std::random_device crypto_random_generator;
   std::uniform_int_distribution<int> int_distribution(0,9);
   
   int actual_distribution[10] = {0,0,0,0,0,0,0,0,0,0};
   
   for(int i = 0; i < 10000; i++) {
       int result = int_distribution(crypto_random_generator);
       actual_distribution[result]++;
   }
   for(int i = 0; i < 10; i++) {
       std::cout << actual_distribution[i] << " ";
   }
   
   return 0;
}
```
## 65.3: Using the generator for multiple distributions
```
include <iostream>
#include <random>
int main()
{
   std::default_random_engine pseudo_random_generator;
   std::uniform_int_distribution<int> int_distribution(0, 9);
   std::uniform_real_distribution<float> float_distribution(0.0, 1.0);
   std::discrete_distribution<int> rigged_dice({1,1,1,1,1,100});
   
   std::cout << int_distribution(pseudo_random_generator) << std::endl;
   std::cout << float_distribution(pseudo_random_generator) << std::endl;
   std::cout << (rigged_dice(pseudo_random_generator) + 1) << std::endl;
   
   return 0;
}
```

# 66: Date and time using <chrono> header

# 67: Sorting

## 67.1: Sorting and sequence containers
std::sort

The std::sort algorithm in C++ uses a variant of the QuickSort algorithm called IntroSort, which combines QuickSort, HeapSort, and InsertionSort. The implementation of std::sort is typically based on a hybrid approach that uses different sorting algorithms depending on the input size and other factors.


# Chapter 68: Enumeration

## 68.1: Iteration over an enum
```
enum E {
    Begin,
    E1 = Begin,
    E2,
    // ..
    En,
    End
};
for (E e = E::Begin; e != E::End; ++e) {
    // Do job with e
}
```

with enum class, operator ++ has to be implemented:
```
E& operator ++ (E& e)
{
    if (e == E::End) {
        throw std::out_of_range("for E& operator ++ (E&)");
    }
    e = E(static_cast<std::underlying_type<E>::type>(e) + 1);
    return e;
}
```
In container
```
enum E {
    E1 = 4,
    E2 = 8,
    // ..
    En
};
std::vector<E> build_all_E()
{
    const E all[] = {E1, E2, /*..*/ En};
   
    return std::vector<E>(all, all + sizeof(all) / sizeof(E));
}
std::vector<E> all_E = build_all_E();
for (std::vector<E>::const_iterator it = all_E.begin(); it != all_E.end(); ++it) {
     E e = *it;
    // Do job with e;
}
```
or std::initializer_list and a simpler syntax:
```
enum E {
    E1 = 4,
    E2 = 8,
    // ..
    En
};
constexpr std::initializer_list<E> all_E = {E1, E2, /*..*/ En};
for (auto e : all_E) {
    // Do job with e
}

```

## Scoped enums
These are enumerations whose members must be qualiﬁed
with enumname::membername
```
enum class rainbow {
    RED,
    ORANGE,
    YELLOW,
    GREEN,
    BLUE,
    INDIGO,
    VIOLET
};

rainbow r = rainbow::INDIGO;
```
enum classes cannot be implicitly converted to ints without a cast. So int x = rainbow::RED is `invalid`.

Scoped enums also allow you to specify the underlying type, which is the type used to represent a member. By
default it is int.
```
enum class piece : char {
    EMPTY = '\0',
    X = 'X',
    O = 'O',
};
```

## Basic Enumeration Declaration
```
enum myEnum
{
    enumName1 = 1, // value will be 1
    enumName2 = 2, // value will be 2
    enumName3,     // value will be 3, previous value + 1
    enumName4 = 7, // value will be 7
    enumName5,     // value will be 8
    enumName6 = 5, // value will be 5, legal to go backwards
    enumName7 = 3, // value will be 3, legal to reuse numbers
    enumName8 = enumName4 + 2, // value will be 9, legal to take prior enums and adjust them
};
```
 in switch
 ```
 enum State {
    start,
    middle,
    end
};
...
switch(myState) {
    case start:
       ...
    case middle:
       ...
} // warning: enumeration value 'end' not handled in switch [-Wswitch]
 ```

# 69: Iteration
for, while, do, continue, break
## range-based for loop
```
std::vector<int> primes = {2, 3, 5, 7, 11, 13};
for(auto prime : primes) {
    std::cout << prime << std::endl;
}
```

# 70 Regular expressions
`bool regex_match`
```
const auto input = "Some people, when confronted with a problem, think \"I know, I'll use regular
expressions.\""s;
smatch sm;
cout << input << endl;
// If input ends in a quotation that contains a word that begins with "reg" and another word
beginning with "ex" then capture the preceding portion of input
if (regex_match(input, sm, regex("(.*)\".*\\breg.*\\bex.*\"\\s*$"))) {
    const auto capture = sm[1].str();
   
    cout << '\t' << capture << endl; 
   
    // Search our capture for "a problem" or "# problems"
    if(regex_search(capture, sm, regex("(a|d+)\\s+problems?"))) {

        const auto count = sm[1] == "a"s ? 1 : stoi(sm[1]);
        cout << '\t' << count << (count > 1 ? " problems\n" : " problem\n"); 
        cout << "Now they have " << count + 1 << " problems.\n"; 

    }
}
```

## regex_iterator Example
```
enum TOKENS {
    NUMBER,
    ADDITION,
    SUBTRACTION,
    MULTIPLICATION,
    DIVISION,
    EQUALITY,
    OPEN_PARENTHESIS,
    CLOSE_PARENTHESIS
};

vector<TOKENS> tokens;
const regex re{ "\\s*(\\(?)\\s*(-?\\s*\\d+)\\s*(\\)?)\\s*(?:(\\+)|(-)|(\\*)|(/)|(=))" };
for_each(sregex_iterator(cbegin(input), cend(input), re), sregex_iterator(), [&](const auto& i) {
    if(i[1].length() > 0) {
        tokens.push_back(OPEN_PARENTHESIS);
    }
   
    tokens.push_back(i[2].str().front() == '-' ? NEGATIVE_NUMBER : NON_NEGATIVE_NUMBER);
   
    if(i[3].length() > 0) {
        tokens.push_back(CLOSE_PARENTHESIS);
    }        
   
    auto it = next(cbegin(i), 4);
   
    for(int result = ADDITION; it != cend(i); ++result, ++it) {
        if (it->length() > 0U) {
            tokens.push_back(static_cast<TOKENS>(result));
            break;
        }
    }
});
match_results<string::const_reverse_iterator> sm;
if(regex_search(crbegin(input), crend(input), sm, regex{ tokens.back() == SUBTRACTION ?
"^\\s*\\d+\\s*-\\s*(-?)" : "^\\s*\\d+\\s*(-?)" })) {
    tokens.push_back(sm[1].length() == 0 ? NON_NEGATIVE_NUMBER : NEGATIVE_NUMBER);
}

```

## regex Anchors
* ^ which asserts the start of the string
* $ which asserts the end of the string
* \b which asserts a \W character or the beginning or end of the string
* \B which asserts a \w character
```
auto input = "+1--12*123/+1234"s;
smatch sm;
if(regex_search(input, sm, regex{ "(?:^|\\b\\W)([+-]?\\d+)" })) {
    do {
        cout << sm[1] << endl;
        input = sm.suffix().str();
    } while(regex_search(input, sm, regex{ "(?:^\\W|\\b\\W)([+-]?\\d+)" }));
}
```

## regex_replace
## regex_token_iterator

## Quantiﬁers
regex_match(input, regex("\\d+"))  
regex_match(input, regex("\\d{7,}"))  
regex_match(input, regex("\\d?\\d{7,10}"))  
regex_match(input, regex("(?:\\d{3,4})?\\d{7}"))


## Splitting a string
```
std::vector<std::string> split(const std::string &str, std::string regex)
{
    std::regex r{ regex };
    std::sregex_token_iterator start{ str.begin(), str.end(), r, -1 }, end;
    return std::vector<std::string>(start, end);
}
split("Some  string\t with whitespace ", "\\s+"); // "Some", "string", "with", "whitespace"
```

# 72: Exceptions
```
#include <iostream>
#include <string>
#include <stdexcept>
int main() {
  std::string str("foo");
 
  try {
      str.at(10); // access element, may throw std::out_of_range
  } catch (const std::out_of_range& e) {
      // what() is inherited from std::exception and contains an explanatory message
      std::cout << e.what();
  }
}
```
```
std::string str("foo");
 
try {
    str.reserve(2); // reserve extra capacity, may throw std::length_error
    str.at(10); // access element, may throw std::out_of_range
} catch (const std::length_error& e) {
    std::cout << e.what();
} catch (const std::out_of_range& e) {
    std::cout << e.what();
}
```

## 72.2 Rethrow (propagate) exception
```
try {
    ... // some code here
} catch (const SomeException& e) {
    std::cout << "caught an exception";
    throw;
}
```

To rethrow a managed std::exception_ptr, the C++ Standard Library has the rethrow_exception function that
can be used by including the <exception> header in your program.
```
#include <iostream>
#include <string>
#include <exception>
#include <stdexcept>
 
void handle_eptr(std::exception_ptr eptr) // passing by value is ok
{
    try {
        if (eptr) {
            std::rethrow_exception(eptr);
        }
    } catch(const std::exception& e) {
        std::cout << "Caught exception \"" << e.what() << "\"\n";
    }
}
 
int main()
{
    std::exception_ptr eptr;
    try {
        std::string().at(1); // this generates an std::out_of_range
    } catch(...) {
        eptr = std::current_exception(); // capture
    }
    handle_eptr(eptr);
} // destructor for std::out_of_range called here, when the eptr is destructed
```

## Throw and rethrow?
If you decide to rethrow an exception, prefer using the rethrow statement instead of throwing the same exception object using throw. rethrow preserves the original stack trace of the exception. throw on the other hand resets the stack trace to the last thrown position.  
Bad:
```
try {
  somethingRisky();
} catch (e) {
  if (!canHandle(e)) throw e;
  handle(e);
}
```
Good:
```
try {
  somethingRisky();
} catch (e) {
  if (!canHandle(e)) rethrow;
  handle(e);
}
```

# 72.4: Custom exception
You shouldn't throw raw values as exceptions, instead use one of the standard exception classes or make your
own.  
Having your own exception class inherited from std::exception is a good way to go about it. Here's a custom
exception class which directly inherits from std::exception:
```
#include <exception>
class Except: virtual public std::exception {
   
protected:
    int error_number;               ///< Error number
    int error_offset;               ///< Error offset
    std::string error_message;      ///< Error message
   
public:
    /** Constructor (C++ STL string, int, int).
     *  @param msg The error message
     *  @param err_num Error number
     *  @param err_off Error offset
     */
    explicit
    Except(const std::string& msg, int err_num, int err_off):
        error_number(err_num),
        error_offset(err_off),
        error_message(msg)
        {}
    /** Destructor.
     *  Virtual to allow for subclassing.
     */
    virtual ~Except() throw () {}
    /** Returns a pointer to the (constant) error description.
     *  @return A pointer to a const char*. The underlying memory
     *  is in possession of the Except object. Callers must
     *  not attempt to free the memory.
     */
    virtual const char* what() const throw () {
       return error_message.c_str();
    }
   
    /** Returns error number.
     *  @return #error_number
     */
    virtual int getErrorNumber() const throw() {
        return error_number;
    }
   
    /**Returns error offset.
     * @return #error_offset
     */
    virtual int getErrorOffset() const throw() {
        return error_offset;
    }
};
```


## 72.5: std::uncaught_exceptions
當子function throw了一個沒有被catch抓取的exception，std::uncaught_exceptions 可以回傳這些exception的數量int. 藉此caller function檢測是否要做特殊處理，同時可以以此知道是不是在stack unwinding (由exception造成)。   
Detects if the current thread has a live exception object, that is, an exception has been thrown or rethrown and not yet entered a matching catch clause   

---
\# Note: stack unwinding   
Stack unwinding refers to the process of cleaning up the call stack when an exception is thrown. During stack unwinding, the program jumps back through the call stack, executing destructors of objects in reverse order of their construction. This process continues until an appropriate catch block is found to handle the exception or until the stack is fully unwound. Stack unwinding ensures that resources are properly released and allows for exception handling and propagation to occur in a controlled manner.

---
```
#include <exception>
#include <iostream>
#include <stdexcept>
 
struct Foo
{
    char id{'?'};
    int count = std::uncaught_exceptions();
    
    ~Foo()
    {
        std::cout << id << ":" <<  std::uncaught_exceptions() << " count\n";
        count == std::uncaught_exceptions()
            ? std::cout << id << ".~Foo() called normally\n"
            : std::cout << id << ".~Foo() called during stack unwinding\n";
    }
};
 
int main()
{
    Foo f{'f'};
 
    try
    {
        Foo g{'g'};
        std::cout << "Exception thrown\n";
        throw std::runtime_error("test exception");
    }
    catch (const std::exception& e)
    {
        std::cout << "Exception caught: " << e.what() << '\n';
    }
}
```
output:
```
Exception thrown
g:1 count
g.~Foo() called during stack unwinding
Exception caught: test exception
f:0 count
f.~Foo() called normally
```


## 72.7: Nested exception
```
#include <stdexcept>
#include <exception>
#include <string>
#include <fstream>
#include <iostream>
struct MyException
{
    MyException(const std::string& message) : message(message) {}
    std::string message;
};
void print_current_exception(int level)
{
    try {
        throw;
    } catch (const std::exception& e) {
        std::cerr << std::string(level, ' ') << "exception: " << e.what() << '\n';
    } catch (const MyException& e) {
        std::cerr << std::string(level, ' ') << "MyException: " << e.message << '\n';
    } catch (...) {
        std::cerr << "Unkown exception\n";
    }
}
void print_current_exception_with_nested(int level =  0)
{
    try {
        throw;
    } catch (...) {
        print_current_exception(level);
    }    
    try {
        throw;
    } catch (const std::nested_exception& nested) {
        try {
            nested.rethrow_nested();
        } catch (...) {
            print_current_exception_with_nested(level + 1); // recursion
        }
    } catch (...) {
        //Empty // End recursion
    }
}
// sample function that catches an exception and wraps it in a nested exception
void open_file(const std::string& s)
{
    try {
        std::ifstream file(s);
        file.exceptions(std::ios_base::failbit);
    } catch(...) {
        std::throw_with_nested(MyException{"Couldn't open " + s});
    }
}
 
// sample function that catches an exception and wraps it in a nested exception
void run()
{
    try {
        open_file("nonexistent.file");
    } catch(...) {
        std::throw_with_nested( std::runtime_error("run() failed") );
    }
}
 
// runs the sample function above and prints the caught exception
int main()
{
    try {
        run();
    } catch(...) {
        print_current_exception_with_nested();
    }
}
```
```
exception: run() failed
 MyException: Couldn't open nonexistent.file
  exception: basic_ios::clear
```
If you work only with exceptions inherited from std::exception, code can even be simpliﬁed.


# noexcept

The noexcept specifier does not affect the code execution or flow directly. It provides a guarantee or contract about the behavior of the function with regards to exceptions.

The noexcept specifier on a function declaration indicates that the function is not expected to throw any exceptions. If a function is declared with noexcept, it means that the function guarantees not to throw any exceptions. However, it does not prevent exceptions from being thrown by other functions called within that noexcept function.

If an exception is thrown from within a noexcept function, the behavior depends on whether the exception is caught within that function or any higher-level functions. If the exception is caught within the noexcept function or a higher-level function, it can be handled and does not propagate further. If the exception is not caught within the noexcept function or any higher-level functions, it will result in program termination.

On the other hand, if a function is not declared as noexcept, it means that it may potentially throw exceptions. Exceptions thrown from non-noexcept functions can be caught and handled by suitable exception handlers, including catch blocks in higher-level functions or the main function.

In summary, the noexcept specifier provides information about the exception-safety guarantee of a function, but it does not directly control the behavior of exception handling. Exceptions can still be caught and handled regardless of whether a function is declared with noexcept.


# Lambdas
## lambda expression
```
[](){}   // An empty lambda
```
\# Note: enclosing scope : where lambda exist. like finction, class, struct, a block of code, global scope.  
The range of the enclosing scope is determined by where the lambda expression is defined.

## Capture list
```
int a = 0;                       // Define an integer variable
auto f = []()   { return a*9; }; // Error: 'a' cannot be accessed
auto f = [a]()  { return a*9; }; // OK, 'a' is "captured" by value
auto f = [&a]() { return a++; }; // OK, 'a' is "captured" by reference
                                 //      Note: It is the responsibility of the programmer
                                 //      to ensure that a is not destroyed before the
                                 //      lambda is called.
auto b = f();                    // Call the lambda function. a is taken from the capture list and
not passed here.
```
```
#include <iostream>

int main() {
    int x = 10;
    int y = 20;

    // Capture variables by value
    auto lambda1 = [=]() {
        std::cout << "Captured variables by value: " << x << ", " << y << std::endl;
    };

    // Capture variables by reference
    auto lambda2 = [&]() {
        x += 5; // Modifying x inside the lambda affects the original variable
        std::cout << "Captured variables by reference: " << x << ", " << y << std::endl;
    };

    // Capture specific variable individually
    auto lambda3 = [y]() {
        // Only captures y by value, x is not accessible inside this lambda
        std::cout << "Captured variable individually: " << y << std::endl;
    };

    lambda1(); // Output: Captured variables by value: 10, 20
    lambda2(); // Output: Captured variables by reference: 15, 20
    lambda3(); // Output: Captured variable individually: 20

    // Modifying y outside the lambdas doesn't affect the captured variables
    y = 30;

    lambda1(); // Output: Captured variables by value: 10, 20
    lambda2(); // Output: Captured variables by reference: 20, 30
    lambda3(); // Output: Captured variable individually: 20

    return 0;
}
```
In this example, we have three lambdas (lambda1, lambda2, and lambda3) that capture variables using different capture modes. lambda1 captures variables x and y by value using [=], lambda2 captures variables x and y by reference using [&], and lambda3 captures only variable y individually using [y].

* Capture list: Captures variables from the enclosing scope, controls their accessibility, and allows you to use them inside the lambda. Can capture variables by value or by reference.
* Parameter list: Defines the parameters of the lambda function, allowing you to pass additional data or values to the lambda when invoking it. These parameters are separate from the variables captured in the capture list.

\#`Note` : when you capture a variable by value in a lambda expression, it creates a copy of that variable inside the lambda. By default, this copy is considered to be read-only.
like [=], [y] in example.

## Parameter list
we can ommit empty ()
```
auto call_foo  = [x](){ x.foo(); };
auto call_foo2 = [x]{ x.foo(); };
//same
```

## Calling a lambda
A lambda expression's result object is a closure, which can be called using the operator() (as with other function
objects):  
Q: can I explain closure same as function object ?  
A: Yes, you can explain a closure as a function object. A closure is an object that encapsulates a lambda expression and allows it to be called like a regular function. It combines the lambda expression and the captured variables (if any) into a callable entity.
```
int multiplier = 5;
auto timesFive = [multiplier](int a) { return a * multiplier; };
std::out << timesFive(2); // Prints 10
multiplier = 15;
std::out << timesFive(2); // Still prints 2*5 == 10
```

## Return Type
By default, the return type of a lambda expression is deduced.
```
[](){ return true; };  //empty
```
In this case the return type is bool.
```
[]() -> bool { return true; };
```

## Mutable Lambda
Const varible in scope or use capture list by value "[=], [variable]". Data will be treated as read-only.  
mutable is a solution.
```
auto func = [c = 0]() mutable {++c; std::cout << c;};
```

* copy-constructible (constructor take itself)  
capture list need copy-constructible to get parameter.

unique ptr for lambda
```
auto p = std::make_unique<T>(...);
[p = std::move(p)]() {
    return p->createWidget();
};
```
As p a unique_ptr, there is only one dynamically allocated object. The purpose of using std::move is to indicate that the ownership of the std::unique_ptr is being transferred to the lambda and that the original std::unique_ptr (p) should no longer be used.

It's important to note that after the lambda is executed or goes out of scope, the captured std::unique_ptr will be destroyed, and the destructor of std::unique_ptr will be called. If the captured std::unique_ptr owns a valid object, the object will be deleted.


## Recursive lambdas
* Use std::function   
We can have a lambda capture a reference to a not-yet constructed std::function:
```
std::function<int(int, int)> gcd = [&](int a, int b){
    return b == 0 ? a : gcd(b, a%b);
};
```
* *Using two smart pointers:
```
auto gcd_self = std::make_shared<std::unique_ptr< std::function<int(int, int)> >>();
*gcd_self = std::make_unique<std::function<int(int, int)>>(
  [gcd_self](int a, int b){
    return b == 0 ? a : (**gcd_self)(b, a%b);
  };
};
```

## Default capture
By default, local variables that are not explicitly speciﬁed in the capture list, cannot be accessed from within the
lambda body.
```
int a = 1;
int b = 2;
// Default capture by value
[=]() { return a + b; }; // OK; a and b are captured by value
// Default capture by reference
[&]() { return a + b; }; // OK; a and b are captured by reference
```

```
int a = 0;
int b = 1;
[=, &b]() {
    a = 2; // `Illegal`; 'a' is capture by value, and lambda is not 'mutable', read-only
    b = 2; // OK; 'b' is captured by reference
};
```
Also, be aware that the default capture clauses, both [=] and [&], will also capture this implicitly.

## Class lambdas and capture of this
 when a lambda captures this by value, it has access to all the non-static member variables and member functions of the enclosing class
```
class Foo
{
private:
    int i;
   
public:
    Foo(int val) : i(val) {}
   
    void Test()
    {
        // capture the this pointer by value
        auto lamb = [this](int val)
        {
            i = val;
        };
       
        lamb(30);
    }
};
```
The lambda function can modify the i member variable directly since it has captured this by value.
* [=] for copy object, not affect original object.
* [&] reference object, will affect original object.

## Generic lambdas
 This allows a lambda to be more generic
```
auto twice = [](auto x){ return x+x; };
int i = twice(2); // i == 4
std::string s = twice("hello"); // s == "hellohello"
```
This is implemented in C++ by making the closure type's operator() overload a template function. The following
type has equivalent behavior to the above lambda closure:
```
struct _unique_lambda_type
{
  template<typename T>
  auto operator() (T x) const {return x + x;}
};
```

Generic lambdas can take arguments by reference as well, using the usual rules for auto and &. If a generic
parameter is taken as auto&&, this is a forwarding reference to the passed in argument and not an rvalue reference:
```
auto lamb1 = [](int &&x) {return x + 5;};
auto lamb2 = [](auto &&x) {return x + 5;};
int x = 10;

lamb1(x); 
// Illegal; must use `std::move(x)` for `int&&` parameters.

lamb2(x); 
// Legal; the type of `x` is deduced as `int&`.
```

## std::move()
two main usages:
* Move Semantics:   
When used with an object, std::move() casts that object to an rvalue reference, allowing it to be efficiently moved rather than copied. Move semantics enable the transfer of resources, such as dynamically allocated memory, from one object to another, avoiding unnecessary copying. It is used to invoke the move constructor or move assignment operator of a type.
* Converting Lvalue to Rvalue Reference:  
std::move() can be used to convert an lvalue to an rvalue reference. This is done by casting the lvalue to an rvalue reference, indicating that it can be treated as an rvalue for the purpose of overload resolution. This usage is often seen in perfect forwarding scenarios, where you want to forward an argument, preserving its value category (lvalue or rvalue).

Both usages involve the same function std::move(), but they serve different purposes. The first usage facilitates efficient resource transfer during move operations, while the second usage is related to perfect forwarding and preserving value categories.

It's important to note that using std::move() does not guarantee that an object's properties will be transferred or that it will behave as an rvalue. It simply enables the move semantics or converts an lvalue to an rvalue reference for overload resolution. The actual behavior depends on how the type handles move operations or overload resolution.


Lambda functions can be variadic and perfectly forward their arguments:
```
auto lam = [](auto&&... args){return f(std::forward<decltype(args)>(args)...);};
```
or
```
auto lam = [](auto&&... args){return f(decltype(args)(args)...);};
```

## Syntax:  "..."
* Variadic function parameters:
```
void myFunction(int arg1, int arg2, ...);
```
* Variadic templates:
```
template <typename... Args>
void myFunction(Args... args);
```
* Fold expressions:
```
auto sum = (... + args);  // Expands to sum = ((arg1 + arg2) + arg3) + ...
```

# Generalized capture
```
auto p = std::make_unique<T>(...);
auto lamb = [p = std::move(p)]() //Overrides capture-by-value of `p`.
{
  p->SomeFunc();
};
```
```
auto lamb_copy = lamb; //Illegal
auto lamb_move = std::move(lamb); //legal.
```
std::unique_ptr is a move-only type, which means it cannot be copied.  
 this case, std::move(p) is used to move the ownership of the std::unique_ptr<T> object p


 ## Generalized capture
 In the given code, [&v = a] is used to create a reference v that is bound to the variable a.  
 The syntax &v = a specifies that v is an alias or reference to the variable a.
 ```
 int a = 0;
auto lamb = [&v = a](int add) //Note that `a` and `v` have different names
{
  v += add; //Modifies `a`
};

lamb(20); //`a` becomes 20.
```

## Conversion to function pointer
```
auto sorter = [](int lhs, int rhs) -> bool {return lhs < rhs;};
using func_ptr = bool(*)(int, int);
func_ptr sorter_func = sorter; // implicit conversion
```
```
func_ptr sorter_func2 = +sorter; // enforce implicit conversion
```
Version ≥ C++14
```
auto sorter = [](auto lhs, auto rhs) { return lhs < rhs; };
using func_ptr = bool(*)(int, int);
func_ptr sorter_func = sorter;  // deduces int, int
// note however that the following is ambiguous
// func_ptr sorter_func2 = +sorter;
```


# Threading

* Free functions (executes a function on a separate thread)
```
#include <iostream>
#include <thread>
 
void foo(int a)
{
    std::cout << a << '\n';
}
 
int main()
{
    // Create and execute the thread
    std::thread thread(foo, 10); // foo is the function to execute, 10 is the
                                 // argument to pass to it
 
    // Keep going; the thread is executed separately
 
    // Wait for the thread to finish; we stay here until it is done
    thread.join();
 
    return 0;
}
```
* Member function
```
#include <iostream>
#include <thread>
 
class Bar
{
public:
    void foo(int a)
    {
        std::cout << a << '\n';
    }
};
int main()
{
    Bar bar;
   
    // Create and execute the thread
    std::thread thread(&Bar::foo, &bar, 10); // Pass 10 to member function
 
    // The member function will be executed in a separate thread
 
    // Wait for the thread to finish, this is a blocking operation
    thread.join();
 
    return 0;
}
```
* Functor object example
```
#include <iostream>
#include <thread>
 
class Bar
{
public:
    void operator()(int a)
    {
        std::cout << a << '\n';
    }
};
 
int main()
{
    Bar bar;
   
    // Create and execute the thread
    std::thread thread(bar, 10); // Pass 10 to functor object
 
    // The functor object will be executed in a separate thread
 
    // Wait for the thread to finish, this is a blocking operation
    thread.join();
 
    return 0;
}
```
* Lambda expression example
```
#include <iostream>
#include <thread>
 
int main()
{
    auto lambda = [](int a) { std::cout << a << '\n'; };
    // Create and execute the thread
    std::thread thread(lambda, 10); // Pass 10 to the lambda expression
 
    // The lambda expression will be executed in a separate thread
 
    // Wait for the thread to finish, this is a blocking operation
    thread.join();
return 0;
}
```
## Passing a reference to a thread
You `cannot` pass a reference (or const reference) directly to a thread.  
std::thread will copy/move them. Instead, use std::reference_wrapper:
```
void foo(int& b)
{
    b = 10;
}
int a = 1;
std::thread thread{ foo, std::ref(a) }; //'a' is now really passed as reference
thread.join();
std::cout << a << '\n'; //Outputs 10
```
```
void bar(const ComplexObject& co)
{
    co.doCalculations();
}
ComplexObject object;
std::thread thread{ bar, std::cref(object) }; //'object' is passed as const&
thread.join();
std::cout << object.getResult() << '\n'; //Outputs the result
```

## Using std::async instead of std::thread
std::async is also able to make threads. Compared to std::thread it is considered less powerful but easier to use
when you just want to run a function asynchronously.
```
#include <iostream>
unsigned int square(unsigned int i){
    return i*i;
}
int main() {
    auto f = std::async(std::launch::async, square, 8);
    std::cout << "square currently running\n"; //do something while square is running
    std::cout << "result is " << f.get() << '\n'; //getting the result from square
}
```
* std::async is a higher-level interface  
* It returns a std::future object that holds the result or exception of the asynchronous task.  
* std::async automatically manages the thread's lifetime. do not need to manually start, join, or detach the thread

## Basic Synchronization
The simplest is `std::mutex`. To lock a mutex    
The simplest lock type is `std::lock_guard`:  
```
std::mutex m;
void worker() {
    std::lock_guard<std::mutex> guard(m); // Acquires a lock on the mutex
    // Synchronized code here
} // the mutex is automatically released when guard goes out of scope
```
## thread pool
```
struct tasks {
  // the mutex, condition variable and deque form a single
  // thread-safe triggered queue of tasks:
  std::mutex m;
  std::condition_variable v;
  // note that a packaged_task<void> can store a packaged_task<R>:
  std::deque<std::packaged_task<void()>> work;
  // this holds futures representing the worker threads being done:
  std::vector<std::future<void>> finished;
  // queue( lambda ) will enqueue the lambda into the tasks for the threads
  // to use.  A future of the type the lambda returns is given to let you get
  // the result out.
  template<class F, class R=std::result_of_t<F&()>>
  std::future<R> queue(F&& f) {
    // wrap the function object into a packaged task, splitting
    // execution from the return value:
    std::packaged_task<R()> p(std::forward<F>(f));
    auto r=p.get_future(); // get the return value before we hand off the task
    {
      std::unique_lock<std::mutex> l(m);
      work.emplace_back(std::move(p)); // store the task<R()> as a task<void()>
    }
    v.notify_one(); // wake a thread to work on the task
    return r; // return the future result of the task
  }
  // start N threads in the thread pool.
  void start(std::size_t N=1){
    for (std::size_t i = 0; i < N; ++i)
    {
      // each thread is a std::async running this->thread_task():
      finished.push_back(
        std::async(
          std::launch::async,
          [this]{ thread_task(); }
        )
      );
    }
  }
  // abort() cancels all non-started tasks, and tells every working thread
  // stop running, and waits for them to finish up.
  void abort() {
    cancel_pending();
    finish();
  }
  // cancel_pending() merely cancels all non-started tasks:
  void cancel_pending() {
    std::unique_lock<std::mutex> l(m);
    work.clear();
  }
  // finish enques a "stop the thread" message for every thread, then waits for them:
  void finish() {
    {
      std::unique_lock<std::mutex> l(m);
      for(auto&&unused:finished){
        work.push_back({});
      }
    }
    v.notify_all();
    finished.clear();
  }
  ~tasks() {
    finish();
  }
  private:
  // the work that a worker thread does:
  void thread_task() {
    while(true){
      // pop a task off the queue:
      std::packaged_task<void()> f;
      {
        // usual thread-safe queue code:
        std::unique_lock<std::mutex> l(m);
        if (work.empty()){
          v.wait(l,[&]{return !work.empty();});
        }
        f = std::move(work.front());
        work.pop_front();
      }
      // if the task is invalid, it means we are asked to abort:
      if (!f.valid()) return;
      // otherwise, run the task:
      f();
    }
  }
};
```
## Operations on the current thread
* `get_id` Returns the id of the thread
* `sleep_for` Sleeps for a speciﬁed amount of time
* `sleep_until` Sleeps until a speciﬁc time
* `yield` Reschedule running threads, giving other threads priority
## Condition Variables
One waits on a std::condition_variable with a std::unique_lock<std::mutex>. This allows the code to safely examine shared state before deciding whether or not to proceed with acquisition.  
Below is a producer-consumer sketch that uses std::thread, std::condition_variable, std::mutex, and a few
others to make things interesting.

# Futures and Promises
Promises and Futures are used to ferry a single object from one thread to another. 

`A std::promise` object is set by the thread which generates the result.  
`A std::future` object can be used to retrieve a value, to test to see if a value is available, or to halt execution until
the value is available.

```
{
        auto promise = std::promise<std::string>();
       
        auto producer = std::thread([&]
        {
            promise.set_value("Hello World");
        });
       
        auto future = promise.get_future();
       
        auto consumer = std::thread([&]
        {
            std::cout << future.get();
        });
       
        producer.join();
        consumer.join();
}
```


# Atomic Types
```
#include <atomic>
#include <thread>
#include <iostream>

    //function will add all values including and between 'a' and 'b' to 'result'
void add(int a, int b, std::atomic<int> * result) {
    for (int i = a; i <= b; i++) {
        //atomically add 'i' to result
        result->fetch_add(i);
    }
}
int main() {
    //atomic template used to store non-atomic objects
    std::atomic<int> shared = 0;
    //create a thread that may run parallel to the 'main' thread
    //the thread will run the function 'add' defined above with parameters a = 1, b = 100, result =
&shared
    //analogous to 'add(1,100, &shared);'
    std::thread addingThread(add, 1, 10000, &shared);
    //print the value of 'shared' to console
    //main will keep repeating this until the addingThread becomes joinable
    while (!addingThread.joinable()) {
        //safe way to read the value of shared atomically for thread safe read
        std::cout << shared.load() << std::endl;  
    }

    //rejoin the thread at the end of execution for cleaning purposes
    addingThread.join();
   
    return 0;
}
```

## constexpr variables
A variable declared constexpr is implicitly const and its value may be used as a constant expression.  

With constexpr the compile-time evaluated expression is replaced with the result.
```
int main()
{
   constexpr int N = 10 + 2;
   cout << N;
}
```
will produce:
```
cout << 12;
```
A pre-processor based compile-time macro would be diﬀerent
```
#define N 10 + 2
int main()
{
    cout << N;
}
```
```
cout << 10 + 2;
```
A const variable is a variable which needs memory for its storage. A constexpr does not. A constexpr produces
compile time constant, which cannot be changed.

---
```
int main()
{
   const int size1 = 10;
   const int size2 = abs(10);
   int arr_one[size1];
   int arr_two[size2];
}
```
With most compilers the second statement will fail (const int size2 = abs(10);)  
The second variable size2 is assigned some value that is decided at runtime.  
Even though you know it is 10, for the compiler it is not compile-time

This means that a `const` `may or may not be a true compile-time` constant.  
It also need to be a `constexpr` function or it won't work.
```
int main()
{
    constexpr int size = 10;
    int arr[size];
}
```

A `constexpr` expression must `evaluate to a compile-time value`. Thus, you `cannot` use:
```
constexpr int size = abs(10);
```

# Storage class speciﬁers
## extern
* It can be used to declare a variable without deﬁning it
```
// global scope
int x;             // definition; x will be default-initialized
extern int y;      // declaration; y is defined elsewhere, most likely another TU
extern int z = 42; // definition; "extern" has no effect here (compiler may warn)
```
* It gives external linkage to a variable at namespace scope even if const or constexpr would have otherwise
caused it to have internal linkage.
```
// global scope
const int w = 42;            // internal linkage in C++; external linkage in C
static const int x = 42;     // internal linkage in both C++ and C
extern const int y = 42;     // external linkage in both C++ and C
namespace {
    extern const int z = 42; // however, this has internal linkage since
                             // it's in an unnamed namespace
}
```
* It redeclares a variable at block scope if it was previously declared with linkage. Otherwise, it declares a new
variable with linkage, which is a member of the nearest enclosing namespace.
```
// global scope
namespace {
    int x = 1;
    struct C {
        int x = 2;
        void f() {
            extern int x;           // redeclares namespace-scope x
            std::cout << x << '\n'; // therefore, this prints 1, not 2
        }
    };
}
void g() {
    extern int y; // y has external linkage; refers to global y defined elsewhere
}
```

## register
Version ≥ `C++17`  
`The keyword register is unused and reserved. A program that uses the keyword register is ill-formed.`

A storage class speciﬁer that hints to the compiler that a variable will be heavily used.

might choose to store such a variable in a CPU register so that it can be accessed in fewer
clock cycles
```
register int i = 0;
while (i < 100) {
    f(i);
    int g = i*i;
    i += h(i, g);
}
```


# Unit Testing in C++
## Google Test
Google Test is a C++ testing framework maintained by Google. It requires building the gtest library and linking it to
your testing framework when building a test case ﬁle.
```
// main.cpp
#include <gtest/gtest.h>
#include <iostream>
// Google Test test cases are created using a C++ preprocessor macro
// Here, a "test suite" name and a specific "test name" are provided.
TEST(module_name, test_name) {
    std::cout << "Hello world!" << std::endl;
    // Google Test will also provide macros for assertions.
    ASSERT_EQ(1+1, 2);
}
// Google Test can be run manually from the main() function
// or, it can be linked to the gtest_main library for an already
// set-up main() function primed to accept Google Test test cases.
int main(int argc, char** argv) {
    ::testing::InitGoogleTest(&argc, argv);
    return RUN_ALL_TESTS();
}
// Build command: g++ main.cpp -lgtest
```
## Catch
Catch is a header only library that allows you to use both TDD and BDD unit test style.
The following snippet is from the Catch documentation page at this link:
https://github.com/catchorg/Catch2/blob/devel/docs/tutorial.md

# Build Systems
## CMake
CMake ﬁles are always named "CMakeLists.txt"  
Exist in every project's root directory (and possibly in sub-directories too.)
```
cmake_minimum_required(VERSION 2.4)
project(HelloWorld)
add_executable(HelloWorld main.cpp)
```
## GNU make
Makeﬁle
```
# Set some variables to use in our command
# First, we set the compiler to be g++
CXX=g++
# Then, we say that we want to compile with g++'s recommended warnings and some extra ones.
CXXFLAGS=-Wall -Wextra -pedantic
# This will be the output file
EXE=app
SRCS=main.cpp
# When you call `make` at the command line, this "target" is called.
# The $(EXE) at the right says that the `all` target depends on the `$(EXE)` target.
# $(EXE) expands to be the content of the EXE variable
# Note: Because this is the first target, it becomes the default target if `make` is called without
target
all: $(EXE)
# This is equivalent to saying
# app: $(SRCS)
# $(SRCS) can be separated, which means that this target would depend on each file.
# Note that this target has a "method body": the part indented by a tab (not four spaces).
# When we build this target, make will execute the command, which is:
# g++ -Wall -Wextra -pedantic -o app main.cpp
# I.E. Compile main.cpp with warnings, and output to the file ./app
$(EXE): $(SRCS)
    @$(CXX) $(CXXFLAGS) -o $@ $(SRCS)
# This target should reverse the `all` target. If you call
# make with an argument, like `make clean`, the corresponding target
# gets called.
clean:
    @rm -f $(EXE)
```

```
CXX=g++
CXXFLAGS=-Wall -Wextra -pedantic
EXE=app
SRCS_GLOB=src/*.cpp
SRCS=$(wildcard $(SRCS_GLOB))
OBJS=$(SRCS:.cpp=.o)
all: $(EXE)
$(EXE): $(OBJS)
    @$(CXX) -o $@ $(OBJS)
depend: .depend
.depend: $(SRCS)
    @-rm -f ./.depend
    @$(CXX) $(CXXFLAGS) -MM $^>>./.depend
clean:
    -rm -f $(EXE)
    -rm $(OBJS)
    -rm *~
    -rm .depend
include .depend
```
Dependency Generation:

* The depend target generates the dependency file .depend, which lists the dependencies of each source file. It uses the -MM flag with $(CXX) to automatically generate the dependencies based on the #include directives in the source files.
* By including .depend in the Makefile (include .depend), the dependencies are automatically included in the build process. This means that if a.cpp is edited, b.cpp will not be recompiled unless the header files it depends on (a.hpp in this case) are modified.


## Autotools (GNU)
* Autoconf
* Automake (not to be confused with make)

```
./configure && make && make install
```



# Proﬁling
## Proﬁling with gcc and gprof
The GNU gprof proﬁler, gprof, allows you to proﬁle your code. To use it, you need to perform the following steps:
1. Build the application with settings for generating proﬁling information
2. Generate proﬁling information by running the built application
3. View the generated proﬁling information with gprof

In order to build the application with settings for generating proﬁling information, we add the -pg ﬂag. So, for
example, we could use
```
$ gcc -pg *.cpp -o app
```
```
$ gcc -O2 -pg *.cpp -o app
```
```
$ ./app
```
This should produce a ﬁle called gmon.out.
```
$ gprof app gmon.out
// or
$ gprof app gmon.out | less
```


## Generating callgraph diagrams with gperf2dot

For more complex applications, ﬂat execution proﬁles may be diﬃcult to follow. This is why many proﬁling tools
also generate some form of annotated callgraph information.  
gperf2dot converts text output from many proﬁlers
```
# compile with profiling flags  
g++ *.cpp -pg
# run to generate profiling data                                            
./main
# translate profiling data to text, create image    
gprof ./main | gprof2dot -s | dot -Tpng -o output.png
```
## Proﬁling CPU Usage with gcc and Google Perf Tools
1. Install Google Perf Tools
2. Compile your code as usual
3. Add the libprofiler proﬁler library to your library load path at runtime
4. Use pprof to generate a ﬂat execution proﬁle, or a callgraph diagram

For example:
 compile code
 ```
g++ -O3 -std=c++11 main.cpp -o main
```
 run with profiler
 ```
LD_PRELOAD=/usr/local/lib/libprofiler.so CPUPROFILE=main.prof CPUPROFILE_FREQUENCY=100000 ./main
```
* CPUPROFILE indicates the output ﬁle for proﬁling data
* CPUPROFILE_FREQUENCY indicates the proﬁler sampling frequency;  

Use pprof to post-process the proﬁling data.
You can generate a ﬂat call proﬁle as text:
```
$ pprof --text ./main main.prof
PROFILE: interrupts/evictions/bytes = 67/15/2016
pprof --text --lines ./main main.prof
Using local file ./main.
Using local file main.prof.
Total: 67 samples
22  32.8%  32.8%       67 100.0% longRunningFoo ??:0
20  29.9%  62.7%       20  29.9% __memmove_ssse3_back
/build/eglibc-3GlaMS/eglibc-2.19/string/../sysdeps/x86_64/multiarch/memcpy-ssse3-back.S:1627
4   6.0%  68.7%        4   6.0% __memmove_ssse3_back
/build/eglibc-3GlaMS/eglibc-2.19/string/../sysdeps/x86_64/multiarch/memcpy-ssse3-back.S:1619
3   4.5%  73.1%        3   4.5% __random_r /build/eglibc-3GlaMS/eglibc-2.19/stdlib/random_r.c:388
3   4.5%  77.6%        3   4.5% __random_r /build/eglibc-3GlaMS/eglibc-2.19/stdlib/random_r.c:401
2   3.0%  80.6%        2   3.0% __munmap
/build/eglibc-3GlaMS/eglibc-2.19/misc/../sysdeps/unix/syscall-template.S:81
2   3.0%  83.6%       12  17.9% __random /build/eglibc-3GlaMS/eglibc-2.19/stdlib/random.c:298
2   3.0%  86.6%        2   3.0% __random_r /build/eglibc-3GlaMS/eglibc-2.19/stdlib/random_r.c:385
2   3.0%  89.6%        2   3.0% rand /build/eglibc-3GlaMS/eglibc-2.19/stdlib/rand.c:26
1   1.5%  91.0%        1   1.5% __memmove_ssse3_back
/build/eglibc-3GlaMS/eglibc-2.19/string/../sysdeps/x86_64/multiarch/memcpy-ssse3-back.S:1617
1   1.5%  92.5%        1   1.5% __memmove_ssse3_back
/build/eglibc-3GlaMS/eglibc-2.19/string/../sysdeps/x86_64/multiarch/memcpy-ssse3-back.S:1623
1   1.5%  94.0%        1   1.5% __random /build/eglibc-3GlaMS/eglibc-2.19/stdlib/random.c:293
1   1.5%  95.5%        1   1.5% __random /build/eglibc-3GlaMS/eglibc-2.19/stdlib/random.c:296
1   1.5%  97.0%        1   1.5% __random_r /build/eglibc-3GlaMS/eglibc-2.19/stdlib/random_r.c:371
1   1.5%  98.5%        1   1.5% __random_r /build/eglibc-3GlaMS/eglibc-2.19/stdlib/random_r.c:381
1   1.5% 100.0%        1   1.5% rand /build/eglibc-3GlaMS/eglibc-2.19/stdlib/rand.c:28
0   0.0% 100.0%       67 100.0% __libc_start_main /build/eglibc-3GlaMS/eglibc-2.19/csu/libc-
start.c:287
0   0.0% 100.0%       67 100.0% _start ??:0
0   0.0% 100.0%       67 100.0% main ??:0
0   0.0% 100.0%       14  20.9% rand /build/eglibc-3GlaMS/eglibc-2.19/stdlib/rand.c:27
0   0.0% 100.0%       27  40.3% std::vector::_M_emplace_back_aux ??:0
```