# Homelab — Inventário e documentação da infraestrutura

> **Projeto:** Personal Homelab — IT Infrastructure & Networking  
> **Estado:** Documentação inicial (inventário parcial)  
> **Data do levantamento:** 20 de outubro de 2026  
> **Plataforma:** Proxmox Virtual Environment 9.2.2  
> **Nó:** `homelab`

## 1. Objetivo

Este documento apresenta a infraestrutura atual do meu laboratório pessoal (*homelab*), utilizado para desenvolver competências práticas em virtualização, administração de sistemas Windows e Linux e redes de computadores.

O ambiente está alojado num computador desktop físico, no qual instalei o Proxmox VE através de uma pen USB de arranque. Após a instalação do hipervisor, criei máquinas virtuais com diferentes sistemas operativos para atividades de aprendizagem, configuração e testes.

O levantamento é baseado na configuração que descrevi e numa captura de ecrã da interface de administração. Os elementos que não podem ser confirmados por estas fontes estão identificados como **Por confirmar**, para evitar apresentar suposições como factos.

## 2. Visão geral da arquitetura

```text
                   +----------------------------+
                   | Computador desktop físico |
                   | Servidor Proxmox VE 9.2.2  |
                   | Nó: homelab               |
                   +-------------+--------------+
                                 |
                       Virtualização (KVM)
                                 |
          +----------------------+----------------------+
          |                      |                      |
    VMs Windows             VMs Ubuntu             Armazenamento
    100, 101, 104           102, 103          local / local-lvm /
                                                localnetwork
```

> **Nota:** Este diagrama é uma representação lógica simplificada do inventário observado. Não mostra ligações físicas, bridges, VLANs, endereços IP ou o posicionamento dos discos, porque esses dados ainda não foram recolhidos.

## 3. Hardware do servidor físico

| Componente | Informação atual |
|---|---|
| Tipo de equipamento | Computador desktop (PC fixo) |
| Função | Host de virtualização para o homelab |
| Processador (CPU) | **Por confirmar** |
| Memória RAM total | **Por confirmar** |
| Discos físicos (tipo/capacidade) | **Por confirmar** |
| Interfaces de rede físicas | **Por confirmar** |
| Modelo de motherboard | **Por confirmar** (opcional) |
| Tipo de ligação à rede | **Por confirmar** |

### Recolha de informação do hardware

Executar os seguintes comandos na **shell do host Proxmox**, depois de confirmar que se está no nó correto:

```bash
lscpu
free -h
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
ip -br link
```

**O que registar:** modelo e número de cores/threads da CPU, quantidade total de RAM, discos disponíveis e interfaces de rede. Não publicar números de série nem endereços MAC reais sem necessidade.

## 4. Instalação e gestão do Proxmox VE

O Proxmox VE foi instalado diretamente no computador desktop, utilizando uma pen USB de arranque como meio de instalação. Depois da instalação, o ambiente passou a ser gerido pela interface web do Proxmox, a partir da qual foram criadas e administradas máquinas virtuais.

| Parâmetro | Valor |
|---|---|
| Hipervisor | Proxmox Virtual Environment (PVE) |
| Versão apresentada | 9.2.2 |
| Nome do nó | `homelab` |
| Número de nós observados | 1 |
| Método de instalação | Pen USB de arranque |
| Disco onde o Proxmox está instalado | **Por confirmar** |
| Configuração de rede do host | **Por confirmar** |
| Configuração de backups | **Por confirmar** |

**Validação sugerida:**

```bash
pveversion -v
hostnamectl
```

A versão 9.2.2 é a que aparece na interface capturada; o resultado de `pveversion -v` permitirá registar os pacotes e versões efetivamente instalados no momento da recolha.

## 5. Inventário de máquinas virtuais

A árvore de recursos do Proxmox mostra **cinco máquinas virtuais**:

| VMID | Nome apresentado na interface | Sistema operativo / finalidade aparente | Estado confirmado |
|---|---|---|---|
| 100 | `Copy-of-VM-windows-11` | Windows 11 — cópia de VM | **Parada** no momento da captura |
| 101 | `windows-11` | Windows 11 | Por confirmar |
| 102 | `Ubuntu-desktop` | Ubuntu Desktop | Por confirmar |
| 103 | `ubuntu-server` | Ubuntu Server | Aparece em execução na árvore de recursos |
| 104 | `Windows-server-graphic` | Windows Server com interface gráfica (a confirmar) | Por confirmar |

Os sistemas operativos e as finalidades acima foram inferidos **dos nomes atribuídos às VMs**; as versões internas e a configuração de cada sistema ainda precisam de validação.

### Detalhes observados para a VM 100

A VM `100 (Copy-of-VM-windows-11)` estava selecionada na captura de ecrã, permitindo identificar:

| Propriedade | Valor observado |
|---|---|
| Identificador | `100` |
| Nome | `Copy-of-VM-windows-11` |
| Estado | `stopped` |
| vCPUs | 4 |
| Memória configurada | 4,00 GiB |
| Tamanho apresentado do disco de arranque | 32,00 GiB |
| QEMU Guest Agent | Não configurado, conforme painel de resumo |
| Nó | `homelab` |

