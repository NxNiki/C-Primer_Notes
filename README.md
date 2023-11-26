# Cpp Primer_Notes
notes of book C++ Primer

## compiling a C++ program:
By default, the command to run the GNU compiler is g++:

`g++ -o prog1 prog1.cc`

The -o prog1 is an argument to the compiler and names the file in which to put the executable file. This command generates an executable file named prog1 or prog1.exe, depending on the operating system. Compilers usually include options to generate warnings about problematic constructs. It is usually a good idea to use these options. Our preference is to use -Wall with the GNU compiler, and to use /W4 with the Microsoft compilers.

## section 1.4
When we enter input to a program from the keyboard, different operating systems use different conventions to allow us to indicate end-of-file. On Windows systems we enter an end-of-file by typing a control-z—hold down the Ctrl key and press z— followed by hitting either the Enter or Return key. On UNIX systems, including on Mac OS X machines, end-of-file is usually control-d.

### section 1.5.1
Every class defines a type. The type name is the same as the name of the class.
the class author determines all the operations that can be used on objects of the class type.

Most operating systems support ﬁle redirection, which lets us associate a
named ﬁle with the standard input and the standard output:

`$ addItems <infile >outfile`

Assuming $ is the system prompt and our addition program has been compiled into an executable ﬁle named addItems.exe (or addItems on UNIX systems), this command will read transactions from a ﬁle named infile and write its output to a ﬁle
named outfile in the current directory.

## Section 2.1 Primitive Built-in Types
A char is guaranteed big enough to hold numeric values corresponding to the characters in the machine’s basic character set. A char is the same size as a machine byte (8 bits).

- Use an unsigned type when you know the values cannot be negative.
- Use int for integer arithmetic. short is usually too small, and in practice, long often has the same size as int. If your data values are larger than the minimum guaranteed size of an int, then use long long.
- Do not use plain char or bool in arithmetic expressions. Use them only to hold characters or truth values. Computations using char are especially problematic because char is signed on some machines and unsigned on others. If you
need a tiny integer, explicitly specify either a signed char or an unsigned char.
- Use double for floating-point computations; float usually does not have enough precision, and the cost of double-precision calculations versus single-precision is negligible. In fact, on some machines, double-precision operations are faster than single. The precision offered by long double is usually unnecessary and often entails considerable run-time cost.

### section 2.1.2 Type conversations
- If we assign an out-of-range value to an object of unsigned type, the result is the remainder of the value modulo the number of values the target type can hold.
  an 8-bit unsigned char can hold values from 0 through 255, inclusive. If we assign a value outside this range, the compiler assigns the remainder of that value modulo 256. Therefore, assigning –1 to an 8-bit unsigned char gives that object the value 255.

- If we assign an out-of-range value to an object of signed type, the result is undefined. The program might appear to work, it might crash, or it might produce garbage values.
Undefined behavior results from errors that the compiler is not required (and sometimes cannot) to detect. Even if the code compiles, a program that executes an undefined expression is in error.
Programs should usually avoid implementation-defined behavior, such as assuming that the size of an int is a fixed and known value. Such programs are said to be nonportable.

Converting an int to unsigned executes the same way as if we assigned the int to an unsigned:

```C++
unsigned u = 10;
int i = -42;
std::cout << i + i << std::endl; // prints -84
std::cout << u + i << std::endl; // if 32-bit ints, prints 4294967264
```

Converting a negative number to unsigned behaves exactly as if we had attempted to assign that negative value to an unsigned object.
The value “wraps around” as described above.

```C++
unsigned u1 = 42, u2 = 10;
std::cout << u1 - u2 << std::endl; // ok: result is 32
std::cout << u2 - u1 << std::endl; // ok: but the result will wrap around
```

Expressions that mix signed and unsigned values can yield surprising results when the signed value is negative. It is essential to remember that signed values are automatically converted to unsigned.

```C++
unsigned a = 1;
int b = -1;
std::cout << a * b << std::endl; // ok: but the result will wrap around
```

