
# 📜 Dovecot Logs: Ubicación, Estructura y Análisis Completo 🚀
El registro de logs en Dovecot es esencial para monitorear el servicio, solucionar problemas y mejorar la seguridad. A continuación, te explico dónde se almacenan, cómo interpretarlos y cómo detectar eventos clave como logins, correos movidos, errores de autenticación y más. 🔍

📍 1. Ubicación de los Logs de Dovecot 🗂️
Los logs de Dovecot pueden almacenarse en diferentes archivos, dependiendo de la configuración del servidor:

📌 Principales archivos de logs:

- 📨 /var/log/mail.log → Registro global de correo (Dovecot, Postfix, etc.)
- 📄 /var/log/dovecot.log → Logs exclusivos de Dovecot
- 🔎 /var/log/dovecot-info.log → Eventos informativos (logins, correos movidos)
- ⚠️ /var/log/dovecot-debug.log → Logs en modo depuración
- ✉️ /var/log/dovecot-sieve.log → Registros de filtros Sieve (si están activados)
📌 Verificar si los logs están activados:
Si los archivos de logs no existen, revisa la configuración en:
- 📁 /etc/dovecot/conf.d/10-logging.conf
  ```
  log_path = /var/log/dovecot.log
  auth_verbose = yes
  auth_debug = yes
  auth_debug_passwords = no
  ```
📌 Esto garantiza que se almacenen registros detallados de la actividad de Dovecot.

📜 2. Estructura de los Logs de Dovecot 🧐
Un log de Dovecot típico sigue la siguiente estructura:
```
Feb 02 12:34:56 server dovecot: imap(user@example.com): Logged in
Feb 02 12:35:10 server dovecot: imap(user@example.com): Disconnected: Logged out
Feb 02 12:40:20 server dovecot: auth-worker(1234): sql(user@example.com): Password mismatch
Feb 02 12:42:50 server dovecot: imap(user@example.com): Move: INBOX -> Trash
```
📌 Explicación de cada parte del log:

- 🔢 Posición	📝 Descripción
- 🕒 Fecha y hora	Feb 02 12:34:56 → Cuándo ocurrió el evento
- 🖥️ Nombre del servidor	server → Servidor donde ocurrió el evento
- 📌 Proceso involucrado	dovecot: imap → Servicio afectado (imap, pop3, auth, lmtp, etc.)
- 👤 Usuario afectado	(user@example.com) → Cuenta de correo en cuestión
- 🔍 Mensaje del evento	Logged in, Disconnected, Password mismatch, Move: INBOX -> Trash
📌 Cada log muestra una acción específica del servicio, lo que facilita su interpretación.

🔍 3. Cómo Buscar Eventos Específicos en los Logs 🔎
Puedes filtrar los logs para encontrar información clave con los siguientes comandos:

🟢 Ver intentos de login exitosos
```
grep "Logged in" /var/log/dovecot.log
```
📌 Muestra los usuarios que iniciaron sesión con éxito.

🔴 Ver intentos de autenticación fallidos (contraseña incorrecta)
```
grep "Password mismatch" /var/log/dovecot.log
```
📌 Muestra intentos de acceso fallidos por contraseña incorrecta.

🗑️ Ver correos eliminados o movidos a la papelera
```
grep "Move: .* -> Trash" /var/log/dovecot.log
```
📌 Muestra cuándo y quién movió correos a la papelera.

📂 Ver correos movidos entre carpetas
```
grep "Move:" /var/log/dovecot.log
```
📌 Registra cuándo un usuario movió correos de una carpeta a otra.

🌍 Ver direcciones IP de los intentos de acceso
```
grep "rip=" /var/log/dovecot.log
```
📌 Lista las direcciones IP desde donde se realizaron intentos de conexión.

⚠️ Ver errores de IMAP o POP3
```
grep "Error:" /var/log/dovecot.log
```
📌 Filtra errores relacionados con el acceso IMAP/POP3.

🚨 4. Interpretación de Errores Comunes y Soluciones 🛠️
Aquí tienes algunos mensajes de error frecuentes y cómo solucionarlos:

