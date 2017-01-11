CPP-ARC
=======

[![Build Status](https://img.shields.io/travis/macmade/CPP-ARC.svg?branch=master&style=flat)](https://travis-ci.org/macmade/CPP-ARC)
[![Issues](http://img.shields.io/github/issues/macmade/CPP-ARC.svg?style=flat)](https://github.com/macmade/CPP-ARC/issues)
![Status](https://img.shields.io/badge/status-inactive-lightgray.svg?style=flat)
![License](https://img.shields.io/badge/license-boost-brightgreen.svg?style=flat)
[![Contact](https://img.shields.io/badge/contact-@macmade-blue.svg?style=flat)](https://twitter.com/macmade)  
[![Donate-Patreon](https://img.shields.io/badge/donate-patreon-yellow.svg?style=flat)](https://patreon.com/macmade)
[![Donate-Gratipay](https://img.shields.io/badge/donate-gratipay-yellow.svg?style=flat)](https://www.gratipay.com/macmade)
[![Donate-Paypal](https://img.shields.io/badge/donate-paypal-yellow.svg?style=flat)](https://paypal.me/xslabs)

About
-----

**C++ Automatic Reference Counting**  
This project intends to simplify memory management in C++, using reference counting.  
When allocating memory with the `new` operator, CPP-ARC automatically adds a few bytes of memory to manage a **retain count** on the memory area.

Memory can then be **retained** or **released**.  
An allocation sets the retain count to 1. A retain increments it, and a release decrements it.  
When the retain count reaches 0, the memory is freed.

C++ classes are fully compatible with this scheme, as the destructors are called when the memory is freed.

This project also includes a template used to auto-release allocated memory, when it's not used anymore.

Documentation
-------------

### How-to: Retain / Release

Memory allocated with `new` (primitive types or classes) can be retained or released.  
To do so, simply call:

    template< typename T > T * CPPARC::retain( T * p );
    template< typename T > void CPPARC::release( T * p );

So for instance:

    class Point
    {
    	public:
    		
			int x;
			int y;
    };
    
	int main( void )
	{
		Point * p = new Point();
		char  * s = new char[ 10 ];
		
		CPPARC::retain( p );
		CPPARC::release( p );
		
		// s will be freed here
		CPPARC::release( s );
		
		// p will be freed here - destructor will be called
		CPPARC::release( p );
		
		return 0;
	}

### How-to: Auto-released pointers

Allocated memory can also be auto-released, when it's not used anymore.  
In order to to this, you can use the `CPPARC::AutoPointer` template class:

    class Point
    {
    	public:
    		
			int x;
			int y;
    };

	int main( void )
	{
		CPPARC::AutoPointer< Point > p = new Point();
		
		// Nothing to do here, as the Point object will be automatically freed.
		
		return 0;
	}
	
The `AutoPointer` template also provides direct access to class members:

    class Foo
    {
    	public:
    		
			void test( void )
			{
				std::cout << "hello, world" << std::endl;
			}
    };

	int main( void )
	{
		CPPARC::AutoPointer< Foo > f = new Foo();
		
		// Will output "hello, world", just as a normal pointer
		f->test();
		
		return 0;
	}
	
You can of course use the `AutoPointer` template for functions/methods with arguments, or as return type:

    class Point
    {
    	public:
    		
			int x;
			int y;
    };
    
	CPPARC::AutoPointer< Point > getPoint( void )
	{
		return new Point();
	}
	
	int main( void )
	{
		CPPARC::AutoPointer< Point > p = getPoint();
		
		// Nothing to do here, memory will be freed automatically.
		
		return 0;
	}

License
-------

CPP-ARC is released under the terms of the Boost Software License - Version 1.0.

Repository Infos
----------------

    Owner:			Jean-David Gadina - XS-Labs
    Web:			www.xs-labs.com
    Blog:			www.noxeos.com
    Twitter:		@macmade
    GitHub:			github.com/macmade
    LinkedIn:		ch.linkedin.com/in/macmade/
    StackOverflow:	stackoverflow.com/users/182676/macmade
