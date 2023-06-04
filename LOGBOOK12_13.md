# Trabalho semana 12/13
# Public-Key Infrastructure (PKI) Lab

## Tarefa 1
Copiar /usr/lib/ssl/openssl.cnf para o diretório atual com o comando:

cp /usr/lib/ssl/openssl.cnf ~/

<img width="443" alt="1" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/49475069-c983-4929-93d0-a8c9db6bfa87">


Abrir openssl.cnf e descomentar unique_subject.

<img width="402" alt="1" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/37905176-90cc-4d1f-a6e4-c2ceff867e9d">

Criar subdiretórios com os seguintes comandos:
mkdir demoCA
cd demoCA
mkdir certs crl newcerts

touch index.txt serial

<img width="398" alt="2" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/4f54ded9-c7c6-4d20-8ede-c0dc986af356">

echo "1000" > /serial
<img width="345" alt="3" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/5cb776c8-49e5-4f1a-a067-427509444cdb">


<img width="406" alt="4" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/6a5391ec-31a3-4041-80e9-ac7cccb95319">

Voltar para o diretório onde está o documento openssl e executar os seguintes comandos:
openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 \ -keyout ca.key -out ca.crt
openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 \-keyout ca.key -out ca.crt \-subj "/CN=www.modelCA.com/O=Model CA LTD./C=US" \-passout pass:dees
openssl req -in server.csr -text -noout
openssl rsa -in server.key -text -noout

O output são dois documentos: ca.cert e ca.key.

Quando abrimos ca.cert podemos verificar que é um certificado logo pela primeira linha "Certificate" e que está assinado, pela linha "Signature Algorithm: sha256WithRSAEncryption".

<img width="391" alt="5" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/89036412-070a-4a33-8727-ec9b19740f8e">

Em ca.key encontram-se os valores de modulus, public exponent, private exponent e dois primos prime1 e prime2.

## Tarefa 2
Gerar um par de chaves, uma privada e uma publica, com o seguinte comando:


<img width="409" alt="7" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/19005827-9e4f-4e94-bb95-d7619c998854">

Que nos dá como output server.csr e server.key.
Para ver o contéudo da chave podemos correr o comando: openssl rsa -in server.key -text -noout, onde podemos ver os seguintes componentes: modulus, publicExponent, privateExponent, prime1, prime2, exponent1, exponent2 e coefficient.

Correr o comando:
openssl req -new -key server.key -out server.csr -subj "/CN=www.bank32.com" -addext "subjectAltName = DNS:www.bank32A.com, DNS:www.bank32B.com"
para adicionar dois nomes alternavios ao certificate signing request.


## Tarefa 3

Correr o seguinte comando:
openssl ca -config myCA_openssl.cnf -policy policy_anything \-md sha256 -days 3650 \-in server.csr -out server.crt -batch \-cert ca.crt -keyfile ca.key
Para tornar o server.csr num certificado X509. Temos como outpur server.crt. 

<img width="410" alt="1" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/0117f2c7-128b-40ce-ab90-384427e24484">

<img width="408" alt="2" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/0dd605c3-ee43-417c-b39a-629e7a182858">

Descomentar 'copy_extensions = copy' no openssl.cnf:

<img width="412" alt="9" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/de2721f6-96de-4242-bd94-98d74d63f6c4">

Após correr o seguinte comando, conseguimos verificar que os nomes alternativos foram incluidos nos ficheiros.


<img width="414" alt="4" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/15148d86-45ee-4a1f-9c50-32d683a23ccf">

<img width="401" alt="3" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/9c33548a-1269-4403-8753-2126c1c25bff">


## Tarefa 4

Ir para o diretório image_www em Labsetup e verificar o documento bank32_apache_ssl.conf:


<img width="406" alt="5" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/d320f609-4629-4f87-a81e-ec8a90418130">

Construir o docker com dcbuild, fazer dcup e noutra janela do terminal correr os seguintes comandos:


<img width="407" alt="6" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/49f5db2b-b27d-4cbe-8b81-4a260e423118">

Abrimos o website:

<img width="413" alt="7" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/6f758d16-3172-4ab0-87cd-211ae39a4133">

Como podemos verificar pela imagem, o website não é seguro


<img width="416" alt="8" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/62507cc4-9755-4549-b91f-b8d834717bf7">

Para alterar isso, adicionamos o certificado image_www/certs/modelCA.crt, e já conseguimos acessar o nosso website seguramente:


<img width="416" alt="10" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/64804f41-8944-4bf2-b39c-9c014b5dbf0b">


## Tarefa 5

<img width="381" alt="Capture" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/c10a8815-eea1-42a0-bbbc-0c8f72566f5c">

Após adicionarmos www.example.com como host, este é o resultado:

<img width="233" alt="11" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/565e0f5a-9b93-4640-9c6e-185829489ee6">

Isto ocorre porque o website não está no certificado.


## Tarefa 6

Adicionar em /etc/hosts:


<img width="408" alt="ESTE2" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/a94dfa6b-3e92-44ce-8854-a8c4a1dc2bd7">


Correr:
openssl req -new -key server.key -out server.csr -subj "/CN=www.bank32.com" -addext "subjectAltName = DNS:www.bank32A.com, DNS:www.bank32B.com, DNS:www.instagram.com"

openssl ca -config openssl.cnf -policy policy_anything \-md sha256 -days 3650 \-in server.csr -out server.crt -batch \-cert ca.crt -keyfile ca.key

<img width="415" alt="ESTE" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/1a4b6efd-10b0-4485-98ee-2e68c8431634">



cp server.crt ~/Share/Labsetup/volumes

<img width="256" alt="6 2" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/3cc36c1c-c1df-4fb5-8a8d-007a43fb1036">

Alterar no documento bank32_apache_ssl.conf:

<img width="338" alt="6 3" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/df4ddc27-3fab-480f-b35b-f2b5d2f352e9">


Adicionar o certifcado ao browser; quando o user tenta acessar www.instagram.com vai apar o nosso website.

<img width="524" alt="final" src="https://github.com/DCC-FCUP-SP/sp2223-t04g08/assets/126345503/87c7b156-fbd9-42a2-aa66-b7f557614069">


