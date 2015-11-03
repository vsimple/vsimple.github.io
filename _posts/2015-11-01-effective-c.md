---
layout: post
title: "Effective C++"
description: ""
category: C++
tags: [Effective C++]
---
{% include JB/setup %}

<a name="top"></a>

###Items
---
* [Item 1: View C++ as a federation of languages.](#1) 
* [Item 2: Prefer consts, enums, and inlines to #defines.](#2) 
* [Item 3: Use const whenever possible.](#3) 
* [Item 4: Make sure that objects are initialized before they're used.](#4) 
* [Item 5: Know what functions C++ silently writes and calls.](#5) 

---
	
<h4 id="1"><a href="#top"><font color="blue">Item 1: View C++ as a federation of languages.</font></a></h4>

> C++ is a multiparadigm programming language, one supporting a commbination of procedural, object-oriented, functional, generic, and metaprogramming features.
	
	| sublanguages           | keywords                   |
	| -----------------------|----------------------------|
	| C                      | superior                   |
	| Object-Oriented C++    | object-oriented            |
	| Template C++           | the generic programming    |
	| The STL                | a template library         |
	
> it's a federation of four sublanguages, each with its own conventions.

<h4><font color="#FF0000">Things to Remember</font></h4>
	Rules for effective C++ programming vary, depending on the part of C++ you are using.


<h4 id="2"><a href="#top"><font color="blue">Item 2: Prefer consts, enums, and inlines to #defines.</font></a></h4>

	prefer the compiler to the preprocessor
	
> the preprocessor's blind substitution of the macro name in multiply copies, while the use of the constant should never result in more than one copy.

> Class-specific constants that are static and of integral type(e.g., integer, chars, bools) are an exception. You can declare them and use them without providing a definition.


{% highlight c++ linenos=table %}
#include <iostream>                                                    
using namespace std;

struct GamePlayer {
    static const int NumTurns = 5;
    int n[ NumTurns ];
};

const int GamePlayer::NumTurns;

int main()
{
    cout << GamePlayer::NumTurns << endl;
    cout << &GamePlayer::NumTurns << endl;
    return 0;
}                                                                
{% endhighlight %}


<h4><font color="#FF0000">Things to Remember</font></h4>
	For simple constants, prefer const objects or enums to #defines.
	
	For function-like macros, prefer inline functions to #defines.
	
<h4 id="3"><a href="#top"><font color="blue">Item 3: Use const whenever possible.</font></a></h4>

> const Ration operation*( const Rational &lhs, const Rational &rhs );
> make (a*b) = c error

	bitwise constness and logical constness

<h4><font color="#FF0000">Things to Remember</font></h4>
	Declaring something const helps compilers detect usage errors. const can be applied to objects at any scope, to function parameters and return types, and to member function as a while.
	
	Comilers enforce bitwise constness, but you should program using logical constness.
	
	When const and non-const member functions have essentially identical implementations, code duplication can be avoid by having the non-const version call the const version.

<h4 id="4"><a href="#top"><font color="blue">Item 4: Make sure that objects are initialized before they're used.</font></a></h4>

> always initialize your objects before you use them.
> make sure that all constructors initialize everything in the object.
> data members of an object are initialized before the body of a constructor is entered.

> The assignment-based version first called default constructors to initialize data members, then promptly assigned new values on top of the default-constructed ones.
> For objects of built-in type, there is no difference in cost between initialization and assignment.
> compilers will automatically call default constructors for data members of user-defined types when those data members have no initializers on the member initialization list.

> data members that are const or are references must be initialized;
> the easiest choice is to always use the initialization list.

> base classes are initialized before derived classes, and within a class, data members are initialized in the order in which they are declared.

> A static object is one that exists from the time it's constructed until the end of the program.

<h4><font color="#FF0000">Things to Remember</font></h4>
	Manually initialize objects of built-in type, because C++ only sometimes initializes them itself.
	
	In a constructor, prefer use of the member initialization list to assignment inside the body of the constructor. List data members in the initialization list in the same order they're declared in the class.
	
	Avoid initialization order problems across translation units by replacing non-local static objects with local static objects.
	
<h4 id="5"><a href="#top"><font color="blue">Item 1: View C++ as a federation of languages.</font></a></h4>

<h4><font color="#FF0000">Things to Remember</font></h4>
	Compilers may implicity generate a class's default constructor, copy constructor, copy assignment operator, and destructor.
