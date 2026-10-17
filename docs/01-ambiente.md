# 01 · Ambiente / Environment

[← Voltar ao README](../README.md)

## 🇧🇷 Português

### Plataforma de virtualização
VirtualBox · versão `[preencher]`

### Máquinas virtuais
| VM | Função | SO | vCPU | RAM | Disco | VLAN |
|---|---|---|---|---|---|---|
| fw-opnsense | Firewall / roteador | OPNsense `[versão]` | `[ ]` | `[ ]` | `[ ]` | Trunk |
| srv-zabbix | Monitoramento | `[SO]` + Zabbix `[versão]` | `[ ]` | `[ ]` | `[ ]` | 30 |
| cli-admin | Cliente de teste (Admin) | `[SO]` | `[ ]` | `[ ]` | `[ ]` | 10 |
| cli-user | Cliente de teste (Usuário) | `[SO]` | `[ ]` | `[ ]` | `[ ]` | 20 |

### Redes virtuais
- `fw-opnsense`: adaptador 1 em **NAT** (WAN) e adaptador 2 em **Rede Interna** `lab-lan` (LAN)
- Demais VMs: adaptador em **Rede Interna** `lab-lan`
- Detalhes em [02 · OPNsense](02-opnsense.md)

### Requisitos do computador host
`[preencher: CPU, RAM total, espaço em disco]`

### Observações
`[anote qualquer dificuldade ou decisão tomada nesta etapa]`

---

## 🇺🇸 English

### Virtualization platform
VirtualBox · version `[fill in]`

### Virtual machines
| VM | Role | OS | vCPU | RAM | Disk | VLAN |
|---|---|---|---|---|---|---|
| fw-opnsense | Firewall / router | OPNsense `[version]` | `[ ]` | `[ ]` | `[ ]` | Trunk |
| srv-zabbix | Monitoring | `[OS]` + Zabbix `[version]` | `[ ]` | `[ ]` | `[ ]` | 30 |
| cli-admin | Test client (Admin) | `[OS]` | `[ ]` | `[ ]` | `[ ]` | 10 |
| cli-user | Test client (User) | `[OS]` | `[ ]` | `[ ]` | `[ ]` | 20 |

### Virtual networks
- `fw-opnsense`: adapter 1 on **NAT** (WAN) and adapter 2 on **Internal Network** `lab-lan` (LAN)
- Other VMs: adapter on **Internal Network** `lab-lan`
- Details in [02 · OPNsense](02-opnsense.md)

### Host machine requirements
`[fill in: CPU, total RAM, disk space]`

### Notes
`[write down any difficulty or decision made in this step]`
