
Aquí tienes un resumen visual de la instalación y configuración completa de Dovecot en un servidor LAMP con iconos y pasos organizados de manera clara:

📩 Instalación y Configuración de Dovecot en un Servidor LAMP
🔹 1. Instalación de Dovecot
```
sudo apt update && sudo apt install dovecot-core dovecot-imapd dovecot-pop3d
```
📌 Instala los paquetes principales de Dovecot para manejar IMAP y POP3.

🔹 2. Configuración General
📁 Archivos clave:

/etc/dovecot/dovecot.conf → Configuración principal
/etc/dovecot/conf.d/10-auth.conf → Autenticación
/etc/dovecot/conf.d/10-mail.conf → Configuración de correo
/etc/dovecot/conf.d/10-ssl.conf → Seguridad SSL
🔧 Edita /etc/dovecot/dovecot.conf y ajusta los protocolos:

```
protocols = imap pop3 lmtp
```
📌 Activa los protocolos necesarios (IMAP y POP3).

🔹 3. Configuración de la Autenticación
🔑 Edita /etc/dovecot/conf.d/10-auth.conf:

```
disable_plaintext_auth = yes
auth_mechanisms = plain login
```
🔍 Habilita autenticación con usuarios del sistema:

```
  driver = pam
}
userdb {
  driver = passwd
}
```
📌 Usa autenticación con PAM y contraseñas del sistema.

🔹 4. Configuración de los Correos
📂 Edita /etc/dovecot/conf.d/10-mail.conf y configura el directorio de correos:
```
mail_location = maildir:/var/mail/vhosts/%d/%n
```
📌 Usa formato Maildir en una carpeta dedicada.

🔹 5. Seguridad y SSL
🔒 Edita /etc/dovecot/conf.d/10-ssl.conf:

```
ssl = required
ssl_cert = </etc/letsencrypt/live/tu-dominio.com/fullchain.pem
ssl_key = </etc/letsencrypt/live/tu-dominio.com/privkey.pem
```
📌 Habilita SSL con Let's Encrypt para conexiones seguras.

🔹 6. Crear Usuarios de Correo
👤 Crear usuarios y permisos:
```
sudo adduser usuario_mail
sudo mkdir -p /var/mail/vhosts/tu-dominio.com/usuario_mail
sudo chown -R vmail:vmail /var/mail/vhosts
```
📌 Crea usuarios y asigna permisos adecuados.

🔹 7. Reiniciar y Probar
🔄 Reiniciar Dovecot:

```
sudo systemctl restart dovecot
sudo systemctl enable dovecot
```
🛠️ Probar con Telnet o Netcat:
```
telnet localhost 143
```
📌 Verifica que IMAP funciona correctamente.

✅ Dovecot Listo y Seguro
🎯 Resumen Final: ✅ Instalado y configurado correctamente
🔐 Seguridad activada con SSL
📂 Almacenamiento de correos en Maildir
🛠️ Usuarios creados y autenticación funcionando

📩  Configuración avanzada de Dovecot en un Servidor LAMP

🔹 1. Habilitar la indexación para mejorar el rendimiento
📁 Archivo: /etc/dovecot/conf.d/10-mail.conf
```
maildir_very_dirty_syncs = yes
mail_cache_min_mail_count = 10
```
📌 Mejora la velocidad de acceso a correos al usar caché y sincronización eficiente.

🔹 2. Configurar el puerto IMAP y POP3 en IPv4
📁 Archivo: /etc/dovecot/dovecot.conf
```service imap-login {
  inet_listener imap {
    port = 143
  }
}
service pop3-login {
  inet_listener pop3 {
    port = 110
  }
}
```
📌 Asegura que los servicios IMAP y POP3 solo escuchen en IPv4.

🔹 3. Habilitar IMAP sobre SSL/TLS
📁 Archivo: /etc/dovecot/conf.d/10-ssl.conf
```
ssl = required
ssl_cert = </etc/letsencrypt/live/tu-dominio.com/fullchain.pem
ssl_key = </etc/letsencrypt/live/tu-dominio.com/privkey.pem
📌 Obliga a los clientes a usar conexiones cifradas.
```

🔹 4. Usar autenticación con base de datos MySQL
📁 Archivo: /etc/dovecot/conf.d/auth-sql.conf.ext
```
passdb {
  driver = sql
  args = /etc/dovecot/dovecot-sql.conf.ext
}
```
📌 Permite la autenticación contra una base de datos MySQL en lugar de usuarios del sistema.

