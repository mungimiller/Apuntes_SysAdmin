1. **top**  
   - **Explicación:** Ejecuta top en modo batch para generar un informe completo de procesos y recursos, ideal para guardar logs o análisis automatizados.  
   - **Ejemplo:** `top -b -n 1 > /var/log/top_report_$(date +%F).log`

2. **htop**  
   - **Explicación:** Usa htop para una vista interactiva y colorida de los procesos, con capacidad para ordenar y filtrar, útil para diagnósticos en tiempo real.  
   - **Ejemplo:** `htop --sort-key=PERCENT_CPU`

3. **ps**  
   - **Explicación:** Muestra una lista detallada de procesos, con formato personalizado para resaltar consumo de recursos y relaciones padre-hijo.  
   - **Ejemplo:** `ps -eo pid,ppid,user,%cpu,%mem,cmd --sort=-%cpu | head -n 20`

4. **netstat**  
   - **Explicación:** Lista conexiones de red activas y puertos en escucha, filtrando conexiones críticas para análisis de seguridad y rendimiento.  
   - **Ejemplo:** `netstat -tulnp | grep -E ':(80|443)'`

5. **ss**  
   - **Explicación:** Alternativa moderna a netstat que muestra información detallada de sockets, ideal para diagnosticar problemas de red en producción.  
   - **Ejemplo:** `ss -tulpn | grep -E ':(22|80)'`

6. **tcpdump**  
   - **Explicación:** Captura tráfico en una interfaz de red con filtros precisos; útil para depurar problemas de red o analizar ataques.  
   - **Ejemplo:** `tcpdump -i eth0 'tcp port 80 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)' -c 100 -w /tmp/capture_$(date +%s).pcap`

7. **strace**  
   - **Explicación:** Rastrea llamadas al sistema de un proceso, incluyendo subprocesos y tiempos de espera, ideal para identificar cuellos de botella o errores.  
   - **Ejemplo:** `strace -f -tt -T -p 1234 2>&1 | tee /tmp/strace_1234.log`

8. **lsof**  
   - **Explicación:** Lista archivos y sockets abiertos, filtrando conexiones específicas o procesos que usan archivos críticos en entornos de producción.  
   - **Ejemplo:** `lsof -iTCP -sTCP:LISTEN -P > /tmp/lsof_listen.log`

9. **systemctl**  
   - **Explicación:** Gestiona servicios con systemd, mostrando estados completos y logs integrados sin paginación, ideal para scripts de monitorización.  
   - **Ejemplo:** `systemctl status nginx.service --no-pager`

10. **journalctl**  
    - **Explicación:** Extrae registros del sistema para un servicio específico dentro de un rango de tiempo, permitiendo filtrar y exportar logs para análisis forense.  
    - **Ejemplo:** `journalctl -u sshd --since "2025-03-20 00:00:00" --until "2025-03-20 23:59:59" -n 100 > /var/log/sshd_20250320.log`

11. **dmesg**  
    - **Explicación:** Visualiza y filtra mensajes del kernel con marcas de tiempo, útil para identificar errores de hardware o controladores.  
    - **Ejemplo:** `dmesg -T | grep -i 'error' > /var/log/dmesg_errors.log`

12. **vmstat**  
    - **Explicación:** Monitorea estadísticas de memoria, procesos y CPU en intervalos definidos, generando reportes que ayudan a detectar problemas de rendimiento.  
    - **Ejemplo:** `vmstat 2 15 > /var/log/vmstat_$(date +%F).log`

13. **iostat**  
    - **Explicación:** Proporciona estadísticas detalladas de I/O, ideal para identificar cuellos de botella en discos y particiones en entornos de alta carga.  
    - **Ejemplo:** `iostat -dx 2 10 > /var/log/iostat_$(date +%F).log`

