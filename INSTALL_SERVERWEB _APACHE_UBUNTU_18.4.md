##INTALL APACHE WEB-SERVICE UBUNTU 18.4




- PHP
-------------------
STEP 1 INSTALL PHP
-------------------
$ sudo apt install php libapache2-mod-php php-mysql php-cli


- MYSQL
---------------------------
STEP 1 INSTALL MYSQL-SERVER
---------------------------
$ sudo apt install mysql-server

Você será perguntado se você quer configurar o VALIDATE PASSWORD PLUGIN.
$ sudo mysql_secure_installation



 - APACHE
----------------------
STEP 1 INSTALL APACHE
----------------------
$ sudo apt update
$ sudo apt install apache2


------------------------------
STEP 2 ADJUSTING THE FIREWALL
------------------------------
List the ufw application profiles by typing:

$ sudo ufw app list
Output:
	Available applications:
	  Apache
	  Apache Full
	  Apache Secure
	  OpenSSH

As you can see, there are three profiles available for Apache:

Apache::::::::::This profile opens only port 80 (normal, unencrypted web traffic)
Apache Full:::::This profile opens both port 80 (normal, unencrypted web traffic) and port 443 (TLS/SSL encrypted traffic)
Apache Secure:::This profile opens only port 443 (TLS/SSL encrypted traffic)

$ sudo ufw allow 'Apache'


You can verify the change by typing:
$ sudo ufw status

Output
	Status: active
	To                         Action      From
	--                         ------      ----
	OpenSSH                    ALLOW       Anywhere                  
	Apache                     ALLOW       Anywhere                  
	OpenSSH (v6)               ALLOW       Anywhere (v6)             
	Apache (v6)                ALLOW       Anywhere (v6)

--------------------------------
STEP 3 CHECKING YOUR WEB SERVER
--------------------------------

$ sudo systemctl status apache2

Output
	● apache2.service - The Apache HTTP Server
	   Loaded: loaded (/lib/systemd/system/apache2.service; enabled; vendor preset: enabled)
	  Drop-In: /lib/systemd/system/apache2.service.d
	           └─apache2-systemd.conf
	   Active: active (running) since Tue 2018-04-24 20:14:39 UTC; 9min ago
	 Main PID: 2583 (apache2)
	    Tasks: 55 (limit: 1153)
	   CGroup: /system.slice/apache2.service
	           ├─2583 /usr/sbin/apache2 -k start
	           ├─2585 /usr/sbin/apache2 -k start
	           └─2586 /usr/sbin/apache2 -k start


Try typing this at your server's command prompt:
$ hostname -I OR $ curl -4 icanhazip.com


Open is browser enter link:
$ http://your_server_ip

--------------------------------------------
STEP 4 GERENCIAMENTO DE PROCESSOS DO APACHE
--------------------------------------------

Informar ao Apache para interpretar arquivo .PHP
$ sudo vim /etc/apache2/mods-enabled/dir.conf

<IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
</IfModule>

... EDITAR PARA 

<IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

(( realizar o restart do servidor ))

	Start Apache
	$ sudo systemctl start apache2

	Stop Apache
	$ sudo systemctl stop apache2

	Restart Apache
	$ sudo systemctl restart apache2

	Reload Apache
	$ sudo systemctl reaload apache2

	Status Apache
	$ sudo systemctl status apache2

	Disabled Apache - O Apache inicia automaticamento no inicio do servidor. Usando esta opçao recurso fica desativado
	$ sudo systemctl disabled apache2

	Enabled Apache - O oposto do 'disabled'.
	$ sudo systemctl enabled apache2


Para uso de Reencrita de URL "http://exemplo.com/nome_do_artigo"
$ sudo a2enmod rewrite 

$ sudo nano /etc/apache2/apache2.conf
<Directory /var/www/>
   Options Indexes FollowSymLinks
   AllowOverride None ===========--------> ALTERAR PARA AllowOverride All
   Require all granted
</Directory> 

(( realizar restart no apache ))
--------------------------------------------------
STEP 5 TESTANDO PROCESSAMENTO DE ARQUIVOS PHP/HTML
--------------------------------------------------

$ sudo nano /var/www/html/info.php
<?PHP
	phpinfo();
?>
