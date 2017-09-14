---
layout: post
title: "Create and consume C++ Class DLL on Windows"
date: 2017-09-04 22:49:23 +0700
comments: true
categories: [cpp, visual studio, windows]
---
{% img left /images/logo/cpp.png %}

while visiting clients of the company I work on, sometime I still found some applications especially desktop application build on unmanaged code (such as Delphi, Visual Basic 6, C++, etc). Even though at the time of this blog post, many application build on .NET (managed code) on Windows platform. There are various reasons why they do not migrate to managed code which has some advantages over unmanaged code (such as the application still run well with the version of OS they use, rewrite app will need extra cost, etc). This means unmanaged code application is not dead at all for LOB app, even though the percentage is much lower than the managed one.

Maybe this topic seems out of date topic in the .NET era, but at least this post as a note for my self in case I need it on the other day.

While developing an application, usually we want to share some of our code with other application. Dynamic Link Library (DLL) is Microsoft's implementation of the shared library concept in the Microsoft Windows. The term DLL in this post will refer to unmanaged code and only focus to the one build with Visual C++ compiler on Windows environment.

When we create a DLL, we also create a .lib file that contains information of exported class or functions. When we build an executable that calls the DLL, the linker uses the exported symbols in the .lib file to store this information for the loader. When the loader loads a DLL, the DLL is mapped into the memory space of the executable.

An executable file links to (or loads) a DLL in one of two ways, *implicit* or *explicit linking*. In this post will create simple sample both of them how C++ class exported in the two ways. The samples in this post created using IDE Microsoft Visual Studio 2013 Ultimate. To simplify the code, I just created a single solution contains a Win32 DLL project and a console application client. The DLL project contains classes for both sample *implicit* and *explicit linking*. Either the console application contains sample code for *implicit* and *explicit linking* caller. Here is the classes I use in this sample.

{% img center /images/post/2017-09-04-classdiagram.png %}


## Implicit Linking
*Implicit linking*, where the operating system loads the DLL when the executable using it is loaded. The executable client calls the exported functions of the DLL just as if the functions were statically linked and contained within the executable. *Implicit linking* is sometimes referred to as static load or load-time dynamic linking<sup>[4]</sup>. Now let's create a sample of DLL with *implicit linking*.

First of all, create an empty solution in Visual Studio by selecting ```File``` > ```New Project``` > scroll down on the left pane, expand ```Other Project Types``` > ```Visual Studio Solutions``` select ```Black Solution```. Fill the solution name as ```VCppDLL```.

Now we have an empty solution in Visual Studio. Right click the ```VCppDLL``` solution > ```Add``` > ```New Project```. In the left pane of the New Project dialog box, expand Installed templates ```Visual C++```, and then select ```Win32```. Fill the project name as **MathWin32DLL**, then click OK.

On the Win32 Application Wizard dialog in the ```Application Settings``` part, select ```DLL``` and check ```Empty project```, then click Finish

{% img center /images/post/2017-09-04-Win32AppWizard.png %}

Now we have an empty C++ DLL project in the Visual Studio solution. As the class diagram picture above, let create a simple ```BaseMath``` class. Right click the MathWin32DLL project > ```Add``` > ```Class```. On ```Visual C++``` template on the left pane dialog, select ```C++ Class``` > click ```Add```. On the ```Generic C++ Class Wizard```, fill the ```Class name``` as **BaseMath** then click ```Finish```. Edit the ```BaseMath.h``` with the following code.

```cpp BaseMath.h
// If you are building the DLL project on the command line,
// use the /D compiler option to define the MATHDLLWIN32_EXPORTS symbol.

#pragma once

#ifdef MATHWIN32DLL_EXPORTS
#define Math_API __declspec(dllexport)
#else
#define Math_API __declspec(dllimport)
#endif // MATHWIN32DLL_EXPORTS

#include <string>

using namespace std;

namespace core
{
	class BaseMath
	{
	public:
		virtual void Destroy()
		{
			delete this;
		}

		virtual string Say(string& s) = 0;
		virtual double Calculate(const double a, const double b) = 0;
	};
}
```

We can delete ```BaseMath.cpp``` file since we will make the ```BaseMath``` as an *abstract class*.


In Visual Studio, by default the New Project template for a DLL adds PROJECTNAME_EXPORTS to the defined preprocessor symbols for the DLL project. We can see the preprocessor symbols definition in ```Property Pages``` of **MathWin32DLL** project in the ```Configuration Properties``` > ```C/C++``` > ```Preposesor``` > ```Preposesor Definitions```.

