# COMANDOS DO PROJETO SQUID 🦑

Comando necessários para dar seguimento ao projeto.

---

## 🔧 Instalação e Configuração Básica

**Instalar o Squid no Ubuntu/Debian:**
```bash
sudo apt update && sudo apt install squid -y
```

**Editar o arquivo de configuração principal:**
```bash
sudo nano /etc/squid/squid.conf
```

---

## 🚀 Comandos de Gerenciamento do Serviço

**Iniciar o Squid:**
```bash
sudo systemctl start squid
```

**Reiniciar após alterações:**
```bash
sudo systemctl restart squid
```

**Verificar status:**
```bash
sudo systemctl status squid
```

---
🔒 Bloqueio de Sites de Apostas/Adultos(ACL)

Adicione no `squid.conf` para bloquear URLs listadas em um arquivo externo:

```squidconf
# Bloqueia domínios listados em /etc/squid/bloqueio_bets.txt
acl bloqueio_bets_adultos url_regex -i "/etc/squid/bloqueio_bets_adultos.txt"
http_access deny bloqueio_bets_adultos
http_access allow all
```
### Observações:
Crie o arquivo `bloqueio_bets.txt` com um URL por linha no diretório `/etc/squid/` do servidor.:
   ```
   SITES NO LINK: https://dontpad.com/bloqueio_sites_bets_adultos
   ```
