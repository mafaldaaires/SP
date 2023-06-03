# SEED Labs – Environment Variable and Set-UID Program Lab
## Semana 4 
### Task 1: Manipulating Environment Variable
printenv : comando faz print de todas as variáveis ambiente
![image](https://user-images.githubusercontent.com/116459746/230239251-0847070e-d4f8-4b27-a682-4e2e14e2d8af.png)

printenv PWD : especifica a variável ambiente selecionada

env | grep pwd : especifica a variável ambiente selecionada
<img width="250" alt="image" src="https://user-images.githubusercontent.com/116459746/230239379-320e5388-e141-44e5-a466-353dac2858ef.png">

### Task 2: Passing Environment Variables from Parent Process to Child Process
<img width="316" alt="image" src="https://user-images.githubusercontent.com/116459746/230239664-b9ffad1a-1c53-4341-8dc4-ad4b2031d1e3.png">
Comparamos as diferenças no programa myprintenv.c quando usamos parent process ( com output redirecionado para task2.1. )  e quando usamos child process ( com output redirecionado para task2.2. );
A comparação foi feita com o comando diff.

Com isto observamos que não haviam diferenças, o que significa que o processo pai e processo filho apresentam as mesmas variáveis de ambiente, ou seja as variáveis são herdadas do pai para o filho.

###  Task 3: Environment Variables and execve()
#####  STEP 1: compilar e correr myenv.c onde o terceiro argumento da função execve( ) é NULL
<img width="228" alt="image" src="https://user-images.githubusercontent.com/116459746/230239800-2178333a-408c-4835-8bf6-4c52b250ce32.png">
As variáveis de ambiente não foram passadas para o child process com o terceiro argumento NULL.

##### STEP 2: compilar e correr myenv.c substituindo o terceiro argumento NULL por environ
![image](https://user-images.githubusercontent.com/116459746/230239964-08110199-4370-405a-8a12-5b476f7d0c8f.png)

##### STEP 3:
Com a função execve( ) o processo “atual” é substituído pelo programa especificado no seu primeiro argumento em vez de criar um novo processo como acontece na função fork( ).

Porém se o terceiro argumento de execve( ) for definido como environ, as variáveis de ambiente do processo atual serão passadas ao programa especificado. Caso contrário, se for definido como NULL nenhuma variável de ambiente será passada.

### Task 4: Environment Variables and system()

![image](https://user-images.githubusercontent.com/116459746/230240296-9a018c22-eca3-4947-86d5-b6c46a52b64e.png)

O programa task4.c corre o comando env através da função system( ). Com isto, as variáveis de ambiente do processo que realiza a chamada são passadas para o novo programa /bin/sh.


### Task 5: Environment Variable and Set-UID Programs

![image](https://user-images.githubusercontent.com/116459746/230240358-6cc9930e-cb0d-4dcc-b785-5650aeba323b.png)

![image](https://user-images.githubusercontent.com/116459746/230240430-cba032ea-da82-4ad1-9413-9fb618377726.png)

O programa task5.c faz print de todas as variáveis ambiente.

Depois de corrermos o SET-UID verificamos que a variável ”LD_LIBRARY_PATH” não é passada  para o child process;

Isto acontece, pois a variável “LD_LIBRARY_PATH” é ignorada pelo vinculado dinâmico, pois caso contrário poderia ser controlada maliciosamente ao ser executada dentro de um programa Set-UID, o que pode levar a violações de segurança.

### Task 6: The PATH Environment Variable and Set-UID Programs

![image](https://user-images.githubusercontent.com/116459746/230240639-99b7ca10-9098-4976-8211-300efb165569.png)

O código fornecido executa o comando ls no terminal através da função system( ).
Este programa é um programa SET-UID vulnerável já que é usado o absolute path ls para executar o comando ls, estando o relative path associado à variável de ambiente PATH;  esta variável pode ser manipulada direcionando o  para um programa malicioso com o mesmo nome do PATH absoluto para o comando ls (ex: ls.c).

O programa seguinte ls.c será considerado o nosso programa com código malicioso que se encontra numa pasta Task6 para o qual, mais para a frente, vai ser apontada a variável PATH.

![image](https://user-images.githubusercontent.com/116459746/230241945-2b3afef2-e75a-449d-bd00-71853e09d735.png)

De seguida tornamos e verificamos o programa fornecido como Set-UID.

![image](https://user-images.githubusercontent.com/116459746/230241985-a8d33700-fc36-4863-a547-d89cce75acb0.png)

Alteramos o caminho da variável PATH para apontar para a pasta Task6.
Para superar o mecanismo de segurança do shell dash de descartar os privilégios do programa SET-UID ao ser chamado, vinculamos o /bin/sh a outro shell (zsh).

![image](https://user-images.githubusercontent.com/116459746/230242025-57a8dd82-bc3a-4be0-b454-18a5bc15697f.png)

Corremos o código fornecido e verificamos que o nosso código malicioso foi selecionado ao invés do /bin/ls.

<img width="327" alt="image" src="https://user-images.githubusercontent.com/116459746/230242068-388c0f3a-6873-49f3-9cdc-d28e6ae73292.png">

Depois alterámos a variável PATH para que o nosso  programa ls seja primeiro encontrado ao invés de /bin/ls.
Concluímos que a variável PATH pode ser modificada de modo a apontar para pastas com códigos maliciosos.

### Task 7: The LD PRELOAD Environment Variable and Set-UID Programs

##### Step1
<img width="277" alt="image" src="https://user-images.githubusercontent.com/116459746/230240803-1cf9663b-1de6-4c56-9ee5-3d350e46d738.png">
<img width="482" alt="image" src="https://user-images.githubusercontent.com/116459746/230240834-5c345c0f-6bc8-4ce8-850c-788631b641a2.png">
<img width="427" alt="image" src="https://user-images.githubusercontent.com/116459746/230240854-59692704-0860-481a-bf6a-d669e22122c6.png">
<img width="252" alt="image" src="https://user-images.githubusercontent.com/116459746/230240876-3e3d4874-8d14-46b6-ac67-79dcc065d3c4.png">

##### Step 2
1. Make myprog a regular program, and run it as a normal user.  
<img width="482" alt="image" src="https://user-images.githubusercontent.com/116459746/230241361-9aa1f578-69bd-44dd-b55a-62e6f32dbc78.png">

2. Make myprog a Set-UID root program, and run it as a normal user. 
<img width="327" alt="image" src="https://user-images.githubusercontent.com/116459746/230241398-99881006-c777-4410-8763-7b8823f11ab8.png">

3. Make myprog a Set-UID root program, export the LD PRELOAD environment variable again in the root account and run it. 
<img width="439" alt="image" src="https://user-images.githubusercontent.com/116459746/230241447-53e5607a-b5ea-4409-9d5b-d274ec7aa66f.png">

In root do:
Export LD_PRELOAD again and
<img width="439" alt="image" src="https://user-images.githubusercontent.com/116459746/230242478-fc453200-18de-4f5a-b966-454e75c0ef8b.png">

4. Make myprog a Set-UID user1 program, export the LD PRELOAD environment variable again in the user1 account and run it. 

![image](https://user-images.githubusercontent.com/116459746/230242497-93faccb4-016e-4ad0-9021-873c17af6f5d.png)

In user1, we export LD_PRELOAD again, because child process does not inherit environment variables LD_* .

Just like we do in user1 should be the same in root.

### Task8 : Invoking External Programs Using system( ) versus execve( )
##### Step 1

![image](https://user-images.githubusercontent.com/116459746/230242648-e254fc25-4be5-405f-9ff9-aa4bc1b0c1b9.png)

![image](https://user-images.githubusercontent.com/116459746/230242656-6001e9fb-4f62-46de-8741-2d0a0f3bc7fe.png)

##### Step 2

![image](https://user-images.githubusercontent.com/116459746/230242787-e5f1ffe2-8be8-468b-91e1-cea006867d5e.png)

![image](https://user-images.githubusercontent.com/116459746/230242801-9a6758e2-3c7c-4961-a1a7-fcb9f241faa6.png)

### Task 9 : Capability Leaking

<img width="482" alt="image" src="https://user-images.githubusercontent.com/116459746/230242847-a15ff416-2f42-4d08-a5df-3c3d6e6167f6.png">
<img width="391" alt="image" src="https://user-images.githubusercontent.com/116459746/230242862-38caee0f-e1c1-4638-9298-2b6b788824cb.png">




