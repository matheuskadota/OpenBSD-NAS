# Cadeia de boot

Como o Raspberry Pi 5 inicia o OpenBSD neste projeto.

```text
Raspberry Pi 5
   │
   ▼
Bootloader na EEPROM ──── modo "SD Card Boot"
   │
   ▼
config.txt (partição FAT "BOOT")
   │   arm_64bit=1 · enable_uart=1 · dtoverlay=disable-bt · kernel=u-boot.bin
   ▼
u-boot.bin ─────────────── U-Boot fornecido pelo OpenBSD
   │   uboot.env: bootdelay=-2 → autoboot imediato, sem interrupção
   │   bootcmd=bootflow scan
   ▼
efi/boot/bootaa64.efi ──── carregador EFI do OpenBSD
   │   /etc/boot.conf: set tty fb0 → console no HDMI
   ▼
Kernel OpenBSD 8.0/arm64
```

## Mídia

- microSD SanDisk de 64 GB no slot interno.
- Partição 1: FAT `BOOT` (16 MiB; sistema de arquivos de 8 MB, ~90% ocupado).
- Partição 4: OpenBSD (disklabel com `/`, swap, `/tmp`, `/var`, `/usr`, `/usr/X11R6`, `/usr/local`, `/usr/src`, `/usr/obj`, `/home`).

## Personalizações

| Item | Valor | Motivo |
|---|---|---|
| `uboot.env` | ambiente padrão do U-Boot com `bootdelay=-2` | impedir que ruído na serial ou teclas espúrias interrompam o autoboot |
| `/etc/boot.conf` | `set tty fb0` | o console padrão do OpenBSD no Pi é a serial; isto o leva para o HDMI |

Todo o resto da partição `BOOT` é idêntico à imagem oficial.

## Observações

- Com `bootdelay=-2`, o prompt do U-Boot só aparece se o boot falhar.
- O U-Boot carrega o `uboot.env` **no lugar** do ambiente padrão inteiro. Após o upgrade para o 8.0, o arquivo antigo continuou funcionando; se algum boot futuro falhar, regenerar o arquivo a partir do ambiente padrão do U-Boot atual.
- Upgrades de versão (`sysupgrade`) regravam firmware e U-Boot na partição `BOOT`, mas mantêm o `uboot.env`.
- Antes de qualquer upgrade, gerar uma imagem completa do microSD.

## Recuperação

1. Gravar a última imagem no cartão a partir do desktop:
   `zstd -dc bsd-AAAA-MM-DD.img.zst | sudo dd of=/dev/sdX bs=4M status=progress conv=fsync`
2. Se só a partição `BOOT` estiver danificada, restaurar os arquivos dela a partir da cópia avulsa feita junto com a imagem.
