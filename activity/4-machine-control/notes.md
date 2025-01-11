## Activity
### 3. Basic Control Flow
1. Because JE subtracts the two operands and check if the result is zero.
2. In addition to the changes of "a.k.a." instructions to "name" instruction, additional hex values are added, putatively the binary representation of the instruction and addresses.
3. They are descending by 2 bytes. ~~The second byte's value is the address of the destination after jump instruction.~~
   **The second byte's value is equal to the number of bytes in between the address of destination and the address of the first byte after the jump instruction. Put another way, it's the value to add to %rip if the jump is taken.**

### 4. Comparisons and Conditional Set instructions
4. :bangbang: This problem cannot be solved due to gdb run issue. :bangbang:
   **%rsi and %rdi contain function arguments. %rax contains the return value**
5. 
    | Arg1 | Arg2 | setg | seta |
    |------|------|------|------|
    | 0    | 0    | 0    | 0    |
    | 0    | 1    | 0    | 0    |
    | 1    | 0    | 1    | 1    |
    | -1   | 1    | 0    | 1    |
    | 0    | -1   | 1    | 0    |

6. 
```c
    int setg(int16_t a, int16_t b) {
        return a > b;
    }
    int seta(uint16_t a, uint16_t b) {
        return a > b;
    }
```

### 5. Tests and Conditional Move Instructions
7. :bangbang: This problem cannot be solved due to gdb run issue. :bangbang: #TODO
    ** Both arguments are the same -- TEST is being asked to set condition codes based on the bitwise AND of a register with itself.**
8. 
    | Arg1 | Arg2 | cmove |
    |------|------|-------|
    | 0    | 0    | 0     |
    | 0    | 2    | 2     |
    | 1    | 2    | 0     |
    | -1   | 2    | 0     |

### 6. Loops
9. DONE
10. 
    - for loop
        i : %eax, len: %esi
    - while loop
        i : %eax, len: %esi
    - do-while loop
        i : %eax, len: %esi
11. The codes for for loop and while loop were same. 
    However, the code for do-while loop was different because it first adds to return value and then checks if the index is less than the length.

### 7. Switch Statements
12. 
    ```c
    // %rdi = a and val, %rsi = b, %rdx = c, %rcx = dest
    void switcher(long a, long b) {
        long val;
        switch (a) {
            case 5:
                c = b^15;
            case 0: 
                val = c + 112;
                break;
            case 2/7: 
            case 2/7: L5
                val = (c + b) << 2;
                break;
            case 4:
                val = a;
                break;
            default L2: 
                val = b;
        }
        *dest = val; -> L6
    }
    ```