### section 2.1.3 Literal

A value, such as 42, is known as a literal because its value is self-evident. Every literal has a type. The form and value of a literal determine its type.

```C++
20 /* decimal */
024 /* octal */
0x14 /* hexadecimal */
```

By default, decimal literals are signed.
Octal and hexadecimal literals can be either signed or unsigned types.

Character and Character String Literals:

```C++
’a’ // character literal
"Hello World!" // string literal
```

The type of a string literal is an array of constant chars. The compiler appends a null character (’\0’) to every string literal. Thus, the actual size of a string literal is one more than its apparent size.

```C++
\n // newline
\v // vertical tab
\\ // backslash
\r // carriage return


\t // horizontal tab
\b // backspace
\? // question mark
\f // formfeed

\a // alert (bell)
\" // double quote
\' // single quote
```

Specifying the Type of a Literal: 
```
L’a’ // wide character literal, type is wchar_t
u8"hi!" // utf-8 string literal (utf-8 encodes a Unicode character in 8 bits)
42ULL // unsigned integer literal, type is unsigned long long
1E-3F // single-precision floating-point literal, type is float
3.14159L // extended-precision floating-point literal, type is long double
```

## 2.2 Variables

### Variable Definitions
```C++
// ok: price is defined and initialized before it is used to initialize discount
double price = 109.99, discount = price * 0.16;
// ok: call applyDiscount and use the return value to initialize salePrice
double salePrice = applyDiscount(price, discount);
```
initialization and assignment are different operations in C++.

List Initialization:
```C++
int units_sold = 0;
int units_sold = {0};
int units_sold{0};
int units_sold(0);
```

The compiler will not let us list initialize variables of built-in
type if the initializer might lead to the loss of information
```C++
long double ld = 3.1415926536;
int a{ld}, b = {ld}; // error: narrowing conversion required
int c(ld), d = ld; // ok: but value will be truncated
```

Default Initialization:

Variables defined outside any function body are initialized to zero.

variables of built-in type defined inside a function are uninitialized. 
The value of an uninitialized variable of built-in type is undefined.
It is an error to copy or otherwise try to access the value of a variable whose value is undefined.


### section 2.2.2 Variable Declarations and Definitions
A declaration makes a name known to the program. A file that wants
to use a name defined elsewhere includes a declaration for that name. A definition
creates the associated entity.

```
extern int i; // declares but does not define i
int j; // declares and defines j

// Any declaration that includes an explicit initializer is a definition. 
extern double pi = 3.1416; // definition, doing so overrides the extern
```
Variables must be defined exactly once but can be declared many times.


## Section 2.3 Compound Types:
### section 2.3.1 references:
A reference defines an alternative name for an object.

```C++
int ival = 1024;
int &refVal = ival; // refVal refers to (is another name for) ival
int &refVal2; // error: a reference must be initialized
```

When we define a reference, instead of copying the initializer’s value, we bind the reference to its initializer. Once initialized, a reference
remains bound to its initial object. There is no way to rebind a reference to refer to a different object.

```C++
refVal = 2; // assigns 2 to the object to which refVal refers, i.e., to ival
int ii = refVal; // same as ii = ival

// ok: refVal3 is bound to the object to which refVal is bound, i.e., to ival
int &refVal3 = refVal;
// initializes i from the value in the object to which refVal is bound
int i = refVal; // ok: initializes i to the same value as ival
```

Reference definition:
```C++
int i = 1024, i2 = 2048; // i and i2 are both int s
int &r = i, r2 = i2; // r is a reference bound to i ; r2 is an int
int i3 = 1024, &ri = i3; // i3 is an int ; ri is a reference bound to i3
int &r3 = i3, &r4 = i2;  // both r3 and r4 are references
```

```C++
int &refVal4 = 10; // error: initializer must be an object
double dval = 3.14;
int &refVal5 = dval; // error: initializer must be an int object
```






