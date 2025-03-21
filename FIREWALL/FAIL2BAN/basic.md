### 25 Configuraciones de Seguridad de Fail2ban

1. **Jail para SSHD Avanzado**  
   - **Explicación:** Configuración avanzada para proteger el servicio SSH, con límites estrictos de reintentos y tiempos de baneo escalables.  
   - **Ejemplo:**  
     ```ini
     [sshd]
     enabled   = true
     port      = ssh
     filter    = sshd
     logpath   = /var/log/auth.log
     maxretry  = 3
     bantime   = 3600
     findtime  = 600
     action    = iptables[name=SSH, port=ssh, protocol=tcp]
     ```

2. **Jail para Apache-Auth**  
   - **Explicación:** Protege el acceso a áreas restringidas de Apache, detectando múltiples fallos de autenticación.  
   - **Ejemplo:**  
     ```ini
     [apache-auth]
     enabled   = true
     port      = http,https
     filter    = apache-auth
     logpath   = /var/log/apache2/error.log
     maxretry  = 5
     bantime   = 7200
     findtime  = 600
     action    = iptables[name=Apache-Auth, port=http, protocol=tcp]
     ```

3. **Jail para Nginx-Auth**  
   - **Explicación:** Configuración para monitorear intentos fallidos de acceso en Nginx, protegiendo el panel de administración.  
   - **Ejemplo:**  
     ```ini
     [nginx-auth]
     enabled   = true
     port      = http,https
     filter    = nginx-auth
     logpath   = /var/log/nginx/error.log
     maxretry  = 4
     bantime   = 3600
     findtime  = 600
     action    = iptables-multiport[name=Nginx-Auth, port="80,443", protocol=tcp]
     ```

4. **Jail para Postfix**  
   - **Explicación:** Protege el servicio de correo Postfix, detectando intentos de autenticación fallida y abuso.  
   - **Ejemplo:**  
     ```ini
     [postfix]
     enabled   = true
     port      = smtp,submission
     filter    = postfix
     logpath   = /var/log/mail.log
     maxretry  = 3
     bantime   = 3600
     findtime  = 600
     action    = iptables[name=Postfix, port=smtp, protocol=tcp]
     ```

5. **Jail para Dovecot**  
   - **Explicación:** Configuración para proteger el servicio de correo Dovecot, limitando intentos de acceso erróneos.  
   - **Ejemplo:**  
     ```ini
     [dovecot]
     enabled   = true
     port      = pop3,pop3s,imap,imaps
     filter    = dovecot
     logpath   = /var/log/mail.log
     maxretry  = 4
     bantime   = 7200
     findtime  = 600
     action    = iptables[name=Dovecot, port=imap, protocol=tcp]
     ```

6. **Jail para ProFTPD**  
   - **Explicación:** Protege el servidor FTP ProFTPD, detectando accesos no autorizados y previniendo ataques de fuerza bruta.  
   - **Ejemplo:**  
     ```ini
     [proftpd]
     enabled   = true
     port      = ftp,ftp-data,ftps,ftps-data
     filter    = proftpd
     logpath   = /var/log/proftpd/proftpd.log
     maxretry  = 3
     bantime   = 3600
     findtime  = 600
     action    = iptables[name=ProFTPD, port=ftp, protocol=tcp]
     ```

7. **Jail para vsftpd**  
   - **Explicación:** Configuración dedicada a vsftpd, con parámetros optimizados para detectar y bloquear múltiples intentos fallidos de login.  
   - **Ejemplo:**  
     ```ini
     [vsftpd]
     enabled   = true
     port      = ftp,ftp-data
     filter    = vsftpd
     logpath   = /var/log/vsftpd.log
     maxretry  = 4
     bantime   = 5400
     findtime  = 600
     action    = iptables-multiport[name=vsftpd, port="21,20", protocol=tcp]
     ```

8. **Jail para Protección de wp-login (WordPress)**  
   - **Explicación:** Evita ataques de fuerza bruta al área de login de WordPress, monitorizando los intentos fallidos.  
   - **Ejemplo:**  
     ```ini
     [wordpress]
     enabled   = true
     port      = http,https
     filter    = wordpress
     logpath   = /var/log/nginx/access.log
     maxretry  = 5
     bantime   = 7200
     findtime  = 600
     action    = iptables-multiport[name=WordPress, port="80,443", protocol=tcp]
     ```

9. **Jail para phpMyAdmin**  
   - **Explicación:** Protege el acceso a phpMyAdmin mediante la detección de intentos de acceso maliciosos.  
   - **Ejemplo:**  
     ```ini
     [phpmyadmin]
     enabled   = true
     port      = http,https
     filter    = phpmyadmin
     logpath   = /var/log/nginx/error.log
     maxretry  = 3
     bantime   = 7200
     findtime  = 600
     action    = iptables-multiport[name=phpMyAdmin, port="80,443", protocol=tcp]
     ```

