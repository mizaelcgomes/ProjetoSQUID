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
## 🔒 Bloqueio de Sites de Apostas/Adultos(ACL)

Adicione no `squid.conf` a partir da linha 1544 para bloquear URLs listadas em um arquivo externo:

```squidconf
acl bloqueio_bets_adultos url_regex -i "/etc/squid/bloqueio_bets_adultos.txt"
http_access deny bloqueio_bets_adultos
http_access allow all
```
### Observações:
Crie o arquivo `bloqueio_bets_adultos.txt` com um URL por linha no diretório `/etc/squid/` do servidor.:
   ```
   [Lista dos Sites](https://dontpad.com/bloqueio_sites_bets_adultos).
   ```
⚠️ **Requisito**: Sempre que adicionar um ACL, reiniciar o servidor usando o comando `sudo systemctl restart squid`.

## 🚫 Página Personalizada de Acesso Bloqueado

Adicione no `squid.conf` na linha onde há escrito `error_directory`(Utilize `Ctrl` + `W` para procurar):

```squidconf
# Define o diretório de erros em Português
error_directory /usr/share/squid/errors/Portuguese

# Redireciona para uma página web customizada ao bloquear uma ACL
deny_info https://www.acesso-proibido.netlify.app bloqueio_bets_adultos
```
⚠️ **Requisito**: Sempre que adicionar um ACL, reiniciar o servidor usando o comando `sudo systemctl restart squid`.
