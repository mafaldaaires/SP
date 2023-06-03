# CTF 10
## Desafio 1

Usar ataque XSS.

Na porta 5002 do servidor ctf-sp.dcc.fc.up.pt, encontra-se um servidor web.
Após abrirmos o servidor web deparamo-nos com um input box.

Escrevemos a seguinte linha no input box:

    <script> document.getElementById('giveflag').click() </script>
 
![S103](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/53b94cea-c1e6-4c03-a50e-7e75833ea7c7)

![S104](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/b811bb5e-0251-4a4d-aeee-522283b1bf32)

![S105](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/2381636d-bb06-4419-8374-9eb8e8d94540)

Após o administrador aceitar o pedido para fornecer a flag, podendo demorar alguns segundos, aparece a flag.

## Desafio 2

Na porta 5000 do servidor ctf-sp.dcc.fc.up.pt, encontra-se um servidor web em PHP.

Após abrirmos o servidor web vemos:

- Uma página login

![s106](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/be641fc2-c8b3-4980-a40e-ec58cec4cd7f)

- Uma página secundária com dois recursos: **PING A HOST** e **CHECK YOUR NETWORK SPEED**

![s107](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/16f890d8-ffc8-4919-8902-f3640a4b76b7)

- Uma página secundária com o **SPEED TEST RESULT**

![s108](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/0dadb5ff-4589-4166-a99a-78a8cfff4632)

- Resultados do recurso **PING A HOST** (**PING RESULTS**)

![s109](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/764b49e4-1bb5-44f0-b411-611fe5b0b90a)

### ATAQUE
Descobrimos que podemos percorrer os arquivos usando o comando cd, tal como no terminal do nosso computador.

Contrariamente ao terminal, na barra **PING A HOST** é requerido o caracter ";" para finalizar o comando ping.

Para aceder à flag que se encontra no ficheiro /flags/flag.txt, basta escrever a seguinte linha na barra de input do **PING A HOST**:

    ; cd /flags; cat flags.txt

![s1011](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/47ef18b4-0182-498a-abe2-298436b907eb)

![s1012](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/e8c8e654-d7c6-4243-a7b7-da012214db78)

E nos resultados **PING RESULTS** imprimem a flag.
