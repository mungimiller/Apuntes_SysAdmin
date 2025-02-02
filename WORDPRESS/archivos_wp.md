<p align="center">
  <img src="https://s.w.org/about/images/logos/wordpress-logo-stacked-rgb.png" alt="Descripción de la imagen">
</p>


--------------------------------


## 📁 Archivos Principales

- 🌐 **`index.php`** - **La puerta de entrada de todos los visitantes a tu sitio.** Es el archivo principal que carga WordPress.
- ⚙️ **`wp-config.php`** - **El archivo de configuración que contiene la información de la base de datos y otras configuraciones importantes.**
- 🔑 **`wp-login.php`** - **El archivo que maneja el inicio de sesión de los usuarios.**
- ✍️ **`wp-signup.php`** - **Permite a los usuarios registrarse en tu sitio.**
- ⏰ **`wp-cron.php`** - **Maneja las tareas programadas en tu sitio.**
- 🔗 **`wp-links-opml.php`** - **Exporta los enlaces en formato OPML.**
- 🚀 **`wp-load.php`** - **Carga el entorno de WordPress.**
- 📧 **`wp-mail.php`** - **Maneja el correo entrante para los sitios que utilizan la función de correo de WordPress.**
- 🛠️ **`wp-settings.php`** - **Configura los ajustes principales y carga los archivos necesarios para iniciar WordPress.**
- 💬 **`wp-comments-post.php`** - **Maneja el envío de comentarios en tu sitio.**
- ✅ **`wp-activate.php`** - **Activa las cuentas de los usuarios en instalaciones de WordPress Multisite.**
- 📰 **`wp-blog-header.php`** - **Carga el entorno de WordPress y el encabezado del blog.**
- 📡 **`xmlrpc.php`** - **Proporciona servicios XML-RPC para interactuar con tu sitio de WordPress de forma remota.**
- 🔄 **`wp-trackback.php`** - **Maneja las peticiones de trackback.**
- 🛠️ **`wp-admin.php`** - **Punto de entrada principal para la administración del sitio de WordPress.**
- 🛠️ **`wp-comments-popup.php`** - **Maneja la visualización emergente de comentarios.**
- 📤 **`wp-app.php`** - **Proporciona un punto de entrada para las aplicaciones que utilizan el Protocolo AtomPub.**

---

#  👥 Archivos de wp-admin
<details>
  <summary><b>Desplegar menu</b></summary>


- 🛠 **admin-ajax.php**: Maneja las solicitudes AJAX desde el panel de administración y el front-end.
- 🦶 **admin-footer.php**: Incluye el pie de página que aparece en todas las páginas del panel de administración.
- 🛠 **admin-functions.php**: Contiene funciones específicas del panel de administración.
- 🦶 **admin-header.php**: Incluye la cabecera que aparece en todas las páginas del panel de administración.
- 📧 **admin-post.php**: Procesa los datos de las entradas (posts) al ser enviadas.
- 🛠 **admin.php**: La puerta de entrada principal para cargar las páginas del panel de administración.
- 📥 **async-upload.php**: Maneja la carga de archivos de manera asíncrona (AJAX).
- 📝 **comment.php**: Proporciona funcionalidad para gestionar los comentarios.
- 🛠 **credits.php**: Muestra una página con créditos y agradecimientos a los contribuidores de WordPress.
- 🖼 **custom-background.php**: Maneja la personalización de los fondos de pantalla.
- 🖼 **custom-header.php**: Maneja la personalización de las cabeceras del sitio.
- 🛠 **customize.php**: Proporciona la interfaz del personalizador de temas.
- ✏️ **edit-comments.php**: Pantalla para editar, moderar y gestionar comentarios.
- ✏️ **edit-form-advanced.php**: Formulario avanzado para editar entradas.
- ✏️ **edit-form-blocks.php**: Formulario para editar entradas utilizando el editor de bloques.
- 🔗 **edit-link-form.php**: Formulario para editar enlaces (obsoleto).
- ✏️ **edit.php**: Pantalla principal para editar entradas.
- 🛡 **erase-personal-data.php**: Maneja las solicitudes para borrar datos personales.
- 📤 **export-personal-data.php**: Maneja las solicitudes para exportar datos personales.
- 📤 **export.php**: Permite exportar el contenido del sitio en un archivo XML.
- 📜 **freedoms.php**: Muestra una página sobre las libertades de WordPress.
- 🖼 **image-edit.php**: Proporciona la interfaz para editar imágenes.
- ✏️ **import.php**: Proporciona una interfaz para importar contenido desde diferentes plataformas.
- 🏠 **index.php**: La página de inicio del panel de administración.
- 🛠 **install-helper.php**: Ayuda en el proceso de instalación.
- 🛠 **install.php**: Maneja el proceso de instalación de WordPress.
- 🔗 **link-add.php**: Añade nuevos enlaces (obsoleto).
- 🔗 **link-manager.php**: Gestiona los enlaces (obsoleto).
- 🔗 **link-parse-opml.php**: Analiza archivos OPML para importar enlaces.
- 🔗 **link.php**: Maneja la edición y gestión de enlaces.
- 🛠 **load-scripts.php**: Carga scripts de JavaScript.
- 🛠 **load-styles.php**: Carga archivos CSS.
- 🛠 **maintenance.php**: Modo de mantenimiento.
- 🖼 **media-new.php**: Añade nuevos archivos multimedia.
- 🖼 **media-upload.php**: Maneja la subida de archivos multimedia.
- 📜 **menu-header.php**: Maneja la construcción del menú de navegación.
- 📜 **menu.php**: Gestiona la navegación y los menús.
- 📝 **moderation.php**: Maneja la moderación de comentarios.
- 🏠 **my-sites.php**: Muestra una lista de los sitios en una red multisite.
- 📜 **nav-menus.php**: Proporciona la interfaz para gestionar los menús de navegación personalizados.
- 🏠 **network.php**: Gestiona la administración de una red multisite.
- ⚙️ **options-discussion.php**: Configura opciones de discusión y comentarios.
- ⚙️ **options-general.php**: Configura opciones generales del sitio.
- 🛠 **options-head.php**: Cabecera para páginas de opciones.
- ⚙️ **options-media.php**: Configura opciones de medios.
- ⚙️ **options-permalink.php**: Configura los enlaces permanentes (permalinks).
- ⚙️ **options-privacy.php**: Configura las opciones de privacidad.
- ⚙️ **options-reading.php**: Configura opciones de lectura.
- ⚙️ **options-writing.php**: Configura opciones de escritura.
- ✏️ **plugin-editor.php**: Proporciona una interfaz para editar archivos de plugins.
- ➕ **plugin-install.php**: Permite instalar nuevos plugins.
- 🔌 **plugins.php**: Gestiona la lista de plugins instalados.
- 📝 **post-new.php**: Crea una nueva entrada.
- ✏️ **post.php**: Maneja la edición de entradas individuales.
- 🛡 **privacy.php**: Gestiona las opciones de privacidad.
- 👤 **profile.php**: Permite a los usuarios editar su perfil.
- 📝 **revision.php**: Muestra revisiones de entradas.
- 🛠 **setup-config.php**: Guía al usuario a través del proceso de configuración inicial.
- 🏥 **site-health-info.php**: Proporciona información sobre el estado de salud del sitio.
- 🏥 **site-health.php**: Muestra la página de salud del sitio.
- 🏷 **term.php**: Maneja la edición de términos (categorías, etiquetas).
- 🎨 **themes.php**: Gestiona la interfaz de administración de temas.
- 🛠 **tools.php**: Proporciona herramientas adicionales para la gestión del sitio.
- 🔄 **update-core.php**: Gestiona las actualizaciones del núcleo de WordPress.
- 🔄 **update.php**: Maneja las actualizaciones del sitio.
- 🖼 **upload.php**: Muestra la biblioteca de medios.
- 🛠 **upgrade.php**: Maneja el proceso de actualización.
- 👤 **user-edit.php**: Edita los perfiles de usuario.
- ➕ **user-new.php**: Añade nuevos usuarios.
- 👥 **users.php**: Gestiona la lista de usuarios.
- 🖥 **widgets.php**: Proporciona la interfaz para gestionar los widgets.
- 🔧 **xmlrpc.php**: Maneja las solicitudes XML-RPC para interacciones remotas.

