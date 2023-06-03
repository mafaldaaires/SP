# SEED Labs – Format String Attack Lab
## Semana 7

* Turning off Countermeasure 

The modern operating systems uses address space randomization to randomize the starting address of heap and stack. For this lab, we have to turn off the address randomization so that we know the exact addresses.

![F2 1](https://user-images.githubusercontent.com/116512784/230683386-30e733af-881c-47d1-9694-c6e3f70bb34f.png)


* The vulnerable program

![F2 2](https://user-images.githubusercontent.com/116512784/230686510-543fb5fc-9994-471e-8ac1-027582127775.png)

We then set up the lab environment using docker-compose.yml file.

### Task1 
### Crashing the Program

We will send a benign message to this server using command *echo hello* meanings sending message "hello" to the server:

![Task1(1)](https://user-images.githubusercontent.com/116512784/230725164-233c3192-8294-48e8-9d06-a4481ed52c40.png)
![Task1(3)](https://user-images.githubusercontent.com/116512784/230725169-eca7900d-e0c9-47c7-bab4-a75eb8886e70.png)

and using command *cat file* by creating the file **file.txt** and sending the content of the file to the server:

![Task1(21)](https://user-images.githubusercontent.com/116512784/230725088-c420ffaf-8566-4eca-8321-09a1df89c693.png)
![Task1(2)](https://user-images.githubusercontent.com/116512784/230725093-1a8952c3-40f2-476b-8bc2-2509673df383.png)

### Task 2 
### Printing Out the Server Program’s Memory

#### Task 2.A : Stack Data
Knowing that 1 byte=8 bits, to print out 4 bytes we would need 32 %x format specifiers.

![2 A(1)](https://user-images.githubusercontent.com/116459746/235203984-f4f573d1-1944-47da-a1a8-a98ca50de60d.png)
![2 A(2)](https://user-images.githubusercontent.com/116459746/235203797-35752df4-0eaf-468e-b996-eb93f8510cfa.png)


#### Task 2.B : Heap Data


### Task 3
### Modifying the Server Program’s Memory

#### Task 3.A : Change the value to a different value

#### Task 3.B : Change the value to 0x5000 
