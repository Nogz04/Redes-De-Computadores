<h2 align="center">🎉 TRABALHO DE MALUCO 🎉</h2>
<p align="center"></p>

### 📚 Resumo
> Este trabalho demonstra um passo-a-passo de como configurar um PC Linux para atuar como roteador e controlar os acessos de um PC Windows. Utilizando ferramentas como SSH, Apache2, SQUID e IP Tables, o objetivo é restringir o acesso a determinados sites no Windows por meio de um servidor proxy configurado no Linux. Além disso, o trabalho envolve a criação de sub-interfaces de rede, configuração de rotas, e bloqueio de palavras e sites específicos via proxy, redirecionando o usuário a uma página personalizada criada no Apache2.
---

### 🗂️ Sumário
1. [👥 Integrantes do Grupo](#-integrantes-do-grupo)
2. [🚀 Iniciando no Linux](#-iniciando-no-linux)
   - [🗺️ Planejar as Redes](#-planejar-as-redes)
   - [🔑 Instalar o SSH no Linux](#-instalar-o-ssh-no-linux)
   - [🌐 Instalar o Apache 2 no Linux](#-instalar-o-apache-2-no-linux)
   - [📡 IP Tables no Linux](#-ip-tables-no-linux)
   - [🌍 Criar Sub-interfaces no Linux](#-criar-sub-interfaces-no-linux)
   - [🔄 Configurar Rotas](#-configurar-rotas)
   - [🚫 Bloquear Sites com Proxy](#-bloquear-sites-com-proxy)
3. [💻 No Windows 11](#-no-windows-11)


---

### 👥 Integrantes do Grupo
- **Grupo 5:** Matheus Nogueira Albuquerque, Bruno Difante e Anthony Guedes <br><br>

**🛠️ FERRAMENTAS UTILIZADAS:** <br><br>
- SSH, Linux, Windows, Apache 2, Rotas, Sub-interface e Proxy (SQUID e IP TABLES) <br> <br>
![image](https://github.com/user-attachments/assets/af91417f-d6e5-4e4d-9154-84681caba696)


---

### 🚀 INICIANDO NO LINUX:
## 🔍 Passo a Passo
1. **🗺️ Planejar as redes**
   - Definir a topologia de rede, incluindo dispositivos e conexões
      - **LAN:** 192.168.1.0/29
      - **WAN:** 200.10.10.0/30
  
   -  Definir a topologia de rede, incluindo dispositivos e conexões
      - **LAN:** 192.168.1.32/29
      - **WAN:** 200.10.10.16/30
    
   ![image](https://github.com/user-attachments/assets/c2c167ea-b8b4-4fdf-8cd3-14f8f1e3892a)


2. **🔑 Instalar o SSH no Linux**
   - Para instalação, siga as orientações abaixo:
     ```bash
     sudo apt-get update
     sudo apt-get install openssh-client
     ```

   - Criar usuário:
     ```bash
     sudo adduser username

     Ex: sudo adduser Grupo5
     ```

   - Adicionar usuário na lista do SUDO:
     ```bash
     sudo usermod -aG sudo username

     Ex: sudo usermod -aG sudo Grupo5
     ```

   - Entrar como super usuário:
     ```bash
     sudo su
     ```

   - Logar usuário / Mudar usuário:
     ```bash
     sudo su username

     Ex: sudo su Grupo5
     ```

3. **🌐 Instalar o Apache 2 no Linux**
   - Para instalação, siga as orientações abaixo:
     ```bash
     sudo apt update
     sudo apt install apache2
     ```

   - Inicie o serviço:
     ```bash
     sudo systemctl start apache2
     ```

   - Criando a página:
     ```bash
     sudo nano /var/www/html/grupo5.html
     ```

   - O conteúdo da página:
     ```html
     <!DOCTYPE html>
     <html lang="pt-BR">
     <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Página do Grupo 5</title>
     </head>
     <body>
         <h1>Bem-vindo à nossa página, UFNers!</h1>
         <p>Esta é uma página criada com Apache2.</p>
     </body>
     </html>
     ```
   
   

   
   - Para salvar e sair da "criação html":
     ```bash
     Ctrl + O e Enter
     depois
     Crtl + X
     ```

      - Ficará assim: <br>
      ![image](https://github.com/user-attachments/assets/a69179bf-7741-48e5-a524-86b30d910155)

   - Configurando permissões:
     ```bash
     sudo chown -R www-data:www-data /var/www/html/
     sudo chmod -R 755 /var/www/html/
     ```

   - (Opcional) Habilitar no firewall:
     ```bash
     sudo ufw allow 'Apache'
     ```

5. **📡 IP TABLES no Linux**
   - Instalação:
     ```bash
     sudo apt install iptables
     sudo apt install iptables-persistent
     sudo systemctl enable netfilter-persistent
     ```

6. **🌍 Criar Sub-interfaces no Linux**
   - Primeiramente, instale o net-tools:
     ```bash
     sudo apt install net-tools
     ```

   - Mostrar roteador:
     ```bash
     sudo ifconfig
     ```

   - Adicionar a sub-interface (O IP será diferente conforme o grupo):
     ```bash
     sudo ifconfig enp0s31f6:0 192.168.1.33 netmask 255.255.255.248
     ```

7. **🔄 Configurar Rotas**
   - Exibir rotas:
     ```bash
     sudo route
     ```

8. **🚫 Bloquear sites com Proxy**
   - Baixar o SQUID:
     ```bash
     sudo apt-get install squid
     ```

   - Verificar a instalação:
     ```bash
     sudo service squid status
     ```

   - Configurar o SQUID:
     ```bash
     cd /etc/squid
     ```

   - Criar arquivo para os sites bloqueados:
     ```bash
     sudo touch /etc/squid/sites_bloqueados.txt
     ```

   - Entrar no arquivo dos sites bloqueados:
     ```bash
     sudo nano /etc/squid/sites_bloqueados.txt

     Coloque os sites que deseja bloquear, coloque . antes.
     Ex: .G1.com
         .iffarroupilha.edu.br
     ```

     - Criar arquivo para as palavras bloqueadas:
     ```bash
     sudo touch /etc/squid/palavras_bloqueadas.txt
     ```

   - Entrar no arquivo das palavras bloqueadas:
     ```bash
     sudo nano /etc/squid/palavras_bloqueadas.txt

     Coloque as palavras que deseja bloquear
     Ex: games
         movies
     
     ```

   - Configurações do SQUID:
     ```bash
     # Define a porta do proxy
     http_port 3128

     # Bloqueia o acesso aos sites que estão no arquivo "sites_bloqueados.txt"
     acl sites_bloqueados url_regex -i "/etc/squid/sites_bloqueados.txt"
     http_access deny sites_bloqueados

     # Bloqueia o acesso aos sites que contém as palavras que estão no arquivo "palavras_bloqueadas.txt"
     acl palavras_bloqueadas url_regex -i "/etc/squid/palavras_bloqueadas.txt"
     http_access deny palavras_bloqueadas

     #Redireciona para o site criado com Apache2:
     deny_info http://192.168.1.33/grupo5.html sites_bloqueados
     deny_info http://192.168.1.33/grupo5.html palavras_bloqueadas
     ```

   - Fazer backup do SQUID:
     ```bash
     sudo cp squid.conf squid.conf.backup
     ```

   - Apagar o SQUID e criar novo:
     ```bash
     sudo rm squid.conf
     sudo nano squid.conf
     ```

   - Reiniciar SQUID (Qualquer modificação deve ser reiniciada):
     ```bash
     sudo systemctl stop squid
     sudo systemctl start squid
     ```

9. **💻 No Windowns 11 agora:**
   - Abra as configurações.
      - Pesquise na barra de pesquisa "Proxy" e vá em "Configurações de proxy.
         - Vá em usar um servidor proxy e mude para "Ativado".
           - Coloque o IP novo (192.168.1.33) criado pela sub-interface e coloque a porta (3128) que foi colocada no squid.conf
             
   - Após isso pesquisa "Painel de Controle".
      - Vá em "Rede e internet" --> Central de Rede e Compartilhamento
      - Clique em "Ethernet" e vá em "Propriedades" --> "Protocolo IP Versão 4 (TCP/IPv4) e coloque as seguintes configurações, com base no seu IP: 
        - ![image](https://github.com/user-attachments/assets/dc27daa3-5630-4519-a892-a718935e187d)
           - O "8.8.8.8" é o DNS do Google, coloque igual e vá em "OK". <br> <br>
           
   Após isso, já que o IP e a Porta já esta nas configurações do Proxy e as configurações do IPv4 configuradas, tente abrir um site bloqueado pelo Windows, ele não deve abrir o site e o usuário deve ser redirecionado para a página criada com o Apache2.
     
#NOSSO GRUPO FICOU ATÉ O FINAL DA AULA FINAL DO TRABALHO.

