# 02 · Instalação do OPNsense no VirtualBox / OPNsense on VirtualBox

[← README](../README.md) · [← 01 Ambiente](01-ambiente.md)

## 🇧🇷 Português

### Objetivo
Instalar o OPNsense em uma VM, deixar a WAN com acesso à internet e a LAN acessível pelo navegador a partir de um cliente de teste.

### 1. Baixar a ISO
1. Acesse a página de downloads do OPNsense (opnsense.org/download).
2. Escolha arquitetura **amd64**, tipo **dvd** e um espelho de download.
3. Descompacte o arquivo `.bz2` para obter a `.iso`.

### 2. Criar a VM no VirtualBox
| Configuração | Valor sugerido |
|---|---|
| Nome | `fw-opnsense` |
| Tipo / Versão | BSD / FreeBSD (64-bit) |
| Memória | 2048 MB |
| Processadores | 2 vCPU |
| Disco | 20 GB (VDI, alocação dinâmica) |
| ISO | a ISO descompactada, no leitor óptico |

### 3. Configurar as placas de rede (VM desligada)
| Adaptador | Conectado a | Função | Observações |
|---|---|---|---|
| 1 | **NAT** | WAN | Recebe IP por DHCP do VirtualBox (normalmente 10.0.2.x) |
| 2 | **Rede Interna**, nome `lab-lan` | LAN | Tipo **Intel PRO/1000 MT Desktop**; em Avançado, **Modo Promíscuo: Permitir Tudo** |

> O nome da rede interna diferencia maiúsculas de minúsculas. Todas as VMs que precisarem conversar com a LAN devem usar exatamente o mesmo nome.
>
> Anote o **endereço MAC** de cada adaptador (Avançado). Ele ajuda a descobrir qual interface do OPNsense (`em0`, `em1`) é qual.

### 4. Instalar
1. Inicie a VM. No login do instalador, use usuário `installer` e senha `opnsense`.
2. Escolha o layout de teclado (para ABNT2, selecione o português do Brasil).
3. Instalação guiada (UFS ou ZFS, ambos servem para o lab), selecione o disco e confirme.
4. Defina a **senha do root** e anote em um gerenciador de senhas.
5. Ao terminar, escolha **Complete Install** e reinicie.
6. **Remova a ISO** do leitor virtual antes do reinício, para não voltar ao instalador.

### 5. Conferir WAN e LAN no console
No menu do console, use a opção **1) Assign interfaces** e confira:
- **WAN** = placa do adaptador 1 (NAT)
- **LAN** = placa do adaptador 2 (rede interna)

Se estiver invertido, reatribua pela mesma opção. Depois, na opção **2) Set interface IP address**, confirme que a LAN está em `192.168.1.1/24` com DHCP habilitado.

### 6. Acessar a interface web
1. Crie ou use a VM `cli-admin` com o adaptador 1 em **Rede Interna** `lab-lan`.
2. Ela deve receber um IP `192.168.1.x` por DHCP.
3. No navegador, abra `https://192.168.1.1` (aceite o aviso de certificado).
4. Login: usuário `root` e a senha definida na instalação.
5. Conclua o assistente inicial: nome do host, DNS, fuso horário (`America/Recife`) e confirme WAN e LAN.

### 7. Validação
| Teste | Resultado esperado | Resultado obtido |
|---|---|---|
| Console OPNsense, opção 7 (Ping), destino 8.8.8.8 | Respostas | `[preencher]` |
| `cli-admin` recebe IP 192.168.1.x | Sim | `[preencher]` |
| `cli-admin` pinga 192.168.1.1 | Sucesso | `[preencher]` |
| `cli-admin` pinga 8.8.8.8 | Sucesso (via NAT) | `[preencher]` |
| Interface web abre em https://192.168.1.1 | Sim | `[preencher]` |

### Problemas comuns neste passo
| Sintoma | Causa provável | O que verificar |
|---|---|---|
| VM não inicia ou fica lenta | Virtualização desativada na BIOS | Ativar VT-x/AMD-V |
| Cliente não pega IP | Nomes de rede interna diferentes ou modo promíscuo desligado | Nome exato `lab-lan` e Permitir Tudo |
| WAN sem IP | Adaptador 1 não está em NAT, ou interfaces invertidas | Opção 1 do console e MACs |
| Volta ao instalador após reiniciar | ISO ainda montada | Remover a ISO |

