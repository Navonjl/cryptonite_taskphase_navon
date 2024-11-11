# GDB Baby steps

**Flag:** `picoCTF{549698}`

So I solved this in nearly 3 different ways:

- fire up ida64 and throw the binary at it, giving me my first succesfull RE flag (with a bit of help from chatgpt, as I didnt know what the h in ``86342h`` meant.)

Probably the easiest way to do it - cyberchef did gimme some pain trying to do hex to decimal, so used another tool.

- ``disassemble main``

open up gdb and run that - ez.

Now for the dynamic methord - _proper_ gdb

This took me a fair minute to crack.

- dynamic analysis with gdb

So, if you are stupid like me, you probably tried something to the effect of -
```gdb
(gdb) break main
(gdb) start
(gdb) i r eax
eax            0x55555129          1431654697
// hold up whaaaa
```

you try a few more times and slowly start doubting your sanity. Is it really decimal ? is this some Big endian / little  endian bullshit ?

Nope, It's just that breakpoints are only set at the start of a function. Of course.
You lurk around more and find that you can move at the instruction level (huh) using ``ni`` (next instruction). (Use layout asm (_after_ disassemble main)) for more cool points)

After _all_ that, you get your flag.

References:
chatgpt, Stackoverflow and murking around.

# ARMssembly 1

**Flag:** `picoCTF{000000E8}`

Probably the hardest to do by hand.
I have a life, so asked ChatGPT to inline comment everything to end up with this beauty -

```asm
.arch armv8-a                   // Set the architecture to ARMv8-A
.file	"chall_1.c"               // Specify the source file name
.text                           // Begin the text (code) section
.align	2                       // Align the following code on a 4-byte boundary
.global	func                    // Declare `func` as a global symbol (function accessible from other files)
.type	func, %function          // Define `func` as a function

func:
	sub	sp, sp, #32             // Allocate 32 bytes on the stack for local variables
	str	w0, [sp, 12]           // Store input argument `w0` at offset 12 on the stack
	mov	w0, 87                 // Move constant 87 into `w0`
	str	w0, [sp, 16]           // Store 87 at offset 16 on the stack
	mov	w0, 3                  // Move constant 3 into `w0`
	str	w0, [sp, 20]           // Store 3 at offset 20 on the stack
	mov	w0, 3                  // Move constant 3 into `w0` again
	str	w0, [sp, 24]           // Store 3 at offset 24 on the stack
	ldr	w0, [sp, 20]           // Load the value at offset 20 (3) into `w0`
	ldr	w1, [sp, 16]           // Load the value at offset 16 (87) into `w1`
	lsl	w0, w1, w0             // Left shift `w1` (87) by `w0` (3), resulting in 87 << 3 = 696
	str	w0, [sp, 28]           // Store the result (696) at offset 28 on the stack
	ldr	w1, [sp, 28]           // Load the value at offset 28 (696) into `w1`
	ldr	w0, [sp, 24]           // Load the value at offset 24 (3) into `w0`
	sdiv	w0, w1, w0            // Divide `w1` (696) by `w0` (3), resulting in 232
	str	w0, [sp, 28]           // Store the result (232) at offset 28 on the stack
	ldr	w1, [sp, 28]           // Load the value at offset 28 (232) into `w1`
	ldr	w0, [sp, 12]           // Load the original input argument (from offset 12) into `w0`
	sub	w0, w1, w0            // Subtract `w0` (input) from `w1` (232), storing the result in `w0`
	str	w0, [sp, 28]           // Store the result at offset 28 on the stack
	ldr	w0, [sp, 28]           // Load the final result into `w0` to prepare for returning
	add	sp, sp, 32             // Deallocate the 32 bytes on the stack
	ret                         // Return from the function, `w0` holds the result
	.size	func, .-func          // Set the size of `func` from start to the current location

	.section	.rodata            // Read-only data section for constant strings
	.align	3                    // Align on an 8-byte boundary
.LC0:
	.string	"You win!"          // Define a null-terminated string "You win!"
	.align	3                    // Align on an 8-byte boundary
.LC1:
	.string	"You Lose :("       // Define a null-terminated string "You Lose :("

	.text                           // Begin the text (code) section again
	.align	2                       // Align the following code on a 4-byte boundary
	.global	main                    // Declare `main` as a global symbol (entry point)
	.type	main, %function         // Define `main` as a function

main:
	stp	x29, x30, [sp, -48]!   // Save frame pointer (x29) and link register (x30), and allocate 48 bytes on stack
	add	x29, sp, 0            // Set the frame pointer to the current stack pointer
	str	w0, [x29, 28]         // Store the first argument `w0` at offset 28 in the stack frame
	str	x1, [x29, 16]         // Store the second argument `x1` (argv) at offset 16 in the stack frame
	ldr	x0, [x29, 16]         // Load `argv` (pointer to command-line arguments) into `x0`
	add	x0, x0, 8             // Adjust `x0` to point to the first argument (argv[1])
	ldr	x0, [x0]              // Dereference argv[1] (get pointer to the first argument string)
	bl	atoi                  // Call `atoi` to convert the argument string to an integer
	str	w0, [x29, 44]         // Store the result of `atoi` (converted integer) at offset 44 in the stack frame
	ldr	w0, [x29, 44]         // Load the integer (argv[1] as an integer) into `w0`
	bl	func                  // Call `func` with this integer as an argument
	cmp	w0, 0                 // Compare the result of `func` with 0
	bne	.L4                   // If result is not zero, branch to .L4 (losing message)
	adrp	x0, .LC0             // Load the address of "You win!" message into `x0` (high part)
	add	x0, x0, :lo12:.LC0    // Adjust `x0` to the exact address of "You win!"
	bl	puts                  // Call `puts` to print "You win!"
	b	.L6                   // Branch to .L6 (end of `main`)
.L4:
	adrp	x0, .LC1             // Load the address of "You Lose :(" message into `x0` (high part)
	add	x0, x0, :lo12:.LC1    // Adjust `x0` to the exact address of "You Lose :("
	bl	puts                  // Call `puts` to print "You Lose :("
.L6:
	nop                         // No operation (placeholder)
	ldp	x29, x30, [sp], 48    // Restore frame pointer and link register, deallocate 48 bytes from stack
	ret                         // Return from `main`
	.size	main, .-main          // Set the size of `main` from start to the current location
	.ident	"GCC: (Ubuntu/Linaro 7.5.0-3ubuntu1~18.04) 7.5.0"  // Compiler identification
	.section	.note.GNU-stack,"",@progbits  // Note section for stack
```

Unfun.

anyways, w1 - w2 == 0 for the flag, giving us (post hexing and padding) 000000E8.
Some probable other stuff I could have tried include -
- Get ghidra on this (I'd rather not tho) and get the c psedocode.
- Get a armv8 emulator and step through to understand/bruteforce all possible values.
	- It's actually really hard to get a proper emulator/debugger, which is funny in a way.

# Vault door 3

**Flag:** `picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_79958f}`

There is liteally no steps here, just reverse it using a bitta python

```py
s = "jU5t_a_sna_3lpm18g947_u_4_m9r54f"

buffer = ["z"] * len(s)

for i in range(8):
    buffer[i] = s[i];

for i in range(8,16):
    buffer[23-i] = s[i];

for i in range(16,32,2):
    buffer[46-i] = s[i];

for i in range(17, 32,2):
    buffer[i] = s[i]

print(''.join(buffer))
```
There u go.
