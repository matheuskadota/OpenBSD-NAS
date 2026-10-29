# Configuração aplicada

Configuração em vigor no servidor `bsd` (OpenBSD 8.0/arm64). Comandos executados a partir do desktop, via SSH.

## OpenSSH

Chave dedicada no desktop (`~/.ssh/id_ed25519_bsd`) com entrada `Host bsd` em `~/.ssh/config`, instalada com `ssh-copy-id`.

Bloco no **topo** de `/etc/ssh/sshd_config` (no `sshd_config` vale o primeiro valor encontrado):

```text
PermitRootLogin no
PasswordAuthentication no
KbdInteractiveAuthentication no
AllowUsers matt
```

`KbdInteractiveAuthentication no` é necessário no OpenBSD: sem ele, a senha continua sendo aceita pelo método keyboard-interactive (BSD Auth).

Verificação: `doas sshd -T | grep -Ei '^(permitrootlogin|passwordauthentication|kbdinteractiveauthentication|allowusers) '`.

## doas

`/etc/doas.conf`:

```text
permit persist :wheel
```

## PF

`/etc/pf.conf`:

```text
lan = "cad0"
set skip on lo
block return
pass out
block return out log proto { tcp udp } user _pbuild
pass in on $lan inet6 proto icmp6 icmp6-type { neighbrsol neighbradv routeradv }
pass in on $lan inet proto icmp from ($lan:network) icmp-type echoreq
pass in on $lan proto tcp from ($lan:network) to ($lan) port ssh
```

- `($lan:network)` acompanha a sub-rede entregue pelo DHCP.
- O bloqueio padrão não usa `log`, para não gravar ruído de broadcast no microSD.
- Novos serviços (ex.: Samba, porta 445) recebem uma regra `pass in` própria, restrita a `($lan:network)`.

## Operação 24/7

- `noatime` em todas as partições FFS de `/etc/fstab` (reduz escritas no microSD).
- Fora do boot: `xenodm` (GUI), `slaacd` (sem IPv6), `sndiod` (sem áudio).
- GUI sob demanda: login no console e `startx` (`~/.xinitrc` → `~/.xsession`).
- IP reservado no roteador para o MAC de `cad0`.

## Backups do sistema

Guardados no disco do desktop, nunca no próprio Pi:

- imagem completa do microSD (`dd` + `zstd`) antes de cada upgrade;
- cópia avulsa da partição `BOOT`;
- `tar` de `/etc` e `/home/matt` (contém as chaves de host do SSH — não compartilhar).

## Atualizações

- Correções: `doas syspatch`, e `doas reboot` se o kernel mudar.
- Pacotes: `doas pkg_add -u`.
- Versão: `doas sysupgrade` a cada release, sempre depois de uma imagem do microSD.
