### Configuraciones Avanzadas de SSH (sshd_config)

1. **Deshabilitar el Acceso Root y Forzar Claves Públicas**  
   - **Explicación:** Previene accesos directos de root y obliga al uso de autenticación por clave pública para minimizar riesgos.  
   - **Ejemplo:**  
     ```conf
     PermitRootLogin no
     PasswordAuthentication no
     PubkeyAuthentication yes
     ```

2. **Forzar Protocolos y Cifrado Moderno**  
   - **Explicación:** Especifica versiones, algoritmos de intercambio de claves, ciphers y MACs robustos para mitigar vulnerabilidades.  
   - **Ejemplo:**  
     ```conf
     Protocol 2
     KexAlgorithms curve25519-sha256,curve25519-sha256@libssh.org
     Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com
     MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com
     ```

3. **Ajuste de LoginGraceTime y MaxAuthTries**  
   - **Explicación:** Reduce la ventana de tiempo para autenticarse y el número de intentos para disminuir la posibilidad de ataques de fuerza bruta.  
   - **Ejemplo:**  
     ```conf
     LoginGraceTime 30
     MaxAuthTries 3
     ```

4. **Configuración de ClientAliveInterval y ClientAliveCountMax**  
   - **Explicación:** Establece parámetros para desconectar sesiones inactivas, previniendo conexiones colgadas o intrusiones por medio de túneles no deseados.  
   - **Ejemplo:**  
     ```conf
     ClientAliveInterval 300
     ClientAliveCountMax 2
     ```

5. **Deshabilitar UseDNS para Acelerar Conexiones**  
   - **Explicación:** Evita retrasos en la conexión al no realizar búsquedas inversas de DNS para los clientes.  
   - **Ejemplo:**  
     ```conf
     UseDNS no
     ```

6. **Ajustar MaxSessions y MaxStartups para Control de Conexiones**  
   - **Explicación:** Limita el número de sesiones simultáneas y el ratio de conexiones entrantes no autenticadas para mitigar ataques DoS.  
   - **Ejemplo:**  
     ```conf
     MaxSessions 10
     MaxStartups 5:30:60
     ```

7. **Restringir el Acceso por Usuario o Grupo**  
   - **Explicación:** Permite solo a usuarios o grupos específicos conectarse al servidor, fortaleciendo la seguridad de acceso.  
   - **Ejemplo:**  
     ```conf
     AllowUsers admin user1
     # o alternativamente:
     AllowGroups sshusers
     ```

8. **Configurar Banner de Seguridad Personalizado**  
   - **Explicación:** Muestra un mensaje de advertencia legal y de seguridad a los usuarios antes de la autenticación.  
   - **Ejemplo:**  
     ```conf
     Banner /etc/ssh/banner.txt
     ```

9. **Ajustar la Autenticación Basada en PAM y 2FA**  
   - **Explicación:** Integra módulos PAM para forzar autenticación de dos factores (por ejemplo, OTP) y reforzar la seguridad.  
   - **Ejemplo:**  
     ```conf
     UsePAM yes
     ChallengeResponseAuthentication yes
     ```

10. **Configurar ListenAddress para Interfaces Específicas**  
    - **Explicación:** Escucha conexiones solo en direcciones IP específicas, limitando la exposición del servicio SSH.  
    - **Ejemplo:**  
      ```conf
      ListenAddress 192.168.1.10
      ListenAddress 2001:db8::1
      ```

11. **Ajustar Timeouts de Conexión TCPKeepAlive**  
    - **Explicación:** Usa parámetros de keepalive para detectar conexiones caídas y liberar recursos rápidamente.  
    - **Ejemplo:**  
      ```conf
      TCPKeepAlive yes
      ```

12. **Configurar PermitUserEnvironment para Personalización Avanzada**  
    - **Explicación:** Permite a los usuarios cargar variables de entorno definidas en archivos especiales, controlando el entorno remoto.  
    - **Ejemplo:**  
      ```conf
      PermitUserEnvironment yes
      ```