14. **sar**  
    - **Explicación:** Recopila datos de rendimiento del sistema en intervalos, permitiendo analizar tendencias históricas de uso de CPU, memoria y red.  
    - **Ejemplo:** `sar -u 1 10 > /var/log/sar_cpu_$(date +%F).log`

15. **nmon**  
    - **Explicación:** Herramienta interactiva para monitorear el rendimiento global del sistema y exportar datos a archivos CSV para análisis posterior.  
    - **Ejemplo:** `nmon -f -s 2 -c 60`  
    *(Genera un archivo nmon en el directorio actual con datos recolectados cada 2 segundos durante 2 minutos.)*

16. **iftop**  
    - **Explicación:** Monitoriza el uso del ancho de banda en tiempo real, mostrando conexiones y volúmenes de datos, ideal para identificar tráfico inusual.  
    - **Ejemplo:** `sudo iftop -i eth0 -B -t > /tmp/iftop_report.txt`

17. **nmap**  
    - **Explicación:** Escanea rangos de IP para identificar hosts activos y puertos abiertos, con detección de servicios y versión de software.  
    - **Ejemplo:** `nmap -sS -sV -O -p 1-65535 192.168.1.0/24 -oN /tmp/nmap_scan.txt`

18. **curl**  
    - **Explicación:** Recupera contenido web y cabeceras HTTP; en este ejemplo se envía un POST JSON y se sigue redirecciones, útil para testear APIs.  
    - **Ejemplo:** `curl -X POST -H "Content-Type: application/json" -d '{"user":"admin"}' -L https://api.example.com/login`

19. **wget**  
    - **Explicación:** Descarga contenido web de forma recursiva, respetando marcas de tiempo y límites de profundidad, ideal para hacer copias de seguridad de sitios.  
    - **Ejemplo:** `wget -r -N -l 2 -P /backup/web https://example.com`

20. **rsync**  
    - **Explicación:** Sincroniza directorios de forma eficiente entre servidores o backups locales, utilizando compresión, preservación de permisos y conexión segura por SSH.  
    - **Ejemplo:** `rsync -avz --delete -e "ssh -p 2222" /var/www/ user@backupserver:/backup/www/`

21. **tmux**  
    - **Explicación:** Crea y administra sesiones de terminal multiplexadas, permitiendo desconexión y reconexión sin interrumpir procesos críticos en servidores remotos.  
    - **Ejemplo:** `tmux new -s prod_maintenance`  
    *(Dentro de tmux, se pueden dividir ventanas y usar comandos de gestión de sesiones.)*

22. **sed**  
    - **Explicación:** Realiza transformaciones de texto en archivos, con edición in situ y creación de copias de respaldo, ideal para automatizar cambios en configuraciones.  
    - **Ejemplo:** `sed -i.bak 's/PermitRootLogin yes/PermitRootLogin no/g' /etc/ssh/sshd_config`

23. **awk**  
    - **Explicación:** Procesa y analiza archivos de texto complejos; este ejemplo filtra logs para sumar errores específicos y calcular totales.  
    - **Ejemplo:** `awk '/ERROR/ {count++} END {print "Total errores:", count}' /var/log/app.log`

24. **grep**  
    - **Explicación:** Busca patrones en archivos con contexto expandido y opciones de recursividad, útil para analizar logs de gran tamaño.  
    - **Ejemplo:** `grep -RinC 3 "SegFault" /var/log/ > /tmp/segfaults.log`

25. **find**  
    - **Explicación:** Busca archivos que cumplen condiciones específicas (por ejemplo, modificados recientemente o de cierto tamaño) y ejecuta acciones sobre ellos.  
    - **Ejemplo:** `find /var/log -type f -mtime -3 -size +1M -exec ls -lh {} \;`

26. **locate**  
    - **Explicación:** Busca rápidamente archivos mediante una base de datos indexada; se actualiza la base de datos antes de la búsqueda para mayor precisión.  
    - **Ejemplo:** `sudo updatedb && locate -i "nginx.conf"`

