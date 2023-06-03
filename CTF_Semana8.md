# CTF 8

## Desafio 1

Na porta 5003 do servidor ctf-sp.dcc.fc.up.pt, encontra-se um servidor web em PHP. Ao entrar neste servidor, deparamo-nos com uma página de login.

Olhando para o ficheiro index.php, nós concluimos que o servidor é vulnerável na seguinte linha:

![3](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/47b6ecc2-9c89-4c1b-a6ea-b84f855c7fb0)

Para encontrar o username e a password certas, recorremos ao ataque SQL Injection.

Recorremos assim à expressão __' or ''= '__ para o username e para a password.

![4](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/0d455fc7-6ded-414e-8f17-4a98990a7eae)

![5](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/bbe4af64-a6ed-4ebe-8836-199a0a1a2be2)


## Desafio 2

![s83](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/e4cce197-675c-4780-9f98-330c8e0f8100)

"PIE enabled" refere-se a uma técnica de segurança que torna a posição de execução do código em memória aleatório, tornando mais difícil explorar vulnerabilidades de BOF(Buffer Overflow).

A vulnerabilidade está na linha seguinte, onde a função get() lê um byte do stdin até que \0 ou \n seja encontrado.

![6](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/e7b7014b-7e67-4fd4-b48a-143a1ae639b2)

Esta vulnerabilidade permite-nos sobrescrever o valor do endereço de retorno, injetar shellcode e, em seguida, saltar para esse código, sendo executado automaticamente.

<details><summary>exploit-example.c</summary>
<p>

![s84](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/5032ab88-59eb-47cc-883a-86b9d9976b74)
  
</p>
</details>

Correndo o exploit, conseguimos aceder à flag.

![s85](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/18ec4d86-2906-45c8-a825-51955de67495)

![s86](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/c12cc7ef-25f2-4075-97d6-f277388ac7e3)
