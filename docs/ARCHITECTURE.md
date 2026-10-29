# Arquitetura

## Visão geral

O **OpenBSD-NAS** é um NAS doméstico baseado em Raspberry Pi 5 executando OpenBSD.

A arquitetura não depende do PCIe FFC externo nem do FAN header do Raspberry Pi, que estão danificados.

```text
                 Rede local (Gigabit)
                         │
                 ┌───────▼────────┐
                 │ Raspberry Pi 5 │
                 │  OpenBSD 8.0   │
                 │ sistema: µSD   │
                 └───┬────────┬───┘
                  USB 3.0  USB 3.0
                     │        │
                    D1       D2
                     └─RAID1──┘
```

## Camadas

| Camada | Escolha | Estado |
|---|---|---|
| Hardware | Raspberry Pi 5 (8 GB) + discos SATA por USB 3.0 | Pi em uso; discos a definir |
| Sistema | OpenBSD 8.0/arm64 no microSD | ✅ |
| Administração | OpenSSH, só chave | ✅ |
| Privilégio | `doas` (grupo `wheel`) | ✅ |
| Firewall | PF, bloqueio por padrão | ✅ |
| Armazenamento | FFS2 sobre softraid RAID1 | planejado |
| Compartilhamento | Samba; NFS opcional | planejado |

## Sistema e dados separados

```text
microSD ── OpenBSD (sistema)
USB/SATA ─ dados (/srv/nas)
```

O sistema permanece no microSD. Os dados ficam em discos dedicados, montados pelo DUID com `noatime,nodev,nosuid`. Uma falha nos discos não impede o boot; uma falha no microSD se resolve gravando a última imagem.

## Administração

```text
Desktop ──SSH (chave Ed25519)──▶ bsd (OpenBSD)
```

- login só por chave; senha, keyboard-interactive e root bloqueados;
- `AllowUsers matt`;
- comandos privilegiados via `doas`;
- a GUI existe para uso local eventual (`startx`), mas não sobe no boot.

## Compartilhamento de arquivos

- **Samba** como principal: atende Linux, Windows e celular. SMB3 mínimo, ligado só a `cad0`, sem acesso de convidado.
- **NFS** opcional para clientes Linux. O `nfsd` do OpenBSD implementa NFSv2/v3, sem autenticação de usuário no protocolo, então a proteção depende de `/etc/exports` e do PF.

## Firewall

```text
LAN
 ├── SSH  → permitido só da rede local
 ├── ping → permitido só da rede local
 ├── SMB  → será permitido só da rede local
 └── resto → bloqueado
```

O NAS não é exposto à Internet.

## Segurança

- nenhum serviço público;
- administração só por OpenSSH com chave;
- serviços limitados à LAN pelo PF;
- menor privilégio (`doas`, `AllowUsers`);
- `syspatch` e `pkg_add -u` regulares;
- backups independentes do armazenamento principal.

## Redundância ≠ backup

```text
RAID1 (softraid) → protege contra falha de um disco
backup           → protege contra apagamento, corrupção, roubo e falha do NAS
```

Dados importantes terão uma cópia fora do NAS. Destino a definir.

## Boot

Definido: o sistema inicia a partir do microSD no slot interno. Detalhes em [BOOT.md](BOOT.md).

## Ordem de implementação

```text
1. OpenBSD no Pi                     ✅
2. OpenSSH, doas, PF                 ✅
3. Upgrade 8.0 e ajustes 24/7        ✅
4. Teste do hardware de armazenamento
5. Discos (softraid RAID1)
6. Compartilhamento (Samba)
7. Monitoramento e backups
8. Testes de aceitação
9. Case definitiva / case 3D
```

Cada etapa deve estar funcional e compreendida antes da próxima.
