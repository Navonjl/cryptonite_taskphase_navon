Very RE. Very Cool.

# vault-door-training

Flag: `w4rm1ng_Up_w1tH_jAv4_be8d9806f18`
open the file chat - it's a java file. That's it. The flag is in the file.

# Shop

Flag: `picoCTF{b4d_brogrammer_3da34a8f}`

gonna try doing this blind - brb

```
❯ nc mercury.picoctf.net 10337
Welcome to the market!
=====================
You have 40 coins
	Item		Price	Count
(0) Quiet Quiches	10	12
(1) Average Apple	15	8
(2) Fruitful Flag	100	1
(3) Sell an Item
(4) Exit
Choose an option:
```
So a integer overflow ? 

After much fiddiling, I realized that this is infact golang, which means they probably have good integer parsing. So I tried to input a negative number, and it worked!

```
❯ nc mercury.picoctf.net 10337
Welcome to the market!
=====================
You have 40 coins
	Item		Price	Count
(0) Quiet Quiches	10	12
(1) Average Apple	15	8
(2) Fruitful Flag	100	1
(3) Sell an Item
(4) Exit
Choose an option: 
0
How many do you want to buy?
-12
You have 160 coins
	Item		Price	Count
(0) Quiet Quiches	10	24
(1) Average Apple	15	8
(2) Fruitful Flag	100	1
(3) Sell an Item
(4) Exit
Choose an option: 
2
How many do you want to buy?
1
Flag is:  [112 105 99 111 67 84 70 123 98 52 100 95 98 114 111 103 114 97 109 109 101 114 95 51 100 97 51 52 97 56 102 125]
```
```py
>>> x  = "[112 105 99 111 67 84 70 123 98 52 100 95 98 114 111 103 114 97 109 109 101 114 95 51 100 97 51 52 97 56 102 125]"
>>> x = ','.join(x.split())
>>> x = eval(x)
>>> print(*map(chr,x),sep='')
picoCTF{b4d_brogrammer_3da34a8f}
```