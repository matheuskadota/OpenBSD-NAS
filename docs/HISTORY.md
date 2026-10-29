# Histórico

Registro cronológico do projeto. Datas em 2026.

## Agosto — planejamento

- **14/08** — criação do repositório com o planejamento inicial: Raspberry Pi 5, OpenBSD, OpenSSH, Radxa Quad SATA HAT QF4 e case passiva EDATEC.
- Pesquisa de alternativas de armazenamento, incluindo gabinete USB multibaias com fonte própria (ex.: ORICO WS400C3).

## Setembro — a saga do boot

- **12/09** — montagem parcial de uma ponte serial com Arduino UNO e divisor resistivo, para acompanhar o console serial do Pi.
- **16/09** — criação de um `uboot.env` com `bootdelay=-2`, para que o U-Boot inicie o boot imediatamente, sem possibilidade de interrupção. Hipótese: ruído no RX da serial interrompia o autoboot.
- **19/09** — instalador do OpenBSD 7.9 funcionando no Pi 5. No mesmo dia, um U-Boot 2025.07 foi compilado localmente com um patch de PCIe; esse build **não** foi usado (o patch só afeta o Pi 4).

## Outubro — sistema instalado

- **14/10** — OpenBSD 7.9/arm64 instalado no microSD de 64 GB e iniciando sozinho. Console movido para o HDMI com `set tty fb0` em `/etc/boot.conf`.
- Instalação: disco inteiro, layout automático, sets via HTTP; hostname `bsd`, usuário `matt`, teclado `br`, rede `cad0` por DHCP, fuso `America/Sao_Paulo`.
- Ambiente gráfico mínimo e customização leve (xenodm, Conky, Firefox).
- Wi-Fi não funcionou no instalador (`sdmmc1: can't supply bus power`); não é necessário para o NAS.

## 29/10 — análise e base do servidor

- **Análise técnica** das mídias do projeto (somente leitura): imagens oficiais íntegras, partição `BOOT` idêntica à oficial exceto pelo `uboot.env` (ver [BOOT.md](BOOT.md)).
- **OpenSSH**: chave Ed25519 do desktop instalada; login só por chave; senha, keyboard-interactive e root bloqueados; `AllowUsers matt`.
- **`doas`** configurado para o grupo `wheel`.
- **PF**: entrada bloqueada por padrão; SSH e ping só a partir da rede local.
- **Imagem completa do microSD** (OpenBSD 7.9) gerada no desktop: 64 GB → 3,2 GB com zstd, verificada.
- **Upgrade para OpenBSD 8.0** com `sysupgrade`, seguido de `sysmerge`, `pkg_add -u` e `syspatch`. O boot continuou funcionando com o U-Boot novo e o `uboot.env` antigo.
- **Ajustes para 24/7**: `noatime` em todas as partições; `xenodm`, `slaacd` e `sndiod` fora do boot; GUI disponível sob demanda com `startx`.
- **Backup de `/etc` e `/home`** do 8.0 copiado para o desktop.
- **Temperatura** em repouso, na case oficial sem ventoinha: ~63 °C (`hw.sensors.bcmtmon0.temp0`).
- **Hardware**: nada de armazenamento comprado ainda. A QF4 foi reavaliada como o maior risco do projeto (ver [HARDWARE.md](HARDWARE.md)).
