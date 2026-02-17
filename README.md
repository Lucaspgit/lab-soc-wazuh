# Relatório do Lab SOC Wazuh

Este documento registra as detecções realizadas no laboratório SOC com **Wazuh**, incluindo monitoramento de integridade, eventos do Windows, serviços, logs e ataques simulados. Todas as evidências estão documentadas com prints de tela e descrições técnicas.

---

## 1. Status do Ambiente

### 1.1 Agente Ativo
![Figura 1 – Agente Ativo](./prints/agente%20ativo.png)  
O agente Wazuh encontra-se ativo e reportando corretamente para o manager.

### 1.2 Dashboard
![Figura 2 – Dashboard Wazuh](./prints/dashboard-running.png)  
Painel principal do Wazuh mostrando status do cluster e alertas em tempo real.

### 1.3 Indexer e Manager
![Figura 3 – Indexer em execução](./prints/indexer-running.png)  
![Figura 4 – Manager em execução](./prints/manager-running.png)  
Ambiente operacional do Elasticsearch e Manager do Wazuh funcionando corretamente.

---

## 2. Monitoramento de Integridade

### 2.1 Integridade de Arquivos Alterada
![Figura 5 – Integrity Checksum Changed](./prints/04-fim-integrity-checksum-changed.png)  
**Evento:** Alteração de checksum detectada  
**Relevância:** Confirmação de alterações em arquivos críticos do sistema  

---

## 3. Contas e Logon

### 3.1 Criação de Conta Local
![Figura 6 – Criação de Conta Local](./prints/05-deteccao-criacao-conta-local.png)  
**Evento:** Criação de conta local (Event ID 4720)  
**MITRE TTP:** Persistence – Account Manipulation (T1136)

### 3.2 Tentativa de Logon Falha
![Figura 7 – Tentativa de Logon Falha](./prints/07-deteccao-tentativa-logon-falha-4625.png)  
**Evento:** Logon falho (Event ID 4625)  
**MITRE TTP:** Credential Access – Brute Force (T1110)

---

## 4. Processos e Serviços

### 4.1 Criação de Processo
![Figura 8 – Criação de Processo](./prints/06-deteccao-criacao-processo-4688.png)  
**Evento:** Novo processo criado (Event ID 4688)  
**Observação:** Detecta execução de processos não autorizados.

### 4.2 Criação de Serviço
![Figura 9 – Criação de Serviço](./prints/09-deteccao-criacao-servico-7045.png)  
**Evento:** Serviço instalado (Event ID 7045)  
**MITRE TTP:** Persistence / Privilege Escalation – Windows Service (T1543.003)

---

## 5. Limpeza e Dump de Logs

### 5.1 Limpeza de Logs
![Figura 10 – Limpeza de Logs](./prints/11-deteccao-limpeza-logs-1102.png)  
**Evento:** Audit log cleared (Event ID 1102)  
**MITRE TTP:** Defense Evasion – Indicator Removal (T1070)

### 5.2 Dump de Credenciais (LSASS)
![Figura 11 – Dump de Credenciais](./prints/12-deteccao-dump-credenciais.png)  
**Evento:** Dump de LSASS detectado  
**MITRE TTP:** Credential Access – LSASS Memory (T1003.001)

---

## 6. Movimento Lateral

### 6.1 Lateral Movement
![Figura 12 – Lateral Movement](./prints/14-deteccao-lateral-movement.png)  
**Evento:** Tentativa de movimentação lateral entre hosts  
**MITRE TTP:** Lateral Movement – Remote Services (T1021)

---

## 7. Visão Geral de Ameaças

![Figura 13 – Threats](./prints/theaths.png)  
Resumo das ameaças detectadas, incluindo risco e severidade.

---

## Observações Finais

1. Todas as imagens estão na pasta `prints/`.  
2. O relatório segue padrão de SOC, ligando evidências a Event IDs e MITRE ATT&CK.  
3. Cada evento foi monitorado via Wazuh, com alertas disparados conforme regras configuradas.  

---

**Fim do Relatório**

