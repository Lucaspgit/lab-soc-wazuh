# 🛡️ Wazuh SOC Lab – Threat Detection & Incident Response

Projeto prático de laboratório SOC utilizando **Wazuh v4.14.3 OVA**, simulando ataques reais em ambiente controlado para detecção, análise e resposta a incidentes de segurança.

---

## 📌 Objetivo

Demonstrar habilidades práticas de:

- Monitoramento com SIEM
- Análise de logs Windows e Linux
- Investigação de incidentes
- Mapeamento MITRE ATT&CK
- Resposta a incidentes
- Documentação técnica de eventos de segurança

---

## 🏗️ Arquitetura do Laboratório

- **SIEM:** Wazuh 4.14.3 (Manager + Indexer + Dashboard)
- **Servidor:** Wazuh OVA
- **Endpoints monitorados:**
  - Windows (com Sysmon)
  - Linux (SSH)
- **Virtualização:** VirtualBox

---

# 🔎 Incidentes Simulados e Detectados

---

## 1️⃣ Brute Force Attack – Windows Logon Failure

**Event ID:** 4625  

### Descrição
Múltiplas tentativas de autenticação falhas detectadas contra conta local.

### Evidências do Log
- LogonType: 3
- Status: 0xC000006D
- SubStatus: 0xC000006A

### Classificação
- **Tática:** Credential Access  
- **MITRE ATT&CK:** T1110 – Brute Force  

### Resposta
- Verificação do IP de origem  
- Bloqueio via firewall  
- Monitoramento contínuo da conta afetada  

---

## 2️⃣ SSH Brute Force – Linux

### Descrição
Múltiplas tentativas de login via SSH utilizando credenciais inválidas.

### Evidência

Failed password for invalid user


### Classificação
- **Tática:** Credential Access  
- **MITRE ATT&CK:** T1110 – Brute Force  

### Resposta
- Identificação do IP atacante  
- Implementação de fail2ban  
- Recomendação de autenticação via chave SSH  

---

## 3️⃣ Criação de Conta Local (Persistence)

**Event ID:** 4720  

### Descrição
Nova conta criada no sistema operacional.

### Classificação
- **Tática:** Persistence  
- **MITRE ATT&CK:** T1136 – Create Account  

### Resposta
- Validação com administrador  
- Auditoria da conta criada  
- Revisão de privilégios  

---

## 4️⃣ Modificação de Conta

**Event ID:** 4738  

### Descrição
Conta existente sofreu alteração de atributos.

### Classificação
- **Tática:** Persistence  
- **MITRE ATT&CK:** T1098 – Account Manipulation  

---

## 5️⃣ Execução de Processo Suspeito

**Event ID:** 4688  

### Descrição
Criação de novo processo detectada via log de segurança.

### Monitoramento Analisado
- Parent process  
- Command line  
- Contexto do usuário  

### Classificação
- **Tática:** Execution  
- **MITRE ATT&CK:** T1059 – Command and Scripting Interpreter  

---

## 6️⃣ File Integrity Monitoring (FIM)

### Descrição
Alteração detectada em arquivo monitorado pelo módulo de integridade do Wazuh.

### Classificação
- **Tática:** Defense Evasion  
- **MITRE ATT&CK:** T1070 – Indicator Removal  

---

## 7️⃣ Criação de Serviço no Windows

**Event ID:** 7045  

### Descrição
Novo serviço instalado no sistema.

### Classificação
- **Tática:** Persistence  
- **MITRE ATT&CK:** T1543 – Create or Modify System Process  

---

## 8️⃣ Limpeza de Logs

**Event ID:** 1102  

### Descrição
Log de segurança do Windows foi apagado.

### Classificação
- **Tática:** Defense Evasion  
- **MITRE ATT&CK:** T1070 – Clear Windows Event Logs  

---

# 📊 Monitoramento com Wazuh

Validação dos serviços:

```bash
systemctl status wazuh-manager
systemctl status wazuh-indexer
systemctl status wazuh-dashboard


Todos operando corretamente no ambiente do laboratório.

---

# 🧠 Mapeamento MITRE ATT&CK Utilizado

| Técnica | Descrição |
|----------|------------|
| T1110 | Brute Force |
| T1136 | Create Account |
| T1098 | Account Manipulation |
| T1059 | Command Execution |
| T1543 | Create Service |
| T1070 | Log Clearing / Defense Evasion |

---

# 👨‍💻 Autor

**Lucas**  
Estudante de Segurança da Informação  
Foco em SOC Analyst / Blue Team  