## Carpetas dentro de `wp-admin`

- 📁 **css/**: Contiene archivos CSS que definen los estilos del panel de administración.
- 📁 **images/**: Contiene imágenes utilizadas en la interfaz del panel de administración.
- 📁 **includes/**: Contiene archivos PHP adicionales que proporcionan funcionalidad específica para el backend.
- 📁 **js/**: Contiene archivos JavaScript utilizados para mejorar la funcionalidad y la interactividad del panel de administración.
- 📁 **maint/**: Archivos relacionados con las tareas de mantenimiento.
- 📁 **network/**: Gestiona la administración de una red multisite.
- 📁 **user/**: Contiene archivos para la gestión de usuarios y perfiles.

# Archivos en la carpeta `wp-admin/users`

- 📝 `about.php` - Proporciona información sobre la versión actual de WordPress y sus características.
- 🔧 `admin.php` - Archivo principal de administración que actúa como controlador para diferentes secciones del panel de administración.
- 📜 `contribute.php` - Contiene información sobre cómo contribuir al proyecto WordPress.
- ⭐ `credits.php` - Muestra los créditos de los desarrolladores y colaboradores de WordPress.
- 🕊️ `freedoms.php` - Describe las libertades del software libre que WordPress proporciona.
- 📑 `index.php` - Archivo de índice que redirige a la página principal de administración.
- 🍔 `menu.php` - Define y controla la estructura del menú de administración.
- 🛡️ `privacy.php` - Información sobre las políticas de privacidad de WordPress.
- 🧑‍💻 `profile.php` - Permite a los usuarios editar su propio perfil.
- ✏️ `user-edit.php` - Permite editar los detalles de un usuario específico.

</details>

## 🖼️ Archivos de Plantillas

- 🚫 **`404.php`** - **Muestra un mensaje de error cuando una página no se encuentra.**
- 📂 **`archive.php`** - **Muestra los archivos de tus publicaciones.**
- 💬 **`comments.php`** - **Muestra y gestiona los comentarios.**
- 📜 **`footer.php`** - **El pie de página de tu sitio.**
- 🏷️ **`header.php`** - **El encabezado de tu sitio.**
- 📝 **`single.php`** - **Muestra una única publicación.**
- 📄 **`page.php`** - **Muestra una página individual.**

---

## 🎨 Archivos del Tema

- 🧩 **`functions.php`** - **Añade funciones personalizadas a tu tema.**
- 🎨 **`style.css`** - **Contiene los estilos CSS de tu tema.**

---

## 📄 Otros Archivos

- 📄 **`license.txt`** - **Contiene los términos de la licencia de WordPress.**
- 📘 **`readme.html`** - **Muestra información sobre tu instalación de WordPress.**

---