{% img center /images/post/2017-09-04-preposesordefinitions.png %}

In the code of ```BaseMath.h```, when ```MATHWIN32DLL_EXPORTS``` symbol is defined, the ```Math_API``` symbol is set to ```__declspec(dllexport) ``` modifier otherwise it is set to ```__declspec(dllimport)```. The ```__declspec(dllexport) ``` modifier can be applied to classes, functions, or variables that tells the compiler and linker to export them from the DLL so that it can be used by other applications.

Meanwhile when we include ```BaseMath.h``` in client project, ```Math_API``` is set to ```__declspec(dllimport)```. This modifier optimizes the import of the exported class in an application.


For the next, let's create another class called ```AddOperationMath```. Edit the ```AddOperationMath.h``` and ```AddOperationMath.cpp``` respectively as follow.


```cpp AddOperationMath.h
#pragma once
#include "BaseMath.h"

using namespace std;

namespace core
{
  // MS Visual C++ compiler emits C4275 warning about not exported base class.
	class Math_API AddOperationMath : public BaseMath
	{
	public:
		AddOperationMath();
		virtual ~AddOperationMath();


		string Say(string& s);
		double Calculate(const double a, const double b);

		static double Add(const double a, const double b);
	};
}

```

```cpp AddOperationMath.cpp

#include "AddOperationMath.h"

namespace core
{
	AddOperationMath::AddOperationMath()
	{
	}

	AddOperationMath::~AddOperationMath()
	{
	}

	string AddOperationMath::Say(string& s)
	{
		string result =  s + " is calling add operation of class AddOperationMath";
		return result;
	}

	double AddOperationMath::Calculate(const double a, const double b)
	{
		return a + b;
	}

	double AddOperationMath::Add(const double a, const double b)
	{
		return a + b;
	}
}
```

The class ```AddOperationMath``` inherits from ```BaseMath```. We also mark the ```AddOperationMath``` class with ```Math_API``` macro that's defined in ```BaseMath.h``` which means we will expose the ```AddOperationMath``` class in the DLL to executable client application. When we compile the DLL project, we should get a warning

``` text
warning C4275: non dll-interface class 'core::BaseMath' used as base for dll-interface class 'core::AddOperationMath'

```

In this case, ideally we should export (mark with ```Math_API``` macro) both ```core::BaseMath``` and ```core::AddOperationMath``` to make the compiler does not fire the warning message.

To complete our sample, let's create another project called ***MathWin32ClientConsole*** as we did the creation of ***MathWin32DLL*** project, except select ```Console Application``` instead of ```DLL``` in the ```Application Settings``` dialog.

In the ***MathWin32ClientConsole*** project, right click > ```Add``` > ``` New Item```. Select ```Visual C++``` project template on the left pane, then select ```C++ File (.cpp)```. Fill the name with ```Main.cpp```.

To make the ***MathWin32ClientConsole*** project has reference to ***MathWin32DLL*** project, right click ***MathWin32ClientConsole*** project > ```Properties```. Scroll up the ```Property Pages``` dialog, expand ```Common Properties``` on the left pane > select ```References```. Click ```Add New Reference``` button, select ```Projects``` and check the ```MathWin32DLL``` > ```OK```. Now you should see ```MathWin32DLL``` added to the ```References``` pane as the following picture.

 {% img center /images/post/2017-09-04-MathWin32ClientConsoleReference.png %}

 To make the ```AddOperationMath``` class is recognized in the ***MathWin32ClientConsole*** project, we have to include ```AddOperationMath.h```. We can copy the ```AddOperationMath.h``` and ```BaseMath.h``` to the ***MathWin32ClientConsole*** project. But it is not a good way in our scenario, because if we make changes to one of them, we have to recopy it to the ***MathWin32ClientConsole*** project directory. To avoid this manual copy, we can include the ***MathWin32DLL*** project directory to the ***MathWin32ClientConsole*** so that we can include any header files of ***MathWin32DLL*** to ***MathWin32ClientConsole*** if needed. To do that open the Property pages of ***MathWin32ClientConsole***, select ```Configuration Properties``` > ```C/C++``` > ```General```. Select the drop-down control next to the ```Additional Include Directories``` edit box, and then choose ```<Edit...>```. Select the top pane of the ```Additional Include Directories``` dialog box to enable an edit control. In the edit control, fill ```$(SolutionDir)\MathWin32DLL``` which tells to Visual Studio to scan or search header files that we include in directory ```MathWin32DLL``` inside solution directory.

 {% img center /images/post/2017-09-04-IncludeMathWin32DLLDirectory.png %}

