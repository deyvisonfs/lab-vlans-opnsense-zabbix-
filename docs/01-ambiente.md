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
- `OPNsense`: adaptador 1 em **Host-Only** `vboxnet0` (LAN/gerência, 192.168.56.2/24), adaptador 2 em **Bridge** (WAN, DHCP) e adaptador 3 em **Rede Interna** `lab-lan` (tronco das VLANs)
- VMs do lab (clientes e Zabbix): adaptador em **Rede Interna** `lab-lan`, na VLAN correspondente
- Detalhes em [02 · OPNsense](02-opnsense.md) e [03 · VLANs](03-vlans.md)

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
- `OPNsense`: adapter 1 on **Host-Only** `vboxnet0` (LAN/management, 192.168.56.2/24), adapter 2 on **Bridge** (WAN, DHCP) and adapter 3 on **Internal Network** `lab-lan` (VLAN trunk)
- Lab VMs (clients and Zabbix): adapter on **Internal Network** `lab-lan`, on the matching VLAN
- Details in [02 · OPNsense](02-opnsense.md) and [03 · VLANs](03-vlans.md)

### Host machine requirements
`[fill in: CPU, total RAM, disk space]`

### Notes
`[write down any difficulty or decision made in this step]`
