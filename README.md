# OpenBSD-NAS

NAS doméstico baseado em **Raspberry Pi 5 + OpenBSD**, desenvolvido como um projeto pessoal de armazenamento em rede, administração Unix e segurança.

O projeto prioriza simplicidade, baixo consumo, operação 24/7 e reaproveitamento do hardware existente.

## Objetivos

- Construir um NAS doméstico compacto.
- Utilizar **OpenBSD** como sistema operacional.
- Administrar o servidor principalmente através de **OpenSSH**.
- Utilizar até **4 unidades SATA**.
- Evitar dependência da interface PCIe do Raspberry Pi.
- Utilizar refrigeração completamente passiva no Raspberry Pi.
- Desenvolver uma case personalizada através de impressão 3D.
- Aprender administração de sistemas BSD/Unix durante o desenvolvimento.

## Hardware planejado

| Componente | Modelo |
|---|---|
| SBC | Raspberry Pi 5 |
| Case do Raspberry Pi | EDATEC ED-Pi5Case-B |
| Extensor GPIO | EDATEC ED-Pi5Case-HA-B |
| Controladora SATA | Radxa Quad SATA HAT QF4 |
| Interface dos discos | USB 3.0 |
| Armazenamento | Até 4× SATA SSD/HDD |
| Case externa | Projeto próprio impresso em 3D |
| Rede | Gigabit Ethernet |

## Software

- OpenBSD
- OpenSSH
- FFS2
- PF
- ferramentas nativas do OpenBSD

Serviços adicionais poderão ser adicionados conforme a evolução do projeto.

## Restrições do hardware

O Raspberry Pi 5 utilizado neste projeto possui dois conectores fisicamente danificados:

- conector **FAN de 4 pinos indisponível**;
- conector/trava **PCIe FFC indisponível**.

Por isso, a arquitetura foi projetada deliberadamente sem depender dessas interfaces.

A refrigeração será passiva através da case EDATEC e o armazenamento SATA será conectado através das portas USB 3.0 do Raspberry Pi.

## Arquitetura

```text
             Rede local
                 │
          Gigabit Ethernet
                 │
        ┌────────▼────────┐
        │ Raspberry Pi 5  │
        │    OpenBSD      │
        │    OpenSSH      │
        └────────┬────────┘
                 │
              USB 3.0
                 │
        ┌────────▼────────┐
        │ Radxa Quad SATA │
        │    HAT QF4      │
        └───┬───┬───┬────┘
            │   │   │
          SATA SATA SATA SATA
            │   │   │   │
           SSD SSD SSD SSD
```

## Estrutura do repositório

```text
OpenBSD-NAS/
├── README.md
├── docs/
│   ├── HARDWARE.md
│   └── ARCHITECTURE.md
└── case/
```

A pasta `case/` será utilizada futuramente para os arquivos relacionados à case personalizada, incluindo modelos 3D, medidas e documentação de montagem.

## Estado atual

**Fase: planejamento de hardware**

- [x] Raspberry Pi 5 definido
- [x] OpenBSD definido
- [x] OpenSSH definido
- [x] Refrigeração passiva definida
- [x] Case EDATEC selecionada
- [x] Extensor GPIO selecionado
- [x] Controladora SATA selecionada
- [ ] Comprar componentes
- [ ] Definir SSDs/HDDs
- [ ] Testar Radxa QF4 no OpenBSD
- [ ] Projetar case 3D
- [ ] Montar protótipo
- [ ] Instalar OpenBSD
- [ ] Configurar armazenamento
- [ ] Configurar compartilhamento em rede
- [ ] Configurar backups
- [ ] Documentar montagem final

## Licença

A licença do projeto será definida posteriormente.