> Os valores de CPU, memória e disco acima referem-se **apenas à VM 100** e não à capacidade total do servidor.

### Recolha de dados detalhados das VMs

Na shell do Proxmox:

```bash
qm list
qm config 100
qm config 101
qm config 102
qm config 103
qm config 104
```

Guardar para cada VM: versão do sistema operativo, finalidade, vCPUs, RAM, capacidade e localização do disco virtual, interface(s) de rede, bridge(s), modo de arranque e estado do QEMU Guest Agent. Antes de publicar ficheiros de configuração, remover dados que possam expor a infraestrutura doméstica.

## 6. Containers LXC

**Não foram identificados containers LXC na árvore de recursos visível da captura.** Isto não prova que não existam containers noutros estados ou não mostrados pela interface.

Para validar o inventário:

```bash
pct list
```

| Campo | Valor |
|---|---|
| Containers observados na captura | Nenhum |
| Inventário de containers no host | **Por confirmar com `pct list`** |

## 7. Armazenamento

A interface web mostra os seguintes recursos de armazenamento:

| Identificador no Proxmox | Visível na captura | Tipo / função efetiva |
|---|---|---|
| `local` | Sim | **Por confirmar** |
| `local-lvm` | Sim | **Por confirmar** |
| `localnetwork` | Sim | **Por confirmar** |

**Importante:** os nomes `local`, `local-lvm` e `localnetwork` identificam recursos configurados no Proxmox, mas a captura não permite confirmar o tipo, a localização, a capacidade ou a utilização efetiva de cada um. Em particular, o nome `localnetwork` não comprova, por si só, que seja um armazenamento de rede.

Para obter os detalhes:

```bash
pvesm status
cat /etc/pve/storage.cfg
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
```

Documentar: tipo de storage, capacidade total e utilizada, localização, conteúdos permitidos (por exemplo, imagens de VM, ISOs ou backups) e eventual redundância. Verificar e ocultar informação sensível antes de publicar configurações.

## 8. Rede e interfaces

A captura não apresenta o separador de rede do nó nem a configuração das interfaces físicas ou virtuais. Por isso, a topologia de rede ainda não pode ser documentada com precisão.

| Elemento | Estado |
|---|---|
| Interfaces de rede físicas | Por confirmar |
| Bridges Linux (`vmbr*`) | Por confirmar |
| Endereço IP de gestão do Proxmox | Por confirmar — **não publicar o IP público** |
| Interfaces de rede atribuídas a cada VM | Por confirmar |
| VLANs | Por confirmar |
| Gateway / routing | Por confirmar |
| Firewall | Por confirmar |

Comandos úteis na shell do host:

```bash
ip -br address
ip -br link
ip route
cat /etc/network/interfaces
```

A configuração `/etc/network/interfaces` deve ser revista e sanitizada antes de integrar o portefólio público. Um diagrama físico e outro lógico serão produzidos após o levantamento da rede.

## 9. Captura de ecrã e evidência

A imagem abaixo documenta o inventário inicial, o nó `homelab`, os storages visíveis e o painel de resumo da VM 100.

![Interface Proxmox VE — inventário inicial do homelab](images/proxmox-vms-overview.png)

**Evidência:** captura da interface web do Proxmox VE, obtida em 20/10/2026.

## 10. Limitações do levantamento inicial

Este inventário ainda não permite concluir:

- Modelo e capacidade do hardware físico.
- Onde está instalado o Proxmox e em que discos estão guardadas as VMs.
- Configuração real e capacidades dos storages.
- Número e tipo de interfaces físicas, bridges e redes virtuais.
- Configuração detalhada e estado atual de todas as VMs.
- Existência de containers LXC fora do que aparece na imagem.
- Existência de backups, monitorização, VLANs ou outros serviços.

Estes pontos serão preenchidos com evidência recolhida diretamente do host e das VMs, preservando a segurança e privacidade da rede doméstica.

## 11. Próximas tarefas de documentação

- [x] Identificar a plataforma de virtualização e o nome do nó.
- [x] Registar as máquinas virtuais apresentadas na interface.
- [x] Identificar os storages apresentados na interface.
- [x] Guardar a captura de ecrã inicial.
- [ ] Recolher o inventário completo de hardware.
- [ ] Confirmar versões, recursos e funções de cada VM.
- [ ] Confirmar a existência ou ausência de containers LXC.
- [ ] Documentar os storages e respetivas capacidades.
- [ ] Mapear as interfaces de rede e Linux bridges.
- [ ] Desenhar a topologia física e lógica efetiva.
- [ ] Documentar políticas de backup e executar um teste de restauro.

## 12. Nota de segurança

Este repositório destina-se a fins educativos e de portefólio profissional. Antes de publicar documentação, comandos ou capturas, serão removidos ou ocultados endereços IP públicos, credenciais, tokens, chaves privadas, identificadores sensíveis e quaisquer outros dados que possam comprometer o ambiente doméstico.

---

*Documento inicial — será atualizado à medida que o inventário for confirmado.*
