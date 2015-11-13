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
* [Item 1:  View C++ as a federation of languages.](#1) 
* [Item 2:  Prefer consts, enums, and inlines to #defines.](#2) 
* [Item 3:  Use const whenever possible.](#3) 
* [Item 4:  Make sure that objects are initialized before they're used.](#4) 
* [Item 5:  Know what functions C++ silently writes and calls.](#5) 
* [Item 6:  Explicitly disallow the use of compiler-generated functions you do not want.](#6) 
* [Item 7:  Declare destructors virtual in polymorphic base classes.](#7) 
* [Item 8:  Prevent exceptions from leaving destructors.](#8)
* [Item 9:  Never call virtual functions during construction or destruction.](#9)
* [Item 10: Have assignment operators return a reference to *this.](#10)
* [Item 11: Handle assignment to self in operator=.](#11)
* [Item 12: Copy all parts of an object.](#12)
* [Item 13: Use objects to manage resources.](#13)
* [Item 14: Think carefully about copying behavior in resource-manageing classes.](#14)
* [Item 15: Provide access to raw resources in resource-managing classes.](#15)
* [Item 16: Use the same form in corresponding uses of new and delete.](#16)
* [Item 17: Store newed objects in smart pointers in standalone statements.](#17)
* [Item 18: Make interfaces easy to use correctly and hard to use incorrectly.](#18)
* [Item 19: Treat class design as type design.](#19)
* [Item 20: Prefer pass-by-reference-to-const to pass-by-value.](#20)

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

<h4 id="6"><a href="#top"><font color="blue">Item 6: Explicitly disallow the use of compiler-generated functions you do not want.</font></a></h4>

there are two methods.

> To disallow functionality automatically provided by compilers, declare the corresponding member functions private and give no implementations. Using a base class like Uncopyable is one way to do this.

{% highlight c++ linenos=table %}
class Uncopyable {                                                     
protected:
    Uncopyable() {}
    ~Uncopyable() {}
private:
    Uncopyable( const Uncopyable & );
    Uncopyable &operator=( const Uncopyable & );
};

class HomeForSale : private Uncopyable {
    int a = 0;
};

int main()
{
    HomeForSale s1;
    HomeForSale s2( s1 );
    s1 = s2;
    return 0;
}                                                              
{% endhighlight %}

> Using delete and default.
	
{% highlight c++ linenos=table %}
class HomeSale {                                                       
public:
    HomeSale() = default;
    HomeSale( const HomeSale & ) = delete;
    HomeSale &operator=( const HomeSale & ) = delete;
private:
    int a = 0;
};

int main()
{
    HomeSale s1;
    HomeSale s2( s1 );
    s1 = s2;
    return 0;
}                                                             
{% endhighlight %}

<h4 id="7"><a href="#top"><font color="blue">Item 7: Declare destructors virtual in polymorphic base classes.</font></a></h4>

> The implementation of virtual functions requires that objects carry information that can be used at runtime to determine which virtual functions should be invoked on the object.
this information typically takes the form of a pointer called a vptr("virtual table pointer"). The vptr points to an array of function pointers called a vtbl("virtual table"); 
each class with virtual functions has an associated vtbl. When a virtual function is invoked on an object, the actual function called is determined by following the object's vptr
to a vtbl and then looking up the appropriate function pointer in the vtbl.

> The bottom line is that gratuitously declaring all destructors virtual is just as wrong as never declaring them virtual.

<h4><font color="#FF0000">Things to Remember</font></h4>

	Polymorphic base classes should declare virtual destructors. If a class has any virtual functions, it should have a virtual destructor.
	
	Classes not designed to be base classes or not designed to be used polymorphically should not declare virtual destructors.

<h4 id="8"><a href="#top"><font color="blue">Item 8: Prevent exceptions from leaving destructors.</font></a></h4>

<h4><font color="#FF0000">Things to Remember</font></h4>

	Destructors should never emit exceptions. If functions called in a destructor may throw, the destructor should catch any exceptions, then swallow them or terminate the program.
	
	If class clients need to be able to react to exceptions thrown during an operation, the class should provide a regular function that performs the operation.

<h4 id="9"><a href="#top"><font color="blue">Item 9: Never call virtual functions during construction or destruction.</font></a></h4>

> During base class construction, virtual functions never go down into derived classes.

> An Object doesn't become a derived class object until execution of a derived class contruction begins.

> Upon entry to the base class destructor, the object becomes a base class object.

> The only way to avoid this program is to make sure that none of your constructors or destructors call virtual functions on the object beging
created or destroyed and that all the functions they call obey the same constraint.

<h4><font color="#FF0000">Things to Remember</font></h4>

	Don't call virtual functions during construction or destruction, because such calls will never go to a more derived class than that of the currently executing constructor or destructor.
	
<h4 id="10"><a href="#top"><font color="blue">Item 10: Have assignment operators return a reference to *this.</font></a></h4>

<h4><font color="#FF0000">Things to Remember</font></h4>

	Have assignment operators return a reference to *this.

<h4 id="11"><a href="#top"><font color="blue">Item 11: Handle assignment to self in operator=.</font></a></h4>

Four steps to improve the operator=.

{% highlight c++ linenos=table %}
Widget &Widget::operator=( const Widget &rhs )                         
{
	delete pb;
	pb = new Bitmap( *rhs.pb );
	return *this;
}
	
Widget &Widget::operator=( const Widget &rhs )
{
	if( this != &rhs )
	{
		delete pb;
		pb = new Bitmap( &rhs.pb );
	}
	return *this;
}
	
Widget &Widget::operator=( const Widget &rhs )
{
	Bitmap *pOrig = pb;
	pb = new Bitmap( *rhs.pb );
	delete pOrig;
	return *this;
}
	
Widget &Widget::operator=( const Widget &rhs )
{
	Widget temp( rhs );
	swap( temp );
	return *this;
}
{% endhighlight %}

<h4><font color="#FF0000">Things to Remember</font></h4>

	Make sure operator= is well-behaved when an object is assigned to itself. Techniques include comparing addresses of source and target objects, careful statement ordering, and copy-and-swap.
	
	Make sure that any function operating on more than one object behaves correctly if two or more of the objects are the same.

<h4 id="12"><a href="#top"><font color="blue">Item 12: Copy all parts of an object.</font></a></h4>

<h4><font color="#FF0000">Things to Remember</font></h4>

	Copying functions should be sure to copy all of an object's data members and all of its base class parts.
	
	Don't try to implement one of the copying functions in terms of the other. Instead, put common functionality in a third function that both call.

<h4 id="13"><a href="#top"><font color="blue">Item 13: Use objects to manage resources.</font></a></h4>

> by putting resources inside objects, we can rely on C++'s automatic destructor invocation to make sure that the resources are released.

> RAII: Resource Acquistion Is Initialization

<h4><font color="#FF0000">Things to Remember</font></h4>

	To prevent resources leaks, use RAII objects that acquire resources in their constructors and release them in their destructors.
	
	Two commonly useful RAII classes are tr1::shared_ptr and auto_ptr. tr1::shared_ptr is usually the better choice, because its behavior when copied is intuitive. Copying an auto_ptr sets it to null.

<h4 id="14"><a href="#top"><font color="blue">Item 14: Think carefully about copying behavior in resource-manageing classes.</font></a></h4>

> Copy the underlying resource.

> Transfer ownership of the underlying resource.

<h4><font color="#FF0000">Things to Remember</font></h4>

	Copying an RAII object entails copying the resource it manages, so the copying behavior of the resource determines the copying behavior of the RAII object.
	
	Common RAII class copying behaviors are disallowing copying and performing reference counting, but other behaviors are possible.
	


<h4 id="15"><a href="#top"><font color="blue">Item 15: Provide access to raw resources in resource-manageing classes.</font></a></h4>

<h4><font color="#FF0000">Things to Remember</font></h4>

	APIs often require access to raw resources, so each RAII class should offer a way to get at the resource it manages.
	
	Access may be via explicit conversion or implicit conversion. In general, explicit conversion is safer, but implicit conversion is more convenient for clients.

<h4 id="16"><a href="#top"><font color="blue">Item 16: Use the same form in corresponding uses of new and delete.</font></a></h4>

> When you employ a new expression, two things happen: 1. memory is allocated( operator new ); 2. one or more constructors are called for that memory.

> When you employ a delete expression, two other things happen: one or more destructors are called for the memory, then the memory is deallocated( operator delete ).

<h4><font color="#FF0000">Things to Remember</font></h4>

	If you use [] in a new expression, you must use [] in the corresponding delete expression. If you don't use [] in a new expression, you mustn't use [] in the corresponding delete expression.

<h4 id="17"><a href="#top"><font color="blue">Item 17: Store newed objects in smart pointers in standalone statements.</font></a></h4>

<h4><font color="#FF0000">Things to Remember</font></h4>

	Store newed objects in smart pointers in standalone statements. Failure to do this can lead to subtle resource leaks when exceptions are thrown.

<h4 id="18"><a href="#top"><font color="blue">Item 18: Make interfaces easy to use correctly and hard to use incorrectly.</font></a></h4>

> unless there's a good reason not to, have your types behave consistently with the built-in types.

<h4><font color="#FF0000">Things to Remember</font></h4>

	Good interfaces are easy to use correctly and hard to use incorrectly. You should strive for these characteristics in all your interfaces.
	
	Ways to facilitate correct use include consistency in interfaces and behavioral compatibility with built-in types.
	
	Ways to prevent errors include creating new types, restricting operations on types, constraining object values, and eliminating client resource management responsibilities.
	
	tr1::shared_ptr supports custom deleters. This prevents the cross-DLL problem, can be used to automatically unlock mutexes, etc.

<h4 id="19"><a href="#top"><font color="blue">Item 19: Treat class design as type design.</font></a></h4>

* How should objects of your new type be created and destroyed?
* How should object initialization differ from object assignment?
* What does it mean for objects of your new type to be passed by value?
* What are the restrictions on legal values for your new type?
* Does your new type fit into an inheritance graph?
* What kind of type conversion are allowed for your new type?
* What operators and functions make sense for the new type?
* What standard functions should be disallowed?
* Who should have access to the members of your new type?
* What is the "undeclared interface" of your new type?
* How general is your new type?
* Is a new type really what you need?

<h4><font color="#FF0000">Things to Remember</font></h4>

	Class design is type design. Before defining a new type, be sure to consider all the issues discussed in this Item.

<h4 id="20"><a href="#top"><font color="blue">Item 20: Prefer pass-by-reference-to-const to pass-by-value.</font></a></h4>

> references are typically implemented as pointers, so passing something by reference usually means really passing a pointer.

<h4><font color="#FF0000">Things to Remember</font></h4>

	Prefer pass-by-reference-to-const over pass-by-value. It's typically more efficent and it avoids the slicing problem.
	
	The rule doesn't apply to built-in types and STL iterator and function object types. For them, pass-by-value is usually appropriate.

