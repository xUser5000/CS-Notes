# Explanation
- `T A[L]`
	- Allocates a contiguous region of `L * sizeof(T)` bytes in memory.
	- Identifier `A` can be used as a pointer to array element 0
- `T A[R][C]` ( #multidimensional_arrays )
	- Declares a 2d array of data type `T`.
	- If `sizeof(T)` is K bytes, then the total allocated size for the array is *R * C * K* bytes.
	- It's laid out in memory in row-major ordering.![[Row-major ordering of 2d arrays.png]]
	- To access `A[i][j]` is equivalent to accessing:
		- `A + i*(C*K) + j*K`
		- `A + (i * C + j) + k`

# Sources
- Extends [[Data Structures#Sources]]