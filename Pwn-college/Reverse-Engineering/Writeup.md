## Reverse Engineering Challenges
[Challenges](https://dojo.pwn.college/challenges/reversing) found on [dojo.pwn.college](https://dojo.pwn.college/).  
To solve this challenges i used [Radare2](https://github.com/radareorg/radare2).

### Solved Levels
- [Level 0x00](#level-0x00)
- [Level 0x01](#level-0x01)
- [Level 0x02](#level-0x02)
- [Level 0x03](#level-0x03)

### Level 0x00
Let's run it and enter fake key.  
![](screenshots/lvl0/test.png)  
  
As we can see, our input is printed in HEX format.  
And below it displays the key we need.  
To convert HEX to ASCII we can use `rax2 -s` command.  
![](screenshots/lvl0/unhex.png)  
  
Now we can to solve this challenge with the valid key.  
![](screenshots/lvl0/flag.png)  

### Level 0x01
Let's run it and enter fake key.  
![](screenshots/lvl1/test.png)  
  
But now it's not printing key for us.  
Let's check it in Radare2.  
Skip some undefined functions and move to the end of a `main ()` function.  
![](screenshots/lvl1/keycheck.png)  
  
We see piece of the code that checks the key.  
Second `memcmp ()` function's parameter is the our key.  
Let's try to use it.  
![](screenshots/lvl1/flag.png)

### Level 0x02
Let's run it and enter fake key.  
![](screenshots/lvl2/test.png)  
  
This is similar to what we saw in the [Level 0x00](#level-0x00).  
But now it's just swap two characters.  
Just repeat this trick and swap this characters.  
![](screenshots/lvl2/unhex.png)  
  
Now let's try it.  
![](screenshots/lvl2/flag.png)

### Level 0x03
Now we'll now see any useful things in the output.  
Let's move to Radare2 and view key check.  
In this case we have swap of the two last characters.  
![](screenshots/lvl3/swap.png)  
  
We can get the string just and swap this characters.  
![](screenshots/lvl3/flag.png)

