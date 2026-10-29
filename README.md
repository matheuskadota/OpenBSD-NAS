# OpenBSD-NAS

NAS doméstico baseado em **Raspberry Pi 5 + OpenBSD**, desenvolvido como projeto pessoal de armazenamento em rede, administração Unix e segurança.

O projeto prioriza simplicidade, baixo consumo, operação 24/7 e reaproveitamento do hardware existente.

## Estado atual

**Fase: sistema base pronto — aguardando hardware de armazenamento**

| Área | Estado |
|---|---|
| OpenBSD no Raspberry Pi 5 (microSD 64 GB) | ✅ OpenBSD **8.0**/arm64, boot autônomo |
| Upgrade 7.9 → 8.0 (`sysupgrade`, `sysmerge`, `pkg_add -u`, `syspatch`) | ✅ 29/10/2026 |
| Administração remota por OpenSSH | ✅ somente chave; senha e root bloqueados |
| `doas` | ✅ grupo `wheel` |
| PF | ✅ entrada bloqueada por padrão; SSH e ping só da LAN |
| Ajustes 24/7 (`noatime`, GUI fora do boot, serviços enxutos) | ✅ |
| Backups do sistema (imagem do microSD + `/etc` e `/home`) | ✅ fora do Pi |
| Temperatura em repouso (case oficial, sem ventoinha) | ✅ ~63 °C |
| Hardware de armazenamento | ⏳ a definir/comprar ([HARDWARE](docs/HARDWARE.md)) |
| Discos (FFS2 + softraid RAID1) | ⏳ |
| Compartilhamento em rede (Samba; NFS opcional) | ⏳ |
| Backups dos dados | ⏳ destino a definir |
| Case definitiva / case 3D | ⏳ |

## Objetivos

- Construir um NAS doméstico compacto.
- Utilizar **OpenBSD** como sistema operacional.
- Administrar o servidor principalmente através de **OpenSSH**.
- Armazenamento SATA conectado por **USB 3.0**.
- Não depender das interfaces danificadas do Raspberry Pi (FAN header e conector PCIe FFC).
- Refrigeração passiva no Raspberry Pi.
- Desenvolver uma case personalizada através de impressão 3D.
- Aprender administração de sistemas BSD/Unix durante o desenvolvimento.

## Hardware

| Componente | Situação |
|---|---|
| SBC | Raspberry Pi 5 — 8 GB (em uso) |
| Sistema | microSD SanDisk 64 GB, slot interno (em uso) |
| Case | case oficial do Raspberry Pi 5, sem ventoinha (em uso) |
| Fonte | fonte oficial do Raspberry Pi 5 (em uso) |
| Rede | Gigabit Ethernet (`cad0`) |
| Armazenamento | a definir — ver [docs/HARDWARE.md](docs/HARDWARE.md) |

## Software

- OpenBSD 8.0 (arm64)
- OpenSSH
- PF
- FFS2 + softraid (planejado)
- Samba (planejado)
- ferramentas nativas do OpenBSD

## Restrições do hardware

O Raspberry Pi 5 utilizado possui dois conectores fisicamente danificados:

- **FAN header de 4 pinos** — sem ventoinha ativa;
- **conector/trava PCIe FFC externo** — sem HATs PCIe/NVMe.

USB e Ethernet **não** são afetados: no Pi 5 eles passam pelo chip RP1, ligado por um PCIe interno que funciona normalmente.

## Arquitetura (planejada)

```text
          Rede local (Gigabit)
                  │
         ┌────────▼────────┐
         │ Raspberry Pi 5  │
         │  OpenBSD 8.0    │
         │  OpenSSH · PF   │
         │  sistema: µSD   │
         └───┬─────────┬───┘
          USB 3.0   USB 3.0
             │         │
          ┌──▼──┐   ┌──▼──┐
          │SATA │   │SATA │
          │ D1  │   │ D2  │
          └──┬──┘   └──┬──┘
             └──RAID1──┘
              (softraid)
               /srv/nas
```

## Documentação

| Documento | Conteúdo |
|---|---|
| [docs/HISTORY.md](docs/HISTORY.md) | histórico do projeto |
| [docs/BOOT.md](docs/BOOT.md) | cadeia de boot do Pi 5 com OpenBSD |
| [docs/SETUP.md](docs/SETUP.md) | configuração já aplicada no sistema |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | arquitetura e decisões |
| [docs/HARDWARE.md](docs/HARDWARE.md) | hardware e opções de armazenamento |
| [docs/ROADMAP.md](docs/ROADMAP.md) | próximas etapas |
| [case/README.md](case/README.md) | case 3D |

## Estrutura do repositório

```text
OpenBSD-NAS/
├── README.md
├── docs/
│   ├── ARCHITECTURE.md
│   ├── BOOT.md
│   ├── HARDWARE.md
│   ├── HISTORY.md
│   ├── ROADMAP.md
│   └── SETUP.md
└── case/
    └── README.md
```

## Licença

A licença do projeto ainda será definida.
