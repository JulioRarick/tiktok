Aqui está um guia completo para instalar e configurar **Debian 12** como servidor em um desktop, incluindo SSH, SSL, IP fixo da universidade, Apache, configuração de domínio e a transferência do backup armazenado em um Ubuntu. Vou dividir o processo por etapas para facilitar a compreensão.

---

## **1. Preparação Inicial:**
Antes de instalar, garanta:
- **ISO do Debian 12:** Baixe do [site oficial](https://www.debian.org/distrib/).
- **Pendrive bootável:** Use `Rufus` (Windows) ou `dd` (Linux) para criar um pendrive de boot:
  ```bash
  sudo dd if=debian-12.iso of=/dev/sdX bs=4M status=progress && sync
  ```
  (Substitua `/dev/sdX` pelo nome correto do seu pendrive.)

- **Backup Pronto:** Certifique-se de que o backup do servidor antigo está acessível no Ubuntu.

---

## **2. Instalando Debian 12 no Desktop:**
1. **Boot pelo pendrive:** Insira o pendrive no desktop e selecione-o como dispositivo de boot.
2. **Instalação:**  
   Durante a instalação:
   - Escolha **servidor básico** ou apenas **Sistema mínimo** (instalaremos o necessário manualmente).
   - Crie um usuário **root** e um usuário comum.
   - Escolha **Manual Partitioning** e configure as partições (por exemplo: `/` e `/home` separadas).
   - **Conexão de rede:** Configure temporariamente com DHCP (vamos definir IP fixo depois).
3. **Grub:** Instale o **Grub** no disco principal.

Após a instalação, reinicie o sistema.

---

## **3. Configuração de Rede com IP Fixo (UnB):**
Você precisará do IP, gateway e DNS fornecidos pela universidade. Modifique o arquivo de configuração de rede:

```bash
sudo nano /etc/network/interfaces
```

Adicione as configurações abaixo:

```
auto enp0s3  # Altere para sua interface de rede
iface enp0s3 inet static
    address 192.168.x.x  # IP fornecido pela UnB
    netmask 255.255.255.0
    gateway 192.168.x.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

Reinicie a interface de rede:
```bash
sudo systemctl restart networking
```

---

## **4. Configuração do SSH:**
1. **Instale o SSH:**
   ```bash
   sudo apt update
   sudo apt install openssh-server
   ```

2. **Configure o SSH:**
   Edite o arquivo de configuração:
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```

   Alterações recomendadas:
   ```
   PermitRootLogin no
   PasswordAuthentication no
   AllowUsers seu_usuario
   ```

3. **Reinicie o serviço SSH:**
   ```bash
   sudo systemctl restart ssh
   ```

4. **Teste o acesso SSH:**  
   Do seu Ubuntu pessoal:
   ```bash
   ssh seu_usuario@192.168.x.x
   ```

---

## **5. Instalando Apache e Configurando Domínios:**
1. **Instale Apache:**
   ```bash
   sudo apt install apache2
   ```

2. **Habilite e inicie o Apache:**
   ```bash
   sudo systemctl enable apache2
   sudo systemctl start apache2
   ```

3. **Configure um domínio:**  
   Crie um Virtual Host para seu domínio:
   ```bash
   sudo nano /etc/apache2/sites-available/seu-dominio.conf
   ```

   Conteúdo do Virtual Host:
   ```
   <VirtualHost *:80>
       ServerName seu-dominio.com
       ServerAlias www.seu-dominio.com
       DocumentRoot /var/www/seu-dominio

       <Directory /var/www/seu-dominio>
           AllowOverride All
       </Directory>

       ErrorLog ${APACHE_LOG_DIR}/seu-dominio-error.log
       CustomLog ${APACHE_LOG_DIR}/seu-dominio-access.log combined
   </VirtualHost>
   ```

4. **Habilite o site e o módulo `rewrite`:**
   ```bash
   sudo a2ensite seu-dominio.conf
   sudo a2enmod rewrite
   sudo systemctl reload apache2
   ```

5. **Crie o diretório do site e adicione um `index.html`:**
   ```bash
   sudo mkdir /var/www/seu-dominio
   echo "<h1>Servidor Configurado</h1>" | sudo tee /var/www/seu-dominio/index.html
   ```

---

## **6. Configurando SSL com Let's Encrypt:**
1. **Instale o Certbot:**
   ```bash
   sudo apt install certbot python3-certbot-apache
   ```

2. **Obtenha e configure o certificado SSL:**
   ```bash
   sudo certbot --apache -d seu-dominio.com -d www.seu-dominio.com
   ```

3. **Teste a renovação automática:**
   ```bash
   sudo certbot renew --dry-run
   ```

---

## **7. Transferindo o Backup do Ubuntu para o Novo Servidor:**
1. **No Ubuntu:**  
   Crie um arquivo compactado do backup:
   ```bash
   tar -czvf backup_servidor.tar.gz /caminho/do/backup
   ```

2. **Transfira o backup para o Debian (via SCP):**
   ```bash
   scp backup_servidor.tar.gz seu_usuario@192.168.x.x:/home/seu_usuario/
   ```

3. **No Debian:**  
   Extraia o backup no local apropriado:
   ```bash
   tar -xzvf /home/seu_usuario/backup_servidor.tar.gz -C /
   ```

---

## **8. Restaurando Configurações e Bancos de Dados:**
1. **Configurações:**  
   Copie arquivos como `/etc` e `/var/www` do backup para os locais corretos:
   ```bash
   sudo cp -r /etc/* /etc/
   sudo cp -r /var/www/* /var/www/
   ```

2. **Bancos de dados MySQL ou PostgreSQL:**  
   Restaure o dump do banco:
   ```bash
   mysql -u root -p < /caminho/backup.sql  # MySQL
   psql -U postgres < /caminho/backup.sql  # PostgreSQL
   ```

---

## **9. Ajustes Finais:**
1. **Atualize o sistema:**  
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Verifique os serviços:**  
   Certifique-se de que **Apache**, **SSH** e outros serviços estão rodando:
   ```bash
   sudo systemctl status apache2
   sudo systemctl status ssh
   ```

3. **Teste o acesso ao domínio e ao servidor:**  
   Abra o navegador e acesse:  
   ```
   http://seu-dominio.com
   https://seu-dominio.com
   ```

---

## **10. Conclusão:**
Este processo garante que o servidor Debian 12 seja configurado corretamente, com Apache, SSH, SSL e IP fixo. A transferência do backup é feita com segurança, e você poderá restaurar todas as configurações e dados da máquina anterior.

Se precisar de mais alguma coisa, é só avisar! 🚀
