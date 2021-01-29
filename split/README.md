# Reverse Engineering

### Radare2

- ![alt text](https://i.imgur.com/uGork1j.png)
  
    - we see the system() memory address
		
- ![alt text](https://i.imgur.com/w035Elo.png)

    - f | grep 'useful'
    - we see that we have two interesting things a function & a variable
- ![alt text](https://i.imgur.com/FIGmifc.png)
  
    - here we have the variable holding the string "/bin/cat flag.txt"
    
- ![alt text](https://i.imgur.com/aGeDcxD.png)
  
    - and here's the usefulFunction

---

# Exploitation
- lets feed 40 A's into the program with 4 B's

![alt text](https://i.imgur.com/GuF98o6.png)
- padding amount: 40
- partial RIP control as seen '0xa42424242'

---

# Exploit Development
- so we need a pop RDI gadget
- and we already know that we need to feed /bin/cat flag.txt to the system() function 
- so:

![alt text](https://i.imgur.com/yDeTepa.png)

- payload explained: padding the buffer size down with 40 A's then popping RDI off the stack
- writing "/bin/cat flag.txt" to the top of the stack putting that know into RDI and calling system() as its only argument

![alt text](https://i.imgur.com/VQBE4oz.png)
