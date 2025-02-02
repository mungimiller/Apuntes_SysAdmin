![image](https://github.com/mungimiller/tickets/assets/73543470/eb7b39f0-ac41-4efa-ba45-98fec579e147)


Esta guía contiene una lista de **50 comandos útiles de WP-CLI** para administrar y mantener sitios de WordPress.

---

# 🔧 Instalación y Actualización

### 📝 Verificar la versión de WP-CLI
```bash
wp --version
```

### 🔍 Verificar la versión de WordPress
```bash
wp core version
```

### 🔄 Comprobar actualizaciones de WordPress
```bash
wp core check-update
```

### ⬆️ Actualizar WordPress a la última versión
```bash
wp core update
```

### 🛠️ Actualizar la base de datos después de actualizar WordPress
```bash
wp core update-db
```

---

# 🔌 Gestión de Plugins

### 📋 Listar todos los plugins instalados
```bash
wp plugin list
```

### 📥 Instalar un plugin
```bash
wp plugin install nombre-del-plugin --activate
```

### ✅ Activar un plugin
```bash
wp plugin activate nombre-del-plugin
```

### 🚫 Desactivar un plugin
```bash
wp plugin deactivate nombre-del-plugin
```

### ❌ Eliminar un plugin
```bash
wp plugin delete nombre-del-plugin
```

### 🔄 Actualizar todos los plugins
```bash
wp plugin update --all
```

### 🚫 Desactivar todos los plugins
```bash
wp plugin deactivate --all
```

### ✅ Activar todos los plugins
```bash
wp plugin activate --all
```

### 🚫 Desactivar un plugin en una red multisite
```bash
wp plugin deactivate nombre-del-plugin --network
```

---

# 🎨 Gestión de Temas

### 📋 Listar todos los temas instalados
```bash
wp theme list
```

### 📥 Instalar un tema
```bash
wp theme install nombre-del-tema --activate
```

### ✅ Activar un tema
```bash
wp theme activate nombre-del-tema
```

### ❌ Eliminar un tema
```bash
wp theme delete nombre-del-tema
```

### 🔄 Actualizar todos los temas
```bash
wp theme update --all
```

### 🔍 Buscar temas en el repositorio de temas de WordPress
```bash
wp theme search nombre-del-tema
```

### 🛠️ Ver detalles de un tema
```bash
wp theme get nombre-del-tema
```

---

# 🛡️ Gestión de Usuarios

### 📋 Listar todos los usuarios
```bash
wp user list
```

### 🆕 Crear un nuevo usuario
```bash
wp user create nombre-usuario email@example.com --role=autor --user_pass=password
```

### 🔄 Cambiar la contraseña de un usuario
```bash
wp user update nombre-usuario --user_pass=nueva-contraseña
```

### ❌ Eliminar un usuario
```bash
wp user delete nombre-usuario --reassign=otro-usuario
```

### 🛠️ Actualizar la información de un usuario
```bash
wp user update nombre-usuario --display_name="Nuevo Nombre"
```

### 🧪 Generar usuarios de prueba
```bash
wp user generate --count=5
```

---

# 📄 Gestión de Publicaciones

### 📋 Listar todas las publicaciones
```bash
wp post list
```

### 🆕 Crear una nueva publicación
```bash
wp post create --post_title="Mi Nueva Publicación" --post_content="Contenido de la publicación." --post_status=publish
```

### 🔄 Actualizar una publicación
```bash
wp post update ID-de-la-publicación --post_title="Título Actualizado"
```

### ❌ Eliminar una publicación
```bash
wp post delete ID-de-la-publicación
```

### 🛠️ Ver el contenido de una publicación
```bash
wp post get ID-de-la-publicación --field=content
```

### 🔄 Cambiar el estado de una publicación
```bash
wp post update ID-de-la-publicación --post_status=draft
```

---

# 🔍 Búsqueda y Reemplazo

### 🔍 Buscar y reemplazar en la base de datos
```bash
wp search-replace 'http://old-url.com' 'http://new-url.com'
```

### 🔍 Buscar y reemplazar en un campo específico
```bash
wp search-replace 'old-text' 'new-text' wp_posts --field=post_content
```

---

## ⚙️ Mantenimiento

### 🧹 Limpiar la caché de transitorios
```bash
wp transient delete --all
```

### 🔧 Optimizar la base de datos
```bash
wp db optimize
```

### 🛠️ Reparar la base de datos
```bash
wp db repair
```

### 🔄 Restablecer la base de datos a su estado inicial
```bash
wp db reset --yes
```

### 💾 Realizar una copia de seguridad de la base de datos
```bash
wp db export nombre-del-archivo.sql
```

### 📥 Importar una base de datos
```bash
wp db import nombre-del-archivo.sql
```

---

# 🔄 Importar y Exportar

### 📤 Exportar el contenido del sitio
```bash
wp export
```

### 📥 Importar contenido a partir de un archivo de exportación
```bash
wp import archivo-de-exportacion.xml --authors=create
```

---

## 🧰 Utilidades Varias

### 🧪 Generar datos de prueba (publicaciones, términos, usuarios)
```bash
wp post generate --count=10
wp term generate --count=10
wp user generate --count=10
```

### 🏗️ Instalar un sitio multisite
```bash
wp core multisite-install --url=example.com --base=/ --subdomains --title="Mi Red" --admin_user=admin --admin_password=password --admin_email=admin@example.com
```

### 🆕 Crear un nuevo sitio en una red multisite
```bash
wp site create --slug=nuevo-sitio --title="Nuevo Sitio" --email=email@example.com
```

### ❌ Eliminar un sitio en una red multisite
```bash
wp site delete ID-del-sitio
```

### 📋 Listar todos los sitios en una red multisite
```bash
wp site list
```

### ✅ Activar un tema en una red multisite
```bash
wp theme enable nombre-del-tema --network
```

### 🚫 Desactivar un tema en una red multisite
```bash
wp theme disable nombre-del-tema --network
```

---
