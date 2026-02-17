# Lab SOC Wazuh

Este repositório documenta a configuração e detecção de eventos de segurança em um laboratório SOC com Wazuh.  
Os tópicos estão organizados por etapas de detecção de ataques e monitoramento do ambiente Windows.

---

## 01 – Agente Ativo
![agente ativo](prints/agente ativo.png)  
Verificação do status do agente Wazuh conectado ao manager.

---

## 02 – Dashboard em execução
![dashboard-running](prints/dashboard-running.png)  
O painel do Wazuh está funcionando corretamente, mostrando alertas e eventos do agente.

---

## 03 – Indexer em execução
![indexer-running](prints/indexer-running.png)  
Confirmação de que o serviço de indexação do Wazuh está ativo, permitindo a ingestão dos logs.

---

## 04 – Integridade de arquivos (Checksum Alterado)
![04-fim-integrity-checksum-changed](prints/04-fim-integrity-checksum-changed.png)  
Detecção de alteração em arquivos monitorados pelo File Integrity Monitoring (FIM), indicando possível modificação não autorizada.

---

## 05 – Criação de Conta Local
![05-deteccao-criacao-conta-local](prints/05-deteccao-criacao-conta-local.png)  
O Wazuh detectou a criação de uma conta local no Windows. Monitoramento importante para identificar ações de invasores que criam contas com privilégios.

---

## 06 – Criação de Processo (Event ID 4688)
![06-deteccao-criacao-processo-4688](prints/06-deteccao-criacao-processo-4688.png)  
Registro da criação de novos processos no sistema. Permite rastrear programas suspeitos que podem executar malware ou dump de credenciais.

---

## 07 – Tentativa de Logon Falha (Event ID 4625)
![07-deteccao-tentativa-logon-falha-4625](prints/07-deteccao-tentativa-logon-falha-4625.png)  
Monitoramento de tentativas de login falhas, útil para identificar ataques de força bruta.

---

## 09 – Criação de Serviço (Event ID 7045)
![09-deteccao-criacao-servico-7045](prints/09-deteccao-criacao-servico-7045.png)  
Detecta a criação de novos serviços, o que pode indicar persistência de malware no sistema.

---

## 11 – Limpeza de Logs (Event ID 1102)
![11-deteccao-limpeza-logs-1102](prints/11-deteccao-limpeza-logs-1102.png)  
Registro da limpeza do log de auditoria, frequentemente usada por invasores para ocultar rastros de atividades maliciosas.

---

## 12 – Dump de Credenciais (LSASS)
![12-dump-credenciais](prints/12-dump-credenciais.png)


