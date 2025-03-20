![image](https://github.com/user-attachments/assets/2fc6768b-0b5a-49e4-88f1-4f4084d7e2f3)


## 📂 Manejo de Archivos y Directorios

-  `ls`<br>
    Descripción: Lista el contenido de un directorio.<br>
    Ejemplo: ls -la /var/log<br>
    Notas: Comando básico en cualquier distribución.

- `cd`<br>
    Descripción: Cambia el directorio actual.<br>
    Ejemplo: cd /etc<br>
    Notas: Esencial en cualquier shell.

- `pwd`<br>
    Descripción: Muestra la ruta del directorio actual.<br>
    Ejemplo: pwd<br>
    Notas: Disponible en todas las distribuciones.

- `mkdir`<br>
    Descripción: Crea un directorio nuevo.<br>
    Ejemplo: mkdir /tmp/nuevo_directorio<br>
    Notas: Útil para organizar archivos.

- `rmdir`<br>
    Descripción: Elimina directorios vacíos.<br>
    Ejemplo: rmdir /tmp/nuevo_directorio<br>
    Notas: Si el directorio contiene archivos, se recomienda rm -r.

- `cp`<br>
    Descripción: Copia archivos o directorios.<br>
    Ejemplo: cp archivo.txt /home/usuario/<br>
    Notas: Con opción -r para directorios.

- `mv`<br>
    Descripción: Mueve o renombra archivos y directorios.<br>
    Ejemplo: mv archivo.txt /home/usuario/documentos/<br>
    Notas: Disponible en todas las distribuciones.

- `rm`<br>
    Descripción: Elimina archivos o directorios (con opción -r para recursividad).<br>
    Ejemplo: rm -rf /tmp/carpeta<br>
    Notas: Uso cuidadoso para evitar pérdidas accidentales.

- `find`<br>
    Descripción: Busca archivos y directorios basándose en criterios.<br>
    Ejemplo: find /var -name "*.log"<br>
    Notas: Muy útil para búsquedas avanzadas.

- `locate`<br>
    Descripción: Busca archivos rápidamente usando una base de datos actualizada.<br>
    Ejemplo: locate passwd<br>
    Notas: Requiere actualizar la base de datos con updatedb.

- `updatedb`<br>
    Descripción: Actualiza la base de datos para el comando locate.<br>
    Ejemplo: sudo updatedb<br>
    Notas: Común en distribuciones con mlocate instalado.

- `cat`<br>
    Descripción: Muestra el contenido de archivos en la salida estándar.<br>
    Ejemplo: cat /etc/hosts<br>
    Notas: Útil para concatenar y visualizar archivos.

- `less`<br>
    Descripción: Permite visualizar archivos largos página por página.<br>
    Ejemplo: less /var/log/syslog<br>
    Notas: Mejor opción que more por sus funcionalidades.

    tail<br>
    Descripción: Muestra las últimas líneas de un archivo.<br>
    Ejemplo: tail -n 20 /var/log/syslog<br>
    Notas: Ideal para ver logs en tiempo real con -f.

    head<br>
    Descripción: Muestra las primeras líneas de un archivo.<br>
    Ejemplo: head -n 10 /var/log/syslog<br>
    Notas: Funciona en cualquier distribución.

    touch<br>
    Descripción: Crea archivos vacíos o actualiza la fecha de modificación.<br>
    Ejemplo: touch nuevo_archivo.txt<br>
    Notas: Fundamental para scripts y pruebas.

    ln<br>
    Descripción: Crea enlaces duros o simbólicos entre archivos.<br>
    Ejemplo: ln -s /ruta/original /ruta/enlace<br>
    Notas: El uso de -s crea enlaces simbólicos; aplicable en cualquier entorno.<br>

    file<br>
    Descripción: Determina el tipo de un archivo.<br>
    Ejemplo: file /bin/ls<br>
    Notas: Útil para identificar archivos binarios, de texto, etc.

    du<br>
    Descripción: Muestra el uso de disco de archivos y directorios.<br>
    Ejemplo: du -sh /home/usuario<br>
    Notas: Disponible en todas las distribuciones.

    df<br>
    Descripción: Informa sobre el espacio en disco de los sistemas de archivos montados.<br>
    Ejemplo: df -h<br>
    Notas: Uso común en la monitorización de almacenamiento.

2. Gestión de Procesos

    ps<br>
    Descripción: Muestra procesos en ejecución.<br>
    Ejemplo: ps aux | grep apache<br>
    Notas: Disponible en todas las distribuciones.

    top<br>
    Descripción: Monitoriza procesos y recursos en tiempo real.<br>
    Ejemplo: top<br>
    Notas: Incluido por defecto en la mayoría de distribuciones.

    htop<br>
    Descripción: Versión interactiva y mejorada de top.<br>
    Ejemplo: htop<br>
    Notas: Puede requerir instalación en algunas distribuciones (por ejemplo, Ubuntu: sudo apt install htop).

    kill<br>
    Descripción: Envía señales a procesos para finalizarlos.<br>
    Ejemplo: kill -9 1234<br>
    Notas: Útil en cualquier entorno; la señal -9 es para forzar la terminación.

    pkill<br>
    Descripción: Envía señales a procesos basándose en su nombre.<br>
    Ejemplo: pkill -f apache<br>
    Notas: Alternativa a kill para procesos específicos.

    killall<br>
    Descripción: Finaliza todos los procesos con un nombre dado.<br>
    Ejemplo: killall firefox<br>
    Notas: Disponible en varias distribuciones, pero puede diferir entre sistemas UNIX.

    bg<br>
    Descripción: Envía un proceso detenido al fondo (background).<br>
    Ejemplo: bg %1<br>
    Notas: Comando interno del shell.

    fg<br>
    Descripción: Trae un proceso en segundo plano al primer plano.<br>
    Ejemplo: fg %1<br>
    Notas: Uso común en bash y otros shells.

    nohup<br>
    Descripción: Ejecuta un comando ignorando la señal SIGHUP (ideal para procesos en background).<br>
    Ejemplo: nohup ./script.sh &<br>
    Notas: Muy usado en entornos de servidor.

    pstree<br>
    Descripción: Muestra la jerarquía de procesos en forma de árbol.<br>
    Ejemplo: pstree -p<br>
    Notas: Puede requerir instalación en algunas distribuciones.

3. Administración de Paquetes

    apt-get<br>
    Descripción: Gestiona paquetes en distribuciones basadas en Debian/Ubuntu.<br>
    Ejemplo: sudo apt-get update && sudo apt-get upgrade<br>
    Notas: Fundamental en Debian, Ubuntu y derivados.

    aptitude<br>
    Descripción: Alternativa interactiva a apt-get en Debian/Ubuntu.<br>
    Ejemplo: sudo aptitude install nginx<br>
    Notas: Ofrece una interfaz de texto más amigable.

    dpkg<br>
    Descripción: Herramienta de bajo nivel para gestionar paquetes .deb.<br>
    Ejemplo: sudo dpkg -i paquete.deb<br>
    Notas: Utilizado en Debian/Ubuntu.

    yum<br>
    Descripción: Gestor de paquetes para distribuciones basadas en RHEL/CentOS.<br>
    Ejemplo: sudo yum install httpd<br>
    Notas: Reemplazado gradualmente por dnf en versiones más recientes.

    dnf<br>
    Descripción: Gestor de paquetes moderno para Fedora, CentOS y RHEL.<br>
    Ejemplo: sudo dnf update<br>
    Notas: Recomendado en sistemas modernos de Red Hat.

    rpm<br>
    Descripción: Herramienta de bajo nivel para gestionar paquetes RPM.<br>
    Ejemplo: rpm -qa | grep httpd<br>
    Notas: Utilizado en CentOS, Fedora, RHEL.

    pacman<br>
    Descripción: Gestor de paquetes para Arch Linux y derivados.<br>
    Ejemplo: sudo pacman -Syu<br>
    Notas: Específico de Arch y distribuciones basadas en este.

    snap<br>
    Descripción: Sistema de paquetes universal para instalar aplicaciones en contenedores<br>.
    Ejemplo: sudo snap install postman<br>
    Notas: Disponible en Ubuntu y otras distribuciones compatibles.

4. Gestión de Usuarios y Permisos

    useradd<br>
    Descripción: Crea nuevos usuarios en el sistema.<br>
    Ejemplo: sudo useradd -m juan<br>
    Notas: Común en cualquier distribución.

    userdel<br>
    Descripción: Elimina un usuario.<br>
    Ejemplo: sudo userdel -r juan<br>
    Notas: La opción -r elimina también el directorio home.

    usermod<br>
    Descripción: Modifica la configuración de un usuario existente<br>.
    Ejemplo: sudo usermod -aG sudo juan<br>
    Notas: Útil para gestionar grupos y permisos.

    passwd<br>
    Descripción: Cambia la contraseña de un usuario.<br>
    Ejemplo: sudo passwd juan<br>
    Notas: Comando universal en Linux.

    groupadd<br>
    Descripción: Crea un grupo nuevo.<br>
    Ejemplo: sudo groupadd desarrolladores<br>
    Notas: Usado en cualquier distribución.

    groupdel<br>
    Descripción: Elimina un grupo.<br>
    Ejemplo: sudo groupdel desarrolladores<br>
    Notas: Asegurarse de que no haya usuarios asignados.

    chown<br>
    Descripción: Cambia el propietario y grupo de archivos y directorios.<br>
    Ejemplo: sudo chown juan:desarrolladores archivo.txt<br>
    Notas: Fundamental para la seguridad de archivos.

    chmod<br>
    Descripción: Modifica los permisos de archivos y directorios.<br>
    Ejemplo: chmod 755 script.sh<br>
    Notas: Utilizado en cualquier entorno UNIX.

    su<br>
    Descripción: Cambia de usuario o ejecuta comandos como otro usuario.<br>
    Ejemplo: su - root<br>
    Notas: Alternativa a sudo, aunque menos segura en algunos casos.

    sudo<br>
    Descripción: Ejecuta comandos con privilegios de superusuario.<br>
    Ejemplo: sudo apt-get update<br>
    Notas: Muy utilizado en Ubuntu y otras distribuciones modernas.

5. Monitorización del Sistema

    free<br>
    Descripción: Muestra la memoria libre y usada.<br>
    Ejemplo: free -h<br>
    Notas: Útil en cualquier distribución.

    vmstat<br>
    Descripción: Informa sobre la memoria virtual, procesos y CPU<br>.
    Ejemplo: vmstat 2 5<br>
    Notas: Herramienta clásica para diagnóstico de rendimiento.

    iostat<br>
    Descripción: Proporciona estadísticas de entrada/salida y CPU.<br>
    Ejemplo: iostat -xz 2 3<br>
    Notas: Puede requerir instalación (por ejemplo, paquete sysstat).

    sar<br>
    Descripción: Recolecta y reporta estadísticas del sistema.<br>
    Ejemplo: sar -u 1 3<br>
    Notas: Parte del paquete sysstat, muy útil en entornos de producción.

    uptime<br>
    Descripción: Muestra el tiempo de actividad del sistema y carga media<br>.
    Ejemplo: uptime<br>
    Notas: Información rápida sobre la estabilidad del sistema.

    dmesg<br>
    Descripción: Visualiza mensajes del kernel, útil para diagnosticar problemas de hardware.<br>
    Ejemplo: dmesg | tail<br>
    Notas: Común en todas las distribuciones.

    systemctl status<br>
    Descripción: Muestra el estado de los servicios gestionados por systemd.<br>
    Ejemplo: systemctl status sshd<br>
    Notas: Utilizado en distribuciones modernas (Ubuntu 16.04+, CentOS 7+).

    journalctl<br>
    Descripción: Consulta los logs del sistema en entornos systemd.<br>
    Ejemplo: journalctl -u apache2<br>
    Notas: Alternativa moderna al archivo /var/log/syslog.

    netstat<br>
    Descripción: Muestra conexiones de red y estadísticas.<br>
    Ejemplo: netstat -tuln<br>
    Notas: Aunque está en desuso en favor de ss, sigue siendo útil en entornos legacy.

    ss<br>
    Descripción: Reemplazo moderno de netstat para estadísticas de sockets.<br>
    Ejemplo: ss -tuln<br>
    Notas: Mejor rendimiento y más opciones; disponible en distribuciones recientes.

6. Red y Servicios

    ifconfig / ip<br>
    Descripción: Configuran y muestran interfaces de red.<br>
    Ejemplo:
        Con ifconfig: ifconfig eth0
        Con ip: ip addr show eth0
        Notas: ifconfig es tradicional, mientras que ip es el comando moderno recomendado.

    ip addr
    Descripción: Muestra direcciones IP asignadas a interfaces.
    Ejemplo: ip addr
    Notas: Integrado en la suite iproute2, ideal para distribuciones modernas.

    ip route
    Descripción: Muestra y gestiona la tabla de rutas del sistema.
    Ejemplo: ip route show
    Notas: Esencial para diagnósticos de conectividad.

    ping
    Descripción: Envía paquetes ICMP para comprobar la conectividad con otro host.
    Ejemplo: ping google.com
    Notas: Básico en cualquier distribución.

    traceroute
    Descripción: Rastrea la ruta que siguen los paquetes hacia un destino.
    Ejemplo: traceroute 8.8.8.8
    Notas: Puede requerir instalación (sudo apt install traceroute en Ubuntu).

    nslookup
    Descripción: Consulta servidores DNS para obtener información de dominios.
    Ejemplo: nslookup example.com
    Notas: Disponible en la mayoría de distribuciones; en desuso a favor de dig en algunos casos.

    dig
    Descripción: Herramienta avanzada para consultas DNS.
    Ejemplo: dig example.com
    Notas: Muy valorado para diagnósticos DNS; se instala en la mayoría de sistemas.

    curl
    Descripción: Transfiere datos desde o hacia un servidor utilizando varios protocolos.
    Ejemplo: curl -I https://www.example.com
    Notas: Útil para probar APIs y conexiones HTTP.

    wget
    Descripción: Descarga archivos desde la web.
    Ejemplo: wget https://www.example.com/archivo.tar.gz
    Notas: Herramienta robusta para descargas en línea de comandos.

    scp
    Descripción: Copia archivos entre hosts de forma segura usando SSH.
    Ejemplo: scp archivo.txt usuario@servidor:/ruta/destino
    Notas: Común en entornos de administración remota.

7. Seguridad

    ufw
    Descripción: Interfaz simplificada para gestionar el firewall en Ubuntu.
    Ejemplo: sudo ufw allow 22/tcp
    Notas: Específico para Ubuntu y distribuciones compatibles con UFW.

    iptables
    Descripción: Herramienta avanzada para gestionar reglas de firewall en Linux.
    Ejemplo: sudo iptables -L -n
    Notas: Utilizado en diversas distribuciones; fundamental para entornos que requieren configuraciones personalizadas.

    fail2ban-client
    Descripción: Gestiona la herramienta Fail2Ban para prevenir ataques de fuerza bruta.
    Ejemplo: sudo fail2ban-client status
    Notas: Muy utilizado en servidores web; compatible con múltiples distribuciones.

    getenforce / setenforce
    Descripción: Verifica y modifica el estado de SELinux (Security-Enhanced Linux).
    Ejemplo:
        getenforce para ver el estado
        sudo setenforce 0 para poner SELinux en modo permisivo
        Notas: Esencial en distribuciones como CentOS, RHEL y Fedora.

    chroot
    Descripción: Cambia la raíz aparente del sistema de archivos para un proceso.
    Ejemplo: sudo chroot /mnt/nuevo_root
    Notas: Útil para entornos de recuperación o pruebas de seguridad.

    ssh
    Descripción: Accede de forma remota y segura a otro servidor.
    Ejemplo: ssh usuario@192.168.1.10
    Notas: Fundamental para administración remota; disponible en todas las distribuciones.

    ssh-keygen
    Descripción: Genera pares de claves SSH para autenticación sin contraseña.
    Ejemplo: ssh-keygen -t rsa -b 4096
    Notas: Recomendado para mejorar la seguridad en conexiones remotas.

8. Diagnóstico y Utilidades

    grep
    Descripción: Busca patrones en archivos o salida de comandos.
    Ejemplo: grep "error" /var/log/syslog
    Notas: Esencial en scripts y análisis de logs.

    awk
    Descripción: Lenguaje de programación y procesamiento de textos orientado a patrones.
    Ejemplo: awk '{print $1}' archivo.txt
    Notas: Muy versátil para informes y análisis de datos.

    sed
    Descripción: Editor de flujo para realizar transformaciones en texto.
    Ejemplo: sed 's/antiguo/nuevo/g' archivo.txt
    Notas: Potente para editar archivos desde scripts.

    diff
    Descripción: Compara archivos y muestra sus diferencias.
    Ejemplo: diff archivo1.txt archivo2.txt
    Notas: Útil para revisiones de código y configuraciones.

    tar
    Descripción: Agrupa varios archivos en uno solo (archivo tar) y/o lo comprime.
    Ejemplo: tar -czvf respaldo.tar.gz /ruta/a/respaldar
    Notas: Estándar en la mayoría de distribuciones.

    gzip / gunzip
    Descripción: Comprime y descomprime archivos con el formato gzip.
    Ejemplo:
        Comprimir: gzip archivo.txt
        Descomprimir: gunzip archivo.txt.gz
        Notas: Ampliamente utilizado para archivos de log y respaldos.

    bzip2
    Descripción: Comprime archivos usando el algoritmo bzip2, que ofrece mayor compresión que gzip.
    Ejemplo: bzip2 archivo.txt
    Notas: Alternativa a gzip en entornos donde el tamaño es crítico.

    unzip
    Descripción: Extrae archivos comprimidos en formato ZIP.
    Ejemplo: unzip archivo.zip
    Notas: Comando universal en sistemas Linux.

    man
    Descripción: Muestra el manual de cualquier comando.
    Ejemplo: man ls
    Notas: Esencial para aprender opciones y detalles de comandos.

    alias
    Descripción: Crea atajos para comandos largos o frecuentes.
    Ejemplo: alias ll='ls -la'
    Notas: Útil para personalizar el entorno de shell.

    history
    Descripción: Muestra el historial de comandos ejecutados.
    Ejemplo: history | grep sudo
    Notas: Facilita la repetición de comandos y depuración.

    env
    Descripción: Muestra o modifica las variables de entorno.
    Ejemplo: env | grep PATH
    Notas: Importante en scripts y configuraciones de usuario.

    export
    Descripción: Define variables de entorno para el shell actual y sus procesos hijos.
    Ejemplo: export PATH=$PATH:/nuevo/directorio
    Notas: Fundamental en scripts de inicialización.

    who
    Descripción: Muestra información sobre los usuarios conectados.
    Ejemplo: who
    Notas: Útil para monitorizar accesos en tiempo real.

    w
    Descripción: Ofrece información detallada de los usuarios conectados y su actividad.
    Ejemplo: w
    Notas: Alternativa a who con más detalles.

    last
    Descripción: Muestra un historial de conexiones de usuarios.
    Ejemplo: last
    Notas: Ideal para auditorías de acceso.

    crontab
    Descripción: Gestiona tareas programadas en el sistema.
    Ejemplo: crontab -e
    Notas: Esencial para automatizar procesos en servidores.

    at
    Descripción: Programa la ejecución única de tareas en un momento específico.
    Ejemplo: echo "sh /ruta/script.sh" | at 02:00
    Notas: Complementa a cron para tareas no recurrentes.

    screen
    Descripción: Permite gestionar sesiones de terminal persistentes.
    Ejemplo: screen -S mi_sesion
    Notas: Ideal para mantener procesos en ejecución tras desconexiones; disponible en la mayoría de distribuciones.

    tmux
    Descripción: Multiplexador de terminales, similar a screen pero con más funcionalidades.
    Ejemplo: tmux new -s mi_sesion
    Notas: Muy popular en entornos de desarrollo y servidores.

    rsync
    Descripción: Sincroniza y copia archivos de manera eficiente entre sistemas.
    Ejemplo: rsync -avz /origen/ usuario@destino:/ruta/
    Notas: Recomendado para respaldos y migraciones.

    strace
    Descripción: Rastrea llamadas al sistema realizadas por un proceso.
    Ejemplo: strace -p 1234
    Notas: Potente herramienta de diagnóstico para desarrolladores y administradores.

    lsof
    Descripción: Lista los archivos abiertos y los procesos que los usan.
    Ejemplo: lsof -i :80
    Notas: Fundamental para identificar qué procesos usan un puerto o archivo.

    time
    Descripción: Mide el tiempo de ejecución de un comando.
    Ejemplo: time ls -la
    Notas: Útil para optimizar y analizar scripts.

    uname
    Descripción: Muestra información sobre el sistema (kernel, arquitectura, etc.).
    Ejemplo: uname -a
    Notas: Esencial para diagnósticos y scripts de entorno.