> Anote aqui o que realmente aconteceu no seu lab: `[preencher]`

### Próximo passo
Passo 3: criar as VLANs 10, 20 e 30 no OPNsense. Observação importante: o VirtualBox não tem switch gerenciável, então os clientes precisarão marcar o tráfego (tag 802.1Q) na própria VM ou haverá uma VM-switch no meio. Essa decisão será documentada em `03-vlans.md`.

---

## 🇺🇸 English

### Goal
Install OPNsense in a VM, give the WAN internet access and make the LAN reachable from a test client's browser.

### 1. Download the ISO
1. Go to the OPNsense download page (opnsense.org/download).
2. Pick architecture **amd64**, image type **dvd** and a mirror.
3. Decompress the `.bz2` file to get the `.iso`.

### 2. Create the VM in VirtualBox
| Setting | Suggested value |
|---|---|
| Name | `fw-opnsense` |
| Type / Version | BSD / FreeBSD (64-bit) |
| Memory | 2048 MB |
| Processors | 2 vCPU |
| Disk | 20 GB (VDI, dynamically allocated) |
| ISO | the decompressed ISO in the optical drive |

### 3. Configure network adapters (VM powered off)
| Adapter | Attached to | Role | Notes |
|---|---|---|---|
| 1 | **NAT** | WAN | Gets an IP via VirtualBox DHCP (usually 10.0.2.x) |
| 2 | **Internal Network**, name `lab-lan` | LAN | Type **Intel PRO/1000 MT Desktop**; under Advanced, **Promiscuous Mode: Allow All** |

> The internal network name is case-sensitive. Every VM that needs to reach the LAN must use exactly the same name.
>
> Write down each adapter's **MAC address** (Advanced). It helps tell which OPNsense interface (`em0`, `em1`) is which.

### 4. Install
1. Start the VM. At the installer login, use user `installer` and password `opnsense`.
2. Pick the keyboard layout.
3. Guided install (UFS or ZFS, both fine for a lab), select the disk and confirm.
4. Set the **root password** and store it in a password manager.
5. When finished, choose **Complete Install** and reboot.
6. **Remove the ISO** from the virtual drive before the reboot, so it doesn't boot the installer again.

### 5. Check WAN and LAN on the console
In the console menu, use **1) Assign interfaces** and verify:
- **WAN** = adapter 1 NIC (NAT)
- **LAN** = adapter 2 NIC (internal network)

If swapped, reassign with the same option. Then, in **2) Set interface IP address**, confirm the LAN is `192.168.1.1/24` with DHCP enabled.

### 6. Access the web interface
1. Create or use the `cli-admin` VM with adapter 1 on **Internal Network** `lab-lan`.
2. It should get a `192.168.1.x` address via DHCP.
3. In the browser, open `https://192.168.1.1` (accept the certificate warning).
4. Log in as `root` with the password set during install.
5. Complete the initial wizard: hostname, DNS, time zone (`America/Recife`) and confirm WAN and LAN.

### 7. Validation
| Test | Expected | Actual |
|---|---|---|
| OPNsense console, option 7 (Ping), target 8.8.8.8 | Replies | `[fill in]` |
| `cli-admin` gets a 192.168.1.x address | Yes | `[fill in]` |
| `cli-admin` pings 192.168.1.1 | Success | `[fill in]` |
| `cli-admin` pings 8.8.8.8 | Success (via NAT) | `[fill in]` |
| Web UI opens at https://192.168.1.1 | Yes | `[fill in]` |

### Common issues in this step
| Symptom | Likely cause | What to check |
|---|---|---|
| VM won't start or is very slow | Virtualization disabled in BIOS | Enable VT-x/AMD-V |
| Client gets no IP | Different internal network names or promiscuous mode off | Exact name `lab-lan` and Allow All |
| WAN has no IP | Adapter 1 not on NAT, or interfaces swapped | Console option 1 and MACs |
| Back to the installer after reboot | ISO still mounted | Remove the ISO |

> Write down what actually happened in your lab: `[fill in]`

### Next step
Step 3: create VLANs 10, 20 and 30 in OPNsense. Important note: VirtualBox has no managed switch, so clients will need to tag traffic (802.1Q) inside the VM, or a switch VM will sit in between. That decision will be documented in `03-vlans.md`.
