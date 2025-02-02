
# 📜 Dovecot Logs: Ubicación, Estructura y Análisis Completo 🚀
El registro de logs en Dovecot es esencial para monitorear el servicio, solucionar problemas y mejorar la seguridad. A continuación, te explico dónde se almacenan, cómo interpretarlos y cómo detectar eventos clave como logins, correos movidos, errores de autenticación y más. 🔍

📍 1. Ubicación de los Logs de Dovecot 🗂️
Los logs de Dovecot pueden almacenarse en diferentes archivos, dependiendo de la configuración del servidor:

📌 Principales archivos de logs:

📨 /var/log/mail.log → Registro global de correo (Dovecot, Postfix, etc.)
📄 /var/log/dovecot.log → Logs exclusivos de Dovecot
🔎 /var/log/dovecot-info.log → Eventos informativos (logins, correos movidos)
⚠️ /var/log/dovecot-debug.log → Logs en modo depuración
✉️ /var/log/dovecot-sieve.log → Registros de filtros Sieve (si están activados)
📌 Verificar si los logs están activados:
Si los archivos de logs no existen, revisa la configuración en:
📁 /etc/dovecot/conf.d/10-logging.conf

ini
Copiar
Editar
log_path = /var/log/dovecot.log
auth_verbose = yes
auth_debug = yes
auth_debug_passwords = no
📌 Esto garantiza que se almacenen registros detallados de la actividad de Dovecot.

📜 2. Estructura de los Logs de Dovecot 🧐
Un log de Dovecot típico sigue la siguiente estructura:

log
Copiar
Editar
Feb 02 12:34:56 server dovecot: imap(user@example.com): Logged in
Feb 02 12:35:10 server dovecot: imap(user@example.com): Disconnected: Logged out
Feb 02 12:40:20 server dovecot: auth-worker(1234): sql(user@example.com): Password mismatch
Feb 02 12:42:50 server dovecot: imap(user@example.com): Move: INBOX -> Trash
📌 Explicación de cada parte del log:

🔢 Posición	📝 Descripción
🕒 Fecha y hora	Feb 02 12:34:56 → Cuándo ocurrió el evento
🖥️ Nombre del servidor	server → Servidor donde ocurrió el evento
📌 Proceso involucrado	dovecot: imap → Servicio afectado (imap, pop3, auth, lmtp, etc.)
👤 Usuario afectado	(user@example.com) → Cuenta de correo en cuestión
🔍 Mensaje del evento	Logged in, Disconnected, Password mismatch, Move: INBOX -> Trash
📌 Cada log muestra una acción específica del servicio, lo que facilita su interpretación.

🔍 3. Cómo Buscar Eventos Específicos en los Logs 🔎
Puedes filtrar los logs para encontrar información clave con los siguientes comandos:

🟢 Ver intentos de login exitosos
bash
Copiar
Editar
grep "Logged in" /var/log/dovecot.log
📌 Muestra los usuarios que iniciaron sesión con éxito.

🔴 Ver intentos de autenticación fallidos (contraseña incorrecta)
bash
Copiar
Editar
grep "Password mismatch" /var/log/dovecot.log
📌 Muestra intentos de acceso fallidos por contraseña incorrecta.

🗑️ Ver correos eliminados o movidos a la papelera
bash
Copiar
Editar
grep "Move: .* -> Trash" /var/log/dovecot.log
📌 Muestra cuándo y quién movió correos a la papelera.

📂 Ver correos movidos entre carpetas
bash
Copiar
Editar
grep "Move:" /var/log/dovecot.log
📌 Registra cuándo un usuario movió correos de una carpeta a otra.

🌍 Ver direcciones IP de los intentos de acceso
bash
Copiar
Editar
grep "rip=" /var/log/dovecot.log
📌 Lista las direcciones IP desde donde se realizaron intentos de conexión.

⚠️ Ver errores de IMAP o POP3
bash
Copiar
Editar
grep "Error:" /var/log/dovecot.log
📌 Filtra errores relacionados con el acceso IMAP/POP3.

🚨 4. Interpretación de Errores Comunes y Soluciones 🛠️
Aquí tienes algunos mensajes de error frecuentes y cómo solucionarlos:

❌ Fallo de autenticación
log
Copiar
Editar
dovecot: auth: Failed password for user@example.com from 192.168.1.1
🔹 Causa: Contraseña incorrecta o usuario inexistente
🔹 Solución: Verificar credenciales con:

bash
Copiar
Editar
doveadm auth test user@example.com
❌ Permisos incorrectos en Maildir
log
Copiar
Editar
dovecot: imap(user@example.com): Error: Mail access failed: Permission denied
🔹 Causa: Problemas de permisos en /var/mail/vhosts/
🔹 Solución: Corregir permisos con:

bash
Copiar
Editar
chown -R vmail:vmail /var/mail/vhosts
chmod -R 700 /var/mail/vhosts
❌ Problema con certificados SSL
log
Copiar
Editar
dovecot: imap-login: Disconnected (auth failed, TLS handshaking failed): user=<user@example.com>, rip=192.168.1.1
🔹 Causa: Certificado SSL inválido o expirado
🔹 Solución: Renovar SSL con:

bash
Copiar
Editar
certbot renew --quiet
systemctl restart dovecot
❌ Intentos de fuerza bruta detectados
log
Copiar
Editar
dovecot: auth-worker(1234): Warning: Brute-force attempt detected from 192.168.1.1
🔹 Causa: Ataques de fuerza bruta en el login
🔹 Solución: Bloquear IP sospechosa con Fail2Ban:

bash
Copiar
Editar
fail2ban-client set dovecot banip 192.168.1.1
🎯 5. Configurar Logs Detallados para Mayor Análisis 🛠️
Si necesitas registros más detallados, activa el modo verbose logging en /etc/dovecot/conf.d/10-logging.conf:

ini
Copiar
Editar
log_path = /var/log/dovecot.log
auth_verbose = yes
auth_debug = yes
auth_debug_passwords = no
mail_debug = yes
📌 Esto aumentará la cantidad de información almacenada en los logs para un análisis más profundo.

✅ 6. Conclusión: Control Total Sobre los Logs de Dovecot 🎯
🔹 Dovecot almacena registros en /var/log/dovecot.log y /var/log/mail.log.
🔹 Los logs muestran logins, correos movidos, intentos fallidos y errores.
🔹 Puedes filtrar logs con grep para encontrar eventos clave.
🔹 Errores comunes incluyen fallos de autenticación, problemas de permisos y SSL expirado.
🔹 Habilitar logs detallados permite una mejor depuración y seguridad.
