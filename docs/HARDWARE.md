# Hardware

Componentes e decisões de hardware do **OpenBSD-NAS**.

## Em uso

| Componente | Detalhe |
|---|---|
| Raspberry Pi 5 | 8 GB de RAM |
| Sistema | microSD SanDisk 64 GB, slot interno |
| Case | case oficial do Raspberry Pi 5, sem ventoinha |
| Fonte | fonte oficial do Raspberry Pi 5 |
| Rede | Gigabit Ethernet (`cad0`) |

### Interfaces indisponíveis

O Raspberry Pi 5 utilizado tem danos físicos em:

- **FAN header de 4 pinos** — o cooler da case oficial não funciona;
- **conector/trava PCIe FFC externo** — sem HATs PCIe/NVMe.

USB, Ethernet e o slot do microSD funcionam normalmente. No Pi 5, USB e Ethernet passam pelo chip RP1, ligado por um PCIe **interno**, que não é afetado.

### Temperatura

Em repouso, na case oficial fechada e sem ventoinha: **~63 °C** (`sysctl hw.sensors`). O firmware reduz o clock perto de 85 °C. Ainda falta medir sob carga, com discos e transferências pela rede.

---

## Armazenamento — em definição

Nenhum hardware de armazenamento foi comprado. Opções avaliadas:

### Opção A — discos USB independentes (preferida para começar)

- 2× SSD SATA de 2,5" em 2 gabinetes USB 3.0 com ponte **ASMedia** (ASM1153E / ASM235CM);
- RAID1 por software (softraid) no OpenBSD;
- alimentados pelo próprio Pi: com a fonte oficial reconhecida, o Pi 5 libera 1,6 A nas portas USB, suficiente para dois SSDs;
- silencioso, frio, sem fonte extra;
- custo por TB maior que HDD.

### Opção B — gabinete USB multibaias com fonte própria

- ex.: ORICO WS400C3, com HDDs de 3,5";
- mais capacidade por custo;
- mais ruído e consumo; a ponte USB do gabinete é um ponto único de falha.

### Radxa Quad SATA HAT QF4 — rebaixada

Era a controladora planejada originalmente. Riscos encontrados:

- feita para o **Raspberry Pi 4**; o Pi 5 não aparece como suportado e o produto está descontinuado/arquivado;
- alimenta o Pi pelos 5 V do GPIO; sem fonte de 5 V/5 A detectada, o Pi 5 limita as portas USB a 600 mA e desativa o boot por USB;
- usar a fonte do Pi e os 5 V da placa ao mesmo tempo é um risco elétrico;
- ventoinha, OLED e linhas de reset dependem de um serviço só para Linux;
- não há relatos de uso em BSD.

Só volta a ser considerada se passar no teste abaixo.

### Teste antes de comprar o restante

1. Comprar **um** gabinete/controladora e **um** disco.
2. Conectar com o Pi já no OpenBSD e conferir o `dmesg` (`umass` → `sdN`).
3. Testar escrita sustentada e religamento.
4. Testar SMART pela ponte USB (`smartctl -d sat`).
5. Medir a temperatura do Pi sob carga.
6. Só então comprar o restante.

### Desempenho esperado

O OpenBSD usa discos USB pelo `umass` (sem UAS), com algo entre 40 e 100 MB/s. A rede Gigabit limita a cerca de 110 MB/s, então o gargalo é a rede de qualquer forma.

---

## Case

- **EDATEC ED-Pi5Case-B** (case passiva de alumínio) continua sendo uma opção para a refrigeração passiva definitiva.
- O extensor GPIO **ED-Pi5Case-HA-B** só é necessário com um HAT no GPIO, como a QF4.
- A case 3D será projetada depois da escolha do armazenamento (ver [case/README.md](../case/README.md)).
