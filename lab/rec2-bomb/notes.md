### Activity 1
- What happens now?

    It doesn't stop at the main function, and it directly prints the value and exits.
- Does the printed value correspond to anything in the C code?
    
    It prints the format string in the printf function.
- What does this print out?

    `$4 = 0x7fffffffe03f "12345"`
- Now what does this print out?
    ```
    Continuing.
    12345
    [Inferior 1 (process 3725737) exited with code 06]
    ```

### Activity 2
- Which is the first argument to main? Why?

    $rdi, because it saves the number 1.

- How could you view the arguments that have been passed to stc?
    
    By checking the registers that save the arguments.

- Which function is in execution now?

    `stc` function

- Where are the "=>" characters printed on the left side?
    
    at the last executed instruction

### Activity 3
- Which register holds the return value of the function?

    `$rax` or `$eax`
- Where is the return value set in compare?

    `%rax` register
- Using nexti or stepi, how does the value in register %rbx change, leading to the cmp instruction?

    I gave 15200 and 8 as inputs. The value in %rbx register changes to 15200, 15205, 15213.