## 9 - Machine Programming: Advanced aka Security
### 1.  Getting started
1. stack, as a local variable
2. **Each call instruction pushes the return address onto the stack.**
3. Because it goes over the boundary of the struct and ~~tries to write on the null ptr.~~
   
   **Writing to s.a[6] overwrote the return address for fun with a nonsense value, causing the ret instruction to transfer control to a memory location that was either unmapped, non-excutable, or contained invalid machine instructions.**

### 2. Gets
4. ~~The last `\0` tells the end of the allocated space.~~ **We don't.**
5. If it is the end of file or encounters newline.
6. **No they purely depend on how many characters the user inputs**

### 3. Overwriting Stack
7. 0x18 = 24 bytes
8. **The buffer was, at most, 24 bytes long. It could have been smaller, because the comipler is obliged to keep the stack pointer aligned and this involves rounding up the amount of space requested. The user may or may not enter a string that is shorter than 24 bytes**
9. 
    |memory|value|   
    | --- | --- |
    | 0x28 | |
    | 0x20 | |
    | 0x18 | return |
    | 0x10 | string |
    | 0x08 | string |
    | 0x00 | string |

10. 
    |memory|value|   
    | --- | --- |
    | 0x28 | |
    | 0x20 | |
    | 0x18 | @AA**0____** |
    | 0x10 | 12345678 |
    | 0x08 | 12345678 |
    | 0x00 | 12345678 |

### 4. Exploit
11. **0x4006d6 (gets) -> 0x4006db (mov) -> 0x4006de (puts) -> 0x4006e3 (add) -> 0x4006e7 (ret) -> [USER INPUT] 0x414140**
12. **When control was goint to be returned to echo()'s caller, control was instead transferred to user input on the stack**
13. `mov $decafbad, %eax`
14. **They should replace the first five characters of the input string**

### 5. Defense
15. Because it provides size parameter. **As long as the buffer pointed to by the s parameteris at least size bytes large, fgets will not overwrite any other data**
16. It cannot expect how the exploit string will affect the other code.

    **Execution would jump to an unknown section of memory, almost certainly causing a crash as in probelm 3**
17. Randomly assign the stack address.
18. ```c
    rax = *(fs+0x28);
    *(rsp+0x8) = rax;
    rax = 0;

    rax = *(rsp+0x8);
    if (rax != *(fs+0x28)) {
        __stack_chk_fail();
    }
    echo();
    ```
19. It will return __stach_chk_fail() since the return address has been modified.

    **(Program terminates before it could jump to exploit code)**
20. we use one more memory to save the original return value, and it consumes time to compare the initial value with the current value.

    **Each function that uses a "canary" is larger and slightly slower. The compiler has extra work to do, to decide which functions need "canaries" and emit the extra instruction for them**

### 6. ROP
21. We overwrote the return address before executing ret.
22. 0xC3
23. **0x4004d3 (inside a different address)**
24. ```c
    mov %rax, %rdi
    ret
    ```
    **instructions starting at the second next stack address prior to running this code block**
25. **If there is no return instruction at the end of the gadget, execution will never jump to the next gadget in the chain**