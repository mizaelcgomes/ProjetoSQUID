# ATIVIDADE SQUID 🦑

### 1. 🔧 **Instalação e Configuração**  
   - Instalar e configurar o Squid no sistema.
   - Editar o arquivo de configuração principal: `/etc/squid/squid.conf`. 

### 2. 🔒 **Bloqueio de Domínios Específicos** 
   - Adicionar uma ACL para bloquear sites com domínios `.gov.br`, `.jus.br` e `.ufc.br`.
   - Criar arquivo `sudo nano /etc/squid/bloqueio_site.txt`

**EXEMPLO:**
Adicione no `squid.conf` abaixo da linha `include /etc/squid/conf.d/*.conf` (utilize `Ctrl` + `W` para procurar) para bloquear URLs listadas em um arquivo externo:

```squidconf
acl bloqueio_bets_adultos url_regex -i "/etc/squid/bloqueio_bets_adultos.txt"
http_access deny bloqueio_bets_adultos
http_access allow all
```
- Criar arquivo e inserir as URLs `sudo nano /etc/squid/bloqueio_bets_adultos.txt`
- Ative a permissão do arquivo com `sudo chmod 640 /etc/squid/bloqueio_bets_adultos.txt`

### 3. 📝 **Registro de Tentativas de Acesso Bloqueado**  
   - Criar um formato personalizado de log com a seguinte estrutura:  
     - `[%{%Y-%m-%d %H:%M:%S}tl]`: Data e hora do acesso 
     - `%>a`: Endereço IP do cliente
     - `%un`: Nome do usuário autenticado  
     - `%ru`: URL solicitada
     - `%{Referer}>h`: Referência (site de origem) da solicitação
     - `%{User-Agent}>h`: Agente do usuário (navegador)

**EXEMPLO:**
Adicione no `squid.conf` abaixo da linha `http_port 3128` (utilize `Ctrl` + `W` para procurar)
```squidconf
# Formato personalizado para logs de bloqueio
logformat logblock [%{%Y-%m-%d %H:%M:%S}tl] %>a %un %ru %<st "%{Referer}>h" "%{User-Agent}>h"

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


### 4. 🌐 **Configuração do Proxy no Navegador**   
   - Configurar o proxy no navegador utilizando o IP do servidor onde o Squid está instalado e a porta padrão `3128`.  
     - **Endereço do proxy**: Insira o IP do servidor.
     - **Porta**: `3128`.

**EXEMPLO:**
**Mozilla Firefox**
1. Abra o menu (três barras no canto superior direito) e vá para **Configurações**.
2. Role até a seção **Rede e Internet** e clique em **Configurações de rede**.
3. Selecione **Configuração manual de proxy**.
   - **Endereço do proxy HTTP**: Insira o IP do servidor onde o Squid está instalado.
   - **Porta**: `3128`.
4. Marque a opção **Usar este servidor proxy para todos os protocolos**.
5. Clique em **OK** para salvar.

### 5. ⚡ **Reinicialização do Serviço**
   - Sempre que fizer alterações no arquivo `squid.conf`, reiniciar o servidor usando o comando:  
     ```bash
     sudo systemctl restart squid
     ```
     