13. **Restringir el Acceso Basado en Dirección IP (Match Address)**  
    - **Explicación:** Usa bloques "Match" para aplicar configuraciones específicas a clientes de ciertas IP o rangos.  
    - **Ejemplo:**  
      ```conf
      Match Address 203.0.113.0/24
          PasswordAuthentication no
          PermitTunnel no
      ```

14. **Ajustar AllowTcpForwarding y X11Forwarding**  
    - **Explicación:** Controla el reenvío de puertos y X11 para evitar que usuarios creen túneles no autorizados.  
    - **Ejemplo:**  
      ```conf
      AllowTcpForwarding no
      X11Forwarding no
      ```

15. **Configuración Avanzada de LogLevel y SyslogFacility**  
    - **Explicación:** Aumenta el nivel de detalle en los logs para auditoría y diagnóstico sin exponer información sensible.  
    - **Ejemplo:**  
      ```conf
      LogLevel VERBOSE
      SyslogFacility AUTHPRIV
      ```

16. **Establecer KeyRegenerationInterval para Renovación de Claves**  
    - **Explicación:** Renueva claves de sesión en intervalos definidos para reducir el riesgo de compromisos a largo plazo.  
    - **Ejemplo:**  
      ```conf
      KeyRegenerationInterval 3600
      ```

17. **Forzar el Uso de Protocolos de Autenticación de Hardware (FIDO2/U2F)**  
    - **Explicación:** Configura SSH para admitir autenticación basada en dispositivos FIDO2 o U2F, reforzando el acceso físico.  
    - **Ejemplo:**  
      ```conf
      AuthenticationMethods publickey,keyboard-interactive
      ```

18. **Deshabilitar HostbasedAuthentication y PermitEmptyPasswords**  
    - **Explicación:** Elimina métodos de autenticación menos seguros para reforzar la integridad de la conexión.  
    - **Ejemplo:**  
      ```conf
      HostbasedAuthentication no
      PermitEmptyPasswords no
      ```

19. **Configurar Opciones de Compression para Optimizar el Ancho de Banda**  
    - **Explicación:** Activa la compresión de datos en la conexión para mejorar el rendimiento en enlaces lentos, controlando la carga del CPU.  
    - **Ejemplo:**  
      ```conf
      Compression delayed
      CompressionLevel 6
      ```

20. **Ajustar PermitTunnel para Controlar la Creación de Túneles VPN**  
    - **Explicación:** Permite o niega la creación de túneles SSH según necesidades de seguridad de red.  
    - **Ejemplo:**  
      ```conf
      PermitTunnel no
      ```

21. **Configurar Banner de Advertencia Legal Obligatorio**  
    - **Explicación:** Muestra mensajes de advertencia legal que cumplan normativas de seguridad en entornos regulados.  
    - **Ejemplo:**  
      ```conf
      Banner /etc/ssh/legal_banner.txt
      ```

22. **Configurar Advanced Connection Multiplexing**  
    - **Explicación:** Habilita y ajusta la reutilización de conexiones mediante ControlMaster y ControlPath para mejorar la eficiencia y seguridad.  
    - **Ejemplo:**  
      ```conf
      # Configurado en el cliente (archivo ~/.ssh/config):
      Host *
          ControlMaster auto
          ControlPath ~/.ssh/sockets/%r@%h:%p
          ControlPersist 600
      ```

23. **Establecer Opciones de ProxyCommand para Acceso a través de Bastion**  
    - **Explicación:** Configura SSH para enrutar conexiones a través de servidores bastión utilizando ProxyCommand.  
    - **Ejemplo:**  
      ```conf
      # Configurado en ~/.ssh/config:
      Host target.internal
          ProxyCommand ssh -W %h:%p bastion.example.com
      ```