27. **crontab**  
    - **Explicación:** Programa tareas automatizadas; en este ejemplo se configura una tarea que ejecuta un script de respaldo cada noche a las 2 AM.  
    - **Ejemplo:**  
    ```bash
    0 2 * * * /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1
    ```  

28. **systemd-run**  
    - **Explicación:** Ejecuta scripts o comandos como servicios transitorios, asignándoles nombres y descripciones para facilitar el monitoreo y la integración con systemd.  
    - **Ejemplo:** `systemd-run --unit=backup_service --description="Ejecuta backup diario" /usr/local/bin/backup.sh`

29. **ip**  
    - **Explicación:** Configura interfaces y rutas de red, añadiendo direcciones IP, rutas estáticas o modificando configuraciones en caliente sin reiniciar servicios.  
    - **Ejemplo:** `ip addr add 192.168.1.50/24 dev eth0 && ip route add default via 192.168.1.1`

30. **iptables**  
    - **Explicación:** Gestiona reglas de firewall a nivel de kernel; este ejemplo inserta una regla para bloquear una IP sospechosa y guarda la configuración para persistencia.  
    - **Ejemplo:**  
    ```bash
    iptables -I INPUT -s 203.0.113.45 -j DROP
    service iptables save
    ```

31. **ufw**  
    - **Explicación:** Administra un firewall simplificado, permitiendo configurar reglas de forma intuitiva y activar el registro para auditoría de seguridad.  
    - **Ejemplo:**  
    ```bash
    ufw allow 22/tcp
    ufw allow 80/tcp
    ufw logging on
    ufw enable
    ```

32. **fail2ban-client**  
    - **Explicación:** Administra Fail2Ban para proteger servicios de ataques de fuerza bruta; en este ejemplo se revisa el estado de la cárcel SSH y se prohíbe manualmente una IP.  
    - **Ejemplo:**  
    ```bash
    fail2ban-client status sshd
    fail2ban-client set sshd banip 198.51.100.23
    ```

33. **diff**  
    - **Explicación:** Compara recursivamente directorios o archivos, mostrando diferencias de contenido, útil para verificar cambios entre configuraciones o versiones de software.  
    - **Ejemplo:** `diff -ruN /etc/nginx/ /backup/nginx_conf/ > /tmp/nginx_diff.patch`

34. **patch**  
    - **Explicación:** Aplica archivos diff a conjuntos de archivos, generando copias de seguridad y permitiendo revertir cambios en caso de errores.  
    - **Ejemplo:** `patch -p1 -b < /tmp/nginx_diff.patch`

35. **tcpflow**  
    - **Explicación:** Captura y reconstruye flujos de datos TCP, separando cada conexión en archivos individuales para un análisis profundo de sesiones de red.  
    - **Ejemplo:** `sudo tcpflow -i eth0 port 443 -o /tmp/tcpflow_logs/`

36. **nethogs**  
    - **Explicación:** Monitorea en tiempo real el uso del ancho de banda por proceso, ayudando a identificar aplicaciones que consumen recursos excesivos.  
    - **Ejemplo:** `sudo nethogs -t eth0`

37. **ethtool**  
    - **Explicación:** Muestra y ajusta parámetros de interfaces Ethernet, permitiendo diagnosticar problemas de velocidad, dúplex o errores físicos en conexiones de red.  
    - **Ejemplo:** `sudo ethtool eth0 | grep -i "Speed\|Duplex"`

38. **perf**  
    - **Explicación:** Registra eventos de rendimiento para identificar cuellos de botella a nivel de CPU y memoria en aplicaciones críticas; genera informes detallados.  
    - **Ejemplo:** `perf record -g -p 1234 -- sleep 15 && perf report > /tmp/perf_report.txt`

