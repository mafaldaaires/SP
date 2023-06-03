# CTF 12

## Desafio 1

Obtemos a flag cifrada (parâmetro __enc_flag__) através do seguinte comando:

![s121](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/8bd9b97f-7cdc-4e53-8584-1190e961434c)

Para decifrar esta flag usamos a seguinte script:

<details><summary>template.py</summary>
<p>

![s122](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/8e19fccc-dc06-4961-a9d2-df7f7071c21f)
  
</p>
</details>

Ao executar a script, a flag decifrada é imprimida.

![s123](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/58615e30-a41d-40fd-be6d-551b388b4208)


## Desafio 2

Na porta 6001 do servidor ctf-sp.dcc.fc.up.pt encontra-se um servidor que, sempre que te conectas envia duas mensagens cifradas que contém a mesma flag. Estas mensagens foram cifradas com RSA, onde foi utilizado o mesmo n mas dois e’s diferentes.

*n*, *e1* e *e2* são nos dados, onde *n* é o módulo e *e1* e *e2* são expoentes públicos.

Para obter as chaves públicas de *e1* e *e2* respetivamente (*c1* e *c2*) basta aceder à porta 6001 do servidor ctf-sp.dcc.fc.up.pt através do terminal.

![s124](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/ec4a63ff-9da1-4f88-a8a8-9d12460344f0)

Para recuperar a flag recorremos à seguinte script:

<details><summary>exploit.py</summary>
<p>

![s125](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/57b2c75f-f37c-4bc9-8824-d4e967a17863)

</p>
</details>

Ao executar a a script, a flag decifrada é imprimida.

![s126](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/7516e599-26c7-48ad-b7cd-11cbe024dd8d)