10. **Jail para MongoDB**  
    - **Explicación:** Configuración para proteger MongoDB, limitando intentos fallidos y evitando conexiones no autorizadas.  
    - **Ejemplo:**  
      ```ini
      [mongodb]
      enabled   = true
      port      = 27017
      filter    = mongodb
      logpath   = /var/log/mongodb/mongod.log
      maxretry  = 3
      bantime   = 3600
      findtime  = 600
      action    = iptables[name=MongoDB, port=27017, protocol=tcp]
      ```

11. **Jail para OpenVPN**  
    - **Explicación:** Protege el servicio OpenVPN detectando intentos fallidos de conexión y abuso de credenciales.  
    - **Ejemplo:**  
      ```ini
      [openvpn]
      enabled   = true
      port      = 1194
      filter    = openvpn
      logpath   = /var/log/openvpn.log
      maxretry  = 3
      bantime   = 7200
      findtime  = 600
      action    = iptables[name=OpenVPN, port=1194, protocol=udp]
      ```

12. **Jail para MySQL**  
    - **Explicación:** Monitorea intentos fallidos de conexión a MySQL, ayudando a prevenir ataques de fuerza bruta sobre la base de datos.  
    - **Ejemplo:**  
      ```ini
      [mysqld-auth]
      enabled   = true
      port      = mysql
      filter    = mysqld-auth
      logpath   = /var/log/mysql/error.log
      maxretry  = 4
      bantime   = 3600
      findtime  = 600
      action    = iptables[name=MySQL, port=3306, protocol=tcp]
      ```

13. **Jail para Exim**  
    - **Explicación:** Configuración para proteger el servidor de correo Exim, detectando intentos de envío masivo o acceso indebido.  
    - **Ejemplo:**  
      ```ini
      [exim]
      enabled   = true
      port      = smtp,submission
      filter    = exim
      logpath   = /var/log/exim/mainlog
      maxretry  = 3
      bantime   = 3600
      findtime  = 600
      action    = iptables[name=Exim, port=smtp, protocol=tcp]
      ```

14. **Jail para Samba**  
    - **Explicación:** Protege servidores Samba mediante la detección de patrones sospechosos en los accesos y la autenticación.  
    - **Ejemplo:**  
      ```ini
      [samba]
      enabled   = true
      port      = 137,138,139,445
      filter    = samba
      logpath   = /var/log/samba/log.smbd
      maxretry  = 4
      bantime   = 7200
      findtime  = 600
      action    = iptables-multiport[name=Samba, port="137,138,139,445", protocol=udp]
      ```

15. **Jail para Tomcat**  
    - **Explicación:** Configuración diseñada para proteger la interfaz administrativa de Tomcat, limitando accesos sospechosos.  
    - **Ejemplo:**  
      ```ini
      [tomcat]
      enabled   = true
      port      = 8080
      filter    = tomcat
      logpath   = /var/log/tomcat/catalina.out
      maxretry  = 3
      bantime   = 3600
      findtime  = 600
      action    = iptables[name=Tomcat, port=8080, protocol=tcp]
      ```

16. **Jail para Cron**  
    - **Explicación:** Vigila el uso indebido del demonio cron, detectando patrones inusuales en los logs de ejecución de tareas programadas.  
    - **Ejemplo:**  
      ```ini
      [cron]
      enabled   = true
      port      = any
      filter    = cron
      logpath   = /var/log/cron
      maxretry  = 3
      bantime   = 1800
      findtime  = 600
      action    = iptables[name=Cron, protocol=tcp]
      ```

17. **Jail para Redis**  
    - **Explicación:** Configuración que protege instancias de Redis, detectando conexiones no autorizadas y abusos en la autenticación.  
    - **Ejemplo:**  
      ```ini
      [redis]
      enabled   = true
      port      = 6379
      filter    = redis
      logpath   = /var/log/redis/redis-server.log
      maxretry  = 3
      bantime   = 3600
      findtime  = 600
      action    = iptables[name=Redis, port=6379, protocol=tcp]
      ```

18. **Jail para Dropbear SSH**  
    - **Explicación:** Protege el servicio Dropbear (SSH ligero) con parámetros ajustados para entornos con recursos limitados.  
    - **Ejemplo:**  
      ```ini
      [dropbear]
      enabled   = true
      port      = 2222
      filter    = dropbear
      logpath   = /var/log/auth.log
      maxretry  = 3
      bantime   = 3600
      findtime  = 600
      action    = iptables[name=Dropbear, port=2222, protocol=tcp]
      ```

