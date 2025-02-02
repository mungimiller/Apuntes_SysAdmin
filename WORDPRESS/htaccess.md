# 🚀 50 Funcionalidades Útiles en .htaccess para Gestionar tu Web

Este documento resume **50 funcionalidades** y ejemplos de código que puedes aplicar en tu archivo `.htaccess` para mejorar la seguridad, el rendimiento y la administración de tu sitio web.

---

## 1. 🚫 Prevenir la Listado de Directorios  
Desactiva la navegación por directorios para evitar que los visitantes vean la estructura de archivos.

Options -Indexes
## 2. ❗ Personalizar Páginas de Error
Muestra páginas de error personalizadas para mejorar la experiencia del usuario.

ErrorDocument 404 /errors/404.html
ErrorDocument 500 /errors/500.html
## 3. 🔒 Redirigir de HTTP a HTTPS
Fuerza el uso de conexiones seguras.
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
## 4. 🌐 Redirigir de WWW a No-WWW
Forzar el dominio sin "www".

apache
Copiar
RewriteEngine On
RewriteCond %{HTTP_HOST} ^www\.(.*)$ [NC]
RewriteRule ^(.*)$ https://%1/$1 [L,R=301]
## 5. 🌐 Redirigir de No-WWW a WWW
Forzar el dominio con "www".

apache
Copiar
RewriteEngine On
RewriteCond %{HTTP_HOST} !^www\. [NC]
RewriteRule ^(.*)$ https://www.%{HTTP_HOST}/$1 [L,R=301]
6. 🚫 Bloquear IPs Específicas
Impide el acceso a usuarios o bots maliciosos.

apache
Copiar
Order Deny,Allow
Deny from 123.456.789.000
## 7. ✅ Permitir Solo IPs Específicas
Restringe el acceso solo a direcciones IP de confianza.

apache
Copiar
Order Deny,Allow
Deny from all
Allow from 111.222.333.444
## 8. ⚡ Habilitar Compresión Gzip
Reduce el tamaño de las respuestas para mejorar la velocidad de carga.

apache
Copiar
<IfModule mod_deflate.c>
  AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css application/javascript
</IfModule>
## 9. ⏳ Aprovechar la Caché del Navegador
Configura encabezados de expiración para recursos estáticos.

apache
Copiar
<IfModule mod_expires.c>
  ExpiresActive On
  ExpiresByType image/jpg "access plus 1 year"
  ExpiresByType image/jpeg "access plus 1 year"
  ExpiresByType image/gif "access plus 1 year"
  ExpiresByType image/png "access plus 1 year"
  ExpiresByType text/css "access plus 1 month"
  ExpiresByType application/pdf "access plus 1 month"
  ExpiresByType application/javascript "access plus 1 month"
  ExpiresByType text/javascript "access plus 1 month"
  ExpiresByType text/html "access plus 600 seconds"
</IfModule>
## 10. 🚫 Prevenir Hotlinking de Imágenes
Impide que otros sitios utilicen tus imágenes.

apache
Copiar
RewriteEngine On
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^https?://(www\.)?tudominio.com/ [NC]
RewriteRule \.(jpg|jpeg|png|gif)$ - [F]
## 11. 📝 Establecer Tipos MIME para Fuentes Web
Asegura que los navegadores reconozcan correctamente las fuentes.

apache
Copiar
AddType application/font-woff2 .woff2
AddType application/font-woff .woff
## 12. 🔒 Proteger el Archivo .htaccess
Evita el acceso público al propio archivo de configuración.

apache
Copiar
<Files .htaccess>
  Order allow,deny
  Deny from all
</Files>
## 13. 🔒 Proteger el Archivo .htpasswd
Impide el acceso al archivo de contraseñas.

apache
Copiar
<Files .htpasswd>
  Order allow,deny
  Deny from all
</Files>
## 14. 🔀 Redirigir Páginas Antiguas a Nuevas
Gestiona cambios de URL con redirecciones 301.

apache
Copiar
Redirect 301 /old-page.html /new-page.html
## 15. 🔗 Reescribir URLs para Mayor Amigabilidad
Ejemplo para transformar URLs dinámicas en amigables.

apache
Copiar
RewriteEngine On
RewriteRule ^producto/([0-9]+)/?$ /producto.php?id=$1 [L,QSA]
## 16. 🚫 Bloquear Tipos de Archivo Específicos
Impide el acceso a archivos con extensiones sensibles.

apache
Copiar
<FilesMatch "\.(bak|inc|sql)$">
  Order allow,deny
  Deny from all