❌ Fallo de autenticación
```
dovecot: auth: Failed password for user@example.com from 192.168.1.1
```
🔹 Causa: Contraseña incorrecta o usuario inexistente
🔹 Solución: Verificar credenciales con:
```
doveadm auth test user@example.com
```
❌ Permisos incorrectos en Maildir
```
dovecot: imap(user@example.com): Error: Mail access failed: Permission denied
```
🔹 Causa: Problemas de permisos en /var/mail/vhosts/
🔹 Solución: Corregir permisos con:
```
chown -R vmail:vmail /var/mail/vhosts
chmod -R 700 /var/mail/vhosts
```
❌ Problema con certificados SSL
```
dovecot: imap-login: Disconnected (auth failed, TLS handshaking failed): user=<user@example.com>, rip=192.168.1.1
```
🔹 Causa: Certificado SSL inválido o expirado
🔹 Solución: Renovar SSL con:
  ```
  certbot renew --quiet
  systemctl restart dovecot
  ```
❌ Intentos de fuerza bruta detectados
```
dovecot: auth-worker(1234): Warning: Brute-force attempt detected from 192.168.1.1
```
🔹 Causa: Ataques de fuerza bruta en el login
🔹 Solución: Bloquear IP sospechosa con Fail2Ban:
  ```
  fail2ban-client set dovecot banip 192.168.1.1
  ```
🎯 5. Configurar Logs Detallados para Mayor Análisis 🛠️
Si necesitas registros más detallados, activa el modo verbose logging en /etc/dovecot/conf.d/10-logging.conf:
```
log_path = /var/log/dovecot.log
auth_verbose = yes
auth_debug = yes
auth_debug_passwords = no
mail_debug = yes
```
📌 Esto aumentará la cantidad de información almacenada en los logs para un análisis más profundo.


# 📜 Logs de Postfix: Estructura, Análisis y Extracción de Información
Postfix, al ser un servidor de correo, genera registros (logs) muy útiles para diagnosticar problemas, optimizar la entrega de correos y monitorear la actividad. A continuación, se detalla cómo interpretar cada parte de estos logs y cómo extraer información relevante.

1. Ubicación de los Logs 📁
Principales archivos:
- 📨 Debian/Ubuntu: /var/log/mail.log
- 📄 CentOS/Fedora: /var/log/maillog
Notas adicionales:
La ubicación puede variar según la configuración de syslog/rsyslog. Revisa los archivos de configuración (por ejemplo, /etc/rsyslog.conf) para confirmar dónde se almacenan.

2. Estructura de un Log de Postfix 📜
Un registro típico de Postfix suele tener el siguiente formato:
```
Feb 02 12:34:56 server postfix/smtp[12345]: ABCDE12345: to=<recipient@example.com>, relay=mail.example.com[192.168.1.1]:25, delay=0.75, delays=0.1/0/0.4/0.25, dsn=2.0.0, status=sent (250 OK)
```
Desglose de cada apartado:
- 🕒 Fecha y Hora:
    - Ejemplo: Feb 02 12:34:56
    - Significado: Momento exacto en que se registró el evento.

- 🖥 Nombre del Servidor:
  - Ejemplo: server
  - Significado: Hostname del equipo que generó el log.

- 🔍 Proceso y PID:
  - Ejemplo: postfix/smtp[12345]
  - Significado: Indica el subproceso de Postfix involucrado (como smtp, smtpd, pickup, etc.) y su ID de proceso (PID).

- 🔖 Queue ID:
  - Ejemplo: ABCDE12345
  - Significado: Identificador único asignado al mensaje dentro de la cola de Postfix, útil para rastrear su recorrido.
    
- 📝 Detalles del Mensaje:
Estos campos ofrecen información específica sobre el manejo del correo:
- `to=`
  - Ejemplo: to=<recipient@example.com>
  - Significado: Dirección del destinatario.
- `relay=`
  - Ejemplo: relay=mail.example.com[192.168.1.1]:25
  - Significado: Servidor relay utilizado para la entrega, su IP y puerto.
