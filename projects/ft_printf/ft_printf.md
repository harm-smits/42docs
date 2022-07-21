# ft_printf
In this project we recreate the printf function and learn about variadic functions. 

### Index:
- [ft_printf](#ft_printf)
		- [Index:](#index)
	- [The behaviour of printf](#the-behaviour-of-printf)
	- [+ **%%**      - Prints a percent sign.](#---------prints-a-percent-sign)
	- [Variadic arguments](#variadic-arguments)
		- [va_arg()](#va_arg)
	- [Steps for writing your own ft_printf](#steps-for-writing-your-own-ft_printf)
	- [Type conversion](#type-conversion)
		- [Hexadecimals](#hexadecimals)

## The behaviour of printf
The prototype of printf is as follows: 
```
int	ft_printf(const char *str, ...)
```
It will return the amount of characters it prints out. The first argument will contain all characters that need to be printed, along with variables if desired. This will be done with a % followed by a letter signifying the data type of the variable. 
In this project we are handling the following cases: 

+ **%c**      - A single character. (putchar)
+ **%s**      - A string. (putstr)
+ **%d/i** - An integer, both cases mean the same thing. (putnbr)
+ **%u**      - Unsigned integer. 
+ **%%**      - Prints a percent sign.
-----------------------------------------
Cases using hexadecimal conversion: 
+ **%x**      - Integer converted to hexidecimal, lowercase format.
+ **%X**      - Same but in uppercase format.
+ **%p**      - A void * pointer argument converted to hexadecimal.

An example of the usage of printf:
```c
int32_t main(void)
{
  ft_printf("Hello %s, do you have %d cookies?", "Djaisin", 42);
  return (EXIT_SUCCESS);
}
```
Which will print: 
``` 
"Hello Djaisin, do you have 42 cookies?"
```

## Variadic arguments
Notice how printf always needs one string as input, but can be followed by any number of arguments. Normally a function has a very specific predetermined amount of arguments and data type. This is done with the magic of variadic functions! Taking in several different data types is only really possible because the first string will contain the information of the given data types. 
You really only need 3 [variadic functions](https://linux.die.net/man/3/va_arg) to make this work: va_start(), va_arg() and va_end(). Include the header <stdarg.h> to use these. 
First you need to initialise a variable with the type va_list, which will contain a list of the variadic arguments provided to the function, then you can use va_start to initialise this list. The first argument will be the va_list variable and the second has to argument that preceeded the variadic arguments in the function. Then at the very end va_end has to be used to free allocated memory in the background. Syntax: 

```c
int32_t	ft_printf(const char *str, ...)
{
  va_list argp; 

  va_start(argp, str); 
  va_end(argp);
}
```
### va_arg()
Now the most important function, va_arg()! Using it is like pulling the next argument out of the given list. First argument is the va_list variable and secondly you need to provide the type, so the variable being pulled out of the list can be correctly initialised. After using it, it will automatically step to the next argument of the list. Syntax example: `va_arg(argp, int);`

## Steps for writing your own ft_printf
+ Initialise va_list by using va_start and already include va_end at the end of your main function
+ Itterate through the given string, write characters if you don't encounter a % and keep count of the amount of characters written
+ If you encounter a %, check which value is given afterwards, use va_arg with the type specified and convert that type to a string and output it to STDOUT.
+ Make sure to return the amount of characters written at the end!
+ If there is no valid pointer given to the %p case, you should print out "0x0" on Mac (it would be "(nil)" on Linux)
+ Compare your printf against the behaviour of the original one, to make sure it is correct

## Type conversion
If you are given an integer for instance, this is not the correct type to create readable output. In this case the functionality of putnbr() from the standard library has to be written, to convert the integer to an array of chars. 

### Hexadecimals 
A hexadecimal is a number with a base of 16 instead of the regular 10. This is how it is represented: 0123456789ABCDEF with F being 15.
Essentially you can use almost the exact same logic you would use to convert and print a regular base 10 integer (like with putnbr) except you change the base of the number in your calculation and map the result to the representation of a hexadecimal number.

<img src="https://user-images.githubusercontent.com/13866954/179416564-44ac2c37-600c-478c-a245-be5e811cfabb.png" height="250"/> <img src="https://user-images.githubusercontent.com/13866954/179412288-8f03a743-bed9-45c5-9188-e72e1c3939d3.png" height="250"/> 
