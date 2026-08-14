# Architecture

## Visão geral

O **OpenBSD-NAS** será um NAS doméstico baseado em Raspberry Pi 5 executando OpenBSD.

A arquitetura foi projetada para não depender de PCIe nem do FAN header do Raspberry Pi.

```text
                     Rede local
                         │
                    Ethernet
                         │
                 ┌───────▼───────┐
                 │ Raspberry Pi 5│
                 │    OpenBSD    │
                 └───────┬───────┘
                         │
                    2× USB 3.0
                         │
                 ┌───────▼───────┐
                 │ Radxa QF4     │
                 │ Quad SATA HAT │
                 └─┬───┬───┬───┬┘
                   │   │   │   │
                 SATA SATA SATA SATA
                   │   │   │   │
                  D1  D2  D3  D4
```

## Camadas

### Hardware

```text
Raspberry Pi 5
      │
Radxa Quad SATA HAT QF4
      │
4× SATA
```

### Sistema operacional

```text
OpenBSD
```

### Administração

```text
OpenSSH
```

### Rede e segurança

```text
PF
OpenSSH
usuários/grupos
permissões Unix
```

### Armazenamento

Inicialmente:

```text
Discos SATA
    │
OpenBSD
    │
FFS2
```

A configuração definitiva dos discos será decidida após testes.

Possibilidades:

- discos independentes;
- espelhamento;
- softraid;
- volumes dedicados a diferentes finalidades.

---

## OpenSSH

O OpenSSH será o principal método de administração do servidor.

```text
Notebook/Desktop
       │
       │ SSH
       ▼
Raspberry Pi 5
     OpenBSD
```

Ele será utilizado para:

- administração;
- configuração;
- manutenção;
- transferência segura de arquivos;
- automação.

Autenticação por chave será priorizada.

---

## Compartilhamento de arquivos

O protocolo definitivo ainda será escolhido.

Possibilidades:

### NFS

Principal opção para máquinas Unix/Linux.

```text
OpenBSD NAS
     │
    NFS
     │
Linux / BSD
```

### Samba

Poderá ser utilizado caso seja necessária compatibilidade SMB com Windows ou outros dispositivos.

Os dois protocolos poderão coexistir caso necessário.

---

## Firewall

O **PF** será utilizado para controlar quais serviços podem ser acessados pela rede.

Conceito:

```text
LAN
 │
 ├── SSH → permitido somente conforme política
 ├── NFS/SMB → rede local
 └── outros serviços → bloqueados por padrão
```

O NAS não será inicialmente exposto diretamente à Internet.

---

## Segurança

Princípios iniciais:

- nenhum serviço público desnecessário;
- administração através de OpenSSH;
- autenticação por chave;
- serviços limitados à LAN;
- PF habilitado;
- princípio do menor privilégio;
- atualizações regulares;
- backups independentes do armazenamento principal.

---

## Armazenamento e redundância

O projeto fará distinção entre:

```text
RAID / redundância
         ≠
       backup
```

Mesmo que futuramente seja utilizado espelhamento ou outro nível de softraid, dados importantes deverão possuir uma cópia independente.

---

## Boot

O método definitivo de boot será definido durante a etapa de instalação.

O armazenamento do sistema operacional e o armazenamento principal do NAS poderão ser separados.

Exemplo:

```text
microSD
└── OpenBSD

SATA
└── dados
```

ou, caso os testes permitam:

```text
SATA
├── OpenBSD
└── dados
```

---

## Desenvolvimento do projeto

A implementação será realizada em etapas:

```text
1. Hardware
       ↓
2. Teste do Radxa QF4
       ↓
3. OpenBSD
       ↓
4. OpenSSH
       ↓
5. Armazenamento
       ↓
6. Compartilhamento de rede
       ↓
7. PF / segurança
       ↓
8. Case 3D
       ↓
9. Operação 24/7
```

A prioridade será manter cada etapa funcional e compreensível antes de adicionar novos serviços.