delay= y delays=
Ejemplo: delay=0.75, delays=0.1/0/0.4/0.25
Significado:
delay=: Tiempo total de retraso en la entrega.
delays=: Desglose del tiempo en diversas etapas (conexión, procesamiento en cola, transmisión, etc.).
dsn=
Ejemplo: dsn=2.0.0
Significado: Código de estado según el estándar DSN (Delivery Status Notification) que indica el resultado de la entrega.
status=
Ejemplo: status=sent
Significado: Estado final del mensaje (por ejemplo, sent, deferred, bounced).
💬 Mensaje de respuesta:
Ejemplo: (250 OK)
Significado: Respuesta del servidor remoto confirmando la acción (en este caso, que el mensaje fue aceptado).
3. Cómo Analizar y Extraer Información de los Logs 🔎
Para diagnosticar problemas o responder consultas, puedes filtrar y analizar los logs de diversas maneras:

a. Identificación de Errores y Estados
Errores de entrega o estados "deferred" y "bounced":
Comando:
bash
Copiar
grep "deferred" /var/log/mail.log
grep "bounced" /var/log/mail.log
Uso: Permite detectar mensajes que no se entregaron correctamente y entender el motivo mediante el campo DSN.
b. Rastrear un Mensaje Específico
Uso del Queue ID para seguir el recorrido de un correo:
Comando:
bash
Copiar
grep "ABCDE12345" /var/log/mail.log
Uso: Muestra todas las entradas asociadas a ese mensaje para verificar cada paso del proceso.
c. Análisis de Tiempos y Retrasos
Revisar los campos delay y delays:
Objetivo: Identificar cuellos de botella en la entrega.
Un valor elevado en delay o en alguna de las subdivisiones de delays (por ejemplo, en la transmisión) puede señalar problemas en el servidor relay o en la red.
d. Monitoreo de Conexiones y Autenticaciones
Filtrar por el proceso smtpd (entradas de conexión entrante):
Comando:
bash
Copiar
grep "postfix/smtpd" /var/log/mail.log
Uso: Útil para ver intentos de conexión, autenticación y posibles accesos no autorizados.
e. Extracción de Información Específica
Por remitente o destinatario:
Comandos:
bash
Copiar
grep "from=<user@example.com>" /var/log/mail.log
grep "to=<user@example.com>" /var/log/mail.log
Uso: Permite agrupar y analizar la actividad relacionada a una cuenta específica.
f. Generación de Reportes y Estadísticas
Herramientas como pflogsumm:
Comando:
bash
Copiar
pflogsumm /var/log/mail.log
Uso: Resume la actividad del servidor, mostrando estadísticas de entrega, errores, volúmenes de correo y tendencias.
4. Ejemplos Prácticos y Comandos Útiles 💡
Ver los registros de un día específico:
bash
Copiar
grep "Feb 02" /var/log/mail.log
Extraer todos los logs relacionados con un dominio concreto:
bash
Copiar
grep "example.com" /var/log/mail.log
Visualizar los últimos registros para una rápida revisión:
bash
Copiar
tail -n 50 /var/log/mail.log
5. Interpretación de Errores Comunes y Soluciones 🚨
Relay Denied:

Ejemplo en log:
log
Copiar
relay=none
Significado: El servidor no permite el reenvío del mensaje a través del relay configurado.
Solución: Revisar la configuración de smtpd_recipient_restrictions y la autenticación para permitir el relay.
Problemas de Autenticación:

Ejemplo en log:
log
Copiar
warning: SASL authentication failed
Significado: Fallo en la autenticación del usuario, generalmente por credenciales incorrectas o mala configuración SASL.
Solución: Verificar y corregir la configuración de SASL y las credenciales de acceso.
Problemas de DNS:

Ejemplo en log:
log
Copiar
host not found, DNS error
Significado: Fallo en la resolución del nombre del servidor.
Solución: Confirmar que los registros DNS están configurados correctamente y que el servidor puede resolver nombres externos.