39. **iperf3**  
    - **Explicación:** Mide el rendimiento y la latencia de la red entre un cliente y un servidor, con intervalos de reporte intermedios para evaluar estabilidad.  
    - **Ejemplo:** `iperf3 -c 192.168.1.150 -t 30 -i 5`

40. **nc (netcat)**  
    - **Explicación:** Transfiere datos de forma simple y rápida entre hosts; en este ejemplo se usa para enviar un archivo comprimido a través de una conexión TCP.  
    - **Ejemplo:**  
    En el servidor:  
    ```bash
    nc -l -p 4444 | tar -xzvf -
    ```  
    En el cliente:  
    ```bash
    tar -czvf - /data/ | nc 192.168.1.150 4444
    ```

41. **dig**  
    - **Explicación:** Realiza consultas DNS avanzadas, mostrando solo las respuestas relevantes y ayudando a diagnosticar problemas de resolución de nombres.  
    - **Ejemplo:** `dig +nocmd google.com any +multiline +noall`

42. **host**  
    - **Explicación:** Proporciona una forma sencilla de obtener registros DNS de un dominio, mostrando detalles de registros A, MX, NS, etc.  
    - **Ejemplo:** `host -a example.com`

43. **openssl**  
    - **Explicación:** Establece conexiones seguras para verificar certificados y configuraciones SSL/TLS, ideal para comprobar la seguridad de servicios web.  
    - **Ejemplo:** `openssl s_client -connect example.com:443 -servername example.com -showcerts`

44. **fuser**  
    - **Explicación:** Identifica y termina procesos que están usando un puerto o archivo específico, útil para liberar recursos o resolver conflictos en servicios.  
    - **Ejemplo:** `fuser -n tcp -k 8080`

45. **lvdisplay**  
    - **Explicación:** Muestra detalles completos de volúmenes lógicos en LVM, permitiendo verificar asignaciones de espacio, atributos y estados para administración de almacenamiento.  
    - **Ejemplo:** `lvdisplay -m /dev/vg0/lv_data`

46. **vgdisplay**  
    - **Explicación:** Ofrece una visión global de los grupos de volúmenes, mostrando información sobre espacio total, usado y libre, crucial para planificar expansiones.  
    - **Ejemplo:** `vgdisplay vg0`

47. **pvdisplay**  
    - **Explicación:** Detalla la información de volúmenes físicos, incluyendo su uso y estado, lo que es fundamental para gestionar y prever fallos en dispositivos de almacenamiento.  
    - **Ejemplo:** `pvdisplay /dev/sda1`

48. **mount**  
    - **Explicación:** Remonta sistemas de archivos aplicando nuevas opciones sin reiniciar el sistema, útil para cambiar permisos o modos de montaje en caliente.  
    - **Ejemplo:** `mount -o remount,rw,exec /mnt/data`

49. **umount**  
    - **Explicación:** Desmonta dispositivos de forma segura; la opción lazy permite liberar el punto de montaje aunque aún esté en uso, ideal para entornos de alta concurrencia.  
    - **Ejemplo:** `umount -l /mnt/usb`

50. **docker ps**  
    - **Explicación:** Lista contenedores Docker en ejecución con un formato personalizado, permitiendo filtrar por estado o nombre para facilitar la administración de entornos de contenedores en producción.  
    - **Ejemplo:** `docker ps --filter "status=running" --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"`


MAS EJEMPLOS:
1. **watch**  
   - **Explicación:** Ejecuta un comando de forma repetida a intervalos definidos para monitorear cambios en tiempo real, ideal para supervisar procesos o recursos.  
   - **Ejemplo:** `watch -n 2 'ps -eo pid,cmd,%cpu --sort=-%cpu | head -n 10'`

2. **lscpu**  
   - **Explicación:** Muestra información detallada sobre la arquitectura de la CPU, útil para ajustar el rendimiento o solucionar problemas de compatibilidad.  
   - **Ejemplo:** `lscpu`

