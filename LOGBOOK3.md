
# Trabalho realizado na Semana #3

## Identificação

- A vulnerabilidade que escolhemos tem o nome de Shellshock, também conhecida por CVE-2014-6271.
- Esta vulnerabilidade afeta o Bash, um interpretador de comandos amplamente utilizado em sistemas Unix e Linux.
- A vulnerabilidade Shellshock permite que um atacante execute código arbitrário em um sistema-alvo, explorando uma falha na forma como Bash avalia as variáveis de ambiente.
- O GNU Bash através de 4.3 processa sequências de caracteres após definições de funções nos valores das variáveis de ambiente, o que permite que atacantes remotos executem código arbitrário por meio de um ambiente criado.

## Catalogação

- Em setembro de 2014, a vulnerabilidade Shellshock foi descoberta por Stephane Chazelas, um investigador de segurança, o qual reportou ao NSCS (Centro Nacional de Segurança Cibernética) francês.
- A solução original para esta vulnerabilidade estava incorreta. O CVE-2014-7169 foi atribuído à vulnerabilidade residual derivado da correção incorreta da vulnerabilidade Shellshock. 
- O CVSS Score (Common Vulnerability Scoring System), um sistema de pontuação utilizado para avaliar a gravidade de vulnerabilidades de segurança, avalia a vulnerabilidade Shellshock com 10 (a pontuação mais alta), o que significa ser uma vulnerabilidade grave que requer atenção imediata.
- Confidentiality Impact, Integrity Impact, Availability Impact: Complete; Access Complexity: Low; Authentication: Not required.

## Exploit

- Qmail SMTP Bash Environment Variable Injection (Shellshock): Este módulo explora uma vulnerabilidade Shellshock no Qmail, um  software público escrito em C que é executado em sistemas Unix; Plataforma: Linux; Autor: Metasploit; Type: Remote.
- Shellshock Bashed CGI RCE': Este módulo explora a vulnerabilidade shellshock no apache cgi. Isso permite executar qualquer carga útil de metasploit que quisermos; Plataforma: CGI; Autor: Fady Mohamed Osman; Type: Webapps.
- Reverse Shellshock: É um exploit que permite que um invasor crie uma shell invertida no servidor de destino afetado pela vulnerabilidade Shellshock, fornecendo assim acesso remoto ao sistema.
- Advantech Switch Bash Environment Variable Code Injection (Shellshock): Este módulo explora a vulnerabilidade Shellshock, uma falha em como o shell Bash lida com variáveis de ambiente externas.

## Ataques

- Em 2014, Yahoo confirmou que os seus utilizadores haviam sido atacados através da vulnerabilidade “Shellshock”.
- Em 2015, no Departamento de Saúde dos EUA, foram roubadas informações sobre cerca de 4 milhões de pacientes.  O sistema havia sido atacados usando a vulnerabilidade “Shellshock”.
- Em 2014, a Asus confirmou que os seus routers tinham sido comprometidos usando a vulnerabilidade “Shellshock”. Os atacantes conseguiram instalar malware nos routers, permitindo que controlassem o tráfego da Internet dos usuários.
- Em 2014, o PayPal também confirmou um ataque aos seus sistemas através da vulnerabilidade “Shellshock”. Os invasores conseguiram aceder informações de login e senha de funcionários, mas não foram capazes de aceder as informações dos clientes.
