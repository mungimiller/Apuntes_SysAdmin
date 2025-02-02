# 📧 Instalación y Configuración Completa de Postfix

Este documento proporciona una guía detallada para instalar y configurar Postfix en un servidor Linux (por ejemplo, Ubuntu/Debian) desde cero. Se incluyen ajustes avanzados para asegurar, optimizar y mantener el servicio de correo.

---

## 1. Requisitos Previos

- **Sistema Operativo:** Servidor Linux (Ubuntu, Debian, etc.) con acceso root o sudo.
- **Dominio Propio:** Un dominio (ej. `example.com`) con:
  - Registro MX apuntando al servidor.
  - Registros DNS SPF, DKIM y DMARC configurados (para mejorar la entregabilidad).
- **Certificados SSL:** Opcionalmente, certificados autofirmados o de Let's Encrypt para TLS.
- **Herramientas básicas:** Editor de texto, terminal y acceso a Internet.

---

## 2. Instalación de Postfix

### 2.1 Actualizar el Sistema
```
sudo apt-get update && sudo apt-get upgrade -y
```
### 2.2 Instalar Postfix
```
sudo apt-get install postfix -y
```
Durante la instalación se te preguntará por el tipo de configuración. Selecciona "Sitio de Internet" (Internet Site).
Introduce tu dominio (ej. example.com) cuando se solicite.

## 3. Configuración Básica de Postfix
### 3.1 Configurar el Archivo main.cf
Edita el archivo /etc/postfix/main.cf para definir los parámetros básicos. Por ejemplo:
```
# Nombre del host del servidor
myhostname = mail.example.com

# Dominio del servidor
mydomain = example.com

# Origen de los correos salientes
myorigin = /etc/mailname

# Dominios locales aceptados
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain

# Redes autorizadas para enviar correo
mynetworks = 127.0.0.0/8, [::1]/128

# Método de entrega (sin relay host, en este caso)
relayhost = 

# Directorio para almacenar los correos en formato Maildir (opcional)
home_mailbox = Maildir/
Guarda y cierra el archivo.
```

### 3.2 Revisión de master.cf
El archivo /etc/postfix/master.cf define los servicios que Postfix ofrece. La configuración por defecto suele funcionar, pero podrás ajustarla en función de necesidades puntuales (por ejemplo, habilitar servicios adicionales o cambiar puertos).

## 4. Configuración Avanzada para un Servicio de Correo Óptimo
### 4.1 Habilitar TLS/SSL
Para asegurar la comunicación mediante conexiones cifradas:

Generar certificados (opcional – autofirmado o usar Let's Encrypt):

Generación de certificados autofirmados:

```
sudo openssl req -new -x509 -days 365 -nodes -out /etc/ssl/certs/postfix.pem -keyout /etc/ssl/private/postfix.key
sudo chmod 600 /etc/ssl/private/postfix.key
```
Configurar TLS en main.cf:

Añade estas líneas:

```
smtpd_tls_cert_file = /etc/ssl/certs/postfix.pem
smtpd_tls_key_file = /etc/ssl/private/postfix.key
smtpd_use_tls = yes
smtpd_tls_session_cache_database = btree:${data_directory}/smtpd_scache
smtp_tls_session_cache_database = btree:${data_directory}/smtp_scache
```
### 4.2 Habilitar Autenticación SASL
Para permitir que usuarios autenticados envíen correos (evitando el uso indebido por terceros):

Instalar módulos SASL:

```
sudo apt-get install libsasl2-modules -y
```
Configurar Postfix para usar SASL:

En main.cf añade:
```
smtpd_sasl_auth_enable = yes
smtpd_sasl_security_options = noanonymous
smtpd_sasl_local_domain = $myhostname
broken_sasl_auth_clients = yes
smtpd_recipient_restrictions = permit_sasl_authenticated, permit_mynetworks, reject_unauth_destination
```
Configurar Cyrus SASL:
Crea (o edita) el archivo /etc/postfix/sasl/smtpd.conf con el siguiente contenido:
```
pwcheck_method: saslauthd
mech_list: PLAIN LOGIN
```
Inicia el servicio SASL:
```
sudo service saslauthd start
```
### 4.3 Configurar DKIM (Opcional pero Recomendado)
Firmar los correos salientes mejora la reputación y la entregabilidad.

Instalar OpenDKIM:
```
sudo apt-get install opendkim opendkim-tools -y
```
Configurar OpenDKIM:

Edita el archivo /etc/opendkim.conf para definir parámetros como Domain, KeyFile, etc.
Genera las llaves DKIM:
```
sudo opendkim-genkey -s default -d example.com
sudo chown opendkim:opendkim default.private
```

Integrar OpenDKIM con Postfix:

Añade en main.cf:
```
milter_default_action = accept
milter_protocol = 6
smtpd_milters = inet:localhost:8891
non_smtpd_milters = inet:localhost:8891
```

Reinicia ambos servicios:
```
sudo systemctl restart opendkim postfix
```
### 4.4 Configuración de SPF y DMARC
Aunque estos registros se configuran en tu DNS, asegúrate de:

SPF: Añadir un registro TXT en tu zona DNS, por ejemplo:
```
"v=spf1 mx a ip4:TU_IP -all"
```
DMARC: Añadir un registro TXT en tu zona DNS, por ejemplo:
```
_dmarc.example.com. IN TXT "v=DMARC1; p=none; rua=mailto:postmaster@example.com"
```
### 4.5 Ajustes de Rendimiento y Seguridad
Puedes mejorar el rendimiento y la seguridad mediante ajustes adicionales en main.cf:

Limitar conexiones y procesos:
```
default_process_limit = 100
smtpd_client_connection_count_limit = 10
smtpd_client_connection_rate_limit = 30
```
Tamaño máximo de mensaje:
```
message_size_limit = 52428800  # 50 MB
```
Control de la cola de correo: Utiliza mailq para revisar la cola y postsuper para gestionar mensajes atascados.

## 5. Reiniciar y Probar el Servicio
Después de realizar todos los cambios, reinicia Postfix:
```
sudo systemctl restart postfix
```
Verifica su estado:
```
sudo systemctl status postfix
```
Realiza pruebas enviando correos y revisa los logs en:
```
tail -f /var/log/mail.log
```
