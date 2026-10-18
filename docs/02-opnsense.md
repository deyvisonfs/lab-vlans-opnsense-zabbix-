# 02 · Instalação do OPNsense no VirtualBox / OPNsense on VirtualBox

[← README](../README.md) · [← 01 Ambiente](01-ambiente.md) · [03 VLANs →](03-vlans.md)

## 🇧🇷 Português

### Objetivo
Instalar o OPNsense em uma VM com três placas de rede: uma de gerência (acesso à interface web pelo PC), uma de WAN (internet) e uma para a rede interna do lab, que vai carregar as VLANs.

### 1. Baixar a ISO
1. Acesse a página de downloads do OPNsense (opnsense.org/download).
2. Escolha arquitetura **amd64**, tipo **dvd** e um espelho de download.
3. Descompacte o arquivo `.bz2` para obter a `.iso`.

### 2. Criar a VM no VirtualBox
| Configuração | Valor |
|---|---|
| Nome | `OPNsense` |
| Tipo / Versão | BSD / FreeBSD (64-bit) |
| Memória | 1 a 2 GB |
| Processadores | 1 a 2 vCPU |
| Disco | 20 GB (VDI, alocação dinâmica) |
| ISO | a ISO descompactada, no leitor óptico |

### 3. Placas de rede (VM desligada)
| Adaptador | Modo no VirtualBox | Placa no OPNsense | Função | IP no OPNsense |
|---|---|---|---|---|
| 1 | **Host-Only** (`vboxnet0`, PC em 192.168.56.1/24) | `em0` | LAN, usada como gerência | `192.168.56.2/24`, fixo, sem gateway |
| 2 | **Bridge** (placa Wi-Fi do PC) | `em1` | WAN | DHCP da rede doméstica |
| 3 | **Rede Interna** `lab-lan` | `em2` | Tronco das VLANs (LabTrunk) | Sem IP |

Em todos os adaptadores: tipo **Intel PRO/1000 MT Desktop** e **Modo Promíscuo: Permitir Tudo**.

> O nome da rede interna diferencia maiúsculas de minúsculas. Todas as VMs que precisarem conversar com o lab devem usar exatamente `lab-lan`.
>
> A rede Host-Only precisa existir e estar selecionada no campo **Nome** do adaptador 1 (Arquivo → Ferramentas → Gerenciador de Rede → Host-Only Networks).

### 4. Instalar
1. Inicie a VM. No login do instalador, use usuário `installer` e senha `opnsense`.
2. Escolha o layout de teclado.
3. Instalação guiada (UFS ou ZFS), selecione o disco e confirme.
4. Defina a **senha do root** e guarde em um gerenciador de senhas.
5. Escolha **Complete Install** e reinicie, **removendo a ISO** do leitor virtual.

### 5. Configuração inicial no console
1. Opção **1 (Assign interfaces)**: WAN = `em1`, LAN = `em0`.
2. Opção **2 (Set interface IP address)**, interface LAN: IPv4 `192.168.56.2`, máscara `24`, sem gateway, DHCP desligado, sem mudar o protocolo da interface web.
3. Opção **2**, interface WAN: DHCP ativado.
4. Confira no menu principal: LAN com `192.168.56.2/24` e WAN com um IP da rede doméstica.

### 6. Interface web
No navegador do PC, acesse `https://192.168.56.2` (aceite o aviso de certificado) e entre como `root`.

> O PC consegue alcançar a rede `192.168.56.0/24` porque o VirtualBox cria no PC uma placa virtual (`vboxnet0`, 192.168.56.1) ligada diretamente a essa rede. Isso é independente da rede doméstica (`192.168.0.0/24`) do Wi-Fi.

Depois, em **Interfaces → Assignments**, adicione a `em2` com a descrição `LabTrunk`, habilite e deixe **IPv4/IPv6 Configuration Type: None**. Opcional: renomear a LAN para `MGMT`.

### 7. Validação
| Teste | Resultado esperado | Resultado obtido |
|---|---|---|
| LAN em `em0` com 192.168.56.2/24 | Sim | Confirmado em Interfaces → Assignments |
| WAN em `em1` com IP por DHCP e gateway da rede doméstica | Sim | IP 192.168.0.x e gateway 192.168.0.1 (Interfaces → Overview) |
| Interface web abre em https://192.168.56.2 | Sim | Sim |
| Console, opção 7 (Ping), destino 8.8.8.8 | Respostas | `[preencher]` |

### Problemas encontrados e soluções
| Sintoma | Causa | Solução |
|---|---|---|
| LAN em `192.168.0.50/24` e WAN em `192.168.0.105/24`, as duas na mesma sub-rede | Os adaptadores 1 e 2 estavam em Bridge, então LAN e WAN ficaram na rede doméstica, sem isolamento e com roteamento ambíguo | Adaptador 1 passou para Host-Only (gerência) e foi criado o adaptador 3 em Rede Interna `lab-lan` para o lab |
| VirtualBox mostrava "Configurações inválidas detectadas" e o botão OK desabilitado | Adaptador 1 em Host-Only com o campo **Nome** em "Não selecionado" | Selecionar a rede `vboxnet0` (criada no Gerenciador de Rede) |
| IP `192.168.0.50` inalcançável após trocar para Host-Only | A Host-Only é outra sub-rede (`192.168.56.0/24`) | Trocar o IP da LAN para `192.168.56.2/24` pelo console (opção 2) |