24. **Habilitar Autenticación Basada en Certificados de Usuario**  
    - **Explicación:** Configura el servidor para aceptar certificados de usuario X.509, integrándose con soluciones PKI.  
    - **Ejemplo:**  
      ```conf
      TrustedUserCAKeys /etc/ssh/ca.pub
      ```

25. **Ajustar el Timeout de la Autenticación para Conexiones Lentas**  
    - **Explicación:** Extiende o reduce el período de gracia durante la autenticación para ambientes con latencia variable.  
    - **Ejemplo:**  
      ```conf
      LoginGraceTime 20
      ```

26. **Restringir el Reenvío de Agentes y Deshabilitar la Delegación de SSH-Agent**  
    - **Explicación:** Evita que agentes SSH sean reenviados, reduciendo el riesgo de secuestro de claves.  
    - **Ejemplo:**  
      ```conf
      AllowAgentForwarding no
      ```

27. **Configurar Opciones Avanzadas de Autenticación de Múltiples Factores**  
    - **Explicación:** Integra métodos MFA (como OTP) mediante módulos PAM para aumentar la seguridad en la autenticación SSH.  
    - **Ejemplo:**  
      ```conf
      ChallengeResponseAuthentication yes
      AuthenticationMethods publickey,keyboard-interactive
      ```

28. **Establecer Opciones de Log de Conexiones para Auditoría Forense**  
    - **Explicación:** Incrementa el detalle de los logs para rastrear conexiones y acciones, facilitando auditorías y análisis post-incidente.  
    - **Ejemplo:**  
      ```conf
      LogLevel DEBUG3
      ```

29. **Configurar Opciones Avanzadas para Reenvío de Puertos (Dynamic y Remote)**  
    - **Explicación:** Permite reenvío de puertos de manera controlada y segura, habilitando solo lo necesario según políticas internas.  
    - **Ejemplo:**  
      ```conf
      AllowTcpForwarding yes
      GatewayPorts no
      ```

30. **Integración con Sistemas de Monitoreo y Gestión de Incidentes**  
    - **Explicación:** Configura parámetros para enviar logs y eventos SSH a SIEMs o sistemas de monitoreo, facilitando la correlación de incidentes.  
    - **Ejemplo:**  
      ```conf
      SyslogFacility AUTHPRIV
      LogLevel VERBOSE
      ```
      
---

### Comandos Avanzados de SSH en Entornos de Producción

1. **Conexión con Verbosidad Extrema para Depuración**  
   - **Explicación:** Conecta al servidor SSH mostrando detalles extensos para solucionar problemas de conexión.  
   - **Ejemplo:**  
     ```bash
     ssh -vvv user@host.example.com
     ```

2. **Establecer Túnel Dinámico para un Proxy SOCKS**  
   - **Explicación:** Crea un túnel SSH que actúa como proxy SOCKS para enrutar tráfico a través del servidor remoto.  
   - **Ejemplo:**  
     ```bash
     ssh -N -D 1080 user@host.example.com
     ```

3. **Reenvío de Puertos Locales para Acceso a Bases de Datos Remotas**  
   - **Explicación:** Encamina el puerto de una base de datos remota a la máquina local para conexiones seguras.  
   - **Ejemplo:**  
     ```bash
     ssh -L 3306:localhost:3306 user@db-server.example.com
     ```

4. **Reenvío de Puertos Remotos para Exponer Servicios Internos**  
   - **Explicación:** Permite que un servicio interno sea accesible desde el servidor remoto mediante reenvío inverso.  
   - **Ejemplo:**  
     ```bash
     ssh -R 8080:localhost:80 user@host.example.com
     ```

5. **Uso de ProxyJump para Saltos Múltiples (Bastión)**  
   - **Explicación:** Conecta a un host final a través de uno o más servidores bastión.  
   - **Ejemplo:**  
     ```bash
     ssh -J bastion.example.com,user@intermediate.example.com user@final.example.com
     ```

6. **Conexión con Reutilización de Conexión (ControlMaster)**  
   - **Explicación:** Usa multiplexación para acelerar conexiones subsecuentes a un mismo host.  
   - **Ejemplo:**  
     ```bash
     ssh -M -S /tmp/ssh_mux_%h_%p_%r user@host.example.com
     ```