3. **lsblk**  
   - **Explicación:** Lista dispositivos de bloques conectados y sus puntos de montaje, facilitando la gestión y diagnóstico de almacenamiento.  
   - **Ejemplo:** `lsblk -o NAME,SIZE,TYPE,MOUNTPOINT`

4. **df**  
   - **Explicación:** Informa sobre el uso de espacio en disco de todas las particiones montadas, crucial para evitar quedadas de espacio.  
   - **Ejemplo:** `df -h`

5. **du**  
   - **Explicación:** Calcula el uso de espacio de archivos y directorios de forma recursiva, permitiendo identificar rápidamente los grandes consumidores de disco.  
   - **Ejemplo:** `du -sh /var/log/*`

6. **dstat**  
   - **Explicación:** Combina estadísticas de CPU, disco, red y más en tiempo real, facilitando un diagnóstico completo del sistema.  
   - **Ejemplo:** `dstat -tcndy`

7. **bmon**  
   - **Explicación:** Monitorea el uso de ancho de banda en las interfaces de red con una interfaz visual en tiempo real.  
   - **Ejemplo:** `bmon -p eth0`

8. **sosreport**  
   - **Explicación:** Recopila una amplia gama de datos del sistema (configuraciones, logs, estado de hardware) para diagnósticos y soporte avanzado.  
   - **Ejemplo:** `sosreport --batch`

9. **ipvsadm**  
   - **Explicación:** Administra el IP Virtual Server para balanceo de carga, permitiendo ver y modificar las reglas de red en clústeres de alta disponibilidad.  
   - **Ejemplo:** `ipvsadm -L -n`

10. **fail2ban-regex**  
    - **Explicación:** Permite probar las expresiones regulares usadas en las reglas de Fail2Ban contra logs reales para verificar su eficacia.  
    - **Ejemplo:** `fail2ban-regex /var/log/auth.log /etc/fail2ban/filter.d/sshd.conf`

11. **auditctl**  
    - **Explicación:** Configura el sistema de auditoría de Linux para vigilar cambios críticos, asegurando la integridad de archivos importantes.  
    - **Ejemplo:** `auditctl -w /etc/passwd -p wa -k passwd_changes`

12. **ausearch**  
    - **Explicación:** Busca y filtra eventos en los registros de auditoría, facilitando análisis forenses en incidentes de seguridad.  
    - **Ejemplo:** `ausearch -k passwd_changes`

13. **acct**  
    - **Explicación:** Habilita la contabilidad de procesos para registrar el uso de recursos por usuario y comando, ayudando en auditorías y optimización.  
    - **Ejemplo:** `sa -u`

14. **mpstat**  
    - **Explicación:** Proporciona estadísticas de CPU por cada procesador, ayudando a identificar desequilibrios en la carga de trabajo.  
    - **Ejemplo:** `mpstat -P ALL 2 5`

15. **numactl**  
    - **Explicación:** Controla la asignación de memoria en sistemas NUMA para mejorar el rendimiento de aplicaciones que requieren alta eficiencia.  
    - **Ejemplo:** `numactl --interleave=all my_application`

16. **fstrim**  
    - **Explicación:** Ejecuta comandos TRIM en dispositivos de bloques (especialmente SSD) para mantener el rendimiento y prolongar la vida útil.  
    - **Ejemplo:** `fstrim -av`

17. **hdparm**  
    - **Explicación:** Evalúa el rendimiento y ajusta parámetros de discos duros, lo que ayuda a diagnosticar y optimizar el acceso a disco.  
    - **Ejemplo:** `hdparm -tT /dev/sda`

18. **smartctl**  
    - **Explicación:** Utiliza la tecnología S.M.A.R.T. para verificar el estado de los discos, permitiendo detectar posibles fallas antes de que ocurran.  
    - **Ejemplo:** `smartctl -a /dev/sda`

