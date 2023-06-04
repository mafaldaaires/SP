# SEED Labs - Public-Key Infrastructure (PKI) Lab
## Semana 12 e 13

### Task 1
#### Becoming a Certificate Authority (CA)

Copy the configuration file (openssl.cnf) into the current directory.

![1](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/d1ad3dd3-215e-4feb-ba20-e2a070697b4e)

According to its [ CA_default ] section, we need to create new sub-directories and files.

[ CA_default ] section:

![2](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/82db898a-ac47-410a-9961-f8c0757847af)

According to [ CA_default ] section we need to:

![3](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/ba773f5b-a88b-46e6-ac27-070b57e101fa)

After this, we return to the parent directory using command *cd ..*

We generate a self-signed certificate for our CA.
This means that this CA is totally trusted and its certificate will serve as the root certificate.

![20](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/84f3d0df-dba4-4518-8ef1-46c24aa62436)

The output of the command are stored in two files: *ca.key* and *ca.crt*.

The file *ca.key* contains the CA's private key and the file *ca.crt* contains the public-key certificate.

                              *ca.crt* : public-key certificate
![21](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/89bcfc3d-034a-4c0d-b8e8-bae6b45c7aa1)

                                  *ca.key*: private key
![23](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/281e2a5d-80e5-4ac9-9515-52885e4e51e1)


#### Questions
1. What part of the certificate indicates this is a CA’s certificate?

Basic constraints: flag identifying certificate is CA.
 
 ![x6](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/dc0b453e-4a2d-43ea-8caa-edacb9e38a0d)

2. What part of the certificate indicates this is a self-signed certificate?

![x1](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/bb5314a3-8c21-4464-954a-b3cd72672fd7)

We can prove this is a self-signed certificate, because __Subject Key Identifier__ and __Authority Key Identifier__ are equal.

3. In the RSA algorithm, we have a public exponent *e*, a private exponent *d*, a modulus *n*, and two secret numbers *p* and *q*, such that n=pq.  Please identify the values for these elements in your certificate and key files.

public expoent *e* is visible both in the certificate file and in the key, taking the value 65537.

private expoent *d*, modulus *n* and the two secret numbers *p* and *q*, also known as *prime1* and *prime2*  respectively are only visible in the key file.

Modulus | Exponents | Primes
:---------:|:---------:|:--------:
![x3](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/ad29bca8-a6d1-4d9a-abe7-768a77555dd3) | ![x4](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/62cf45a3-9643-47d9-ad43-14f554e3a860) | ![x5](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/7d717f67-48b1-45c4-a49d-c146fb29cb33)




### Task 2
#### Generating a Certificate Request for Your Web Server

The command to generate a CSR is quite similar to the one we used in creating the self-signed cer-tificate for the CA.
The -x509 option is necessary, because otherwise it generates a request. With it, it generates a self-signed certificate.

The purpose of this task is generate a CSR for *www.bank32.com*.

![1](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/60e5d0be-eaee-4812-bbc4-49d2a634f188)

The command will generate a pair of public/private key, and then create a certificate signing request from the public key. 

                              decoded content of the CSR
![25](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/b3693c60-14ae-4802-8d49-e0df9e0fe074)

                                        private key files
![26](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/f02e0327-2d9e-4c4b-9cee-28d99dadedab)

Especially for the following tasks, we need to add two alternative names to the certificate signing request. (Corrigir)

![27](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/18d8574b-9f1f-4e4d-a29b-826ef9d143b3)


### Task 3
#### Generating a Certificate for your server

For security reasons, the default setting in *openssl.cnf* does not allow the "openssl ca" command to copy the extension field from the request to the final certificate. To enable that we need to go to the configuration file and uncomment the following line:

![6](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/e9424787-1647-4d83-b14f-6d831aa84719)

The following command turns the certificate signing request (server.csr) into an X509 certificate (server.crt), using the CA’s ca.crt and ca.key.

![31](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/f3d636f7-da8b-4264-9cd7-887dcd796afe)

As we print out the decoded content of the certificate we came along with this line, which means the alternative names are included in certificate. 

![x7](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/16d2a3f7-167e-4be7-b952-853987a0f497)

### Task 4
####  Deploying Certificate in an Apache-Based HTTPS Website

We go to the image_www folder.
In there we see a bank32_apache_ssl.cnf file and a DockerFile.
We need to create the environment (docker-compose build and docker-compose up).
In another page, we execute the current process (docker exec -it (process id) /bin/bash).
In there, we write two instructions:

- a2enmod ssl  //Enable the SSL module

- a2ensite bank32_apache_ssl  //Enable the sites described in this file

Now we are all ready to start, so we start the Apache server
##### service apache2 start

After this, we can search for our serve. We go to firefox and search https://www.bank32.com/
However, the link appears to be untrusted, as shown in the following picture.

![x10](https://github.com/DCC-FCUP-SP/sp2223-t04g06/assets/116459746/412529e5-40f7-4091-ba39-64187e5fbdde)

This means that our certificate has an unknown (or untrusted) CA.
In order to fix it, we need to add our CA in the list of truted CAs within Firefox. To do this we need to load a certificate into Firefox, so we search: __about:preferences#privacy__, go to *View Certificates* and import the file "ca.crt".

After this, we can search for our server.

