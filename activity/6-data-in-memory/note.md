## 6 - Data in memory
### Activity 1: Integers and Local Variables
- Problem1
  
  local is stored in `%edi`. The value of local is stored in register, not in memory, so the address of local is not available. 
  **If the code took the address of local, the compiler would have to store the variable on the stack, instead of keeping it in a register**
- Problem2
  ```c
  extern int absp(int *p);
  ```

### Activity 2: Arrays
- Problem3

  The elements are stored in the memory in continous order. (First element: 0x601110, increasing by 4 bytes)

  4 bytes stride. It's appear in disassembly code: `mov (%rdi, %rax, 4), %eax`
  When the index is provided, the address of the element is calculated by `base address + index * 4`

  String: "15213 CSAPP" -> array of 12 characters (including '\0')

- Problem4
  
  The last null character tells where is the end of the string and figures out the length of the string.
  
### Activity 3: Structs
- Problem5
  
  It looks same as the array we saw in problem 3. (4 integer array)

- Problem6

  |    |   |   |   |   |   |   |   |   |
  |--- |---|---|---|---|---|---|---|---|
  |0x00| a |   | b | b | c | c | c | c |
  |0x08| d | d | d | d | d | d | d | d |
  |0x10|   |   |   |   |   |   |   |   |
  |0x18|   |   |   |   |   |   |   |   |

- Prolbem7

  The struct is not ordered by the size of the elements. It will take more space than the struct in problem 6, as it consumes more bytes for padding.

- Problem8

  |    |   |   |   |   |   |   |   |   |
  |--- |---|---|---|---|---|---|---|---|
  |0x00| a |   |   |   |   |   |   |   |
  |0x08| b | b | b | b | b | b | b | b |
  |0x10| c | c |   |   | d | d | d | d |
  |0x18| d |   |   |   |   |   |   |   |
### Activity 4: Arrays of Structs
- Problem9

  8 byte
- Problem10

  8 byte and the last 3 bytes are padding **(after small)**. It requires 3 bytes to align the next element.

  **Padding after small makes struct pair's size be a multiple of 4. That's necessary so that the second element of the array will have an address that's multiple of 4**

- Problem11

  ~~8 byte. Struct is internally an array of elements, so large will require 4 bytes, and small will require 1 byte. The padding is 3 bytes. So the stride of triple is still same with that of pair.~~

  **This struct is smaller, since it requires less padding. Both have elements whose total size is 5 bytes. pair is aligned to the width of int (4 bytes), so it gets padded to a length of 8. triple is only aligned to the width of short (2 bytes), so it only gets padded to a length of 6.**

### Activity 5: 2D Arrays
- Problem12

  inner array: 1 byte, outer array: 3 bytes.

- Probelm13

  row: lea (%rsi, rsi, 2), %rax for 3 strides

  col: %rdx, 1 stride by movzbl

  Not applicable to flipped. 
  The stride of the inner and outher array differs, 
  so the complier will make different assembly code.

- Problem14

  8 bytes. Since the array is composed of pointers.

- Problem15
  
  Yes. Even if they had different lengths, the assembly code would still work.
  The array stores pointer of the inner arrays. 
  Therefore, the stride of the outer array is not affected by the length of the inner array.
- Problem16
### Activity 6: Endianness