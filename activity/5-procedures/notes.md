## 5-Procedures
### Activity 1: Calls
- Problem1. Fill the contents of the stack:
  - 0x00015213 <- $rsp = 0x7fffffffdbd0
  - 0x000000000040117a
- Problem2. What was the meaning of the second number on the stack?
  - ~~the previous frame, where the function was called. Here, the previous frame was main function, and after the current function is done, the stack pointer will be set to the previous frame.~~      
  
  **the function's return address**
- Problem3. What does the ret instruction do?
  - It returns to the caller function.     
  
  **ret pops from the top of the stack to $rip (incrementing %rsp by 8 bytes)**
- Problem4. Given your answer to Problem3, what must it be that `call` does?
  - call the function abs and allocate a new frame for the function.   
  
  **call pushes the value of %rip to the stack (decrementing %rsp by 8 bytes), then unconditionally branches (jumps) to the call address described by its operand.**
- Problem5. returnOneOpt?
  - It jumps directly to the abs function and not calling abs and return. There's no more execution after the call abs, and the returnOneOpt function is done. So after the abs function is done, the stack pointer will be set to the previous frame. However, if more instructions are needed to be executed after the call abs, this optimization cannot be used.
  
  **tail call optimization**

### Activity 2: Arguments and Local Variables
- Problem6. the type of data seeArgs passes.
  string 
  
  **The first argment of printf should always be a format string, which has type const char *. x/s prints out the C string found at the specified location in memory.**
- Problem7. Where did the compiler place arguments 7 and 8?
  ~~%rbp (which is %rsp - 8).~~ It has extra arguments more than 6, and the first 6 arguments are passed through registers. The rest of the arguments are passed through the stack.
  
  **Arguments 7 and 8 were pushed onto the stack in reverse order since the compiler ran out of integer argument registers**
- Problem8. Where does the function getV allocate its array? How does it pass this location to getValue?
  getV allocates the array on the stack. It passes the location of the array in %rdi register to getValue.
  
  **getV passes the address of the array to getValue by using a normal pointer stored in a standard argument register.**
- Problem9. call-preserved by mult4?
  - %eax/%rax contains return values.
  
  **%rbx, %r12, %r13 are call-preserved.**
  - call-preserved / call-clobbered registers
    - call-preserved (callee-save)
  
      when a function returns, these registers must have the same values that they did when the function was called.
      If a function wants to use a call-preserved register, it must save the old value first, and resture it when it's done using the register.
    - call-clobbered (caller-save)

      Don't have to be saved and restored. However, if a function has an important value in a call-clobbered register, 
      and it needs to call some other function, and then use that important value afterward, it has to save the value itself!

- Problem10. What does the function mrec do?
  If the value is 1 return, otherwise subtract 1 and call mrec again. And multiply the inital value to the return value.
  Overall, it calculates the factorial of the given value.