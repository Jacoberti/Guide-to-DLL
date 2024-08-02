### 1. Official Microsoft guide: https://learn.microsoft.com/en-us/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp?view=msvc-170
- Reference: ![Pasted image 20240801125310](https://github.com/user-attachments/assets/66a6af5f-9dc3-4c00-ba6b-ee1f7ce5caa4)

---

### 2. Visual Studio: https://visualstudio.microsoft.com/it/
- Download link: https://visualstudio.microsoft.com/it/thank-you-downloading-visual-studio/?sku=Community&channel=Release&version=VS2022&source=VSLandingPage&cid=2030&passive=false
- Reference: ![Pasted image 20240801125751](https://github.com/user-attachments/assets/6fc28a83-9fa4-4184-96f6-36a3b9e6756b)

---

### 3. On Visual Studio:
- Required:
  `Python development, Game development with C++, Windows application   development, Desktop development with C++`

- Installation details :
  
	 ![Pasted image 20240801130541](https://github.com/user-attachments/assets/487a6588-2d57-43b5-8f33-94459f52558f)


 - Reference:
   ![Pasted image 20240801130716](https://github.com/user-attachments/assets/f6ad7ce7-3ab0-419a-a0c9-d8c65d5c9a14) ![Pasted image 20240801130737](https://github.com/user-attachments/assets/3adbe681-5be0-439a-b8d1-5cc563c972fd)

   
 - Click "install" -> so click "ok" when it is done:
   ![Pasted image 20240801133918](https://github.com/user-attachments/assets/611d8bb2-3067-4011-80c3-f12740f8397b)


---

### 4. Create a project:
- Click on "create a new project" into Visual Studio. (I'm using Visual Studio Community 2022).
- Search for "dy" and click on first result:
  ![Pasted image 20240801140748](https://github.com/user-attachments/assets/c96ffc43-ee78-41ae-b618-a54b5e9a9e1d)


- Click "next" and rename the project as u like, so press the button "create" to finally make our project:
  ![Pasted image 20240801141052](https://github.com/user-attachments/assets/0b27e8d1-0eb6-430c-9d52-29ad50cb7958)

- You should get a similar screen:
  ![Pasted image 20240801141907](https://github.com/user-attachments/assets/92dba56b-0021-4459-9891-07a12c2c65f8)

  
---

### 5. Create a header file:
- Press on Project on the menu tab and click to add item:
  ![Pasted image 20240801142639](https://github.com/user-attachments/assets/a06e0770-b59e-4f62-93d4-f13c1b95a478)


- On the window that will come up click on "view all templates":
  ![Pasted image 20240801143245](https://github.com/user-attachments/assets/694c1835-1b46-4431-af2f-09b35a953f2e)

- Choose the Header File and click the button "Add":
  ![Pasted image 20240801143443](https://github.com/user-attachments/assets/cb08d463-e568-427a-b554-0452430c6b80)


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
  ![Pasted image 20240801150641](https://github.com/user-attachments/assets/343afb36-c80d-4935-b1bf-abf3fd9a3c03)


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

- Now for testing our code go to the tab **Menu** -> **Build** -> **Build solution**:
  ![Pasted image 20240801151445](https://github.com/user-attachments/assets/3f5c563b-e1fa-4693-97fe-1f01e1561f59)

  
- We will find an error, to fix it we will just rename, in the Menu on the right, our _file_name_.h and rename it "MathLibrary.h":
  ![Pasted image 20240801151836](https://github.com/user-attachments/assets/867aab69-34ef-410d-9b7e-af6a7050251f)

	
	![Pasted image 20240801151939](https://github.com/user-attachments/assets/cccd50b1-dfbd-4a28-9460-9fb47bf247be)


- Now retry to Build our C++ file; an output like this should appear in the console:
  ![Pasted image 20240801152145](https://github.com/user-attachments/assets/26b017d4-9459-426e-9a42-673983df9135)



---

### 6. Create a client app that uses our DLL:
- Explain:
  
	When you create a DLL, think about how client apps may use it. To call the functions or access the data exported by a DLL, client source code must have the declarations available at compile time. At link time, the linker requires information to resolve the function calls or data accesses. A DLL supplies this information in an _import library_, a file that contains information about how to find the functions and data, instead of the actual code. And at run time, the DLL must be available to the client, in a location that the operating system can find.
	
	Whether it's your own or from a third-party, your client app project needs several pieces of information to use a DLL. It needs to find the headers that declare the DLL exports, the import libraries for the linker, and the DLL itself. One solution is to copy all of these files into your client project. For third-party DLLs that are unlikely to change while your client is in development, this method may be the best way to use them. However, when you also build the DLL, it's better to avoid duplication. If you make a local copy of DLL files that are under development, you may accidentally change a header file in one copy but not the other, or use an out-of-date library.
	
	To avoid out-of-sync code, we recommend you set the include path in your client project to include the DLL header files directly from your DLL project. Also, set the library path in your client project to include the DLL import libraries from the DLL project. And finally, copy the built DLL from the DLL project into your client build output directory. This step allows your client app to use the same DLL code you build.

- On the Menu Bar choose **File** -> **New** -> **Project** and create a new project:
  ![Pasted image 20240801153504](https://github.com/user-attachments/assets/017952a1-b2f3-4b95-99a9-da15e01a77d3)



- Search for "Console App" in C++:
  ![Pasted image 20240801153637](https://github.com/user-attachments/assets/1613fe62-c24a-4ca2-997c-dc98661bf3ec)



- Rename it and choose the Create button:
  ![Pasted image 20240801153841](https://github.com/user-attachments/assets/71a54ef9-62bc-47ab-8a17-6df2fed90b72)

  
-  Add DLL to our application; Go to **Solution Explorer** -> **Properties**:
  
  ![Pasted image 20240801154438](https://github.com/user-attachments/assets/f9bde53e-5d92-4e81-971e-00232fe28445)

 ^81bae6
- Go to **C/C++** -> **General** -> **Additional Include Directories** and choose the option **Edit**:
  ![Pasted image 20240801154716](https://github.com/user-attachments/assets/df5d285d-a1df-49d3-8c6c-bc444ec257c1)


- Select the folder icon:
  ![Pasted image 20240801154900](https://github.com/user-attachments/assets/c5f696af-de2a-4cb3-a2af-ee807b7a7407)


- Go over the line that u created and select (at the right) the three dots:
  ![Pasted image 20240801155049](https://github.com/user-attachments/assets/e6ea4ec0-48a3-47ea-8b17-96fca0ada387)


- Now select the path to your DLL project:
  	![Pasted image 20240801155233](https://github.com/user-attachments/assets/51364431-ddb2-407c-938c-1f9de407bea3)



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
  
	This code can be compiled, but not linked. If you build the client app now, the error list shows several LNK2019 errors. That's because your project is missing some information: You haven't 			specified that your project has a dependency on the _MathLibrary.lib_ library yet. And, you haven't told the linker how to find the _MathLibrary.lib_ file.
	To fix this issue, you could copy the library file directly into your client app project. The linker would find and use it automatically. However, if both the library and the client app are under 		development, that might lead to changes in one copy that aren't shown in the other. To avoid this issue, you can set the **Additional Dependencies** property to tell the build system that your project 	depends on _MathLibrary.lib_. And, you can set an **Additional Library Directories** path in your project to include the path to the original library when you link.

---

### 7. Import DLL in our application:
- Go to **Property** -> **Linker** -> **Input** -> **Additional Dependencies** and choose the option **Edit**:
  
  ![Pasted image 20240802123812](https://github.com/user-attachments/assets/f865ff0c-a6ae-4957-b426-aa34c0c9199b)

  ![Pasted image 20240802123905](https://github.com/user-attachments/assets/6da2f638-8041-4cd1-b4de-e504675db986)


- Into the Additional Dependencies writes **YourProjectName.lib** and click **Ok**:
  ![Pasted image 20240802125425](https://github.com/user-attachments/assets/f125cbb3-1650-4333-adc1-e143eecde31a)

  
- Now go to **General** -> **Additional Library Directories** and choose the option **Edit**:
  ![Pasted image 20240802124221](https://github.com/user-attachments/assets/473f5a7d-1a21-4280-b8bd-2842b641d000)


- In this window select the **folder icon** and after click on the **three dots**:
  ![Pasted image 20240802124417](https://github.com/user-attachments/assets/0b618209-c7a0-4314-b85f-6227e21b3f16)

  ![Pasted image 20240802124522](https://github.com/user-attachments/assets/8c020927-609b-4e5d-8d08-fe9a9cfd6102)

  
- Select your DLL directory files, The path to our DLL project should look something like this:
`..\..\dll_project\x64\Debug`

- You will probably get an error like this:
  ![Pasted image 20240802130622](https://github.com/user-attachments/assets/b2463df3-b2a2-44a1-a0da-6a8f00a2b3e1)


---

### 8. Last steps to copy our DLL:
- Go to **Property** -> **Built Event** -> **Post-Built Event** -> **Command Line** and choose the option **Edit**:
  ![Pasted image 20240802130832](https://github.com/user-attachments/assets/63a9c359-b19b-405f-9550-203b24525f48)


- Into the pane copy the code below and click **Ok** -> **Apply** -> **Ok**:
`xcopy /y /d "..\..\dll_project\x64\Debug"`

  ![Pasted image 20240802131157](https://github.com/user-attachments/assets/8c8d3b23-58fc-4351-9cdf-e3fc8e88c0db)

  
- Finally on the **Menu bar** click **Build** -> **Build Solution**:
  ![Pasted image 20240802131407](https://github.com/user-attachments/assets/f3bd8a98-d702-4cc0-bc3a-655314177aa9)


- Now start the program on the triangle icon:
  ![Pasted image 20240802133253](https://github.com/user-attachments/assets/3c095433-61c4-4a5c-b129-9aeeaedf5165)

  

---

### 9. End:
- Once you run the program you should get a result like this:
  ![Pasted image 20240802133122](https://github.com/user-attachments/assets/76772ba6-e685-469b-bdbc-d29898f7665e)

  
