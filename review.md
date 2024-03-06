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
    ``
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


