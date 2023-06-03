# CTF 5

## DESAFIO 1
Analisar proteções:

![s5um](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/63dc76b2-b88b-4a9d-aa98-dedee01b868e)

"Ao analisar podemo-nos aperceber que a arquitetura do ficheiro é x86 (Arch), não existe um cannary a proteger o return address (Stack), a stack tem permisssão de execução (NX), e as posições do binário não estão randomizadas (PIE), por fim existem regiões de memória com permissões de leitura, escrita e execução (RWX), neste caso referindo-se à stack."

### "...e tentar responder às seguintes questões"

#### 1. Existe algum ficheiro que é aberto e lido pelo programa?

Sim, podemos observá-lo simplesmente dando como input uma simples string, o que abre o ficheiro "mem.txt".

![s5dois](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/9323199a-8b18-4383-a0f9-8a233af913e2)

#### 2. Existe alguma forma de controlar o ficheiro que é aberto?

O ficheiro a ser aberto está guardado numa variável, por isso sim, é possível!

![1](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/95ee2fb4-dfad-4b40-862f-43e092158540)

#### 3. Existe algum buffer-overflow? Se sim, o que é que podes fazer?
Existe! Podemos aceder à memória na stack fora do buffer defenido.

### Exploit
Sabemos que se escrevermos exatamente 20 caracteres seguido do nome do ficheiro que queremos executar, iremos provocar um overflow com os primeiros 20 caracteres e o nome do ficheiro que pretendemos abrir será rescrito na stack fora do buffer, permitindo assim que este possa ser chamado.

![s5tres](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/36a37816-4eac-4921-bd13-962163d8ef61)

Aplicamos essa alteração ao ficheiro com o exploit-example.py.

<details><summary>exploit-example.py</summary>
<p>

![s5quatro](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/57921722-0e28-4696-9a09-76a6ed42a9d8)
 
</p>
</details>

Agora, correndo o program e... 

![s5cinco](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/95dd71ea-6200-4e80-ae19-a49215d0548c)
 
 obtemos a flag.
 
 ## DESAFIO 2
 
 Analisando as proteções, verificamos que não houve alterações nas permissões do executável:
 
![2](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/fe348af4-862e-4398-8070-fd729b79c889)
 
 ### Questões
 
#### 1 Que alterações foram feitas?
 Inicializaram um array de tamanho 4 entre o meme.txt file e o buffer.
#### 2 Mitigam na totalidade o problema?
 O buffer continua a guardar no buffer por isso não.
#### 3 É possível ultrupassar a mitigação usando uma técnica similar à que foi usada anteriormente?
 Sim.

### Exploit
Através do ficheiro main.c conseguimos saber que 0xfefc2223 é o endereço que permitirá dar overwrite do ficheiro flag.txt.

Adicionamos esse endereço (com a ajuda do pwn) entre os 20 caracteres e "flag.txt" e esse ficheiro será executado.

Mudamos a script dada no Desafio 1 com as instruções necessárias (mencionadas acima):

<details><summary>exploit-example.py</summary>
<p>

![s521](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/c5d0fc1d-2f8d-4f85-8e6f-1b44058d3a5f)
 
</p>
</details>

Executando a script, a flag é imprimida.

![s522](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/f11fa72b-de42-4742-931c-1b2ff1d46711)
