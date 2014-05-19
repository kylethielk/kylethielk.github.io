---
author: Kyle Thielk
comments: true
date: 2013-12-14 14:08:40+00:00
layout: post
slug: pointers-in-objective-c
title: Pointers in Objective-C
wordpress_id: 970
categories:
- Tech
---

Objective-C is a strict superset of C. That means every feature of C is alive and kicking in Objective-C, pointers included. Five years of pointer free programming has made me...rusty. As the saying goes, "There is no better way to learn than to teach".

{% codeblock lang:objective-c %}int a = 2; //Your standard variable
int *b = &a; //An integer pointer

//Prints "The value of a is 2"
NSLog(@"The value of a is %i.",a); 

*b = 3;

//Prints "The value of a is 3"
NSLog(@"The value of a is %i",a); 
{% endcodeblock %}

This is your standard pointer example. _int *b_ is an integer pointer that we are assigning the address of _int a_. The asterisk _*_ is generally called the _dereferencing_ or _indirection_ operator and means "Give me the value stored at the address this pointer points to". The _&_ operator means "Give me the address in memory that this variable is stored at". 

What can often be confusing is that with one line declaration and initialization short-hand, the asterisk is not performing any dereferencing but is actually simply declaring that this is an integer pointer.

{% codeblock lang:objective-c %}

int *b = &a;

//..is the same as 

int *b;
b = &a;
{% endcodeblock %}

This snippet of code illustrates how this actually works in memory.

{% codeblock lang:objective-c %}
int a = 2;
int *b = &a;

NSLog(@"%p -> %i",&a, a);
NSLog(@"%p -> %p",&b, b);
{% endcodeblock %}

The output:

    
    
    <em>0x7fff5fbff88c -> 2
    0x7fff5fbff880 -> 0x7fff5fbff88c</em>
    


left number: the variables location in memory. 
right number: the value it stores.

Remember a pointer is like any other variable, it has its own location in memory but its value is simply another memory address. So when you write "*b" you are saying "Give me the value (2) stored at the address (0x7fff5fbff88c).



## Pointers to Pointers



With pointers to pointers things get a little more complicated. A pointer can actually point to another pointer. Take the following code.

{% codeblock lang:objective-c %}

int a = 2;
int *b = &a;
int **c = &b; //A pointer to a pointer

NSLog(@"%p -> %i",&a, a);
NSLog(@"%p -> %p",&b, b);
NSLog(@"%p -> %p", &c, c); 
{% endcodeblock %}

The output: 

    
    
    <em>0x7fff5fbff88c -> 2
    0x7fff5fbff880 -> 0x7fff5fbff88c
    0x7fff5fbff878 -> 0x7fff5fbff880</em>
    


_int **c_ points to _int *b_ which in turn points to _int a_. To further illustrate the following examples show how we can reference the same thing in multiple ways.

a == *b == **c; //The value 2
&a; == b == *c; //The address of a or 0x7fff5fbff88c

Pointers to pointers have practical uses in Objective-C when a method actually needs to return multiple values. 

{% codeblock lang:objective-c %}
- (BOOL) printNumber:(int)number error:(NSError **)errorPtr;

NSError *anyError;
[myClass printNumber:&anyError]
{% endcodeblock %}

_printNumber_ in this case can return a BOOL with the standard return statement ***and*** an optional error by setting our _*anyError_ pointer to a new NSError object.

For example a sample implementation of printNumber:

{% codeblock lang:objective-c %}
- (BOOL) printNumber:(int)number error:(NSError **)errorPtr
{
    if(number < 0)
    {
        NSError *newError = [MyErrorFactory newErrorWithDesc:@"Negative numbers are not allowed"];
        *errorPtr = newError;
        return NO;
    }
    else
    {
        NSLog(@"Your number is %i",number);
        return YES;
    }
}
{% endcodeblock %}


