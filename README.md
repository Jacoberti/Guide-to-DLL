### 1. Official Microsoft guide: https://learn.microsoft.com/en-us/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp?view=msvc-170
- Reference: ![[Pasted image 20240801125314.png]]

---

### 2. Visual Studio: https://visualstudio.microsoft.com/it/
- Download link: https://visualstudio.microsoft.com/it/thank-you-downloading-visual-studio/?sku=Community&channel=Release&version=VS2022&source=VSLandingPage&cid=2030&passive=false
- Reference: ![[Pasted image 20240801125751.png]]

---

### 3. On Visual Studio:
- Required:
  `Python development, Game development with C++, Windows application   development, Desktop development with C++`

- Installation details :
  
	 ![[Pasted image 20240801130541.png]]

 - Reference:
   ![[Pasted image 20240801130716.png]]![[Pasted image 20240801130737.png]]
   
 - Click "install" -> so click "ok" when it is done:
   ![[Pasted image 20240801133918.png]]

---

### 4. Create a project:
- Click on "create a new project" into Visual Studio. (I'm using Visual Studio Community 2022).
- Search for "dy" and click on first result:
  ![[Pasted image 20240801140748.png]]

- Click "next" and rename the project as u like, so press the button "create" to finally make our project:
  ![[Pasted image 20240801141052.png]]
- You should get a similar screen:
  ![[Pasted image 20240801141907.png]]
  
---

### 5. Create a header file:
- Press on Project on the menu tab and click to add item:
  ![[Pasted image 20240801142639.png]]

- On the window that will come up click on "view all templates":
  ![[Pasted image 20240801143245.png]]
- Choose the Header File and click the button "Add":
  ![[Pasted image 20240801143443.png]]

- Replace the content with this code:
  
``` C++
// MathLibrary.h - Contains declarations of math functions
#pragma once

#ifdef MATHLIBRARY_EXPORTS
#define MATHLIBRARY_API __declspec(dllexport)
#else
#define MATHLIBRARY_API __declspec(dllimport)
#endif

// The Fibonacci recurrence relation describes a sequence F
// where F(n) is { n = 0, a
//               { n = 1, b
//               { n > 1, F(n-2) + F(n-1)
// for some initial integral values a and b.
// If the sequence is initialized F(0) = 1, F(1) = 1,
// then this relation produces the well-known Fibonacci
// sequence: 1, 1, 2, 3, 5, 8, 13, 21, 34, ...

// Initialize a Fibonacci relation sequence
// such that F(0) = a, F(1) = b.
// This function must be called before any other function.
extern "C" MATHLIBRARY_API void fibonacci_init(
    const unsigned long long a, const unsigned long long b);

// Produce the next value in the sequence.
// Returns true on success and updates current value and index;
// false on overflow, leaves current value and index unchanged.
extern "C" MATHLIBRARY_API bool fibonacci_next();

// Get the current value in the sequence.
extern "C" MATHLIBRARY_API unsigned long long fibonacci_current();

// Get the position of the current value in the sequence.
extern "C" MATHLIBRARY_API unsigned fibonacci_index();
```

- Explain:
  #### Preprocessor and Macro
	pragma once: This command prevents the header file from being included multiple times in a single compilation file, preventing duplication.
	
	ifdef MATHLIBRARY_EXPORTS: This block checks whether MATHLIBRARY_EXPORTS is defined.
	
	define MATHLIBRARY_API \_\_declspec(dllexport): If MATHLIBRARY_EXPORTS is defined (that is, when you are compiling the DLL library), MATHLIBRARY_API is defined as \_\_declspec(dllexport). This specifies that functions declared with this macro should be exported from the DLL, making them available to other applications.
	else: If MATHLIBRARY_EXPORTS is not defined (that is, when you are using the DLL library in another application), MATHLIBRARY_API is defined as \_\_declspec(dllimport). This optimizes the import of functions from the DLL.
	endif: Closes the ifdef block.
	Function Declarations.
	extern "C": This command tells the compiler to use C linkage for declared functions, instead of C++, which prevents name decoration (name mangling) and allows compatibility with other languages.
	
	MATHLIBRARY_API: This prefix applies \_\_declspec(dllexport) or \_\_declspec(dllimport) to functions, depending on how the MATHLIBRARY_EXPORTS macro is defined.
  #### Declared functions:
	fibonacci_init(const unsigned long long a, const unsigned long b): Initializes the Fibonacci sequence with initial values a and b. Must be called before any other function in the library.
	fibonacci_next(): Calculates the next value of the Fibonacci sequence. Returns true if the operation is successful and updates the current value and index. Returns false in case of overflow, leaving the current value and index unchanged.
	fibonacci_current(): Returns the current value of the Fibonacci sequence.
	fibonacci_index(): Returns the index of the current value in the Fibonacci sequence.
	Context of Use
	When you compile the DLL library, the MATHLIBRARY_EXPORTS macro is defined in the DLL project, so functions are exported using \_\_declspec(dllexport). When you include this header file in a project that uses the DLL, MATHLIBRARY_EXPORTS is not defined, so functions are imported using \_\_declspec(dllimport).


- Add a new C++ file in the same way that u add added the header file:
  ![[Pasted image 20240801150641.png]]

- Copy the following code and paste it into the C++ file:
  
``` C++
// MathLibrary.cpp : Defines the exported functions for the DLL.
#include "pch.h" // use stdafx.h in Visual Studio 2017 and earlier
#include <utility>
#include <limits.h>
#include "MathLibrary.h"

// DLL internal state variables:
static unsigned long long previous_;  // Previous value, if any
static unsigned long long current_;   // Current sequence value
static unsigned index_;               // Current seq. position

// Initialize a Fibonacci relation sequence
// such that F(0) = a, F(1) = b.
// This function must be called before any other function.
void fibonacci_init(
    const unsigned long long a,
    const unsigned long long b)
{
    index_ = 0;
    current_ = a;
    previous_ = b; // see special case when initialized
}

// Produce the next value in the sequence.
// Returns true on success, false on overflow.
bool fibonacci_next()
{
    // check to see if we'd overflow result or position
    if ((ULLONG_MAX - previous_ < current_) ||
        (UINT_MAX == index_))
    {
        return false;
    }

    // Special case when index == 0, just return b value
    if (index_ > 0)
    {
        // otherwise, calculate next sequence value
        previous_ += current_;
    }
    std::swap(current_, previous_);
    ++index_;
    return true;
}

// Get the current value in the sequence.
unsigned long long fibonacci_current()
{
    return current_;
}

// Get the current index position in the sequence.
unsigned fibonacci_index()
{
    return index_;
}
```

- Now for testing our code go to the tab **Menu -> Build -> Build solution:
  ![[Pasted image 20240801151445.png]]
  
- We will find an error, to fix it we will just rename, in the Menu on the right, our _file_name_.h and rename it "MathLibrary.h":
  ![[Pasted image 20240801151836.png]]
	
	![[Pasted image 20240801151939.png]]

- Now retry to Build our C++ file; an output like this should appear in the console:
  ![[Pasted image 20240801152145.png]]

---

### 6. Create a client app that uses our DLL:
- Explain:
  
	When you create a DLL, think about how client apps may use it. To call the functions or access the data exported by a DLL, client source code must have the declarations available at compile time. At link time, the linker requires information to resolve the function calls or data accesses. A DLL supplies this information in an _import library_, a file that contains information about how to find the functions and data, instead of the actual code. And at run time, the DLL must be available to the client, in a location that the operating system can find.
	
	Whether it's your own or from a third-party, your client app project needs several pieces of information to use a DLL. It needs to find the headers that declare the DLL exports, the import libraries for the linker, and the DLL itself. One solution is to copy all of these files into your client project. For third-party DLLs that are unlikely to change while your client is in development, this method may be the best way to use them. However, when you also build the DLL, it's better to avoid duplication. If you make a local copy of DLL files that are under development, you may accidentally change a header file in one copy but not the other, or use an out-of-date library.
	
	To avoid out-of-sync code, we recommend you set the include path in your client project to include the DLL header files directly from your DLL project. Also, set the library path in your client project to include the DLL import libraries from the DLL project. And finally, copy the built DLL from the DLL project into your client build output directory. This step allows your client app to use the same DLL code you build.

- On the Menu Bar choose **File** -> **New** -> **Project** and create a new project:
  ![[Pasted image 20240801153504.png]]

- Search for "Console App" in C++:
  ![[Pasted image 20240801153637.png]]

- Rename it and choose the Create button:
  ![[Pasted image 20240801153841.png]]
  
-  Add DLL to our application; Go to **Solution Explorer** -> **Properties**:
  
  ![[Pasted image 20240801154438.png]]
 ^81bae6
- Go to **C/C++** -> **General** -> **Additional Include Directories** and choose the option **Edit**:
  ![[Pasted image 20240801154716.png]]

- Select the folder icon:
  ![[Pasted image 20240801154900.png]]

- Go over the line that u created and select (at the right) the three dots:
  ![[Pasted image 20240801155049.png]]

- Now select the path to your DLL project:
  ![[Pasted image 20240801155233.png]]

- At finish press **Ok** -> **Apply** -> **Ok**
- In your application code paste this code when is included our **MathLibrary.h** file:
  
``` C++
// MathClient.cpp : Client app for MathLibrary DLL.
// #include "pch.h" Uncomment for Visual Studio 2017 and earlier
#include <iostream>
#include "MathLibrary.h"

int main()
{
	// Initialize a Fibonacci relation sequence.
	fibonacci_init(1, 1);
	// Write out the sequence values until overflow.
	do {
		std::cout << fibonacci_index() << ": "
			<< fibonacci_current() << std::endl;
	} while (fibonacci_next());
	// Report count of values written before overflow.
	std::cout << fibonacci_index() + 1 <<
		" Fibonacci sequence values fit in an " <<
		"unsigned 64-bit integer." << std::endl;
}
```
  
- Info:
  
	This code can be compiled, but not linked. If you build the client app now, the error list shows several LNK2019 errors. That's because your project is missing some information: You haven't specified that your project has a dependency on the _MathLibrary.lib_ library yet. And, you haven't told the linker how to find the _MathLibrary.lib_ file.
	
	To fix this issue, you could copy the library file directly into your client app project. The linker would find and use it automatically. However, if both the library and the client app are under development, that might lead to changes in one copy that aren't shown in the other. To avoid this issue, you can set the **Additional Dependencies** property to tell the build system that your project depends on _MathLibrary.lib_. And, you can set an **Additional Library Directories** path in your project to include the path to the original library when you link.

---

### 7. Import DLL in our application:
- Go to **Property** [[#^81bae6]] -> **Linker** -> **Input** -> **Additional Dependencies** and choose the option **Edit**:
  
  ![[Pasted image 20240802123812.png]]
  ![[Pasted image 20240802123905.png]]

- Into the Additional Dependencies writes **YourProjectName.lib** and click **Ok**:
  ![[Pasted image 20240802125425.png]]
  
- Now go to **General** -> **Additional Library Directories** and choose the option **Edit**:
  ![[Pasted image 20240802124221.png]]

- In this window select the **folder icon** and after click on the **three dots**:
  ![[Pasted image 20240802124417.png]]
  ![[Pasted image 20240802124522.png]]
  
- Select your DLL directory files, The path to our DLL project should look something like this:
	`..\..\dll_project\x64\Debug

- You will probably get an error like this:
  ![[Pasted image 20240802130622.png]]

---

### 8. Last steps to copy our DLL:
- Go to **Property** -> **Built Event** -> **Post-Built Event** -> **Command Line** and choose the option **Edit**:
  ![[Pasted image 20240802130832.png]]

- Into the pane copy the code below and click **Ok** -> **Apply** -> **Ok**:
  `xcopy /y /d "..\..\dll_project\x64\Debug"`
  ![[Pasted image 20240802131157.png]]
  
- Finally on the **Menu bar** click **Build** -> **Build Solution**:
  ![[Pasted image 20240802131407.png]]

- Now start the program on the triangle icon:
  ![[Pasted image 20240802133253.png]]
  

---

### 9. End:
- Once you run the program you should get a result like this:
  ![[Pasted image 20240802133122.png]]
  
