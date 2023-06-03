# SEED Labs – Buffer Overflow Attack Lab (Set-UID Version)

## Semana 5 e 6

### category-software/Buffer_Overflow_Setuid (32xbit only)

### Task 1 : Invoking the shellcode

* First we download all the necessary files for this lab.
* We disable the Adress Space Randomization so that the stack and heap addresses are always the same in every execution.
* Link /bin/sh to bin/zsh to set off the Set UID preventions.
* Verify if 32-bit version give us access to the shell.
 ![t156](https://user-images.githubusercontent.com/116512784/227749556-fd0e535d-b9e7-4e20-8132-ea9761bd6cf0.png)
Here we did all steps above and verify that a shell was created(for 32 bit)!

### Task 2 : Understanding the Vulnerable Program

* With command make, we ran a makefile which will compile stack.c for the four (L1, L2 ,L3 and L4 BUF_SIZE values) such as turning off the StackGuard and set them a Set-UID program, at once.
* We also check the permissions of the final programs.
![t256](https://user-images.githubusercontent.com/116512784/227750759-0a3d5fab-e2a2-4b39-95da-a3dfb69a7571.png)

### Task 3 : Launching Attack on 32-bit Program
* First we create the empty file called badfile.
* To know the distance of the locations of each variable in the stack we'll start to run the debug version of the program. We will set a break point at function bof() and then run it.
![t3156](https://user-images.githubusercontent.com/116512784/227752370-3b2e0dc9-f2aa-45e2-83a5-8ae6e9de92b9.png)
![3456](https://user-images.githubusercontent.com/116512784/227752670-f49275b0-6643-4484-a629-96b11d856f2a.png)
![3556](https://user-images.githubusercontent.com/116512784/227752712-6e089479-c999-4b95-b6fa-0ea9453b3877.png)

#### Launching Attacks
* Finally, we will change the variables in the python script in order to correctly exploit the vulneability. The buffer will receive the returning file content and will overflow.
* The offset (the distance between the buffer and return address(ret)) is set to 112.
* The ret value is changed to the address we want the program to jump to (ebp).
* The start value is the start of the shellcode in the buffer and is set to 256.

![python_script](https://user-images.githubusercontent.com/116512784/230515580-dcfcecdc-1a39-4136-9f5e-48731d78f0ec.png)
