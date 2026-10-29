# Roadmap

## Concluído

- [x] OpenBSD instalado e iniciando sozinho no microSD
- [x] OpenSSH só por chave
- [x] `doas`
- [x] PF com bloqueio por padrão
- [x] Upgrade para OpenBSD 8.0 + correções
- [x] Ajustes para operação 24/7
- [x] Backups do sistema fora do Pi

## Próximas etapas

1. **Definir e testar o armazenamento**
   - comprar **um** gabinete/controladora e **um** disco primeiro;
   - verificar no `dmesg` (`umass` → `sdN`), escrita sustentada, religamento e SMART (`smartctl -d sat`);
   - medir a temperatura do Pi sob carga;
   - só então comprar o restante.
2. **Discos**: FFS2 sobre softraid RAID1, montado em `/srv/nas` pelo DUID, com `noatime,nodev,nosuid`.
3. **Compartilhamento**: Samba (SMB3, só na interface `cad0`) + regra no PF; NFS opcional para clientes Linux.
4. **Monitoramento**: `/etc/daily.local` com `bioctl`, `df` e SMART; relatório por e-mail local.
5. **Backups dos dados** (RAID ≠ backup): destino a definir; o HD TOSHIBA do desktop **não** será usado.
6. **Testes de aceitação**: reinício, queda de energia, remoção de um disco e reconstrução do RAID, restauração de arquivo.
7. **Case definitiva**: case passiva e/ou case 3D (ver [case/README.md](../case/README.md)).
8. **Repositório**: licença e idioma da documentação.

## Decisões em aberto

- Hardware de armazenamento (ver [HARDWARE.md](HARDWARE.md)).
- Capacidade necessária.
- Destino dos backups dos dados.
- Licença do repositório.
