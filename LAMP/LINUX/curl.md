![image](https://github.com/user-attachments/assets/96fa625e-e712-47b4-be43-c60b0aea3918)

# CURL AL COMPLETO

A continuación se muestra un resumen detallado de las funciones y parámetros más importantes que puedes utilizar con el comando cURL. Ten en cuenta que cURL es muy versátil y soporta múltiples protocolos (HTTP, HTTPS, FTP, SFTP, LDAP, etc.), por lo que existen muchísimas opciones; este resumen abarca las funcionalidades más comunes y útiles, aunque para conocer la lista completa se recomienda revisar la documentación oficial de cURL.

1. Uso Básico
Sintaxis general:
bash
Copiar
curl [opciones] [URL...]
Ejemplo sencillo:
bash
Copiar
curl https://www.example.com
2. Opciones Generales y de Visualización
Ayuda y versión:

-h, --help
Muestra la ayuda y lista de opciones disponibles.
-V, --version
Muestra la versión de cURL y los protocolos soportados.
Mensajes y depuración:

-v, --verbose
Muestra información detallada del proceso de conexión y de la transferencia.
-s, --silent
Ejecuta cURL en modo silencioso, sin mostrar la barra de progreso ni mensajes informativos.
-S, --show-error
Cuando se usa con -s, muestra los errores en caso de que ocurran.
-#
Muestra una barra de progreso en lugar de la salida por defecto.
Ejemplo:

bash
Copiar
curl -v https://www.example.com
3. Opciones de Conexión y Control de Red
Redirecciones:

-L, --location
Sigue automáticamente las redirecciones que devuelva el servidor.
--max-redirs <número>
Limita el número de redirecciones a seguir.
Tiempos de espera y velocidad:

--connect-timeout <segundos>
Define el tiempo máximo para establecer la conexión.
--max-time <segundos>
Tiempo total máximo permitido para la transferencia.
--limit-rate <velocidad>
Limita la velocidad de transferencia (ejemplo: 100k, 1m).
Ejemplo:

bash
Copiar
curl --connect-timeout 10 --max-time 30 --limit-rate 500k https://www.example.com
4. Opciones para Solicitudes HTTP y Envío de Datos
Método HTTP:

-X, --request <método>
Especifica el método HTTP a utilizar (GET, POST, PUT, DELETE, etc.).
Envío de datos:

-d, --data <datos>
Envía datos en una petición (normalmente en POST). Los datos se envían como URL-encoded.
--data-urlencode <dato>
Envía datos codificados en URL, muy útil para caracteres especiales.
-F, --form <nombre=contenido>
Envía datos en formato multipart/form-data, ideal para subir archivos o enviar formularios complejos.
-G, --get
Convierte la solicitud para que sea GET y adjunta los datos a la URL.
Solo cabeceras:

-I, --head
Realiza una petición HEAD para obtener únicamente las cabeceras de la respuesta.
Ejemplo (envío de datos en POST):

bash
Copiar
curl -X POST -d "usuario=juan&clave=1234" https://www.example.com/login
5. Gestión de Encabezados (Headers) y Cookies
Encabezados personalizados:

-H, --header <cabecera>
Añade una cabecera personalizada a la solicitud. Puedes usar varias veces esta opción para múltiples encabezados.
Cookies:

-b, --cookie <dato o archivo>
Envía cookies con la solicitud. Puedes pasar el contenido directamente o leerlas desde un archivo.
-c, --cookie-jar <archivo>
Guarda las cookies recibidas en un archivo, lo que resulta útil para mantener sesiones entre solicitudes.
Ejemplo:

bash
Copiar
curl -H "Accept: application/json" -b "session=abc123" https://www.example.com
6. Autenticación
Autenticación básica y métodos asociados:

-u, --user <usuario:contraseña>
Utiliza autenticación básica HTTP con las credenciales indicadas.
--anyauth
Permite que cURL determine automáticamente el método de autenticación más seguro soportado por el servidor.
--basic
Fuerza el uso de autenticación básica.
--digest
Fuerza el uso de autenticación digest.
--ntlm
Utiliza autenticación NTLM.
--negotiate
Emplea autenticación SPNEGO (Kerberos).
Ejemplo:

bash
Copiar
curl -u admin:pass123 https://www.example.com/privado
7. Configuración de Proxy
Uso de proxy:

-x, --proxy <[protocolo://]host:puerto>
Define un servidor proxy a través del cual se realizará la solicitud.
-U, --proxy-user <usuario:contraseña>
Proporciona las credenciales para autenticar el proxy.
--socks5 <host:puerto>
Configura el uso de un proxy SOCKS5.
Ejemplo:

bash
Copiar
curl -x http://proxy.example.com:8080 https://www.example.com
8. Gestión de la Salida y Transferencia de Archivos
Guardar la salida:

-o, --output <archivo>
Guarda la respuesta en el archivo especificado.
-O, --remote-name
Guarda la respuesta usando el nombre de archivo sugerido por el servidor.
-J, --remote-header-name
Utiliza el nombre de archivo indicado en la cabecera Content-Disposition (si está presente).
--create-dirs
Crea directorios necesarios en la ruta indicada en -o.
Reanudación de descargas y subida de archivos:

-C, --continue-at <offset>
Reanuda una descarga interrumpida a partir del offset indicado.
-T, --upload-file <archivo>
Sube un archivo a un servidor. Funciona con FTP y, en algunos casos, con HTTP PUT.
Ejemplos:

Descarga con nombre remoto:
bash
Copiar
curl -O https://www.example.com/archivo.zip
Descarga reanudable:
bash
Copiar
curl -C - -O https://www.example.com/archivo.zip
Subida vía FTP:
bash
Copiar
curl -T localfile.txt ftp://ftp.example.com/remotefile.txt --user usuario:contraseña
9. Seguridad y Conexiones SSL/TLS
Verificación de certificados:

--insecure, -k
Permite conexiones SSL/TLS sin verificar el certificado del servidor (útil para pruebas, pero no recomendado en producción).
--cacert <archivo>
Utiliza un certificado CA específico para validar el servidor.
--capath <directorio>
Especifica un directorio que contenga certificados CA para la validación.
Certificados de cliente:

--cert <archivo[:contraseña]>
Usa un certificado cliente para la autenticación.
--key <archivo>
Especifica la clave privada asociada al certificado cliente.
Ejemplo:

bash
Copiar
curl --cacert /ruta/ca.pem https://www.example.com
10. Opciones para FTP y Otros Protocolos
Comandos y directorios en FTP:

-Q, --quote <comando>
Envía comandos específicos al servidor FTP antes o después de la transferencia.
--ftp-create-dirs
Crea los directorios necesarios en el servidor FTP si no existen.
--ftp-method <método>
Selecciona el método de operación FTP (por ejemplo, multicwd, nocwd o singlecwd).
Ejemplo:

bash
Copiar
curl -T archivo.txt ftp://ftp.example.com/dir/ --ftp-create-dirs --user usuario:contraseña
11. Depuración y Trazado
Registro detallado:

--trace <archivo>
Guarda un registro completo (incluyendo datos binarios) de la actividad de cURL en un archivo.
--trace-ascii <archivo>
Guarda la traza en formato ASCII, lo que facilita la lectura.
--trace-time
Añade marcas de tiempo a cada línea del registro.
Salida personalizada de información:

-w, --write-out <formato>
Permite especificar una cadena de formato para que cURL imprima información al finalizar la transferencia (por ejemplo, tiempo total, tamaño de la descarga, etc.).
Ejemplo:

bash
Copiar
curl -w "Tiempo total: %{time_total}s\n" -o /dev/null https://www.example.com
12. Configuración Avanzada
Archivos de configuración:

-K, --config <archivo>
Permite que cURL lea opciones de un archivo de configuración en lugar de pasarlas todas por línea de comandos.
Ejemplo de archivo (config.txt):
ini
Copiar
url = "https://www.example.com"
user-agent = "MiAgente/1.0"
Generación de código:

--libcurl <archivo>
Genera código fuente en C que reproduce la misma operación que se realizó con cURL.
Ejemplo usando archivo de configuración:

bash
Copiar
curl -K config.txt
13. Manipulación de DNS y Selección de Interfaz
Resolución forzada:

--resolve <host:puerto:dirección>
Sobrescribe la resolución DNS para un host específico, asignándole una dirección determinada.
Selección de interfaz:

--interface <nombre>
Fuerza el uso de una interfaz de red específica para la conexión.
--local-port <puerto>
Permite especificar el puerto local o un rango de puertos desde los que se debe realizar la conexión.
Ejemplo:

bash
Copiar
curl --resolve www.example.com:80:127.0.0.1 http://www.example.com
14. Otras Opciones Útiles
Compresión:

--compressed
Solicita al servidor que envíe la respuesta comprimida (gzip/deflate) y se encarga de descomprimirla automáticamente.
Envío de datos binarios:

--data-binary <datos>
Envía datos en forma binaria sin ningún procesamiento o alteración (útil para subir archivos o datos que contengan caracteres especiales).
Campos de formulario como cadenas literales:

--form-string <nombre=valor>
Envía un campo de formulario tratando el contenido literalmente, sin interpretar caracteres especiales ni variables.
Reintentos en caso de fallo:

--retry <número>
Reintenta la solicitud el número de veces especificado en caso de error de conexión.
--retry-delay <segundos>
Tiempo de espera entre reintentos.
--retry-max-time <segundos>
Tiempo máximo total que se dedicarán a reintentos.
Ejemplo:

bash
Copiar
curl --compressed --data-binary "@archivo.bin" --retry 3 --retry-delay 5 https://www.example.com/upload
Conclusión
Este resumen cubre las funcionalidades y opciones más comunes de cURL para realizar transferencias de datos, gestionar encabezados, autenticación, manejo de cookies, conexión a través de proxy, transferencia de archivos, seguridad SSL/TLS, y opciones de depuración y configuración avanzada. Dada la gran cantidad de opciones que ofrece cURL (incluyendo algunas específicas para protocolos menos comunes), se recomienda consultar la documentación oficial para profundizar en aquellas funcionalidades que se requieran en casos particulares.

Cada opción puede combinarse según las necesidades de la transferencia, lo que convierte a cURL en una herramienta extremadamente potente y flexible para interactuar con servicios web y otros protocolos desde la línea de comandos.