19. **Jail para Postfix-SASL**  
    - **Explicación:** Protege la autenticación SASL en Postfix, evitando abusos en el inicio de sesión vía correo.  
    - **Ejemplo:**  
      ```ini
      [postfix-sasl]
      enabled   = true
      port      = smtp,submission
      filter    = postfix-sasl
      logpath   = /var/log/mail.log
      maxretry  = 3
      bantime   = 3600
      findtime  = 600
      action    = iptables[name=Postfix-SASL, port=smtp, protocol=tcp]
      ```

20. **Jail para Pure-FTPD**  
    - **Explicación:** Configuración para proteger el servidor Pure-FTPD, detectando intentos de acceso fallidos y ataques automatizados.  
    - **Ejemplo:**  
      ```ini
      [pure-ftpd]
      enabled   = true
      port      = ftp,ftp-data
      filter    = pure-ftpd
      logpath   = /var/log/pure-ftpd/pure-ftpd.log
      maxretry  = 4
      bantime   = 3600
      findtime  = 600
      action    = iptables-multiport[name=PureFTPD, port="21,20", protocol=tcp]
      ```

21. **Jail para cPanel**  
    - **Explicación:** Protege los servicios de cPanel monitorizando intentos fallidos de acceso a su interfaz de administración.  
    - **Ejemplo:**  
      ```ini
      [cpanel]
      enabled   = true
      port      = 2082,2083
      filter    = cpanel
      logpath   = /usr/local/cpanel/logs/access_log
      maxretry  = 5
      bantime   = 7200
      findtime  = 600
      action    = iptables-multiport[name=cPanel, port="2082,2083", protocol=tcp]
      ```

22. **Jail para ModSecurity**  
    - **Explicación:** Monitorea los logs de ModSecurity para detectar ataques web sofisticados y responder de inmediato.  
    - **Ejemplo:**  
      ```ini
      [modsecurity]
      enabled   = true
      port      = http,https
      filter    = modsecurity
      logpath   = /var/log/apache2/modsec_audit.log
      maxretry  = 3
      bantime   = 3600
      findtime  = 600
      action    = iptables-multiport[name=ModSecurity, port="80,443", protocol=tcp]
      ```

23. **Jail para Autoprotección de Fail2ban**  
    - **Explicación:** Configuración interna para proteger el propio servicio fail2ban de manipulaciones o ataques directos.  
    - **Ejemplo:**  
      ```ini
      [fail2ban]
      enabled   = true
      port      = any
      filter    = fail2ban
      logpath   = /var/log/fail2ban.log
      maxretry  = 2
      bantime   = 600
      findtime  = 300
      action    = iptables[name=Fail2ban, protocol=tcp]
      ```

24. **Jail Genérico para Logs Personalizados**  
    - **Explicación:** Permite crear una protección basada en logs personalizados, ideal para aplicaciones internas.  
    - **Ejemplo:**  
      ```ini
      [custom-app]
      enabled   = true
      port      = http,https
      filter    = custom-app
      logpath   = /var/log/custom/app.log
      maxretry  = 4
      bantime   = 3600
      findtime  = 600
      action    = iptables-multiport[name=CustomApp, port="80,443", protocol=tcp]
      ```

25. **Jail para Protección de API REST**  
    - **Explicación:** Configuración especializada para proteger endpoints de API REST contra intentos de abuso y ataques de fuerza bruta.  
    - **Ejemplo:**  
      ```ini
      [api-rest]
      enabled   = true
      port      = http,https
      filter    = api-rest
      logpath   = /var/log/nginx/api_access.log
      maxretry  = 5
      bantime   = 7200
      findtime  = 600
      action    = iptables-multiport[name=APIREST, port="80,443", protocol=tcp]
      ```

---

### 25 Comandos de Fail2ban en Entornos de Producción

1. **Obtener Estado Global**  
   - **Explicación:** Muestra el estado global de fail2ban, listando todas las jails activas y estadísticas generales.  
   - **Ejemplo:**  
     ```bash
     fail2ban-client status
     ```

2. **Estado de Jail Específico (sshd)**  
   - **Explicación:** Verifica el estado y estadísticas específicas de la jail "sshd".  
   - **Ejemplo:**  
     ```bash
     fail2ban-client status sshd
     ```

3. **Desbanear una IP en sshd**  
   - **Explicación:** Elimina el baneo de una IP específica en la jail "sshd".  
   - **Ejemplo:**  
     ```bash
     fail2ban-client set sshd unbanip 192.0.2.1
     ```

4. **Bannear una IP en sshd**  
   - **Explicación:** Fuerza el baneo manual de una IP en la jail "sshd".  
   - **Ejemplo:**  
     ```bash
     fail2ban-client set sshd banip 192.0.2.2
     ```