</FilesMatch>
## 17. ⬇️ Forzar la Descarga de Archivos
Hace que ciertos archivos se descarguen en lugar de abrirse en el navegador.

apache
Copiar
<FilesMatch "\.(?i:pdf)$">
  ForceType application/octet-stream
  Header set Content-Disposition attachment
</FilesMatch>
## 18. 🚫 Bloquear Archivos de Respaldo
Impide el acceso a archivos de respaldo que puedan estar expuestos.

apache
Copiar
<FilesMatch "\.(zip|tar|gz|rar)$">
  Order allow,deny
  Deny from all
</FilesMatch>
## 19. 🔠 Establecer la Codificación de Caracteres
Define la codificación por defecto para el sitio.

apache
Copiar
AddDefaultCharset UTF-8
## 20. 🌐 Habilitar CORS (Cross-Origin Resource Sharing)
Permite compartir recursos con otros dominios.

apache
Copiar
<IfModule mod_headers.c>
  Header set Access-Control-Allow-Origin "*"
</IfModule>
## 21. 🛡️ Añadir Cabeceras de Seguridad
Protege el sitio con cabeceras que mitigan ataques comunes.

apache
Copiar
Header set X-Frame-Options "SAMEORIGIN"
Header set X-XSS-Protection "1; mode=block"
Header set X-Content-Type-Options "nosniff"
## 22. 🔐 Configurar Content Security Policy (CSP)
Restringe los orígenes de los recursos que puede cargar el navegador.

apache
Copiar
Header set Content-Security-Policy "default-src 'self'; script-src 'self' https://trustedscripts.com"
## 23. 🔒 Habilitar HTTP Strict Transport Security (HSTS)
Forzar HTTPS en futuras solicitudes.

apache
Copiar
Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
## 24. 🧹 Eliminar Barras Innecesarias al Final de la URL
Limpia las URLs removiendo la barra final.

apache
Copiar
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)/$ /$1 [L,R=301]
## 25. 🛑 Forzar Barra Final en Directorios
Asegura que los directorios terminen con una barra.

apache
Copiar
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} -d
RewriteRule ^(.*[^/])$ /$1/ [L,R=301]
## 26. 🚫 Bloquear el Acceso a Archivos Ocultos
Impide el acceso a archivos que comienzan con un punto.

apache
Copiar
<FilesMatch "^\.">
  Order allow,deny
  Deny from all
</FilesMatch>
## 27. 🚫 Deshabilitar XML-RPC
Previene ataques de fuerza bruta a través de XML-RPC.

apache
Copiar
<Files xmlrpc.php>
  Order deny,allow
  Deny from all
</Files>
## 28. 🚫 Restringir Acceso por Referrer
Bloquea solicitudes provenientes de referrers no deseados.

apache
Copiar
RewriteEngine On
RewriteCond %{HTTP_REFERER} spamdomain\.com [NC]
RewriteRule .* - [F]
## 29. 🚫 Bloquear Bots No Deseados
Impide el acceso a bots conocidos por comportamientos maliciosos.

apache
Copiar
RewriteEngine On
RewriteCond %{HTTP_USER_AGENT} (BadBot1|BadBot2) [NC]
RewriteRule .* - [F,L]
## 30. 📝 Habilitar el Registro de Mod_Rewrite (Solo para Depuración)
Activa el registro de reescritura para fines de depuración (deshabilitar en producción).

apache
Copiar
RewriteLog "/var/log/apache2/rewrite.log"
RewriteLogLevel 3
## 31. ❌ Personalizar la Página de Error 403
Muestra una página personalizada para accesos prohibidos.

apache
Copiar
ErrorDocument 403 /errors/403.html
## 32. ❌ Personalizar la Página de Error 500
Muestra una página personalizada para errores internos del servidor.

apache
Copiar
ErrorDocument 500 /errors/500.html
## 33. 🚫 Bloquear la Ejecución de PHP en la Carpeta de Subidas
Evita la ejecución de archivos PHP en la carpeta de uploads.

apache
Copiar
<Directory "/ruta/a/wp-content/uploads">
  <Files "*.php">
    Order Deny,Allow
    Deny from all
  </Files>
</Directory>
Nota: Esto puede requerir configuración adicional según el servidor.

## 34. 📱 Redirigir Usuarios Móviles a una Versión Móvil
Detecta agentes móviles y redirige a una versión específica del sitio.

apache
Copiar
RewriteEngine On
RewriteCond %{HTTP_USER_AGENT} "Android|iPhone|iPad" [NC]
RewriteRule ^$ https://m.tudominio.com/ [L,R=302]
## 35. 🚫 Bloquear Acceso con Consultas Sospechosas
Impide solicitudes que contengan parámetros maliciosos.

