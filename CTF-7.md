# CTF 7
## Desafio 1

![s711](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/ecc49b44-5fdf-43b9-a49e-27d92362d562)

- RELR0 parcial. Faz com que o GOT (Global Offset Table) venha antes de BSS (secção usada pelo compilador para variáveis globais e static) elimina o risco de existirem buffer overflows numa variável global que dê overwrite a GOT.

- Com Stack Cannaries. Se o cannary, um valor secreto na stack, for modificado, o programa termina imediatamente.
Porém existem duas maneira de dar a volta a esta proteção, quer seja através de leaking do endereço ou bruteforce do cannary

- A proteção de NX ativada. Para dar a volta a este problema, são usados frequentemente ataques ROP(Return-Oriented Programming).

- Sem a proteção PIE é possível aceder à localização de um ficheiro guardado em memória facilmente, porque é sempre o mesmo (não é aleatório).

### Questões

#### Qual é a linha do código onde a vulnerabilidade se encontra?
Na linha onde acontece "printf(buffer)".
#### O que é que a vulnerabilidade permite fazer?
Não são utilizadas format strings no printf, portanto, é possível obter controlo sobre o formato do output, o que lhe permite aceder ao seu conteúdo de diversas maneiras.
#### Qual é a funcionalidade que te permite obter a flag?
Temos que passar o endereço da variável flag como input do programa seguido do string format "%s" que acede à variável guardada nesse endereço e coloca na consola o seu conteúdo, mais especificamente a flag que pretendemos obter.

### Exploit

Correndo o exploit_example file damos origem a um processo local com identificador pid 3424.
Para sabermos o endereço da flag iremos usar gdb, usando o processo local que está em curso.

![s712](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/5592db96-db4c-441d-8843-c01d5b0c5b32)

![s713](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/c5d7e30b-99ca-49b6-bbdf-7010099f8c6a)

Procuramos pelo endereço da flag e, com isso, reconstruímos a script de modo a retornar-nos a flag.

![s714](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/33addd91-ff9d-4384-8ffb-8788a69e26cb)

Script atualizada:

<details><summary>exploit_example.py</summary>
<p>

![s715](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/cfc9bf58-a1fb-411f-8f66-350a4b6bc08b)
 
</p>
</details>

Correndo a script, temos acesso à flag.

![s716](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/97b9c337-e880-4509-a41e-dd6efa156b93)

## Desafio 2

![s721](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/28539d2e-91f5-427e-95c0-cf230bceee14)

- RELR0 parcial. Faz com que o GOT (Global Offset Table) venha antes de BSS (secção usada pelo compilador para variáveis globais e static) elimina o risco de existirem buffer overflows numa variável global que dê overwrite a GOT.

- Com Stack Cannaries. Se o cannary, um valor secreto na stack, for modificado, o programa termina imediatamente.
Porém existem duas maneira de dar a volta a esta proteção, quer seja através de leaking do endereço ou bruteforce do cannary

- A proteção de NX ativada. Para dar a volta a este problema, são usados frequentemente ataques ROP(Return-Oriented Programming).

- Sem a proteção PIE é possível aceder à localização de um ficheiro guardado em memória facilmente, porque é sempre o mesmo (não é aleatório).

#### Qual é a linha do código onde a vulnerabilidade se encontra? E o que é que a vulnerabilidade permite fazer?
A vulnerabilidade encontra-se na linha onde "gets(buffer)" ocorre.

#### A flag é carregada para memória? Ou existe alguma funcionalidade que podemos utilizar para ter acesso à mesma?
Não, no código a flag não é carregada para a memória. 
Não obstante, se a variável key for equivalente a "0xbeef" é executada a system call "system("/bin/bash")" que nos permite ter acesso à consola em modo interativo e executar o comando "cat flag.txt".

#### Para desbloqueares essa funcionalidade o que é que tens de fazer?
Neste desafio temos o obstáculo acrescido da presença de um canário. Para desbloquearmos a funcionalidade temos de passar o valor correspondente ao mesmo ("0xbeef") através da formatação de strings, ou seja, passando como input o endereço onde está guardada a variável "key" e através do string format '%n' inserir nela o valor "beef".

Primeiro, usamos gdb (debugger) para encontrar o endereço da chave na pilha.

Para isso, temos de criar um exploit de modo a poder pesquisar na nossa memória.

<details><summary>exploit_example.py</summary>
<p>

![s722](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/5adb8891-f732-482a-85ea-cd38fc0f9f05)
 
</p>
</details>

Correndo o exploit_example file damos origem a um processo local com identificador pid 5327.
Para sabermos o endereço da chave na pilha iremos usar gdb, usando o processo local que está em curso.

![s723](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/1a839589-18c9-411d-9c06-0d31ce9d36bf)

![s724](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/5b74077e-f5b4-4c17-956f-faaecea2a0f3)

![s725](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/ae055982-9f3f-4c0b-93fd-a0778fc0cb1e)

Atualizando o nosso exploit para acedermos à flag:

<details><summary>exploit_example.py</summary>
<p>

![s726](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/82d511b2-c2b8-49e8-8cad-a76ad788f367)

</p>
</details>

Correndo o exploit, obtemos a flag.

![s727](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/733eecc2-bce1-43dc-b050-346ff265f5c8)

...

![s728](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/af492b17-d5e4-417b-8292-8a07f6cd19db)

![s729](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/a6c774f1-f207-4f13-854b-c9b64d7bb722)
