## GDB command reference
### Basic commands
- c: continue. resumes program execution
- disassemble: outputs the assembly translation of the function currently being executed, or the translation of a target function if one is supplied.
- help: provides a wealt of information on the following topic.
- i: info. ```e.g. i registers```
- p: print. examine values and execute essentially arbitrary C code from within GDB, using references defined within th e current scope (```e.g. address where the array *arr* is located```)
- q: quit
- r: restarts the program currently being debugged. (program of last execution OR custom command-line arguments)
- x: prints the memory contents of a specific address (dereference) (format: x/length/format address-expression, ```e.g. x/3x 0x42```)

### Other useful tools
- ```gdb -tui ./something```: display the C code being stepped through alongside the standard GDB console
- n: next. executes the next line of C code
- b: break. stop the program whenever it reaches a location.
- explore: follow chains of pointers between structs.
---  

## Activity 1
### Step 1
- %rsp: stack pointer
- %rdi: program counter?
- %rax: save variable?

### Step 2 and 3
- pseudo C function for squreInt
    ```c
    x * x
    ```
- register difference between squareInt and squareLInt  
  registers of squareInt start with e, while those of squareLInt start with r. variable in squareLInt requires more bit compared to that of squareInt.
  ***register r uses full 64-bit x86-64 registers instead of their 32-bit IA32 counterparts***
- Did squareFloat use the same registers from before?   
  No. It uses register that starts with x and use different op.    
  ***This register is a special purpose register for floating-point numbers***
- What is the function *whatIsThis* doing?
  getting the data in two variables from memory and swapping the values in the variables and saving them to corresponding memory.

## Activity 2
### Step 1
examine the assembly of the function *viewThis*
- What are the function's argument?    
  ~~array of integers (32 bits)~~     
  ***A memory address***
- What is the return register of the function?    
  %eax
- Which Instruction initialize the return register?   
  ~~ret~~    
  ***The first one (mov 0x4(%rdi),%eax)***
- What does the function do?   
  sum first four ints of a given array

### Step 2
- What does the function viewThisNext do?   
  return element in given array, where the index of the array was indicated by the second argument

### Step 3
*lea* (load effective address) is a variant of the *mov* instruction, but it does not reference memory. It computes the address for the specified location, otherwise using the memory reference form.
- Write down the four pars of lea's address
  - D: 0
  - B: %rdi
  - I: %rdi
  - S: 2
- with left shift, what value does mx multiply its argument by 
  ~~2~~   
  ***12*** (%rdi + 2*%rdi == 3 * %rdi) -> left shift <<2