7. **Verificar el Estado de la Conexión Multiplexada**  
   - **Explicación:** Consulta el estado de una conexión SSH multiplexada activa.  
   - **Ejemplo:**  
     ```bash
     ssh -O check user@host.example.com
     ```

8. **Cerrar una Conexión Multiplexada Activa**  
   - **Explicación:** Finaliza una conexión multiplexada previamente establecida para liberar recursos.  
   - **Ejemplo:**  
     ```bash
     ssh -O exit user@host.example.com
     ```

9. **Ejecutar Comandos Remotos de Forma Segura**  
   - **Explicación:** Ejecuta un comando en el servidor remoto sin abrir una sesión interactiva.  
   - **Ejemplo:**  
     ```bash
     ssh user@host.example.com "sudo systemctl restart nginx"
     ```

10. **Uso de Agente SSH para Reenvío de Claves**  
    - **Explicación:** Habilita el reenvío del agente SSH para autenticarte en múltiples servidores sin exponer tus claves.  
    - **Ejemplo:**  
      ```bash
      ssh -A user@host.example.com
      ```

11. **Ejecutar SSH en Segundo Plano para Tareas Largas**  
    - **Explicación:** Inicia una conexión SSH en modo background sin asignar TTY, ideal para scripts automatizados.  
    - **Ejemplo:**  
      ```bash
      ssh -f -N user@host.example.com
      ```

12. **Especificar Clave Privada Personalizada en la Conexión**  
    - **Explicación:** Utiliza una clave SSH específica para autenticarte en el servidor.  
    - **Ejemplo:**  
      ```bash
      ssh -i /path/to/private_key user@host.example.com
      ```

13. **Utilizar Opciones de Seguridad en Línea de Comando**  
    - **Explicación:** Desactiva la verificación de host para pruebas, aunque no se recomienda en producción.  
    - **Ejemplo:**  
      ```bash
      ssh -o "StrictHostKeyChecking=no" user@host.example.com
      ```

14. **Conectar a través de un Proxy SOCKS con ProxyCommand**  
    - **Explicación:** Usa netcat (nc) para enrutar la conexión SSH a través de un proxy.  
    - **Ejemplo:**  
      ```bash
      ssh -o "ProxyCommand=nc -X 5 -x proxy.example.com:1080 %h %p" user@host.example.com
      ```

15. **Ejecutar Comandos Remotos y Redirigir Salida a Archivo Local**  
    - **Explicación:** Ejecuta un comando remoto y guarda la salida en un archivo local para análisis.  
    - **Ejemplo:**  
      ```bash
      ssh user@host.example.com "cat /var/log/auth.log" > auth_log_remote.txt
      ```

16. **Reenviar Puertos de Aplicaciones para Depuración Remota**  
    - **Explicación:** Abre un túnel para conectar herramientas de depuración a servicios remotos.  
    - **Ejemplo:**  
      ```bash
      ssh -L 52698:localhost:52698 user@host.example.com
      ```

17. **Uso de Opciones de Cifrado y Kex Específicos desde Línea de Comando**  
    - **Explicación:** Fuerza el uso de algoritmos específicos para pruebas de seguridad o compatibilidad.  
    - **Ejemplo:**  
      ```bash
      ssh -o "KexAlgorithms=curve25519-sha256" -o "Ciphers=chacha20-poly1305@openssh.com" user@host.example.com
      ```

18. **Ejecutar un Comando Remoto y Capturar la Salida de Error**  
    - **Explicación:** Separa la salida estándar y de error al ejecutar comandos remotos para diagnósticos.  
    - **Ejemplo:**  
      ```bash
      ssh user@host.example.com "ls /nonexistent" 2> error.log
      ```

