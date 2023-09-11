# Explanation
- The linker is responsible for
	- *Symbol resolution*:
		- The linker associates each symbol reference with exactly one symbol definition.
		- A *Symbol* is a global variable or a function.
	- *Relocation*: 
		- The linker merges separate code and data sections into a single section.
		- The linker decides on the absolute memory positions of symbols in final executable file.

# FAQ
- Why do linkers exist?
	- To allow programs to be written as a collection of small files that are grouped together (program modularity)
	- To make the compilation process more efficient (refer to [[GNU make]])

# Sources
- [CMU Introduction to Computer Systems - Lecture 13](https://scs.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=0aef84fc-a53b-49c6-bb43-14cb2b175249)