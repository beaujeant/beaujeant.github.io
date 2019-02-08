---
permalink: /appsec101/programming/
title: Programming
parent: Application Security 101
nav_order: 5
---

[<< Assembly](/appsec101/lab/){: .btn .btn-outline }
[Buffer overflow >>](/appsec101/assembly/){: .btn .btn-outline }

Programming
===========

[Learning C](https://www.learn-c.org/) or programming in general is beyond the scope of this course. You are actually expected to understand C and the main concepts of programming to follow this course. This chapter is just a reminder and allow me to have reference to some concepts later in course. The chapter [assembly](/appsec101/assemby/) is meant to match this chapter so you can see the transformation from C to assembly.


Variables
---------

People create programs to automate and fasten the process of data. Everything is about manipulating data. The data is located in memory, so for the CPU to manipulate the data, you need to provide the address in memory where the data is located. When writing code, it would be very confusing and obscure to use directly memory addresses (or offsets) when storing and manipulating data. Instead, we are using __variables__, a kind of _aliases_ for a memory locations. Variable allows developer to have meaningful name to identify/locate data and simplify the memory allocation.

Before a variable is used, it has to be __declared__. The syntax for declaring a variable is the following: `<type> <variable name>`. For instance, if you want to declare an _integer_ with the name _number_, you simply need to type:

```
int number;
```

You will find a list of the different types of variable in chapter [CPU](/appsec101/cpu#data-types). _Integer_ are declared with `int`, _float_ with `float`, _characters_ with `char` and _boolean_ with `bool`.

> Note: More types are available, but those are the most used.

There is a limited set of type available for variable. However, you are still able to define your own custom type with `struct`. Let say for instance you want to create a variable that stores time (date + time):

```
typedef struct
{
   int year;
   int month;
   int day;
   int hours;
   int minutes;
   int seconds;
} date;
```

Now that the type `date` is defined and initialized, we can declare variables (with the name _today_ for instance) with this type:

```
date today;
```

It is also possible to declare arrays of a specific type. In this case, we just add `[<size>]` right after the name of the variable. For instance, the following will declare an array of 11 characters:

```
char string[11];
```

> As mentioned earlier, variable are _alias_ of memory address. So whenever you want to manipulate the data, let say multiple the value pointed by the variable, you simply need to use the variable name, i.e. `number * 3`. However, in some case, you might want to know the actual address the variable is pointing to, you can use the symbol `&` right in front of the variable name, e.g. `&number`.

Declaration of variables tells the compiler (the tool responsible for the translation from C to machine instructions) what type of data the variable is expected to handle. With this information, the compiler will do the following:

* Before compiling, it will verify in the code if the data stored in the variable correspond to the type used when declared
* When using array, it will know the right offset in memory between the elements
* It will write instructions to allocate the appropriate amount of space in memory based on the type
* Select the right machine instructions based on the type

### Type verification

Let's consider the following code:

```
int number;
number = "Hello";
```

The variable `number` is declared as an _integer_, then the string "Hello" is assigned to it (actually the pointer to the string "Hello"), which is not an integer. Therefore, when compiling, the compiler throws the following error message:

```
warning: assignment makes integer from pointer without a cast [-Wint-conversion]
   number = "Hello";
```

Sometime, when possible, the compiler will fix automatically the problem:

```
int number;
number = 3.2;
```

Here, we assign a _float_ to the variable `number` which is supposed to handle _integer_. When compiling, the code is translated with the following assembly instruction:

```
mov DWORD PTR [ebp-0xc],0x3
```

It automatically converted the float `3.2` into the integer (`0x3`) and copied it in memory where the variable `number` is located (i.e. `ebp-0xc`). Don't worry if you didn't understand this last example, we will have a closer look at it in the next chapter.

### Browsing array

Declaring is also important for arrays. Elements in arrays are stored in memory one after the other. But each element takes a specific amount of space in memory depending on the type. For instance, let's consider the following array of 10 integers:

```
int array[10];
```

Let say the beginning of the array is located at the address `0xbfffef00`. The first element (`array[0]`) will be located at `0xbfffef00`, the second element (`array[1]`) will be located at `0xbfffef08` (`0xbfffef00` + size of _one_ integer), the third element (`array[2]`) will be located at `0xbfffef10` (`0xbfffef00` + _two_ time the size of one integer), etc. Now, let's consider the following array of 10 characters:

```
char string[10];
```

This array is located at `0xbfffee00`. The first element (`string[0]`) will be located at `0xbfffee00`, the second element (`string[1]`) will be located at `0xbfffee01` (`0xbfffee00` + size of _one_ character), the third element (`string[2]`) will be located at `0xbfffee00` (`0xbfffee02` + _two_ time the size of one character), etc.

So we can tell `string[2]` is the equivalent of the address `string + (2 * sizeof(char))`:

* `string`: When the variable name of an array is used without `[ ]`, using the name gets you the address in memory of the variable (array)
* `sizeof()`: Gets the size of an object or type
* `char`: Type with which the array has been declared

So basically, the type in the declaration of an array is used to determine where the next element is located (how many bytes should be skipped).

### Allocating memory

Declaring a variable also tells how much space should be allocated in memory. So for instance, if you declare an integer, 4 bytes will be allocated in the stack:

```
int number;
```

Will be translated as the following in machine instruction:

```
sub esp, 0x4
```

Which allocates 4 bytes in the stack. `ESP` points at the top of the stack, and if you look in chapter [memory](/appsec101/memory/), the stack is growing _backward_, so subtracting `ESP` means increasing the size of the stack.

### Assigning value

Once the variable declared, we can assign data and manipulate it. To assign a value to the variable, you simply need to the use the symbol `=`:

```
int number;
number = 3;
```

In a table, you will need to indicate the index (first element being `0`). So the following will set the value `L` at the third element of the array:

```
char string[11];
string[2] = "L";
```

Assigning value on struct is slightly different. The two most common way to set the variable is either by using `{}` with the value separated with a coma, or by using the sub-name separated with a `.`. Here is a mix of both:

```
today = { 2019, 1, 1, 0, 0, 0 };
today.day = 17;
today.hours = 15;
today.minutes = 6;
today.minutes = 45;
```

Now that the variables are declared and set, we can start manipulating them.


Branching
---------

When programming, you will most likely want at some point to verify or test some variables and do something accordingly. For instance, verifying if the password entered by the user is correct or verify if a file exist. These can be done thanks to __branching__. Branching is whenever the program has to choose between two (or more) branches based on one (or more) conditions. The most common form of branch is the __if__/__else__ statements:

```
if( pin_code == 1234 )
{
    printf("PIN correct!");
}
else
{
    printf("Wrong PIN!");
}
```

> __Note__: The _else_ statement is not mandatory.

In this example, the condition is `pin_code == 1234`, which verify whether the variable `pin_code` contains the integer value `1234`. If it is the case, it will print the message "PIN correct!", otherwise, it will print "Wrong PIN!".

You can have more than one conditions concatenated with the operator `&&` (i.e. _AND_) or `||` (i.e. _OR_). With `&&`, both condition must be true, while when using `||`, either or both must be true.

The result of a condition is always seen as a boolean in the __if__ statement. So the condition could simply be a single variable. If the variable is `0`, the condition will be seen as `false`, any other value will be seen as `true`. Condition can also be a comparison as seen earlier:

* `==`: returns `true` if both elements are equal
* `!=`: returns `true` if both elements are different
* `>`: returns `true` if the first element is bigger than the first element
* `>=`: returns `true` if the first element is bigger or equal than the second element
* `<`: returns `true` if the first element is smaller than the first element
* `<=`: returns `true` if the first element is smaller or equal than the second element

The boolean result can also be inverted with the operator `!` (i.e. _NOT_):

```
if( ! (pin_code == 1234) )
{
    printf("Wrong PIN!");
}
```


Loop
----

You may want at some point to repeat multiple times a succession of instructions. Let say you want to initialize a big array with `0`. You could do the following:

```
int table[100];
table[0] = 0;
table[0] = 0;
table[0] = 0;
table[0] = 0;
...
table[99] = 0;
table[100] = 0;
```

In this example, you would have a long repetitive list of instructions. If you suddenly want to initialize the array with `2` instead of `0`, you would have to change 100 lines. Furthermore, what if you don't know the size of the array and this depend on the user input. Instead, you could use the `for` or the `while` loop. Both loops takes one or more condition (like `if/else`). The condition dictate whether the loop should reiterate or not.

```
int table[100];
int i = 0;

while( i < 100 )
{
    table[i] = 0;
    i = i + 1;
}
```

The structure of a `for` loop allows you to condense a little bit the code that manages the loop variable (in this case, the variable `i`):

```
int table[100];

for( int i = 0; i < 100; i = i + 1)
{
    table[i] = 0;
}
```

As you can see, the condition of the `for` loop is split in 3 parts (separated with semi-colon):

* Declaration and assignment of the variable: `int i = 0`
* Condition to match in order to continue the loop: `i < 100`
* The operation to execute at the end of the iteration (usually in order to update the variable): `i = i + 1`

There is still one last loop variant, i.e. the `do while`. Basically, it works like the `while` loop except that the content of the loop is executed at least once, no matter if the condition is met or not.

```
int i = 0;

do
{
    printf("This is executed at least once");
    i = i + 1;
}
while( i <  10);
```


Functions
---------


call, arguments, returns


Common functions
----------------

### printf

### scanf

### strcpy

### malloc

### free


Compiler optimization
---------------------


* [Learning C](https://www.learn-c.org/) https://www.learn-c.org/


[<< Assembly](/appsec101/lab/){: .btn .btn-outline }
[Buffer overflow >>](/appsec101/assembly/){: .btn .btn-outline }
