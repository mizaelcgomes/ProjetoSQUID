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
acl bloqueio_bets_adultos url_regex -i "/etc/squid/bloqueio_bets.txt"
http_access deny bloqueio_bets_adultos
http_access allow all
```

### Observações:
1. Crie o arquivo `bloqueio_bets.txt` com um URL por linha (exemplo do arquivo):
   ```
   .apostas.
   .bet.
   .adulto.
   /conteudo-proibido/
   ```
2. A opção `-i` torna a regex case-insensitive (ignora maiúsculas/minúsculas).
3. A ordem das regras é crítica: **sempre negue antes de permitir**.
