
# Info gathering

![checksec](https://user-images.githubusercontent.com/71098497/106340283-4133cf00-6267-11eb-8562-c552d044a1ab.png)

standard protections...


# Writing in Memory
Next we check where we can read and write in the memory.

![iS](https://user-images.githubusercontent.com/71098497/106340538-20b84480-6268-11eb-820b-129f7aa0de1b.png)


we can see that .data has read and write permissions. Since it is also uninitialized, we can use it to hold strings. Again make note of the address.

Next we need to figure out how to use gadgets to write the string in the .data section. To do this we must utilize pop;pop;ret gadgets and pop;ret gadgets. 



# Useful Gadgets
we use radare2 to see if the executable has either usefulFunction or usefulGadgets like what other ROP emporium challenges had.

![radare2](https://user-images.githubusercontent.com/71098497/106341868-c66db280-626c-11eb-867b-412f70d3255c.png)

![useful](https://user-images.githubusercontent.com/71098497/106340484-f49cc380-6267-11eb-8523-c1d9b8c94ef5.png)

there is a usefulGadgets. We should also make note of the address. Now we will take a look at usefulGadgets.

![r14, r15 string moving](https://user-images.githubusercontent.com/71098497/106341024-b99b8f80-6269-11eb-9535-2d58307f2358.png)

We can see that we can use r14 and r15 registers to move strings into the .data section. Now that's out of the way, we can start looking for pop;pop;ret gadgets.


# Searching for More Gadgets

we can use ropper to search the write4 executable. 

![r14:r15 pop, pop, ret gadget](https://user-images.githubusercontent.com/71098497/106341425-44c95500-626b-11eb-96aa-11342069c1a7.png)

found it! Make note of the address.
<br>
Next we must find the pop;ret gadget (pop rdi). We use ropper again for this task.

![r14:r15 pop, pop, ret gadget](https://user-images.githubusercontent.com/71098497/106341543-aab5dc80-626b-11eb-95a4-e330445a8395.png)


# Problem and Exploit

when we look at the executable, we cannot find system() among the functions in the executable. This was a major hiccup for us. Instead we must resort to print_file. Before we utilize print_file, we have to find the address of it first.
<br>
You can list the functions of the executable in radare2 by inputting:
```
f
```
Find and make note of the address of the print_file function.

Now we will use a write primitive function in our exploit in place of system() to send our payload in order to get the flag. Problem was... we ran into a huge amount of annoying bugs. It took too much time so we will most likely bug fix the exploit in the future. However, I believe we are headed in the right direction. Regardless, take a look at the exploit we developed!