apache
Copiar
RewriteCond %{QUERY_STRING} (?:union|select|drop) [NC]
RewriteRule .* - [F]
## 36. 🔀 Redirigir URLs con Parámetros Específicos
Redirige URLs que contienen ciertos parámetros.

apache
Copiar
RewriteCond %{QUERY_STRING} (^|&)oldparam= [NC]
RewriteRule ^(.*)$ /nuevapagina? [R=301,L]
## 37. 🚫 Bloquear Métodos HTTP No Permitidos
Limita los métodos HTTP solo a GET, POST y HEAD.

apache
Copiar
RewriteEngine On
RewriteCond %{REQUEST_METHOD} !^(GET|POST|HEAD)$
RewriteRule .* - [F]
## 38. 🚫 Prevenir la Listado de Directorios en Subdirectorios
Asegura que la opción de listado de directorios esté desactivada en todas las carpetas.

apache
Copiar
Options -Indexes
## 39. 🔀 Unificar URLs con o sin Barra Final
Gestiona URLs de forma consistente sin importar la presencia de la barra final.

apache
Copiar
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)/$ /$1 [R=301,L]
## 40. 🛠️ Redirigir a una Página de Mantenimiento
Redirige temporalmente a todos los usuarios a una página de mantenimiento.

apache
Copiar
RewriteEngine On
RewriteCond %{REMOTE_ADDR} !^123\.456\.789\.000
RewriteRule ^(.*)$ /maintenance.html [R=302,L]
## 41. 🚫 Bloquear Archivos con Extensiones Específicas
Impide el acceso a archivos con extensiones potencialmente peligrosas.

apache
Copiar
<FilesMatch "\.(inc|bak|old)$">
  Order allow,deny
  Deny from all
</FilesMatch>
## 42. ⬇️ Forzar Descarga para Tipos de Archivo Específicos
Forzar que ciertos archivos se descarguen.

apache
Copiar
<FilesMatch "\.(zip|rar)$">
  ForceType application/octet-stream
  Header set Content-Disposition attachment
</FilesMatch>
## 43. ⏳ Configurar Encabezados Expires para Archivos Específicos
Establece fechas de expiración para tipos de archivo determinados.

apache
Copiar
<IfModule mod_expires.c>
  ExpiresByType image/png "access plus 1 year"
  ExpiresByType image/jpeg "access plus 1 year"
  ExpiresByType text/css "access plus 1 month"
</IfModule>
## 44. 🚫 Prevenir Hotlinking con Mensaje Personalizado
Evita el uso no autorizado de tus imágenes y muestra un mensaje de error.

apache
Copiar
RewriteEngine On
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^https?://(www\.)?tudominio.com/ [NC]
RewriteRule \.(jpg|jpeg|png|gif)$ - [F,L]
## 45. 🔀 Redirigir Secciones Antiguas a Nuevas
Redirige usuarios de una sección antigua a la nueva.

apache
Copiar
RewriteEngine On
RewriteCond %{REQUEST_URI} ^/old-section/
RewriteRule ^(.*)$ /new-section/ [R=301,L]
## 46. 🔗 Regla Básica de Reescritura para URLs Amigables
Ejemplo sencillo de reescritura para transformar parámetros en URLs limpias.

apache
Copiar
RewriteEngine On
RewriteRule ^page/([0-9]+)$ /page.php?id=$1 [L,QSA]
## 47. 🚫 Bloquear Acceso a Directorios Sensibles
Impide el acceso a carpetas que no deben ser públicas.

apache
Copiar
<Directory "/ruta/a/directorio-sensible">
  Order allow,deny
  Deny from all
</Directory>
## 48. 📂 Habilitar Listado de Directorio en una Carpeta Específica
Permite la visualización de directorios solo en carpetas autorizadas.

apache
Copiar
<Directory "/ruta/a/carpeta-publica">
  Options +Indexes
</Directory>
## 49. 🔒 Forzar HTTPS en un Directorio o Archivo Específico
Aplica redirección a HTTPS solo en una parte del sitio.

apache
Copiar
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule ^secure-folder/(.*)$ https://%{HTTP_HOST}/secure-folder/$1 [L,R=301]
## 50. 🚀 Habilitar HTTP/2 Push (si es compatible)
Envía recursos al navegador de forma proactiva para mejorar el rendimiento.

apache
Copiar
<IfModule http2_module>
  H2PushResource "/css/style.css"
  H2PushResource "/js/script.js"
</IfModule>
