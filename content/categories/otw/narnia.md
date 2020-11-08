---
date: 2020-10-09
description: "Pierre Gringoire"
featured_image: ""
tags: ['Binary Exploitation','Buffer Overflow','Wargame']
title: "Narnia"
---


Narnia is one of the best wargames for getting started with Binary Exploitation.It starts with a very common types of exploitation called Buffer Overflow and then the difficulty gradually increases.We are provided with the source code so that's another plus.

It contains 10 levels starting

#### SSH Information
Host:narnia.labs.overthewire.org

port:2226

To ssh into say level0 we type

> ssh `narnia0@narnia.labs.overthewire.org` -p 2226

and then we input the password

**narnia0**

So starting off with 


## Level 0

We login as narnia0 and are greeted by the banner that tells us about the options we have on this machine.We have tools like PEDA,radare2,checksec,pwntools etc.

> cd /narnia/

we find all the files for this wargame i.e from level0 to level8 with their source code and executable files.



### Analysis

Executing the program narnia0 we are asked to correct value from 0x41414141 to 0xdeadbeef,
Inputting the value hello, it gives us bit of detail of buf and val, and also tells us that we are way off

![alt text](/images/Narnia/l0.png "title")


So since we do have the src code, we take a look at it

![alt text](/images/Narnia/l0_src.png "title")

Firstly, we see that the data type for the "var" variable is [long](https://www.tutorialspoint.com/cprogramming/c_data_types.htm) i.e it takes 8 bytes

The character buffer size is 20 bytes

Then comes the command 
**scanf("%24s",&buf)**

What the scanf function does is , it takes 24 characters and copies it to the buffer.
The buffer size is just 20 bytes though, *so what of the remaining 4 bytes?*

Our task in this case is to correctly overwrite the memory location that stores the value **0x41414141**

### Exploitation

So a first few tries for exploiting the program without much effort

- Using "BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB" as input we get 

![alt text](/images/Narnia/l0_BBBB.png)

so we did overwrite tha specific memory location with our text.Analyzing it we find that since the buffer size is 20 bytes and our supply of 47 characters , it is given that that memory location was to be overwritten

Also instead of supplying such a long text, we can use python like this

> python -c 'print("C"* 20+"DDDD")' | ./narnia0


![alt text](/images/Narnia/l0_CCDD.png)



so we have replaced the content of that memory location with 'Ds' with supplying 20C's and 4D's
which makes sense considering the buffer size


So if we apply this...
![alt text](/images/Narnia/l0_daed.png)

This didn't work and also the input we supplied "deadbeef" if we convert the output from program it show us
**daed**

This happened because most of the architecture support little endian Byte Order i.e a byte is written to the memory in reverse order

also we have supply the input in hexadecimal format

Use 
> \x{text}

to use hexadecimal characters

**USE the man ascii command on Linux to find out about the different characters in different base systems**

so to input "deadbeef" in hexadecimal we have to use

> \xef\xbe\xad\xde

so

> python -c 'print("B"* 20+"\xef\xbe\xad\xde")' | ./narnia


![alt text](/images/Narnia/l0_dead.png)

So now our value is "0xdeadbeef"


It's exactly where we want it

Taking a look at src code we find that on providng the value "deadbeef" this program will change the suid to the owner of the program and run /bin/sh

But before when we set the val as "deadbeef", the program just exit.

To prevent that from happening we we will be using *command group* i.e

(command 1;command 2) and pipe the result to our program

![alt text](/images/Narnia/l0_sol.png)

The passwords for levels are in /etc/narnia_pass/narnia{0..9}
___



### Level 1


