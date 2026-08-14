# Hardware

Este documento registra os componentes e decisões de hardware do projeto **OpenBSD-NAS**.

## Raspberry Pi 5

O Raspberry Pi 5 será utilizado como computador principal do NAS.

### Interfaces utilizadas

- Gigabit Ethernet
- USB 3.0
- GPIO de 40 pinos
- microSD, inicialmente para instalação/boot

### Interfaces indisponíveis

O Raspberry Pi utilizado possui danos físicos em:

- **FAN header de 4 pinos**
- **conector/trava PCIe FFC**

Essas interfaces não fazem parte da arquitetura do projeto.

---

## EDATEC ED-Pi5Case-B

Case passiva de alumínio selecionada para refrigerar o Raspberry Pi 5.

### Motivos da escolha

- construção em alumínio;
- excelente dissipação térmica;
- funcionamento fanless;
- adequada para operação 24/7;
- não depende do FAN header;
- acesso ao GPIO através de extensor.

A própria case funcionará como dissipador térmico do Raspberry Pi.

---

## EDATEC ED-Pi5Case-HA-B

Kit de extensão utilizado para levar o GPIO de 40 pinos para cima da ED-Pi5Case-B.

Sua função será permitir a instalação do Radxa Quad SATA HAT acima da case.

```text
Radxa QF4
    │
GPIO estendido
    │
EDATEC ED-Pi5Case-B
    │
Raspberry Pi 5
```

---

## Radxa Quad SATA HAT QF4

Controladora responsável pelos discos SATA.

### Características relevantes

- 4 interfaces SATA;
- conexão de dados através de USB 3.0;
- não utiliza PCIe;
- adequada para SSDs/HDDs SATA;
- alimentação dos discos independente da interface de dados;
- montagem acima do Raspberry Pi através do GPIO.

### Caminho dos dados

```text
SSD/HDD
   │
 SATA
   │
Radxa QF4
   │
 USB 3
   │
Raspberry Pi 5
```

O GPIO não transporta os dados SATA. Ele é utilizado para integração física e funções auxiliares da placa.

---

## Armazenamento

O projeto suportará até quatro unidades SATA.

Configuração ainda não definida:

```text
SATA 1
SATA 2
SATA 3
SATA 4
```

Serão avaliados:

- SSD SATA de 2,5";
- HDD SATA de 2,5";
- HDD SATA de 3,5".

A escolha final levará em conta:

- capacidade;
- consumo;
- temperatura;
- ruído;
- confiabilidade;
- custo;
- alimentação necessária.

---

## Alimentação

A alimentação dos discos não deverá depender exclusivamente do Raspberry Pi.

Especialmente caso HDDs mecânicos sejam utilizados, será adotada alimentação capaz de suportar os picos de corrente durante a partida dos discos.

O projeto da fonte será definido após a escolha definitiva dos dispositivos SATA.

---

## Refrigeração

### Raspberry Pi

Refrigeração:

**100% passiva**

Responsável:

**EDATEC ED-Pi5Case-B**

### Discos

A necessidade de ventilação será determinada pelo tipo e quantidade de unidades instaladas.

SSDs podem permitir uma construção praticamente fanless.

HDDs de 3,5" poderão exigir fluxo de ar dedicado dentro da case NAS.

---

## Case NAS

A estrutura externa será projetada especificamente para este projeto e produzida através de impressão 3D.

Conceito inicial:

```text
┌────────────────────────┐
│       SATA Drive       │
├────────────────────────┤
│       SATA Drive       │
├────────────────────────┤
│       SATA Drive       │
├────────────────────────┤
│       SATA Drive       │
├────────────────────────┤
│   Radxa Quad SATA QF4  │
├────────────────────────┤
│      GPIO Extender     │
├────────────────────────┤
│ EDATEC + Raspberry Pi  │
└────────────────────────┘
```

O projeto deverá priorizar:

- acesso fácil aos discos;
- fluxo de ar;
- organização;
- manutenção;
- baixo ruído;
- dimensões compactas.