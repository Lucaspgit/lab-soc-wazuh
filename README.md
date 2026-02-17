# Lab SOC Wazuh

Este repositório documenta a configuração e detecção de eventos de segurança em um laboratório SOC com Wazuh.  
Os tópicos estão organizados por etapas de detecção de ataques e monitoramento do ambiente Windows, incluindo logs importantes do sistema e alertas do Wazuh.

---

## 01 – Agente Ativo
![agente ativo](prints/agente ativo.png)  
Verificação do status do agente Wazuh conectado ao manager.

## 02 – Dashboard em execução
![dashboard-running](prints/dashboard-running.png)  
O painel do Wazuh está funcionando corretamente, mostrando alertas e eventos do agente.

## 03 – Indexer em execução
![indexer-running](prints/indexer-running.png)  
Confirmação de que o serviço de indexação do Wazuh está ativo, permitindo a ingestão dos logs.

## 04 – Integridade de arquivos (Checksum Alterado)
![04-fim-integrity-checksum-changed](prints/04-fim-integrity-checksum-changed.png)  
Detecção de alteração em arquivos monitorados pelo File Integrity Monitoring (FIM), indicando possível modificação não autorizada.

## 05 – Criação de Conta Local
![05-deteccao-criacao-conta-local](prints/05-deteccao-criacao-conta-local.png)  
O Wazuh detectou a criação de uma conta local no Windows. Monitoramento importante para identificar ações de invasores que criam contas com privilégios.

## 06 – Criação de Processo (Event ID 4688)
![06-deteccao-criacao-processo-4688](prints/06-deteccao-criacao-processo-4688.png)  
Registro da criação de novos processos no sistema. Permite rastrear programas suspeitos que podem executar malware ou dump de credenciais.

## 07 – Tentativa de Logon Falha (Event ID 4625)
![07-deteccao-tentativa-logon-falha-4625](prints/07-deteccao-tentativa-logon-falha-4625.png)  
Monitoramento de tentativas de login falhas, útil para identificar ataques de força bruta.  
Inclui também tentativa de login via **SSH**:

![13-tentativa-ssh](prints/13-tentativa-ssh.png)  
Mostra falha de autenticação no SSH, gerando alerta do Wazuh.

## 09 – Criação de Serviço (Event ID 7045)
![09-deteccao-criacao-servico-7045](prints/09-deteccao-criacao-servico-7045.png)  
Detecta a criação de novos serviços, o que pode indicar persistência de malware no sistema.

## 11 – Limpeza de Logs (Event ID 1102)
![11-deteccao-limpeza-logs-1102](prints/11-deteccao-limpeza-logs-1102.png)  
Registro da limpeza do log de auditoria, frequentemente usada por invasores para ocultar rastros de atividades maliciosas.

## 12 – Dump de Credenciais (LSASS)
![12-dump-credenciais](prints/12-dump-credenciais.png)  
Captura de memória LSASS, indicando possível extração de credenciais do Windows.

## 14 – Movimentação Lateral
![14-deteccao-lateral-movement](prints/14-deteccao-lateral-movement.png)  
Detecta padrões de movimentação lateral em rede, possível exploração de contas comprometidas.

## 15 – Event ID 5007: Alteração de Configuração do Microsoft Defender
![15-evento-5007](prints/15-evento-5007.png)  
Evento gerado quando a configuração do Microsoft Defender Antivírus é alterada.  
- **Valor antigo:** HKLM\SOFTWARE\Microsoft\Windows Defender\CoreService\WdConfigHash = 0x5AAFB843  
- **Novo valor:** HKLM\SOFTWARE\Microsoft\Windows Defender\CoreService\WdConfigHash = 0xE01C0682  
Pode indicar ação manual ou alteração causada por malware.

---

## Arquivo de configuração Wazuh

O arquivo de configuração do agente (`ossec.conf`) está incluído para referência:

<ossec_config>
  <!-- ============================= -->
  <!-- Conexão com o Manager -->
  <client>
    <server>
      <address>192.168.1.13</address>
      <port>1514</port>
      <protocol>tcp</protocol>
    </server>
  </client>

  <!-- ============================= -->
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

  <!-- ============================= -->
  <!-- Sysmon Integration -->
  <localfile>
    <log_format>eventchannel</log_format>
    <location>Microsoft-Windows-Sysmon/Operational</location>
  </localfile>

  <!-- ============================= -->
  <!-- Windows Defender Logs -->
  <localfile>
    <log_format>eventchannel</log_format>
    <location>Microsoft-Windows-Windows Defender/Operational</location>
  </localfile>

  <!-- ============================= -->
  <!-- File Integrity Monitoring -->
  <syscheck>
    <frequency>300</frequency>
    <scan_on_start>yes</scan_on_start>
    <directories realtime="yes">C:\Users</directories>
    <directories realtime="yes">C:\Windows\System32\drivers\etc</directories>
  </syscheck>

  <!-- ============================= -->
  <!-- Rootcheck -->
  <rootcheck>
    <disabled>no</disabled>
  </rootcheck>

  <!-- ============================= -->
  <!-- Vulnerability Detector -->
  <vulnerability-detector>
    <enabled>yes</enabled>
    <interval>60m</interval>
  </vulnerability-detector>

  <!-- ============================= -->
  <!-- Security Configuration Assessment -->
  <sca>
    <enabled>yes</enabled>
    <scan_on_start>yes</scan_on_start>
    <interval>12h</interval>
  </sca>
</ossec_config>


