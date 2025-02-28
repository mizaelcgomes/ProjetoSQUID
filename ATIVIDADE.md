# ATIVIDADE SQUID 🦑

### 1. 🔧 **Instalação e Configuração**  
   - Instalar e configurar o Squid no sistema.
   - Editar o arquivo de configuração principal: `/etc/squid/squid.conf`. 

### 2. 🔒 **Bloqueio de Domínios Específicos** 
   - Adicionar uma ACL para bloquear sites com domínios `.gov.br`, `.jus.br` e `.ufc.br`.
   - Criar arquivo `sudo nano /etc/squid/bloqueio_site.txt`
## Observações:
Adicione no `squid.conf` abaixo da linha `include /etc/squid/conf.d/*.conf` (utilize `Ctrl` + `W` para procurar) para bloquear URLs listadas em um arquivo externo:

```squidconf
acl bloqueio_bets_adultos url_regex -i "/etc/squid/bloqueio_bets_adultos.txt"
http_access deny bloqueio_bets_adultos
http_access allow all
```

### 3. 🚫 **Página de Acesso Restrito**
   - Configurar uma página personalizada de acesso bloqueado usando a URL: [https://acessorestrito-squid.netlify.app/](https://acessorestrito-squid.netlify.app/). 🌐  

### 4. 📝 **Registro de Tentativas de Acesso Bloqueado**  
   - Criar um formato personalizado de log com a seguinte estrutura:  
     - `[%{%Y-%m-%d %H:%M:%S}tl]`: Data e hora do acesso 
     - `%>a`: Endereço IP do cliente
     - `%un`: Nome do usuário autenticado  
     - `%ru`: URL solicitada
     - `%{Referer}>h`: Referência (site de origem) da solicitação
     - `%{User-Agent}>h`: Agente do usuário (navegador)

### 5. 🌐 **Configuração do Proxy no Navegador**   
   - Configurar o proxy no navegador utilizando o IP do servidor onde o Squid está instalado e a porta padrão `3128`.  
     - **Endereço do proxy**: Insira o IP do servidor.
     - **Porta**: `3128`.

### 6. ⚡ **Reinicialização do Serviço**
   - Sempre que fizer alterações no arquivo `squid.conf`, reiniciar o servidor usando o comando:  
     ```bash
     sudo systemctl restart squid
     ```
     
