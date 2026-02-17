# Lab SOC Wazuh

Este repositório documenta a configuração e detecção de eventos de segurança em um laboratório SOC utilizando o Wazuh.  
Os tópicos estão organizados por etapas de monitoramento do ambiente Windows e detecção de ataques.

---

## 01 – Agente Ativo
![01-agente-ativo](prints/agente ativo.png)  
Verificação do status do agente Wazuh conectado ao manager.

## 02 – Dashboard em execução
![02-dashboard-running](prints/dashboard-running.png)  
Painel do Wazuh funcionando corretamente, mostrando alertas e eventos do agente.

## 03 – Indexer em execução
![03-indexer-running](prints/indexer-running.png)  
Confirmação de que o serviço de indexação do Wazuh está ativo, permitindo ingestão dos logs.

## 04 – Integridade de arquivos (Checksum Alterado)
![04-fim-integrity-checksum-changed](prints/04-fim-integrity-checksum-changed.png)  
Detecção de alteração em arquivos monitorados pelo File Integrity Monitoring (FIM), indicando possível modificação não autorizada.

## 05 – Criação de Conta Local
![05-deteccao-criacao-conta-local](prints/05-deteccao-criacao-conta-local.png)  
O Wazuh detectou a criação de uma conta local no Windows. Monitoramento importante para identificar ações de invasores que criam contas com privilégios.

## 06 – Criação de Processo (Event ID 4688)
![06-deteccao-criacao-processo-4688](prints/06-deteccao-criacao-processo-4688.png)  
Registro da criação de novos processos no sistema, permitindo rastrear programas suspeitos ou atividades de malware.

## 07 – Tentativa de Logon Falha (Event ID 4625)
![07-deteccao-tentativa-logon-falha-4625](prints/07-deteccao-tentativa-logon-falha-4625.png)  
Monitoramento de tentativas de login falhas, útil para identificar ataques de força bruta.

## 09 – Criação de Serviço (Event ID 7045)
![09-deteccao-criacao-servico-7045](prints/09-deteccao-criacao-servico-7045.png)  
Detecta a criação de novos serviços, indicando possível persistência de malware no sistema.

## 11 – Limpeza de Logs (Event ID 1102)
![11-deteccao-limpeza-logs-1102](prints/11-deteccao-limpeza-logs-1102.png)  
Registro da limpeza do log de auditoria, frequentemente usada por invasores para ocultar rastros de atividades maliciosas.

## 12 – Dump de Credenciais (LSASS)
![12-dump-credenciais](prints/12-dump-credenciais.png)  
Detecção de dump de credenciais da memória do LSASS, indicando possível exfiltração de senhas e hashes.

## 13 – Tentativa de Login via SSH (Event ID 4625)
![13-tentativa-ssh](prints/13-tentativa-ssh.png)  
Monitoramento de tentativas de login falhas via SSH.  
O Wazuh detectou múltiplas falhas de autenticação para usuários inexistentes, indicando tentativa de força bruta externa.

---

## Arquivo de Configuração do Wazuh (`ossec.conf`)

```xml
<ossec_config>
  <!-- Conexão com o Manager -->
  <client>
    <server>
      <address>192.168.1.13</address>
      <port>1514</port>
      <protocol>tcp</protocol>
    </server>
  </client>

  <!-- Windows Event Logs -->
  <localfile>
    <log_format>eventchannel</log_format>
    <location>Security</location>
  </localfile>
  <localfile>
    <log_format>eventchannel</log_format>
    <location>System</location>
  </localfile>
  <localfile>
    <log_format>eventchannel</log_format>
    <location>Application</location>
  </localfile>

  <!-- Sysmon Integration -->
  <localfile>
    <log_format>eventchannel</log_format>
    <location>Microsoft-Windows-Sysmon/Operational</location>
  </localfile>

  <!-- Windows Defender Logs -->
  <localfile>
    <log_format>eventchannel</log_format>
    <location>Microsoft-Windows-Windows Defender/Operational</location>
  </localfile>

  <!-- File Integrity Monitoring -->
  <syscheck>
    <frequency>300</frequency>
    <scan_on_start>yes</scan_on_start>
    <directories realtime="yes">C:\Users</directories>
    <directories realtime="yes">C:\Windows\System32\drivers\etc</directories>
  </syscheck>

  <!-- Rootcheck -->
  <rootcheck>
    <disabled>no</disabled>
  </rootcheck>

  <!-- Vulnerability Detector -->
  <vulnerability-detector>
    <enabled>yes</enabled>
    <interval>60m</interval>
  </vulnerability-detector>

  <!-- Security Configuration Assessment -->
  <sca>
    <enabled>yes</enabled>
    <scan_on_start>yes</scan_on_start>
    <interval>12h</interval>
  </sca>
</ossec_config>