Now we can include header file defined in ```MathWin32DLL``` from ```MathWin32ClientConsole```. Let create code that call class defined in the DLL.

``` cpp Main.cpp

#include <iostream>
#include <string>

#include "AddOperationMath.h"

using namespace std;


void CallDLLByImplicitLinking(double a, double b, string s);

int main()
{
	double a = 2;
	double b = 4;
	string s = "neutro";

	CallDLLByImplicitLinking(a, b, s);


	cout << "Press any key to exit ";
	cin.get();

	return 0;
}



void CallDLLByImplicitLinking(double a, double b, string s)
{
	cout << a << " + " << b << " = " << core::AddOperationMath::Add(a, b) << endl;

	core::AddOperationMath* math = new core::AddOperationMath();
	cout << math->Say(s) << endl;

	delete math;

	cout << endl << "===============================================================" << endl;
}

```

``` text Output
2 + 4 = 6
neutro is calling add operation of class AddOperationMath

Press any key to exit

```

> There is no need to explicitly specify a calling convention for exporting classes or their methods. By default, the C++ compiler uses the ```__thiscall``` calling convention for class methods. However, due to different naming decoration schemes that are used by different compilers, the exported C++ class can only be used by the same compiler and by the same version of the compiler. Only the MS Visual C++ compiler can use this DLL now. Both the DLL and the client code must be compiled with the same version of MS Visual C++ in order to ensure that the naming decoration scheme matches between the caller and the callee<sup>[5]</sup>


To use a DLL by *implicit linking*, an executable must include the header files that declare the data, functions or C++ classes exported by the DLL in each source file that contains calls to the exported data, functions, and classes.  The classes, functions, and data exported by the DLL must all be marked ```__declspec(dllimport)``` in the header file. From a coding perspective, calls to the exported functions are just like any other function call.

To build the calling executable file, we must link with the import library (.lib). If we use an external makefile or build system, we need to specify the file name of the import library where we list other object (.obj) files or libraries that we link.

The operating system must be able to locate the DLL file when it loads the calling executable. This means that we must deploy or verify the existence of the DLL when our application is installed.


## Explicit Linking
*Explicit linking*, where the operating system loads the DLL on demand at runtime. An executable that uses a DLL by *explicit linking* must make function calls to explicitly load and unload the DLL and to access the functions exported by the DLL. Unlike calls to functions in a statically linked library, the client executable must call the exported functions in a DLL through a function pointer. *Explicit linking* is sometimes referred to as dynamic load or run-time dynamic linking<sup>[4]</sup>.

To use a DLL by *explicit linking*, applications must make a function call to explicitly load the DLL at run time. To explicitly link to a DLL, an application must <sup>[4]</sup>:

1. Call ```LoadLibrary```, ```LoadLibraryEx```, or a similar function to load the DLL and obtain a module handle.

2. Call ```GetProcAddress``` to obtain a function pointer to each exported function that the application calls. Because applications call the DLL functions through a pointer, the compiler does not generate external references, so there is no need to link with an import library. However, you must have a typedef or using statement that defines the call signature of the exported functions that you call.

3. Call ```FreeLibrary``` when done with the DLL.

To create a sample for *explicit linking*, we will use an abstract interface (a class with pure virtual methods, and no data) and create a factory method for object instantiation.

On the ***MathWin32DLL*** create a new class called ```LogarithmicMath```. Edit the header and implementation files as follow

```cpp LogarithmicMath.h

#pragma once

#include "BaseMath.h"

namespace core
{
	class LogarithmicMath : public BaseMath
	{
	public:
		LogarithmicMath();
		virtual ~LogarithmicMath();


		string Say(string& s);
		double Calculate(const double a, const double b);
	};
}

```

```cpp LogarithmicMath.cpp
#include "LogarithmicMath.h"
#include <math.h>

namespace core
{
	LogarithmicMath::LogarithmicMath()
	{
	}


	LogarithmicMath::~LogarithmicMath()
	{
	}

	string LogarithmicMath::Say(string& s)
	{
		string result = s + " is calling Logarithmic operation of class LogarithmicMath";
		return result;
	}

	double LogarithmicMath::Calculate(const double a, const double b)
	{
		return log10(b) / log10(a);
	}
}
```

