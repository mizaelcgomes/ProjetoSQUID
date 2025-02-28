# REQUISITOS DO PROJETO SQUID 🦑

Para executar este projeto, são necessários os seguintes requisitos:

💻 **Sistema Operacional:**
- [Debian 12.9.0](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-12.9.0-amd64-netinst.iso)

⚙️ **Hardware Recomendado:**
- CPU: 2 núcleos (recomendado)
- RAM: 4GB (recomendado)
- Armazenamento: 20GB (recomendado)

🌐 **Conectividade:**
- Acesso à internet para instalação e atualizações (Placa de Rede em Modo Bridge)

🔑 **Permissões:**
- Acesso root ou usuário com permissões sudo. 

🌐 **Software:**
- Squid: Versão 6.13 (`apt install squid`)
- Serviço systemd (`apt install systemd`)
- Editor de texto (`apt install nano`)

📝 **Dependências:**
- Diretório de logs configurado (`/var/log/squid/`)
- Arquivos de configuração acessíveis (`/etc/squid/squid.conf`)

---

## 🔧 Instalação e Configuração Básica

**Instalar o Squid no Ubuntu/Debian:**
```bash
sudo apt update && sudo apt install squid
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
## 🌐 Configuração do Proxy nos Navegadores

Para que o Squid funcione corretamente, é necessário configurar o proxy nos navegadores dos dispositivos que utilizarão o servidor proxy. Abaixo estão as instruções para os navegadores mais comuns:

### **Google Chrome**
1. Abra as **Configurações** do navegador.
2. Vá para **Sistema** > **Configurações de proxy**.
3. Na janela que abrir, configure o proxy manualmente:
   - **Endereço do proxy**: Insira o IP do servidor onde o Squid está instalado.
   - **Porta**: `3128` (ou outra porta configurada no Squid).
4. Salve as alterações.

### **Mozilla Firefox**
1. Abra o menu (três barras no canto superior direito) e vá para **Configurações**.
2. Role até a seção **Rede e Internet** e clique em **Configurações de rede**.
3. Selecione **Configuração manual de proxy**.
   - **Endereço do proxy HTTP**: Insira o IP do servidor onde o Squid está instalado.
   - **Porta**: `3128`.
4. Marque a opção **Usar este servidor proxy para todos os protocolos**.
5. Clique em **OK** para salvar.

### **Outros Navegadores**
Se você estiver usando outro navegador, geralmente a configuração do proxy pode ser feita nas **Configurações de Rede** do navegador. Basta seguir os mesmos princípios:
- **Endereço do proxy**: IP do servidor Squid.
- **Porta**: `3128`.

---
## 🔒 Bloqueio de Sites de Apostas/Adultos(ACL)

Adicione no `squid.conf` abaixo da linha `include /etc/squid/conf.d/*.conf` (utilize `Ctrl` + `W` para procurar) para bloquear URLs listadas em um arquivo externo:

```squidconf
acl bloqueio_bets_adultos url_regex -i "/etc/squid/bloqueio_bets_adultos.txt"
http_access deny bloqueio_bets_adultos
http_access allow all
```
### Observações:
Baixe o arquivo no repositório ou crie o arquivo `bloqueio_bets_adultos.txt` com um URL por linha no diretório `/etc/squid/` do servidor: [Lista dos sites bloqueados](https://dontpad.com/bloqueio_sites_bets_adultos).

- Criar arquivo e inserir as URLs `sudo nano /etc/squid/bloqueio_bets_adultos.txt`
- Ative a permissão do arquivo com `sudo chmod 640 /etc/squid/bloqueio_bets_adultos.txt`

⚠️ **Requisito**: Sempre que fizer alteração, reiniciar o servidor usando o comando `sudo systemctl restart squid`.

---
---
## 🚫 Página Personalizada de Acesso Bloqueado

Adicione no `squid.conf` abaixo da linha onde há escrito `error_directory`(Utilize `Ctrl` + `W` para procurar):

```squidconf
# Define o diretório de erros em Português
error_directory /usr/share/squid/errors/Portuguese

# Redireciona para uma página web customizada ao bloquear uma ACL
deny_info https://www.acesso-proibido.netlify.app acl
```
⚠️ **Requisito**: Sempre que fizer alteração, reiniciar o servidor usando o comando `sudo systemctl restart squid`.

---
---
## 📝 Registro de Tentativas de Acessos Bloqueados

Adicione no `squid.conf` abaixo da linha `http_port 3128` (utilize `Ctrl` + `W` para procurar)
```squidconf
# Formato personalizado para logs de bloqueio
logformat logblock %>a %ui %un [%{%Y-%m-%d %H:%M:%S}tl] "%rm %ru HTTP/%rv" %Hs %<st "%{Referer}>h" "%{User-Agent}>h"

# Arquivo dedicado para registrar acessos bloqueados
access_log /var/log/squid/log_acblock logblock
```

Comandos para Implementação no Servidor:
```bash
# Criar arquivo de log e ajustar permissões
sudo touch /var/log/squid/log_acblock
sudo chown proxy:proxy /var/log/squid/log_acblock
sudo chmod 640 /var/log/squid/log_acblock
```
⚠️ **Requisito**: Sempre que fizer alteração, reiniciar o servidor usando o comando `sudo systemctl restart squid`.

Comando para Monitoramento dos Logs:
```bash
# Verificar tentativas de acesso bloqueado em tempo real
tail -f /var/log/squid/log_acblock
```


### Observações:
Estrutura do Log:
   - `%>a:` Endereço IP do cliente
   - `%ui:` Identificação do usuário (se disponível)
   - `%un:` Nome do usuário autenticado
   - `%tl:` Data e hora do acesso
   - `%rm:` Método HTTP (GET, POST, etc.)
   - `%ru:` URL solicitada
   - `%rv:` Versão do protocolo HTTP
   - `%Hs:` Código de status HTTP retornado pelo Squid
   - `%<st:` Tamanho da resposta enviada ao cliente
   - `%{Referer}>h:` Referência (site de origem) da solicitação
   - `%{User -Agent}>h:` Agente do usuário (navegador)

---