19. **Utilizar la Función de "Bastion Host" con ProxyJump en Cadena**  
    - **Explicación:** Conecta a un destino final pasando por varios saltos de servidores de salto (bastion).  
    - **Ejemplo:**  
      ```bash
      ssh -J bastion1.example.com,bastion2.example.com user@target.example.com
      ```

20. **Ejecutar un Comando de Multiplexación y Verificar Logs de la Sesión**  
    - **Explicación:** Conecta usando ControlMaster y revisa registros para asegurar la correcta compartición de la sesión.  
    - **Ejemplo:**  
      ```bash
      ssh -M -S /tmp/ssh_mux user@host.example.com
      # Luego, en otra terminal:
      ssh -O check user@host.example.com
      ```

21. **Utilizar Rsync sobre SSH para Sincronización Segura de Datos**  
    - **Explicación:** Transfiere archivos y directorios de manera segura entre servidores mediante SSH.  
    - **Ejemplo:**  
      ```bash
      rsync -avz -e "ssh -i /path/to/key" /local/dir/ user@host.example.com:/remote/dir/
      ```

22. **Conectar Usando un Archivo de Configuración SSH Personalizado**  
    - **Explicación:** Especifica un archivo de configuración alternativo para pruebas o entornos aislados.  
    - **Ejemplo:**  
      ```bash
      ssh -F /path/to/custom_ssh_config user@host.example.com
      ```

23. **Extraer la Huella Digital de un Host para Auditoría**  
    - **Explicación:** Utiliza ssh-keyscan para obtener las huellas digitales de los hosts y verificar su autenticidad.  
    - **Ejemplo:**  
      ```bash
      ssh-keyscan -t rsa,ecdsa host.example.com
      ```

24. **Conectar y Forzar la Asignación de un TTY Remoto**  
    - **Explicación:** Fuerza la asignación de TTY para ejecutar comandos interactivos en el host remoto.  
    - **Ejemplo:**  
      ```bash
      ssh -t user@host.example.com "sudo su -"
      ```

25. **Ejecutar un Comando Remoto con un Script Encapsulado**  
    - **Explicación:** Ejecuta un script remoto encapsulado para realizar tareas complejas de administración.  
    - **Ejemplo:**  
      ```bash
      ssh user@host.example.com 'bash -s' < local_script.sh
      ```

26. **Utilizar SSH en Modo Verbose para Depuración de Conexión de Agente**  
    - **Explicación:** Activa la depuración para el reenvío del agente y verificar que las claves se transfieren correctamente.  
    - **Ejemplo:**  
      ```bash
      ssh -A -vvv user@host.example.com
      ```

27. **Ejecutar Comando Remoto y Usar un Pipe para Procesar la Salida**  
    - **Explicación:** Encadena comandos remotos y locales para filtrar y formatear la salida en tiempo real.  
    - **Ejemplo:**  
      ```bash
      ssh user@host.example.com "ps aux" | grep apache2
      ```

28. **Reenviar un Socket Unix a Través de SSH para Depuración**  
    - **Explicación:** Encapsula un socket Unix en un túnel SSH para acceder a servicios locales de forma remota.  
    - **Ejemplo:**  
      ```bash
      ssh -L /tmp/remote.sock:/var/run/docker.sock user@host.example.com
      ```

29. **Utilizar la Opción -o 'LogLevel=DEBUG3' para Capturar Máximo Detalle**  
    - **Explicación:** Incrementa el nivel de log durante una sesión para una depuración detallada de la conexión SSH.  
    - **Ejemplo:**  
      ```bash
      ssh -o "LogLevel=DEBUG3" user@host.example.com
      ```

30. **Ejecutar un Comando Remoto y Transferir Archivos Comprimidos via SCP**  
    - **Explicación:** Combina SSH y SCP para ejecutar tareas remotas y transferir datos comprimidos en un solo flujo.  
    - **Ejemplo:**  
      ```bash
      ssh user@host.example.com "tar czf - /var/log" | dd of=/backup/host_logs_$(date +%F).tar.gz
      ```
