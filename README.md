# Comandos do Projeto Squid 🦑

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

## 🔄 Exemplo de Regra de ACL
Adicione no `squid.conf`:
```conf
acl minha_rede src 192.168.1.0/24
http_access allow minha_rede
```