5. **Recargar la Configuración de Fail2ban**  
   - **Explicación:** Recarga todas las jails y la configuración sin reiniciar el servicio.  
   - **Ejemplo:**  
     ```bash
     fail2ban-client reload
     ```

6. **Recargar Jail Específica**  
   - **Explicación:** Recarga únicamente la configuración de la jail "sshd".  
   - **Ejemplo:**  
     ```bash
     fail2ban-client reload jail sshd
     ```

7. **Ver Versión de Fail2ban**  
   - **Explicación:** Muestra la versión instalada de fail2ban para fines de auditoría.  
   - **Ejemplo:**  
     ```bash
     fail2ban-client version
     ```

8. **Obtener Valor de findtime en sshd**  
   - **Explicación:** Recupera el parámetro "findtime" configurado en la jail "sshd".  
   - **Ejemplo:**  
     ```bash
     fail2ban-client get sshd findtime
     ```

9. **Agregar IP a la Lista de Ignorados en sshd**  
   - **Explicación:** Añade una IP a la lista de ignorados para evitar baneo en la jail "sshd".  
   - **Ejemplo:**  
     ```bash
     fail2ban-client set sshd addignoreip 198.51.100.1
     ```

10. **Ajustar bantime en Jail sshd**  
    - **Explicación:** Modifica el tiempo de baneo (bantime) de la jail "sshd" a 7200 segundos.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client set jail sshd set bantime 7200
      ```

11. **Estado Completo con Todos los Detalles**  
    - **Explicación:** Muestra el estado detallado de todas las jails, incluyendo IPs baneadas y estadísticas.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client status --with-all
      ```

12. **Cambiar Acción de Ban en Apache-Auth**  
    - **Explicación:** Modifica la acción de ban en la jail "apache-auth" para enviar alertas por correo.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client set jail apache-auth actionban sendmail-whois[name=apache-auth]
      ```

13. **Bannear una IP en vsftpd**  
    - **Explicación:** Fuerza el baneo de una IP en la jail "vsftpd" mediante comando manual.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client set jail vsftpd banip 203.0.113.5
      ```

14. **Obtener Valor de maxretry en sshd**  
    - **Explicación:** Recupera el número máximo de reintentos configurado para la jail "sshd".  
    - **Ejemplo:**  
      ```bash
      fail2ban-client get sshd maxretry
      ```

15. **Consultar Logpath de Jail nginx-auth**  
    - **Explicación:** Muestra la ruta del log utilizada por la jail "nginx-auth".  
    - **Ejemplo:**  
      ```bash
      fail2ban-client get nginx-auth logpath
      ```

16. **Detener la Jail sshd**  
    - **Explicación:** Detiene temporalmente la jail "sshd" para mantenimiento o ajustes.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client stop sshd
      ```

17. **Iniciar la Jail sshd**  
    - **Explicación:** Inicia o reactiva la jail "sshd" tras haberla detenido.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client start sshd
      ```

18. **Habilitar la Jail sshd**  
    - **Explicación:** Activa la jail "sshd" si se encuentra deshabilitada, aplicando la configuración actual.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client enable sshd
      ```

19. **Deshabilitar la Jail sshd**  
    - **Explicación:** Desactiva la jail "sshd" para evitar su acción mientras se realizan cambios o pruebas.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client disable sshd
      ```

20. **Resetear la Jail nginx-auth**  
    - **Explicación:** Reinicia el estado interno de la jail "nginx-auth", borrando estadísticas temporales.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client set jail nginx-auth reset
      ```

21. **Reiniciar la Jail sshd**  
    - **Explicación:** Reinicia la jail "sshd" para aplicar nuevos cambios de configuración de forma inmediata.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client set jail sshd restart
      ```

22. **Obtener Estado Detallado en Modo Verbose**  
    - **Explicación:** Muestra el estado de todas las jails con información detallada para auditorías.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client status --verbose
      ```

23. **Ajustar maxretry en Jail sshd**  
    - **Explicación:** Establece el parámetro maxretry en 5 para la jail "sshd", reduciendo el número de intentos permitidos.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client set sshd set maxretry 5
      ```

24. **Ajustar findtime en Jail sshd**  
    - **Explicación:** Modifica el período de tiempo (findtime) para la detección de intentos en la jail "sshd" a 600 segundos.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client set jail sshd set findtime 600
      ```

25. **Ajustar bantime en Jail sshd (Reconfigurar)**  
    - **Explicación:** Reconfigura el tiempo de baneo (bantime) a 3600 segundos en la jail "sshd" para actualizar la política de seguridad.  
    - **Ejemplo:**  
      ```bash
      fail2ban-client set jail sshd set bantime 3600
      ```

