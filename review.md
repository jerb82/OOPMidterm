# Midterm Review

###  Software Design
Software Design - Definition, the two types of design (so far), where they come from, what they are concerned with

**There are many definitions**
* The process of defining software methods, functions, objects, and overall structure and interaction of your code so that the resulting functionality will satisfy user requirements.

**Two layers of design**
#### *Structured design*
- From structured programming

#### *Object-oriented design*
- From object-oriented programming  
---
### Software Design Characteristics
- Might need to know stuff about each..? I hope not

* Compatibility
* Fault-tolerance
* Reliability
* Usability
* Performance
* Scalability
* Robustness
* Extensibility
* Modularity
* Maintainability
* Reusability
* Security

---
### Algorithmic Decomposition  

Decomposition:
Breaking a complex problem or system into a collection of smaller parts  
**A lower-level approach is needed for OOP**  
<mark>Algorithmic Decomposition</mark>:
Breaking a complex *algorithm* into a collection of smaller algorithms  
**How?**  
> Start with the initial algorithm and apply algorithm decomposition recursively  
With good design, smaller algorithms improve *modularity* which improves *maintainabiltiy* and *reusability*  
Possible tradeoff: *efficiency*  

 
Rainfall: Algorithmic Decomposition
1. Input rainfall data
1. Calculate maximum rainfall
1. Calculate average rainfall
1. Output the rainfall report
---
### Structure Diagram
![rainfall report structure chart](structurechart.png)
### check slide 7 + of algorithmic decomposition (add more)
---
### Coding Style
- Given a function declaration or definition, imporve the code using the principles we discussed in class. Proper parameter passing (will touch up on this)

- Code is the only documentation of the design often times  

**Essential Parts of Coding Style:**
- Indentation: "flow of control"
- Newlines
- Whitespace
- Comments: prefer line comments unless multiple lines, doxygen comments when appropriate (more to come)
- Naming conventions

**Characteristics of a Coding Style**
- Consistency
- Scability
- Maintainability  

**example**
### Inconsistent, not scalable, not maintainable

    auto value=*begin; // pivot value is the first element

	auto left = begin;
	auto right= std::prev(end);
	while (std::distance(left,right)>0) {

		// move left-to-right while value is greater than elements
		while(value>= *left && std::distance(left, end) > 0){
		   if (std::next(left) == end)
			    break;

			left=std::next(left);
		  }

		// move right-to-left while value is less than elements
		while (value <*right)
		{
			right= std::prev(right);
		}
		
	// exchange so elements less than value are on the left
	// and elements greater than value are on the right
		std::swap(*left, *right);
	}

	
	std::swap(*begin,*right); // exchange pivot and final location
    
      assert(std::all_of(begin, right, [right](int n){ return n <= *right; }));
      assert(std::all_of(right, end,   [right](int n){ return n >= *right; }));`

*This code has **inconsistency** and **scalability** problems*
- the assert statements at the bottom are not in line with the auto and the while loop
- be consistent with the comments, either have them on the line before or the same line, this is violated
- spacing errors within the while loop (will be seen in the correct version soon)

**corrected:**
### **Consistent, Scalable, Maintainable**


    // pivot value is the first element
    auto value = *begin;

    auto left = begin;
    auto right = std::prev(end);
    while (std::distance(left, right) > 0) {

        // move left-to-right while value is greater than elements
        while (value >= *left && std::distance(left, end) > 0) {
            if (std::next(left) == end)
                break;
            left = std::next(left);
        }

        // move right-to-left while value is less than elements
        while (value < *right) {
            right = std::prev(right);
        }
        
        // exchange so elements less than value are on the left
        // and elements greater than value are on the right
        std::swap(*left, *right);
    }

    // exchange pivot and final location
    std::swap(*begin, *right);

    // post-condition
    assert(std::all_of(begin, right, [right](int n){ return n <= *right; }));
    assert(std::all_of(right, end,   [right](int n){ return n >= *right; }));

* Consistent & clear comments, indentation, spacing
* No unnecessary new lines
* Closing braces are in line correctly and the opening { is placed on the same line, etc.
---
### Function Design
- Given a description, create a function declaration with a proper Doxygen-style comment. Parameter passing **must** follow the coding-style rules. Similar to the Utilties exercise.
```
/*
    Average of two numbers
    @param[in] n1 First input value
    @param[in] n2 Second input value
    @return Average of n1 and n2
*/
double avg(int& n1, int& n2);
```
```
/*
    Limit the range of values within a vector 
    @param[in, out] v1 Vector to be modified
    @param[in] low The lower bound of the range
    @param[in] high The upper bound of the range
    @return Vector with ranged values 
*/
std::vector<int> limit(std::vector<int>& v1, int& low, int& high);
```
**note**: unsure if my parameters are correct for the `std::vector<int> limit(std::vector<int>& v1, int& low, int& high);`  

---
### Generalizing Functions

* Referring to making functions applicable to more situations
* Very related to *concerns*
* Have to not make assumptions (okay lowkey there is a lot to talkl about here so might just have to go to the notes)
*https://mlcollard.net/CPSC421S24/notes/Generalizing-Functions.html?section=020&index=CPSC421-020S24#/*

---
### Physical Organziation

* Include guards  
* Header comments
* function comments
* separate function declarations and definitions



| Type              | Purpose             | Extension | Example       | Roles                  |
|-------------------|---------------------|-----------|---------------|------------------------|
| Include File      | Function declarations | .hpp      | Aggregate.hpp | interface concerns     |
| Implementation File | Function definitions | .cpp      | Aggregate.cpp | implementation concerns |

Separate Functions
```cpp
/*
    Label.cpp
    Implementation file for label functions.
*/