🔹 5. Bloquear ataques de fuerza bruta con Fail2Ban
📁 Archivo: /etc/fail2ban/jail.local
```
[dovecot]
enabled = true
port = pop3,pop3s,imap,imaps
filter = dovecot
logpath = /var/log/mail.log
maxretry = 5
```
📌 Protege contra intentos de acceso no autorizados.

🔹 6. Configurar límites de conexiones simultáneas
📁 Archivo: /etc/dovecot/conf.d/20-imap.conf
```
mail_max_userip_connections = 10
```
📌 Evita abusos al restringir el número de conexiones de un mismo usuario.

🔹 7. Configurar tiempo de inactividad antes de cerrar sesión
📁 Archivo: /etc/dovecot/conf.d/20-imap.conf
```
imap_idle_notify_interval = 2 mins
```
📌 Optimiza el consumo de recursos en conexiones IMAP.

🔹 8. Especificar formato de logs de autenticación
📁 Archivo: /etc/dovecot/conf.d/10-logging.conf
```
log_path = /var/log/dovecot.log
auth_verbose = yes
auth_verbose_passwords = no
```
📌 Permite registrar detalles sobre accesos sin exponer contraseñas.

🔹 9. Configurar compresión de correos
📁 Archivo: /etc/dovecot/conf.d/10-mail.conf
```
maildir_compress = zlib
```
📌 Reduce el espacio en disco utilizado por los correos electrónicos.

🔹 10. Forzar uso de autenticación segura
📁 Archivo: /etc/dovecot/conf.d/10-auth.conf
```
disable_plaintext_auth = yes
auth_mechanisms = plain login
```
📌 Obliga a los clientes a usar autenticación cifrada.

🔹 11. Habilitar soporte para Maildir++
📁 Archivo: /etc/dovecot/conf.d/10-mail.conf
```
mail_location = maildir:/var/mail/vhosts/%d/%n
```
📌 Estructura eficiente de almacenamiento de correos.

🔹 12. Configurar tamaño máximo de correo
📁 Archivo: /etc/dovecot/dovecot.conf
```
mail_max_size = 50M
```
📌 Limita el tamaño de los correos a 50 MB.

🔹 13. Configurar autoexpurgo de correos en Spam y Trash
📁 Archivo: /etc/dovecot/conf.d/15-mailboxes.conf
```
mailbox Trash {
  autoexpunge = 30d
}
mailbox Junk {
  autoexpunge = 15d
}
```
📌 Elimina correos antiguos automáticamente.

🔹 14. Habilitar el protocolo LMTP
📁 Archivo: /etc/dovecot/conf.d/15-lda.conf
```
protocol lmtp {
  postmaster_address = postmaster@tu-dominio.com
}
```
📌 Permite la entrega de correos sin pasar por un MTA como Postfix.

🔹 15. Configurar redirección de correos a múltiples buzones
📁 Archivo: /etc/dovecot/conf.d/90-sieve.conf
```
require ["fileinto"];
if address :is "to" "soporte@tu-dominio.com" {
  fileinto "Support";
}
```
📌 Usa Sieve para filtrar y redirigir correos automáticamente.

🔹 16. Habilitar conexiones TLS para SMTP con Postfix
📁 Archivo: /etc/postfix/main.cf
```
smtpd_tls_security_level = encrypt
smtpd_tls_cert_file = /etc/letsencrypt/live/tu-dominio.com/fullchain.pem
smtpd_tls_key_file = /etc/letsencrypt/live/tu-dominio.com/privkey.pem
```
📌 Asegura las conexiones de envío de correo.

🔹 17. Monitorizar actividad con Dovecot Stats
📁 Archivo: /etc/dovecot/conf.d/10-logging.conf
```
service stats {
  unix_listener stats-reader {
    user = vmail
  }
}
```
📌 Habilita métricas detalladas de Dovecot.

🔹 18. Configurar auto-creación de carpetas de usuario
📁 Archivo: /etc/dovecot/conf.d/15-mailboxes.conf
```
namespace inbox {
  mailbox Drafts {
    auto = subscribe
  }
  mailbox Sent {
    auto = subscribe
  }
  mailbox Junk {
    auto = subscribe
  }
}
```
📌 Asegura que los usuarios tengan carpetas esenciales creadas automáticamente.

🔹 19. Configurar límites de almacenamiento por usuario
📁 Archivo: /etc/dovecot/conf.d/90-quota.conf
```
plugin {
  quota = maildir
  quota_rule = *:storage=1G
}
```
📌 Restringe el espacio máximo de correo para cada usuario.

🔹 20. Deshabilitar soporte para versiones antiguas de SSL
📁 Archivo: /etc/dovecot/conf.d/10-ssl.conf
```
ssl_min_protocol = TLSv1.2
```
📌 Evita vulnerabilidades desactivando SSL obsoleto.


