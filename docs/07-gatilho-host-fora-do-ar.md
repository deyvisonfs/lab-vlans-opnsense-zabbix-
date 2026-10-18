# 07 · Gatilho de host fora do ar e teste de alerta / Host-down trigger and alert test

[← README](../README.md) · [← 06 Kali no Zabbix](06-kali-no-zabbix.md)

## 🇧🇷 Português

### Objetivo
Criar um gatilho que avisa quando um host monitorado para de responder e medir quanto tempo o Zabbix leva para alertar, desligando de verdade a VM do Kali.

### 1. Gatilho
Caminho: **Data collection → Hosts → Kali → Triggers → Create trigger**.

| Campo | Valor |
|---|---|
| Name | `Kali fora do ar (sem resposta do agente)` |
| Severity | `High` |
| Expression | `nodata(/Kali/agent.ping,2m)=1` |

Ele usa o item `agent.ping` do template `Linux by Zabbix agent`, lido a cada 1 minuto. O gatilho dispara quando o Zabbix fica 2 minutos sem receber valor. Escolhi 2 minutos porque um valor menor que o intervalo de coleta geraria falsos alertas, então a meta inicial do README, "alerta em até 1 min", não era realista. Passou a ser **até 3 min** (2 min de `nodata` mais até 1 min de coleta).

### 2. Teste
1. Conferido que o Kali estava sem problemas ativos.
2. No Kali: `sudo poweroff`, às **15:17**.
3. O ZBX do Kali ficou vermelho às **15:19**.
4. Em **Monitoring → Problems**, o problema `Kali fora do ar (sem resposta do agente)` foi registrado às **15:19:21**, severidade `High`.

Tempo de detecção: **cerca de 2 minutos**, dentro do esperado.

![ZBX do Kali vermelho](img/07-kali-zbx-vermelho.png)
*Depois do desligamento, o Kali aparece com ZBX vermelho e 2 problemas ativos; OPNsense e Zabbix seguem verdes.*

![Alerta de host fora do ar](img/07-problemas-alerta.png)
*Problema `Kali fora do ar (sem resposta do agente)`, severidade `High`, registrado às 15:19:21.*

### Validação
| Teste | Esperado | Resultado obtido |
|---|---|---|
| Gatilho criado e ativo no host Kali | Sim | **Sim** |
| ZBX do Kali fica vermelho ao desligar a VM | Sim | **Sim**, às 15:19 |
| Problema `High` registrado | Em até 3 min | **Sim**, às 15:19:21 (cerca de 2 min após o desligamento às 15:17) |

### Achado extra
Na mesma tela havia outro problema ativo no Kali desde as 13:28: `FS [/]: Space is low (used > 80%, total 26.8GB)`, gerado por um gatilho do próprio template. O disco do Kali estava com mais de 80% de uso, e foi o Zabbix que mostrou isso sem eu ter configurado nada.

### Problemas encontrados e soluções
| Sintoma | Causa | Solução |
|---|---|---|
| Meta de "alerta em até 1 min" inviável | O item é lido a cada 1 min e o `nodata` precisa de uma janela maior | Meta ajustada para até 3 min, com `nodata` de 2 min |

### Pontos de melhoria
- Criar uma **ação** (Alerts → Actions) para notificar por e-mail ou mensageria quando o problema abrir. Hoje o alerta só aparece no painel.
- Registrar também o tempo de **recuperação**, ligando a VM de volta e vendo o problema virar Resolved.
- Liberar espaço no disco do Kali ou ajustar o limite do gatilho de espaço.

### Próximo passo
Ações de notificação e gatilhos de CPU e disco, para fechar o item "triggers e alertas" do plano.

---

## 🇺🇸 English

### Goal
Create a trigger that warns when a monitored host stops responding and measure how long Zabbix takes to alert by actually powering off the Kali VM.

### 1. Trigger
Path: **Data collection → Hosts → Kali → Triggers → Create trigger**.

| Field | Value |
|---|---|
| Name | `Kali fora do ar (sem resposta do agente)` |
| Severity | `High` |
| Expression | `nodata(/Kali/agent.ping,2m)=1` |

It uses the `agent.ping` item from the `Linux by Zabbix agent` template, read every 1 minute. The trigger fires when Zabbix gets no value for 2 minutes. I chose 2 minutes because a window shorter than the polling interval would cause false alerts, so the README's original goal, "alert within 1 min", was not realistic. It became **within 3 min** (2 min of `nodata` plus up to 1 min of polling).

### 2. Test
1. Confirmed Kali had no active problems.
2. On Kali: `sudo poweroff`, at **15:17**.
3. Kali's ZBX turned red at **15:19**.
4. In **Monitoring → Problems**, the problem `Kali fora do ar (sem resposta do agente)` was logged at **15:19:21**, severity `High`.

Detection time: **about 2 minutes**, as expected.

![Kali ZBX red](img/07-kali-zbx-vermelho.png)
*After the shutdown, Kali shows a red ZBX and 2 active problems; OPNsense and Zabbix stay green.*

![Host-down alert](img/07-problemas-alerta.png)
*Problem `Kali fora do ar (sem resposta do agente)`, severity `High`, logged at 15:19:21.*

### Validation
| Test | Expected | Actual |
|---|---|---|
| Trigger created and enabled on host Kali | Yes | **Yes** |
| Kali's ZBX turns red when the VM is powered off | Yes | **Yes**, at 15:19 |
| `High` problem logged | Within 3 min | **Yes**, at 15:19:21 (about 2 min after the 15:17 shutdown) |

### Extra finding
The same screen showed another active problem on Kali since 13:28: `FS [/]: Space is low (used > 80%, total 26.8GB)`, raised by a trigger from the template itself. Kali's disk was over 80% full, and Zabbix surfaced it without any configuration from me.

### Issues found and fixes
| Symptom | Cause | Fix |
|---|---|---|
| The "alert within 1 min" goal was not feasible | The item is polled every 1 min and `nodata` needs a larger window | Goal adjusted to within 3 min, with a 2 min `nodata` |

### Improvements
- Create an **action** (Alerts → Actions) to notify by e-mail or messaging when the problem opens. Today the alert only shows on the dashboard.
- Also record the **recovery** time by powering the VM back on and watching the problem turn Resolved.
- Free disk space on Kali or tune the space trigger threshold.

### Next step
Notification actions and CPU and disk triggers, to close the "triggers and alerts" item of the plan.