#include "Label.hpp"
#include "pdfgen.h"

// output the label
void outputLabel(std::string_view label) {

    struct pdf_info info = {
        .creator = "My software",
        .producer = "My software",
        .title = "My document",
        .author = "My name",
        .subject = "My subject",
        .date = "Today"
    };
    struct pdf_doc *pdf = pdf_create(PDF_A4_WIDTH, PDF_A4_HEIGHT, &info);
    pdf_set_font(pdf, "Times-Roman");
    pdf_append_page(pdf);
    pdf_add_text(pdf, NULL, label.data(), 12, 50, 20, PDF_BLACK);
    pdf_save(pdf, "output.pdf");
    pdf_destroy(pdf);
}
```
```cpp

/*
    Label.hpp
    Include file for label functions.
*/

#ifndef INCLUDED_LABEL_HPP
#define INCLUDED_LABEL_HPP

#include <string_view>

// output the label
void outputLabel(std::string_view label);

#endif
```
Various one line comments for functions
Words to **NOT** have in function comments:
- function
- variable
- integer
- integers
- class
- method
- field
- double
- long
- object
- variable
- variables
- contains
- scalable
- reusable
- pass
```cpp
// Update the value at a pointer if the pointer is not null
void updateValue(int* valuePtr, int newValue);

// Add a value to each element
void addToElements(std::vector<int>& vec, int valueToAdd);

// Concatenate two strings
std::string concatenate(std::string_view s1, std::string_view s2);

// Determine if two strings are equal
bool equal(std::string_view s1, std::string_view s2);

// Print a message to the console a specific number of times
void printMessageNTimes(std::string_view message, int nTimes);

// Area of a rectangle
double rectangleArea(double length, double width);

// Determine if a number is even
bool isEven(int number);

// Swap the values of two integers
void swap(int& a, int& b);
```
#### IWYU
**I**nclude **W**hat **Y**ou **U**se  
Include any needed files for .cpp and/or .hpp

---

### Naming & Method Naming Standards
Correct poor names in given list  
**Functions are *actions***
* camelCase, under_score  
~~`getfullName();`~~  
`getFullName();`  
`garbage_collection();`  
`check_static_allocation_size();`


* Correct grammatical structure  
~~`managedResourceRegister();`~~  bad structure  
`registerManagedResource();`  
`// verb phrase`  
`drawContentBorder();`  
`// verb phrase with a prepositional phrase();`  
`performTestsFromZipFile();`  
`// noun phrase`  
`nextArea();`  