19. **mtr**  
    - **Explicación:** Combina traceroute y ping para ofrecer una vista interactiva y continua de la ruta de red y la latencia, útil para diagnosticar problemas de conectividad.  
    - **Ejemplo:** `mtr -r -c 10 example.com`

20. **lsusb**  
    - **Explicación:** Lista todos los dispositivos USB conectados, facilitando la identificación y solución de problemas relacionados con hardware USB.  
    - **Ejemplo:** `lsusb`

21. **lspci**  
    - **Explicación:** Muestra información detallada de dispositivos PCI, esencial para la resolución de problemas de drivers y hardware en servidores.  
    - **Ejemplo:** `lspci -v`

22. **sensors**  
    - **Explicación:** Proporciona lecturas de temperaturas, voltajes y velocidades de ventilador, permitiendo vigilar la salud térmica del sistema.  
    - **Ejemplo:** `sensors`

23. **chkconfig**  
    - **Explicación:** Administra servicios en sistemas SysV, permitiendo habilitar o deshabilitar la carga de servicios al arranque en entornos legacy.  
    - **Ejemplo:** `chkconfig --list | grep httpd`

24. **service**  
    - **Explicación:** Gestiona servicios en sistemas basados en SysV, permitiendo iniciar, detener o reiniciar servicios de manera simple.  
    - **Ejemplo:** `service httpd restart`

25. **update-alternatives**  
    - **Explicación:** Permite gestionar múltiples versiones de una misma aplicación (por ejemplo, Java), facilitando la conmutación entre ellas.  
    - **Ejemplo:** `update-alternatives --config java`

26. **dpkg**  
    - **Explicación:** Gestiona paquetes en distribuciones Debian, mostrando información de versiones e instalando o removiendo software de forma granular.  
    - **Ejemplo:** `dpkg -l | grep nginx`

27. **rpm**  
    - **Explicación:** Administra paquetes en sistemas basados en RPM, verificando la integridad y versión de los paquetes instalados.  
    - **Ejemplo:** `rpm -qa | grep httpd`

28. **gdb**  
    - **Explicación:** Depura aplicaciones en ejecución, permitiendo inspeccionar variables, seguir la traza y analizar fallos para solucionar errores en tiempo real.  
    - **Ejemplo:** `gdb /usr/bin/myapp core`

29. **valgrind**  
    - **Explicación:** Analiza aplicaciones para detectar fugas de memoria y otros errores de ejecución, fundamental para depurar programas en C/C++.  
    - **Ejemplo:** `valgrind --leak-check=full ./myapp`

30. **ab (ApacheBench)**  
    - **Explicación:** Realiza pruebas de rendimiento de servidores web simulando múltiples conexiones simultáneas, ayudando a identificar cuellos de botella.  
    - **Ejemplo:** `ab -n 1000 -c 100 http://example.com/`

31. **siege**  
    - **Explicación:** Ejecuta pruebas de carga en servidores web simulando tráfico de múltiples usuarios para evaluar la robustez del sistema.  
    - **Ejemplo:** `siege -c 50 -t 1M http://example.com/`

32. **xfs_info**  
    - **Explicación:** Muestra detalles de la configuración y parámetros de sistemas de archivos XFS, facilitando la optimización y solución de problemas.  
    - **Ejemplo:** `xfs_info /dev/sda1`

33. **lxc-ls**  
    - **Explicación:** Lista contenedores LXC y muestra su estado, útil para gestionar entornos de virtualización ligera en producción.  
    - **Ejemplo:** `lxc-ls --fancy`

34. **podman ps**  
    - **Explicación:** Lista contenedores gestionados por Podman, una alternativa sin demonio a Docker, permitiendo ver su estado y detalles clave.  
    - **Ejemplo:** `podman ps --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"`

35. **firewall-cmd**  
    - **Explicación:** Gestiona el firewall firewalld en sistemas basados en RHEL, permitiendo agregar o eliminar reglas de forma dinámica.  
    - **Ejemplo:** `firewall-cmd --zone=public --add-port=8080/tcp --permanent && firewall-cmd --reload`

