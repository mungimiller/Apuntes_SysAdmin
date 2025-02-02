# ✨ Apuntes de Mantenimiento y Solución de Problemas en WordPress

Guía rápida de **50 acciones** que puedes realizar para **diagnosticar y solucionar** problemas en WordPress **sin acceder al backend**, usando FTP, phpMyAdmin, WP-CLI o SSH.

---

## 1. Restablecer Contraseña de Administrador (Base de Datos) 🔑
- Ve a la tabla `wp_users` en phpMyAdmin.  
- Edita el campo `user_pass` con la función MD5 o [PHPMail hashing en línea](https://phpsolved.com/php-tools/password-hash-generator/) (recomendado SHA-256 o bcrypt, si tu hosting lo permite).  

## 2. Desactivar Plugins (Renombrando Carpetas) 🚫
- Accede vía FTP a `wp-content/plugins`.  
- Renombra la carpeta del plugin conflictivo (ej. `contact-form-7` a `contact-form-7_old`).  
- WordPress desactiva automáticamente esos plugins.

## 3. Cambiar Tema Activo (Renombrando Carpeta) 🎨
- En `wp-content/themes`, localiza la carpeta del tema activo y cámbiale el nombre.  
- WordPress usará un tema por defecto (ej. TwentyTwenty).

## 4. Corregir o Regenerar `.htaccess` ⚙️
- Haz una copia de seguridad del archivo `.htaccess` en la raíz de WP.  
- Elimínalo o renómbralo y deja que WordPress genere uno nuevo (cuando vuelvas a guardar enlaces permanentes).  
- O bien crea uno nuevo con el contenido básico de WP.

## 5. Incrementar Límite de Memoria en `wp-config.php` 💾

` define( 'WP_MEMORY_LIMIT', '256M' );`
Útil para errores de “memory exhausted”.
##  6. Habilitar Modo Depuración (WP_DEBUG) 🐛
define( 'WP_DEBUG', true );
define( 'WP_DEBUG_LOG', true );
define( 'WP_DEBUG_DISPLAY', false );
Registra errores en el archivo wp-content/debug.log.
## 7. Revisar Permisos de Archivos y Carpetas 🔐
Asegúrate de que los archivos tengan permisos 644 y las carpetas 755.
Permisos incorrectos generan errores 403 o 500.
## 8. Reemplazar Archivos de WordPress “Core” 🔁
Descarga una versión limpia de WordPress.org.
Sube y sobrescribe todo menos la carpeta wp-content.
## 9. Comprobar y Ajustar siteurl y home en la Base de Datos 🌐
En la tabla wp_options, localiza siteurl y home.
Ajusta para que coincidan con tu dominio (especialmente tras migraciones).
## 10. Desactivar “Maintenance Mode” 🧰
Si está atascado en modo mantenimiento, elimina el archivo .maintenance en la raíz de WP.
## 11. Desactivar Todos los Plugins de Golpe (Renombrando “plugins”) 🚫
Si no sabes cuál falla, renombra la carpeta wp-content/plugins a plugins_old.
Esto desactiva todos los plugins a la vez.
## 12. Activar un Tema por Defecto Editando la Base de Datos 🎨
En wp_options, busca las opciones template y stylesheet.
Cámbialas a un tema nativo (ej. twentytwentythree).
## 13. Eliminar Transients Problemáticos en la Base de Datos 🌀
En wp_options, busca entradas con option_name que empiece por _transient_ o _site_transient_.
Elimínalas para limpiar datos temporales corruptos.
## 14. Forzar Modo HTTPS en wp-config.php 🔒
php
Copiar
define('FORCE_SSL_ADMIN', true);
Asegura que el acceso y panel se carguen siempre por https.
## 15. Desactivar Actualizaciones Automáticas 🚧
php
Copiar
define('AUTOMATIC_UPDATER_DISABLED', true);
Evita que WP se actualice solo.
## 16. Definir Revisiones Máximas de Entradas 📝
php
Copiar
define('WP_POST_REVISIONS', 3);
Reduce revisiones para mantener la BD más ligera.
O desactívalas con false.
## 17. Limpiar la Papelera Automáticamente 🗑️
php
Copiar
define('EMPTY_TRASH_DAYS', 7);
Vacía la papelera cada 7 días (ajusta según prefieras).
## 18. Editar Manualmente upload_path en la Base de Datos 📂
En wp_options, revisa upload_path para asegurar que apunte a wp-content/uploads (o la ruta que uses).
19. Forzar la URL de Subidas (upload_url_path) 🌐
En wp_options, localiza upload_url_path y confirma que sea la URL correcta para las imágenes.
## 20. Reemplazar Archivos de Plugins Manualmente 🔄
Descarga la versión oficial del plugin.
Sube y sobrescribe vía FTP.
## 21. Desactivar WP-Cron para Usar Cron del Servidor ⏰
php
Copiar
define('DISABLE_WP_CRON', true);
Configura un cron real en tu hosting para reducir carga y potenciales errores de WP Cron.
## 22. Eliminar Reglas Personalizadas en .htaccess ⚠️
Plugins de seguridad o caché pueden inyectar reglas que causen errores.
Comenta o elimina reglas extrañas y prueba.
## 23. Subir un php.ini Personalizado ⚙️
Para modificar memory_limit, upload_max_filesize, post_max_size, etc., si tu hosting lo permite.
## 24. Revisar el Archivo error_log del Servidor 📄
Busca errores de PHP no visibles en el debug de WP.
Te dará pistas de fallas críticas.
## 25. Escanear y Eliminar Código Malicioso 🦠
Busca patrones como eval(base64_decode()).
Usa herramientas de seguridad o un editor con función “buscar en archivos” para rastrear inyecciones.
## 26. Editar Manualmente el Archivo functions.php del Tema 🖌️
Comentando líneas sospechosas que provocan errores fatales.
Ten cuidado con la sintaxis.
## 27. Establecer un Entorno Local para Pruebas 🏠
Copia tu sitio a MAMP/WAMP/LAMP local.
Realiza cambios sin riesgo a la web en producción.
## 28. Reconstruir Estructura de Enlaces Permanentes 🔗
Renombra o borra .htaccess.
En wp_options, revisa permalink_structure.
WordPress regenerará .htaccess al guardar ajustes de enlaces (cuando puedas acceder).
## 29. Eliminar “Locks” en la Tabla wp_options 🔓
Busca registros como core_updater.lock.
Elimínalos si persisten bloqueos de actualización.
## 30. Revisar y Limpiar Base de Datos con WP-CLI 🚀
bash
Copiar
wp db check
wp db optimize
wp db repair
Optimiza y repara tablas desde la línea de comandos.
## 31. Comprobar Versión de PHP y Compatibilidad ⚠️
Asegúrate de usar una versión de PHP soportada (≥7.4 o 8.x ideal).
Incompatibilidades pueden causar errores fatales.
## 32. Crear un Archivo phpinfo.php para Diagnóstico 🩺
php
Copiar
<?php phpinfo(); ?>
Sube a la raíz de tu WordPress y visita tusitio.com/phpinfo.php para ver la configuración del servidor.
## 33. Habilitar y Usar WP-CLI (si el hosting lo permite) 🖥️
Ejemplos:
bash
Copiar
wp plugin update --all
wp user update 1 --user_pass="NuevaContraseña"
Útil para gestionar todo sin backend.
## 34. Crear un Usuario Administrador Nuevo desde WP-CLI 👤
bash
Copiar
wp user create nuevoadmin correo@dominio.com --role=administrator --user_pass="ContraseñaSegura"
Útil si pierdes acceso al usuario admin principal.
## 35. Vaciar la Caché de un Plugin Manualmente 🧹
Plugins como WP Super Cache o W3 Total Cache generan carpetas en wp-content/cache.
Bórralas o renómbralas para forzar regeneración.
## 36. Revisar el Archivo robots.txt 🤖
Asegúrate de no tener Disallow: / o reglas que bloqueen a Google.
Ubicado en la raíz del sitio.
## 37. Inspeccionar Configuraciones de Hosting (web.config, etc.) ⚙️
En hosting Windows (IIS) se usa web.config.
Busca directivas que entren en conflicto con WordPress.
## 38. Validar Configuración de Multisitio (si aplica) 🏢
En wp-config.php, revisa MULTISITE, SUBDOMAIN_INSTALL, DOMAIN_CURRENT_SITE, etc.
Corrige dominios/rutas si has movido el multisitio.
## 39. Verificar el Prefijo de Tablas en wp-config.php 📋
Si cambiaste wp_ por otro prefijo, asegúrate de reflejarlo en wp-config.php.
Inconsistencias rompen el acceso a la BD.
## 40. Cambiar Salts y Keys de Seguridad 🔑
En WordPress.org Secret Key Service obtén nuevas keys.
Reemplaza en wp-config.php. Forzarás sesión nueva para todos.
## 41. Bloquear Acceso a wp-login.php con .htaccess (IP o Auth) 🚧
apache
Copiar
<Files "wp-login.php">
  Order Deny,Allow
  Deny from all
  Allow from 123.45.67.89
</Files>
Reduce ataques de fuerza bruta.
## 42. Desbloquear la Tabla de Comentarios 📨
En phpMyAdmin, revisa si wp_comments o wp_commentmeta está en estado “Locked”.
Usa “Repair table”.
## 43. Forzar un Chequeo de Salud (Site Health) con WP-CLI 🩺
bash
Copiar
wp site health status
Reporta problemas o recomendaciones, similar a la sección Salud del Sitio en el backend.
## 44. Eliminar Restos de Plugins de Seguridad (Wordfence, Sucuri, etc.) 🛡️
Tras desinstalarlos, quedan tablas como wp_wfConfig o carpetas wflogs.
Limpia manualmente si no planeas usarlo de nuevo.
## 45. Reasignar Contenido a Otro Usuario (Base de Datos) 📄
En wp_posts, cambia post_author al ID de un usuario existente si el autor se corrompió.
Evita perder contenido.
## 46. Aumentar Tiempo Máximo de Ejecución (max_execution_time) ⏱️
ini
Copiar
max_execution_time = 300
En .htaccess o php.ini.
Evita “timeout” en procesos largos.
## 47. Optimizar Imágenes Manualmente 🖼️
Descarga imágenes grandes, usa TinyPNG o similares.
Sube nuevamente para mejorar rendimiento sin plugin.
## 48. Comparar wp-config.php con wp-config-sample.php 📝
A veces hay líneas mal añadidas o faltan definiciones.
Útil si sospechas errores de sintaxis o parámetros extraños.
## 49. Eliminar Usuarios Spam Masivamente 🙅‍♂️
En wp_users y wp_usermeta, filtra correos falsos o nombres sospechosos.
Haz copia de seguridad antes de borrar.
## 50. Clonar el Sitio a un Subdominio de Staging 🌱
Copia archivos y BD a un subdominio (ej. staging.tusitio.com).
Ajusta siteurl y home en wp_options.
Prueba cambios allí antes de aplicarlos al sitio principal.
