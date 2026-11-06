# Desafio Prático: Simulação de Ataques de Força Bruta e Mitigação

Documentação técnica do projeto prático de segurança da informação realizado com **Kali Linux**, **Medusa** e ambientes vulneráveis (**Metasploitable 2** e **DVWA**), focando em simulações de ataques de força bruta e na aplicação de medidas defensivas.

---

## 📋 Índice
1. [Sobre o Projeto](#-sobre-o-projeto)
2. [Arquitetura do Ambiente](#-arquitetura-do-ambiente)
3. [Fase 1: Configuração e Descoberta de Ativos](#-fase-1-configuração-e-descoberta-de-ativos)
4. [Fase 2: Execução de Ataques Simulados](#-fase-2-execução-de-ataques-simulados)
   - [Ataque 1: Força Bruta em FTP](#1-força-bruta-em-ftp-metasploitable-2)
   - [Ataque 2: Automação em Formulário Web (DVWA)](#2-automação-de-tentativas-em-formulário-web-dvwa)
   - [Ataque 3: Password Spraying em SMB](#3-password-spraying-em-smb-com-enumeração)
5. [Fase 4: Recomendações e Mitigação](#-fase-4-recomendações-e-mitigação)

---

# Sobre o Projeto

Este projeto tem como objetivo demonstrar na prática os conceitos de auditoria de segurança e simulação de ameaças em um ambiente controlado. Foram explorados cenários de testes de autenticação em diferentes camadas (serviços de rede e aplicações web), analisando o comportamento dos sistemas frente a ataques automatizados e identificando as devidas formas de remediação.

---

# Arquitetura do Ambiente

O laboratório foi montado utilizando a ferramenta **VirtualBox** em uma rede totalmente isolada para evitar impactos externos:
* **Máquina Atacante:** Kali Linux (Última versão oficial).
* **Máquina Vítima (Alvo):** Metasploitable 2 (Sistema Linux intencionalmente vulnerável).
* **Tipo de Conexão:** Rede Interna (*Internal Network*) ou Adaptador *Host-only*, garantindo comunicação exclusiva entre as VMs.

---

# Fase 1: Configuração e Descoberta de Ativos

Antes de iniciar os testes, foi necessário mapear a rede interna para identificar o endereço IP da máquina alvo (Metasploitable 2).

### Comandos Utilizados:
Utilizando o Kali Linux, executei a varredura da faixa de IP da rede interna:
```bash
# Varredura ARP para identificar hosts ativos
sudo arp-scan -l

# Alternativa utilizando Nmap (Ping Scan na faixa /24)
nmap -sn 192.168.56.0/24

Fase 2: Execução de Ataques Simulados
A ferramenta principal escolhida para os testes de autenticação foi o Medusa, por sua alta performance e suporte a múltiplos módulos de serviços.

1. Força Bruta em FTP (Metasploitable 2)
O objetivo foi testar a fragilidade do serviço FTP (vsftpd) contra uma wordlist de senhas comuns.

Comando:

Bash
medusa -h 192.168.56.101 -u msfadmin -P /usr/share/wordlists/rockyou.txt -M ftp
Explicação dos parâmetros:

-h 192.168.56.101: Define o endereço IP do alvo.

-u msfadmin: Define o nome de usuário fixo que será testado.

-P /usr/share/wordlists/rockyou.txt: Utiliza a famosa wordlist padrão do Kali Linux contendo milhões de senhas em texto plano.

-M ftp: Aciona o módulo de ataque voltado ao protocolo FTP.

2. Automação de Tentativas em Formulário Web (DVWA)
O objetivo foi automatizar o processo de adivinhação de credenciais em uma aplicação web vulnerável (Damn Vulnerable Web Application - DVWA) com o nível de segurança configurado como Low.

Comando:

Bash
medusa -h 192.168.56.101 -U users.txt -P passwords.txt -M http -m URL="/dvwa/vulnerabilities/brute/"
Explicação dos parâmetros:

-U users.txt: Arquivo contendo uma lista de nomes de usuários.

-P passwords.txt: Arquivo contendo uma lista de senhas candidatas.

-M http: Seleciona o módulo web.

-m URL="...": Especifica o caminho exato do endpoint do formulário de login vulnerável dentro da aplicação.

3. Password Spraying em SMB com Enumeração
O objetivo foi mapear usuários válidos no serviço SMB e executar a técnica de Password Spraying (testar uma única senha comum em múltiplos usuários para evitar o bloqueio imediato de contas).

Passo A: Enumeração de Usuários

Bash
enum4linux -U 192.168.56.101
Passo B: Ataque via Medusa (SMB)

Bash
medusa -h 192.168.56.101 -U users.txt -p senha123 -M smb
Explicação: O enum4linux extrai as contas de usuário do sistema remoto via protocolo SMB/RPC. Em seguida, o Medusa utiliza esses usuários (-U users.txt) testando uma senha única (-p senha123) por meio do módulo -M smb.

Executando os Ataques Simulados
A ferramenta principal escolhida para os testes de autenticação foi o Medusa, por sua alta performance e suporte a múltiplos módulos de serviços.

1. Força Bruta em FTP (Metasploitable 2)
O objetivo foi testar a fragilidade do serviço FTP (vsftpd) contra uma wordlist de senhas comuns.

Comando:

Bash
medusa -h 192.168.56.101 -u msfadmin -P /usr/share/wordlists/rockyou.txt -M ftp
Explicação dos parâmetros:

-h 192.168.56.101: Define o endereço IP do alvo.

-u msfadmin: Define o nome de usuário fixo que será testado.

-P /usr/share/wordlists/rockyou.txt: Utiliza a famosa wordlist padrão do Kali Linux contendo milhões de senhas em texto plano.

-M ftp: Aciona o módulo de ataque voltado ao protocolo FTP.

2. Automação de Tentativas em Formulário Web (DVWA)
O objetivo foi automatizar o processo de adivinhação de credenciais em uma aplicação web vulnerável (Damn Vulnerable Web Application - DVWA) com o nível de segurança configurado como Low.

Comando:

Bash
medusa -h 192.168.56.101 -U users.txt -P passwords.txt -M http -m URL="/dvwa/vulnerabilities/brute/"
Explicação dos parâmetros:

-U users.txt: Arquivo contendo uma lista de nomes de usuários.

-P passwords.txt: Arquivo contendo uma lista de senhas candidatas.

-M http: Seleciona o módulo web.

-m URL="...": Especifica o caminho exato do endpoint do formulário de login vulnerável dentro da aplicação.

3. Password Spraying em SMB com Enumeração de Usuários
O objetivo foi mapear usuários válidos no serviço SMB e executar a técnica de Password Spraying (testar uma única senha comum em múltiplos usuários para evitar o bloqueio imediato de contas).

Comando para Enumeração de Usuários:

Bash
enum4linux -U 192.168.56.101
Comando para Ataque via Medusa (SMB):

Bash
medusa -h 192.168.56.101 -U users.txt -p senha123 -M smb
Explicação dos parâmetros:

O enum4linux -U extrai as contas de usuário do sistema remoto via protocolo SMB/RPC.

Em seguida, o Medusa utiliza essas contas (-U users.txt) testando uma senha única (-p senha123) por meio do módulo -M smb.

Medidas de Mitigação e Prevenção
Com base nas vulnerabilidades exploradas durante o laboratório, as seguintes práticas de remediação foram documentadas para ambientes de produção:

Políticas de Senhas Fortes: Obrigar o uso de senhas complexas, longas e com caracteres mistos, invalidando dicionários comuns como o rockyou.

Mecanismos de Bloqueio e Rate Limiting: Implementar bloqueio temporário de endereços IP ou contas após um limite de tentativas falhas (exemplo: utilizando ferramentas como Fail2ban em servidores Linux).

Desativação de Protocolos Inseguros: Substituir serviços legados baseados em texto plano (como FTP e versões antigas do SMB/SMBv1) por alternativas criptografadas e modernas (SFTP, SSH, SMBv3).

Proteção em Aplicações Web: Adicionar validações de entrada, CAPTCHA em formulários de autenticação e controle estrito de taxas de requisição (Rate Limiting).
