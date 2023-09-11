# Explanation
- Properties of procedures:
	- Passing control:
		- When a procedure is called, the CPU starts executing the first instruction in that procedure.
		- After the called procedure finishes execution, control is returned back to the calling procedure.
	- Passing data:
		- A caller can pass arguments to the callee.
		- The callee can return some value to the caller.
	- Memory management:
		- A procedure can allocate space for its local variables during execution.
		- However, it should deallocate that space after it finishes execution.
- #call_stack
	- All of the above properties can be implemented easily using the stack discipline.
	- The stack is a memory segment ([[Memory Layout]]) that's managed using a LIFO policy.
	- In x86-64, the register `%rsp` always points to the address of top of the stack.
	- It grows downwards (towards lower addresses). Consequently, `%rsp` points to the lowest address in the #call_stack.
- Instructions that manipulate the stack pointer `%rsp`:
	- `pushq Src`
		1. Fetch the operand at `Src`.
		2. Decrement `%rsp` by 8.
		3. Write the value of `src` at the address given by `%rsp`.
	- `popq Dest`
		1. Read value at the address given by `%rsp`.
		2. Increment `%rsp` at 8.
		3. Store that value at `Dest`.
	- `call label`
		1. Push the #return_address (address of the next instruction right after `call`) on the #call_stack.
		2. Jump to `label`
	- `ret`
		- Pop the topmost address from the #call_stack.
		- Jump to that address.
- ABI (Application Binary interface):
	- It's a set of conventions/assumptions that are followed in procedure calling.
	- It's called "Binary" because it's designed for machine-level programs.
	- The first six arguments to a procedure is passed to the following registers respectively:
		- `%rdi`
		- `%rsi`
		- `%rdx`
		- `%rcx`
		- `%r8`
		- `%r9`
	- Any extra procedures are passed on the #call_stack.
	- The return value of a procedure will always be stored in the register `%rax`
- #call_stack frames:
	- It's the private space allocated for a procedure on the #call_stack.
	- It gets completely erased when the procedure is no longer executing.
		- PS: "erased" does not mean zeroed out. It just means that this region is no longer part of the #call_stack and will be overridden by subsequent procedure calls.
	- It contains all of the local data of that function such as: variables, arguments, local arrays, and the #return_address.
	- Most of the time, the frame size is known to the compiler beforehand. So, it can allocates the required space just after entering the procedure.
		- A special case is when there is an allocation of a variable-sized local array.
- Recursion:
	- Recursion is implemented using the stack discipline without any special consideration:
	- #call_stack frames mean that each function call has its own private storage.
	- The stack discipline also works for mutual recursion.
		- P calls Q; Q calls P
# Sources
- [CMU Introduction to Computer Systems - Lecture 7](https://scs.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=2994255f-923b-4ad4-8fb4-5def7fd802cd)