Next, create a ```Factory``` class that encapsulates ```LogarithmicMath``` instantiation and will be called from client app.

```cpp Factory.h
#include "BaseMath.h"

using namespace std;

extern "C" Math_API core::BaseMath* __cdecl CreateLogarithmicMath();

```

```cpp Factory.cpp
#include "Factory.h"
#include "LogarithmicMath.h"

using namespace std;

core::BaseMath* CreateLogarithmicMath()
{
	return new core::LogarithmicMath();
}

```

We can see that the ```LogarithmicMath``` class look like a standard C++ class. Instead of directly export the ```LogarithmicMath``` class, we use ```Factory``` that handle the export technics.


In the ```Factory.h``` defined ```extern "C"``` which tells the C++ compiler that the linker should use the C calling convention. It is required in order to prevent the mangling of the function name. So, this function is exposed as a regular C function, and can be easily recognized by any C-compatible compiler. The name itself is exported from the DLL unmangled (```CreateLogarithmicMath```). The ```Math_API``` tells the linker to export the ```CreateLogarithmicMath``` method from the DLL. ```__cdecl``` is the default calling convention for C and C++ programs.

Now let create a sample code in the ***MathWin32ClientConsole*** by editing the ```Main.cpp``` as following.

```cpp Main.cpp

#include <iostream>
#include <string>

#include <Windows.h>

#include "AddOperationMath.h"

using namespace std;

typedef core::BaseMath* (__cdecl *LogarithmicMathFactory)();

void CallDLLByImplicitLinking(double a, double b, string s);
void CallDLLByExplicitLinking(double a, double b, string s);

int main()
{
	double a = 2;
	double b = 4;
	string s = "neutro";

	CallDLLByImplicitLinking(a, b, s);
	CallDLLByExplicitLinking(a, b, s);


	cout << "Press any key to exit ";
	cin.get();

	return 0;
}



void CallDLLByImplicitLinking(double a, double b, string s)
{
	cout << a << " + " << b << " = " << core::AddOperationMath::Add(a, b) << endl;

	core::AddOperationMath* math = new core::AddOperationMath();
	cout << math->Say(s) << endl;

	delete math;

	cout << endl << "===============================================================" << endl;
}


void CallDLLByExplicitLinking(double a, double b, string s)
{
	HMODULE dll = LoadLibrary(L"MathWin32DLL.dll");
	if (!dll)
	{
		cout << "Fail load library" << endl;
		return;
	}

	LogarithmicMathFactory factory = reinterpret_cast<LogarithmicMathFactory>(GetProcAddress(dll, "CreateLogarithmicMath"));

	if (!factory)
	{
		cerr << "Unable to load CreateLogarithmicMath from DLL!\n";
		FreeLibrary(dll);
		return;
	}

	core::BaseMath* instance = factory();
	cout << a << " log (" << b << ") = " << instance->Calculate(a, b) << endl;
	cout << instance->Say(s) << endl;

	instance->Destroy();

	FreeLibrary(dll);

	cout << endl << "===============================================================" << endl;
}

```

Now build and run the ***MathWin32ClientConsole***, we should get the following output.

```text output
2 + 4 = 6
neutro is calling add operation of class AddOperationMath

===============================================================
2 log (4) = 2
neutro is calling Logarithmic operation of class LogarithmicMath

===============================================================
Press any key to exit


```

In order to ensure proper resource release, an abstract interface provides an additional method for the disposal of an instance. In this case we provide ```Destroy``` method. Calling this method manually can be tedious and error prone. It's recommend use smart pointer for auto resource release instead of manual release.

## References New
1. https://docs.microsoft.com/en-us/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp
2. https://msdn.microsoft.com/en-us/library/1ez7dh12.aspx
3. https://docs.microsoft.com/en-us/cpp/build/dlls-in-visual-cpp
4. https://docs.microsoft.com/en-us/cpp/build/linking-an-executable-to-a-dll#determining-which-linking-method-to-use
5. https://www.codeproject.com/Articles/28969/HowTo-Export-C-classes-from-a-DLL
6. http://eli.thegreenplace.net/2011/09/16/exporting-c-classes-from-a-dll
