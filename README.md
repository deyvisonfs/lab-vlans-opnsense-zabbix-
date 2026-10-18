# Lab: VLANs, Firewall OPNsense e Monitoramento com Zabbix

🇧🇷 [Português](#-português) · 🇺🇸 [English](#-english)

---

## 🇧🇷 Português

### Objetivo
Montar uma rede segmentada em VLANs, controlada por um firewall OPNsense e monitorada com Zabbix, documentando cada etapa para que qualquer pessoa consiga reproduzir o lab.

### O que este projeto demonstra
- Segmentação de rede com VLANs (802.1Q) e roteamento entre VLANs
- Regras de firewall por segmento (princípio do menor privilégio)
- DHCP e DNS por VLAN
- Monitoramento de hosts e serviços com Zabbix (itens, triggers e alertas)
- Troubleshooting e validação documentados

### Ambiente
| Item | Valor |
|---|---|
| Virtualização | VirtualBox `[versão]` |
| Firewall | OPNsense `[26.7.7]` |
| Monitoramento | Zabbix `[7]` |
| Clientes de teste | `[ex.: Windows Server, Linux, PC cliente]` |

### Topologia
> Insira aqui a imagem do diagrama: `docs/topologia.png`

```
Internet ── OPNsense (WAN)
               │
           OPNsense (LAN trunk)
               │
            Switch (802.1Q)
        ┌──────┼───────┐
     VLAN 10  VLAN 20  VLAN 30
     Admin    Usuários Servidores
```

### Plano de endereçamento (sugestão, ajuste ao seu lab)
| VLAN | Nome | Rede | Gateway | DHCP |
|---|---|---|---|---|
| 10 | Administração | 192.168.10.0/24 | .1 | Sim |
| 20 | Usuários | 192.168.20.0/24 | .1 | Sim |
| 30 | Servidores | 192.168.30.0/24 | .1 | Não (IP fixo) |

### Passo a passo
1. **Preparar o ambiente virtual** e as interfaces de rede das VMs
2. **Instalar o OPNsense** e configurar WAN/LAN
3. **Criar as VLANs** no OPNsense (Interfaces → Devices → VLAN) e atribuir interfaces
4. **Configurar DHCP** por VLAN
5. **Criar regras de firewall** entre VLANs
6. **Instalar o Zabbix Server** (VM na VLAN 30)
7. **Instalar o Zabbix Agent** nos hosts e cadastrá-los
8. **Criar triggers e alertas** (host fora do ar, uso de CPU, disco)
9. **Validar** (ping, traceroute, testes de bloqueio e de alerta)

> Cada passo terá sua página em `docs/` com comandos, prints e observações.

### Validação
| Teste | Resultado esperado | Resultado obtido |
|---|---|---|
| Ping VLAN 20 → gateway | Sucesso | `[preencher]` |
| Ping VLAN 20 → VLAN 30 | Bloqueado | `[preencher]` |
| Zabbix detecta host desligado | Alerta em até 1 min | `[preencher]` |

### Problemas encontrados e soluções
> Esta é a seção que mais chama atenção de recrutadores. Registre cada erro real: sintoma, causa, solução.

| Sintoma | Causa | Solução |
|---|---|---|
| `[preencher]` | `[preencher]` | `[preencher]` |

### Aprendizados
`[preencher ao final]`

### Autor
Deyvison Ferreira da Silva · Recife, PE · `[link do LinkedIn]`

---

## 🇺🇸 English

### Goal
Build a VLAN-segmented network controlled by an OPNsense firewall and monitored with Zabbix, documenting every step so anyone can reproduce the lab.

### What this project demonstrates
- Network segmentation with VLANs (802.1Q) and inter-VLAN routing
- Per-segment firewall rules (least privilege)
- DHCP and DNS per VLAN
- Host and service monitoring with Zabbix (items, triggers, alerts)
- Documented troubleshooting and validation

### Environment
| Item | Value |
|---|---|
| Virtualization | VirtualBox `[version]` |
| Firewall | OPNsense `[version]` |
| Monitoring | Zabbix `[version]` |
| Test clients | `[e.g., Windows Server, Linux, client PC]` |

### Topology
> Add the diagram image here: `docs/topology.png`

### Addressing plan (suggested, adjust to your lab)
| VLAN | Name | Network | Gateway | DHCP |
|---|---|---|---|---|
| 10 | Administration | 192.168.10.0/24 | .1 | Yes |
| 20 | Users | 192.168.20.0/24 | .1 | Yes |
| 30 | Servers | 192.168.30.0/24 | .1 | No (static) |

### Step by step
1. **Prepare the virtual environment** and VM network interfaces
2. **Install OPNsense** and configure WAN/LAN
3. **Create VLANs** in OPNsense (Interfaces → Devices → VLAN) and assign interfaces
4. **Configure DHCP** per VLAN
5. **Create firewall rules** between VLANs
6. **Install Zabbix Server** (VM on VLAN 30)
7. **Install Zabbix Agent** on hosts and register them
8. **Create triggers and alerts** (host down, CPU, disk usage)
9. **Validate** (ping, traceroute, block and alert tests)

> Each step will have its own page under `docs/` with commands, screenshots and notes.

### Validation
| Test | Expected | Actual |
|---|---|---|
| Ping VLAN 20 → gateway | Success | `[fill in]` |
| Ping VLAN 20 → VLAN 30 | Blocked | `[fill in]` |
| Zabbix detects powered-off host | Alert within 1 min | `[fill in]` |

### Issues found and fixes
| Symptom | Cause | Fix |
|---|---|---|
| `[fill in]` | `[fill in]` | `[fill in]` |

### Lessons learned
`[fill in at the end]`

### Author
Deyvison Ferreira da Silva · Recife, Brazil · `[LinkedIn link]`