### Observações
- A interface com função de WAN foi listada com o identificador `opt1` (descrição WAN), e não `wan`. Funciona, mas será verificado no teste de saída para a internet das VLANs (NAT de saída).

### Próximo passo
[03 · VLANs](03-vlans.md): criar as VLANs 10, 20 e 30 sobre a `em2` e dar IP a cada uma.

---

## 🇺🇸 English

### Goal
Install OPNsense on a VM with three network cards: a management one (web UI access from the PC), a WAN one (internet) and one for the lab's internal network, which will carry the VLANs.

### 1. Download the ISO
1. Go to the OPNsense download page (opnsense.org/download).
2. Pick architecture **amd64**, image type **dvd** and a mirror.
3. Decompress the `.bz2` file to get the `.iso`.

### 2. Create the VM in VirtualBox
| Setting | Value |
|---|---|
| Name | `OPNsense` |
| Type / Version | BSD / FreeBSD (64-bit) |
| Memory | 1 to 2 GB |
| Processors | 1 to 2 vCPU |
| Disk | 20 GB (VDI, dynamically allocated) |
| ISO | the decompressed ISO in the optical drive |

### 3. Network adapters (VM powered off)
| Adapter | VirtualBox mode | OPNsense NIC | Role | OPNsense IP |
|---|---|---|---|---|
| 1 | **Host-Only** (`vboxnet0`, PC at 192.168.56.1/24) | `em0` | LAN, used for management | `192.168.56.2/24`, static, no gateway |
| 2 | **Bridge** (PC's Wi-Fi card) | `em1` | WAN | DHCP from the home network |
| 3 | **Internal Network** `lab-lan` | `em2` | VLAN trunk (LabTrunk) | No IP |

On all adapters: type **Intel PRO/1000 MT Desktop** and **Promiscuous Mode: Allow All**.

> The internal network name is case-sensitive. Every VM that needs to reach the lab must use exactly `lab-lan`.
>
> The Host-Only network must exist and be selected in adapter 1's **Name** field (File → Tools → Network Manager → Host-Only Networks).

### 4. Install
1. Start the VM. At the installer login, use user `installer` and password `opnsense`.
2. Pick the keyboard layout.
3. Guided install (UFS or ZFS), select the disk and confirm.
4. Set the **root password** and store it in a password manager.
5. Choose **Complete Install** and reboot, **removing the ISO** from the virtual drive.

### 5. Initial console configuration
1. Option **1 (Assign interfaces)**: WAN = `em1`, LAN = `em0`.
2. Option **2 (Set interface IP address)**, LAN interface: IPv4 `192.168.56.2`, mask `24`, no gateway, DHCP off, keep the web UI protocol.
3. Option **2**, WAN interface: DHCP enabled.
4. Check the main menu: LAN at `192.168.56.2/24` and WAN with a home-network IP.

### 6. Web interface
In the PC's browser, open `https://192.168.56.2` (accept the certificate warning) and log in as `root`.

> The PC can reach `192.168.56.0/24` because VirtualBox creates a virtual NIC on the PC (`vboxnet0`, 192.168.56.1) directly attached to that network. It is independent from the home network (`192.168.0.0/24`) on the Wi-Fi.

Then, in **Interfaces → Assignments**, add `em2` with the description `LabTrunk`, enable it and set **IPv4/IPv6 Configuration Type: None**. Optional: rename LAN to `MGMT`.

### 7. Validation
| Test | Expected | Actual |
|---|---|---|
| LAN on `em0` with 192.168.56.2/24 | Yes | Confirmed in Interfaces → Assignments |
| WAN on `em1` with DHCP IP and home-network gateway | Yes | IP 192.168.0.x and gateway 192.168.0.1 (Interfaces → Overview) |
| Web UI opens at https://192.168.56.2 | Yes | Yes |
| Console, option 7 (Ping), target 8.8.8.8 | Replies | `[fill in]` |

### Issues found and fixes
| Symptom | Cause | Fix |
|---|---|---|
| LAN at `192.168.0.50/24` and WAN at `192.168.0.105/24`, both on the same subnet | Adapters 1 and 2 were both on Bridge, so LAN and WAN sat on the home network, with no isolation and ambiguous routing | Adapter 1 moved to Host-Only (management) and adapter 3 was created on Internal Network `lab-lan` for the lab |
| VirtualBox showed "Invalid settings detected" and the OK button was disabled | Adapter 1 on Host-Only with the **Name** field set to "Not selected" | Select the `vboxnet0` network (created in Network Manager) |
| IP `192.168.0.50` unreachable after switching to Host-Only | Host-Only is a different subnet (`192.168.56.0/24`) | Change the LAN IP to `192.168.56.2/24` from the console (option 2) |

### Notes
- The WAN-role interface was listed with identifier `opt1` (description WAN) instead of `wan`. It works, but will be checked in the VLANs' internet-access test (outbound NAT).

### Next step
[03 · VLANs](03-vlans.md): create VLANs 10, 20 and 30 on `em2` and give each one an IP.