36. **at**  
    - **Explicación:** Programa la ejecución única de tareas en un momento específico, útil para tareas puntuales que no requieren recurrencia.  
    - **Ejemplo:** `echo "/usr/local/bin/maintenance.sh" | at 03:00`

37. **screen**  
    - **Explicación:** Multiplexa terminales de forma similar a tmux, permitiendo mantener sesiones persistentes y continuar procesos a través de desconexiones.  
    - **Ejemplo:** `screen -S mysession`

38. **rsyslogd**  
    - **Explicación:** Inicia el demonio de rsyslog para gestionar y centralizar la recolección de logs del sistema, facilitando auditorías y diagnóstico.  
    - **Ejemplo:** `rsyslogd -n`

39. **logrotate**  
    - **Explicación:** Rota, comprime y elimina logs antiguos siguiendo una configuración definida, evitando que el sistema se sature de archivos de log.  
    - **Ejemplo:** `logrotate -vf /etc/logrotate.conf`

40. **tshark**  
    - **Explicación:** Versión en línea de comandos de Wireshark para capturar y analizar tráfico de red con filtros avanzados, ideal para análisis forense.  
    - **Ejemplo:** `tshark -i eth0 -f "tcp port 443" -c 100 -w /tmp/tshark_capture.pcap`

41. **ncdu**  
    - **Explicación:** Herramienta interactiva para visualizar el uso de disco en directorios, permitiendo identificar rápidamente archivos o carpetas que consumen mucho espacio.  
    - **Ejemplo:** `ncdu /var`

42. **inotifywait**  
    - **Explicación:** Monitorea eventos del sistema de archivos en tiempo real y puede usarse para disparar acciones automáticas cuando se producen cambios.  
    - **Ejemplo:** `inotifywait -m /etc/ | grep "MODIFY"`

43. **unison**  
    - **Explicación:** Sincroniza directorios de forma bidireccional entre sistemas, asegurando la coherencia en entornos distribuidos o copias de seguridad.  
    - **Ejemplo:** `unison /home/user/ remote://user@backupserver//home/user/ -batch`

44. **zpool**  
    - **Explicación:** Gestiona pools de almacenamiento ZFS, permitiendo crear, extender y monitorizar el estado de la integridad de los datos.  
    - **Ejemplo:** `zpool status tank`

45. **zfs**  
    - **Explicación:** Administra sistemas de archivos ZFS, ofreciendo funcionalidades avanzadas como snapshots y replicación para respaldos seguros.  
    - **Ejemplo:** `zfs snapshot tank/data@backup_$(date +%F)`

46. **etckeeper**  
    - **Explicación:** Versiona el contenido de /etc usando sistemas de control de versiones, facilitando el seguimiento de cambios en la configuración del sistema.  
    - **Ejemplo:** `etckeeper commit "Actualización de configuración"`

47. **dmidecode**  
    - **Explicación:** Extrae información detallada del hardware (BIOS, memoria, CPU) a partir de la tabla DMI, útil para inventarios y diagnósticos.  
    - **Ejemplo:** `dmidecode -t system`

48. **free**  
    - **Explicación:** Muestra la cantidad de memoria física y swap en uso y disponible, permitiendo un diagnóstico rápido del estado de los recursos.  
    - **Ejemplo:** `free -h`

49. **unzip**  
    - **Explicación:** Extrae archivos comprimidos en formato ZIP, permitiendo la rápida implementación o verificación de paquetes de software.  
    - **Ejemplo:** `unzip -o package.zip -d /opt/application`

50. **tar**  
    - **Explicación:** Empaqueta y comprime directorios o archivos, facilitando respaldos y despliegues a través de formatos comprimidos ampliamente aceptados.  
    - **Ejemplo:** `tar -czvf /backup/backup_$(date +%F).tar.gz /var/www/`

