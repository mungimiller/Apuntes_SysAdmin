
### Configuraciones Avanzadas de Wazuh

1. **Monitoreo Avanzado de Logs de Firewall**  
   - **Explicación:** Configura Wazuh para analizar logs de firewalls (iptables, pf, etc.) con decodificadores y reglas personalizadas, permitiendo detectar intrusiones y accesos no autorizados.  
   - **Ejemplo:**  
     ```xml
     <localfile>
       <log_format>syslog</log_format>
       <location>/var/log/firewall.log</location>
     </localfile>
     <rules>
       <decoded_as>firewall</decoded_as>
       <rule id="100100" level="10">
         <if_sid>550</if_sid>
         <match>DROP</match>
         <description>Paquete bloqueado por el firewall</description>
       </rule>
     </rules>
     ```

2. **Integridad de Archivos Críticos**  
   - **Explicación:** Habilita y configura el módulo syscheck para monitorizar cambios en archivos y directorios críticos, con exclusiones y frecuencias adaptadas al entorno.  
   - **Ejemplo:**  
     ```xml
     <syscheck>
       <frequency>7200</frequency>
       <directories check_all="yes" realtime="yes">/etc,/usr/bin</directories>
       <ignore>/etc/mtab,/etc/fstab</ignore>
     </syscheck>
     ```

3. **Reglas Personalizadas para Detección de Rootkit**  
   - **Explicación:** Configura reglas específicas para detectar modificaciones y comportamientos asociados a rootkits utilizando módulos de análisis y rootcheck.  
   - **Ejemplo:**  
     ```xml
     <rootcheck>
       <skip_nfs>yes</skip_nfs>
       <directories>/bin,/sbin,/usr/bin,/usr/sbin</directories>
     </rootcheck>
     <rules>
       <rule id="101000" level="12">
         <decoded_as>rootcheck</decoded_as>
         <match>rootkit</match>
         <description>Posible actividad de rootkit detectada</description>
       </rule>
     </rules>
     ```

4. **Decodificador Personalizado para Logs de Apache**  
   - **Explicación:** Crea un decodificador específico para los logs de Apache, facilitando la detección de patrones de ataques web y anomalías en peticiones HTTP.  
   - **Ejemplo:**  
     ```xml
     <decoder name="apache_custom">
       <program_name>apache2</program_name>
       <regex>^(?<timestamp>\S+\s+\S+)\s+(?<component>\S+):\s+\[(?<level>\w+)\]\s+(?<message>.*)$</regex>
     </decoder>
     ```

5. **Reglas Avanzadas para Detección de Ataques Web**  
   - **Explicación:** Define reglas personalizadas que combinen decodificadores y condiciones para detectar inyecciones SQL, XSS y otros ataques en aplicaciones web.  
   - **Ejemplo:**  
     ```xml
     <rule id="102000" level="15">
       <decoded_as>apache_custom</decoded_as>
       <match>SELECT.*FROM</match>
       <description>Posible inyección SQL detectada</description>
     </rule>
     ```

6. **Active Response para Bloqueo Dinámico de IP**  
   - **Explicación:** Configura respuestas activas que ejecuten scripts personalizados para bloquear IPs automáticamente cuando se detecten comportamientos maliciosos.  
   - **Ejemplo:**  
     ```xml
     <active-response>
       <command>firewall-drop</command>
       <location>local</location>
       <rules_id>102000,102100</rules_id>
       <timeout>600</timeout>
     </active-response>
     ```

7. **Integración con Slack para Alertas Inmediatas**  
   - **Explicación:** Configura la salida de alertas mediante integración con Slack, permitiendo notificaciones en tiempo real al equipo de seguridad.  
   - **Ejemplo:**  
     ```xml
     <integration>
       <name>slack</name>
       <hook_url>https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX</hook_url>
       <levels>10,11,12,13,14,15</levels>
     </integration>
     ```

8. **Configuración de Cluster en el Manager**  
   - **Explicación:** Ajusta la configuración de cluster en el archivo de ossec.conf para balancear la carga y asegurar alta disponibilidad del Wazuh Manager.  
   - **Ejemplo:**  
     ```xml
     <cluster>
       <node_name>manager01</node_name>
       <node_type>master</node_type>
       <port>1516</port>
       <bind_addr>192.168.1.10</bind_addr>
       <nodes>manager02,manager03</nodes>
     </cluster>
     ```

9. **Configuración Multi-Plataforma para Agentes**  
   - **Explicación:** Define configuraciones específicas para agentes Linux, Windows y Mac, con parámetros adaptados para cada sistema operativo.  
   - **Ejemplo:**  
     ```xml
     <agent_config>
       <client>
         <name>LinuxAgent</name>
         <config_profile>linux-default</config_profile>
       </client>
       <client>
         <name>WindowsAgent</name>
         <config_profile>windows-default</config_profile>
       </client>
     </agent_config>
     ```

10. **Syscollector para Inventario de Hardware y Software**  
    - **Explicación:** Activa y configura el módulo syscollector para recabar información detallada del hardware, software y configuraciones del sistema.  
    - **Ejemplo:**  
      ```xml
      <syscollector>
        <scan_on_start>yes</scan_on_start>
        <frequency>43200</frequency>
        <directories>/usr, /opt</directories>
      </syscollector>
      ```

11. **Integración con Herramientas de Vulnerabilidad**  
    - **Explicación:** Configura Wazuh para importar y correlacionar datos de escáneres de vulnerabilidades, ayudando en la remediación proactiva.  
    - **Ejemplo:**  
      ```xml
      <integration>
        <name>vulnerability_scanner</name>
        <file>/var/log/vuln_scanner.log</file>
        <levels>12,13,14,15</levels>
      </integration>
      ```

12. **Monitorización de Contenedores Docker**  
    - **Explicación:** Configura la recolección de logs y métricas de contenedores Docker, integrando decodificadores y reglas para entornos virtualizados.  
    - **Ejemplo:**  
      ```xml
      <localfile>
        <log_format>json</log_format>
        <location>/var/lib/docker/containers/*/*.log</location>
        <decoder>docker</decoder>
      </localfile>
      ```

13. **Monitorización de Cambios en Directorios Críticos**  
    - **Explicación:** Ajusta la configuración para vigilar cambios en directorios sensibles como /etc, generando alertas ante modificaciones inesperadas.  
    - **Ejemplo:**  
      ```xml
      <syscheck>
        <directories realtime="yes" check_all="yes">/etc,/var/lib</directories>
        <frequency>3600</frequency>
      </syscheck>
      ```

14. **Análisis de Actividad de Malware**  
    - **Explicación:** Configura reglas y decodificadores para identificar patrones sospechosos asociados a malware en logs del sistema.  
    - **Ejemplo:**  
      ```xml
      <rules>
        <rule id="103000" level="14">
          <decoded_as>syslog</decoded_as>
          <match>malware_detected</match>
          <description>Actividad sospechosa de malware</description>
        </rule>
      </rules>
      ```

15. **Decodificador para Logs del Sistema Operativo**  
    - **Explicación:** Configura decodificadores que transformen los logs genéricos del sistema en eventos estructurados para análisis detallado.  
    - **Ejemplo:**  
      ```xml
      <decoder name="os_syslog">
        <program_name>syslog</program_name>
        <regex>^(?<timestamp>\w+\s+\d+\s+\d+:\d+:\d+)\s+(?<hostname>\S+)\s+(?<program>\S+):\s+(?<message>.*)$</regex>
      </decoder>
      ```

16. **Decodificador para Logs de Nginx**  
    - **Explicación:** Crea un decodificador optimizado para interpretar los logs de Nginx, facilitando la correlación de eventos web.  
    - **Ejemplo:**  
      ```xml
      <decoder name="nginx">
        <program_name>nginx</program_name>
        <regex>^(?<ip>\S+)\s+-\s+-\s+\[(?<timestamp>[^\]]+)\]\s+"(?<request>[^"]+)"\s+(?<status>\d+)\s+(?<size>\d+)</regex>
      </decoder>
      ```

17. **Integración con ELK para Análisis Centralizado**  
    - **Explicación:** Configura la salida de Wazuh para enviar logs y alertas a un clúster ELK, facilitando análisis centralizados y visualización.  
    - **Ejemplo:**  
      ```xml
      <integration>
        <name>elk</name>
        <log_format>json</log_format>
        <destination>http://elk.example.com:9200</destination>
      </integration>
      ```

18. **Monitoreo de Actividad de Red y Conexiones**  
    - **Explicación:** Ajusta la recolección de datos de red para detectar conexiones inusuales o potencialmente maliciosas en tiempo real.  
    - **Ejemplo:**  
      ```xml
      <localfile>
        <log_format>syslog</log_format>
        <location>/var/log/network.log</location>
      </localfile>
      ```

19. **Detección de Anomalías en Accesos SSH**  
    - **Explicación:** Configura reglas y decodificadores específicos para analizar logs de autenticación SSH y detectar patrones de fuerza bruta o accesos no autorizados.  
    - **Ejemplo:**  
      ```xml
      <decoder name="sshd">
        <program_name>sshd</program_name>
        <regex>^(?<timestamp>\w+\s+\d+\s+\d+:\d+:\d+)\s+(?<host>\S+)\s+sshd\[(?<pid>\d+)\]:\s+(?<message>.*)$</regex>
      </decoder>
      <rules>
        <rule id="104000" level="13">
          <decoded_as>sshd</decoded_as>
          <match>Failed password</match>
          <description>Intento fallido de autenticación SSH</description>
        </rule>
      </rules>
      ```

20. **Reglas para Detección de Actividad en Bases de Datos**  
    - **Explicación:** Define reglas que correlacionen eventos en logs de bases de datos, detectando actividades anómalas o intentos de intrusión.  
    - **Ejemplo:**  
      ```xml
      <localfile>
        <log_format>syslog</log_format>
        <location>/var/log/mysql.log</location>
      </localfile>
      <rules>
        <rule id="105000" level="12">
          <decoded_as>syslog</decoded_as>
          <match>Access denied for user</match>
          <description>Acceso denegado en MySQL</description>
        </rule>
      </rules>
      ```

21. **Integración con Fuentes de Inteligencia de Amenazas**  
    - **Explicación:** Configura Wazuh para importar datos de threat intelligence y correlacionarlos con eventos internos para mejorar la detección de ataques.  
    - **Ejemplo:**  
      ```xml
      <integration>
        <name>threat_intel</name>
        <file>/var/ossec/etc/lists/blacklist.txt</file>
        <levels>14,15</levels>
      </integration>
      ```

22. **Monitorización de Contenedores en Kubernetes**  
    - **Explicación:** Ajusta la configuración para recoger logs de nodos y contenedores en clusters Kubernetes, integrando decodificadores y reglas específicas.  
    - **Ejemplo:**  
      ```xml
      <localfile>
        <log_format>json</log_format>
        <location>/var/log/containers/*.log</location>
      </localfile>
      ```

23. **Reglas para Detectar Accesos No Autorizados a Archivos**  
    - **Explicación:** Configura reglas que analicen eventos de acceso a archivos sensibles, detectando modificaciones o accesos anómalos.  
    - **Ejemplo:**  
      ```xml
      <rules>
        <rule id="106000" level="14">
          <decoded_as>syscheck</decoded_as>
          <match>Changed file</match>
          <description>Modificación en archivo crítico detectada</description>
        </rule>
      </rules>
      ```

24. **Optimización de Decodificadores para Reducir Falsos Positivos**  
    - **Explicación:** Ajusta expresiones regulares y parámetros de decodificadores para mejorar la precisión en la detección de eventos.  
    - **Ejemplo:**  
      ```xml
      <decoder name="optimized_decoder">
        <program_name>custom_app</program_name>
        <regex>^(?<timestamp>\S+)\s+(?<level>\S+):\s+(?<message>.*)$</regex>
        <options>ignore_case</options>
      </decoder>
      ```

25. **Configuración de Alertas Multi-Nivel**  
    - **Explicación:** Define reglas que asignen niveles de alerta diferenciados según la gravedad y el tipo de evento, facilitando la priorización.  
    - **Ejemplo:**  
      ```xml
      <rules>
        <rule id="107000" level="10">
          <decoded_as>syslog</decoded_as>
          <match>Successful login</match>
          <description>Login satisfactorio</description>
        </rule>
        <rule id="107001" level="15">
          <decoded_as>syslog</decoded_as>
          <match>Multiple failed login</match>
          <description>Accesos fallidos repetidos</description>
        </rule>
      </rules>
      ```

26. **Análisis de Logs de VPN para Seguridad de Conexiones Remotas**  
    - **Explicación:** Configura la recolección y análisis de logs de VPN para detectar accesos sospechosos y validar conexiones remotas.  
    - **Ejemplo:**  
      ```xml
      <localfile>
        <log_format>syslog</log_format>
        <location>/var/log/openvpn.log</location>
      </localfile>
      ```

27. **Configuración para Integración con SIEM Externo**  
    - **Explicación:** Exporta eventos y alertas en formato JSON o CEF para su integración con soluciones SIEM comerciales.  
    - **Ejemplo:**  
      ```xml
      <integration>
        <name>siem_integration</name>
        <output_format>json</output_format>
        <destination>192.168.1.50:514</destination>
      </integration>
      ```

28. **Ajustes de Rendimiento para Ambientes de Alta Carga**  
    - **Explicación:** Optimiza parámetros como frecuencia de escaneo y tamaño de buffer para adaptarse a entornos con gran volumen de eventos.  
    - **Ejemplo:**  
      ```xml
      <global>
        <log_format>json</log_format>
        <max_agents>10000</max_agents>
        <queue_size>8192</queue_size>
      </global>
      ```

29. **Configuración de Backups Automatizados de la Configuración**  
    - **Explicación:** Establece scripts y tareas programadas para respaldar la configuración de Wazuh, garantizando la recuperación ante fallos.  
    - **Ejemplo:**  
      ```xml
      <localfile>
        <log_format>syslog</log_format>
        <location>/var/ossec/etc/ossec.conf</location>
      </localfile>
      <!-- Se recomienda usar cron para copiar el archivo de configuración a un repositorio seguro -->
      ```

30. **Reglas para Monitoreo y Correlación de Eventos de Seguridad**  
    - **Explicación:** Define un conjunto de reglas que correlacionen eventos de distintos módulos (syscheck, rootcheck, decoders) para detectar incidentes complejos.  
    - **Ejemplo:**  
      ```xml
      <rules>
        <rule id="108000" level="16">
          <decoded_as>syscheck</decoded_as>
          <match>Modified file</match>
          <description>Modificación crítica detectada; correlacionar con eventos de rootcheck</description>
          <group>correlation</group>
        </rule>
      </rules>
      ```

---

### Comandos Avanzados de Wazuh en Entornos de Producción

1. **Verificar el Estado General del Manager**  
   - **Explicación:** Muestra el estado actual del Wazuh Manager, incluyendo estadísticas de agentes y módulos activos.  
   - **Ejemplo:**  
     ```bash
     wazuh-control status
     ```

2. **Reiniciar el Servicio del Manager con Logs Detallados**  
   - **Explicación:** Reinicia el Wazuh Manager y muestra logs en tiempo real para verificar la correcta aplicación de cambios.  
   - **Ejemplo:**  
     ```bash
     wazuh-control restart && tail -f /var/ossec/logs/ossec.log
     ```

3. **Probar la Configuración de un Agente**  
   - **Explicación:** Verifica la configuración de un agente antes de la conexión, simulando eventos de log para análisis.  
   - **Ejemplo:**  
     ```bash
     wazuh-logtest -a /var/ossec/etc/ossec.conf
     ```

4. **Registrar un Agente al Manager**  
   - **Explicación:** Utiliza el comando de autenticación para registrar un nuevo agente en el Manager de forma segura.  
   - **Ejemplo:**  
     ```bash
     agent-auth -m 192.168.1.10 -p 1515
     ```

5. **Listar Agentes Conectados y su Estado**  
   - **Explicación:** Muestra una lista detallada de todos los agentes registrados y su estado actual.  
   - **Ejemplo:**  
     ```bash
     agent-control -l
     ```

6. **Forzar Sincronización de Configuración entre Agentes**  
   - **Explicación:** Ordena a los agentes actualizar y sincronizar su configuración con el Manager.  
   - **Ejemplo:**  
     ```bash
     agent-control -R
     ```

7. **Ejecutar el Módulo de Análisis en Primer Plano**  
   - **Explicación:** Inicia el módulo de análisis para depurar reglas y decodificadores en tiempo real.  
   - **Ejemplo:**  
     ```bash
     wazuh-analysisd -f
     ```

8. **Ejecutar el Módulo de Active Response Manualmente**  
   - **Explicación:** Prueba manualmente la ejecución de un script de active response para validar su funcionamiento.  
   - **Ejemplo:**  
     ```bash
     /var/ossec/active-response/bin/firewall-drop 192.0.2.100
     ```

9. **Verificar Logs de Rootcheck en Tiempo Real**  
   - **Explicación:** Monitorea en tiempo real los logs generados por el módulo rootcheck para detectar anomalías.  
   - **Ejemplo:**  
     ```bash
     tail -f /var/ossec/logs/rootcheck.log
     ```

10. **Ejecutar el Comando de Prueba de Decodificación**  
    - **Explicación:** Simula la decodificación de un log para verificar que los decodificadores personalizados funcionen correctamente.  
    - **Ejemplo:**  
      ```bash
      wazuh-logtest -a /var/ossec/etc/decoders/0010-wazuh_decoders.xml
      ```

11. **Verificar el Estado del Módulo Syscollector**  
    - **Explicación:** Revisa los registros y estadísticas del módulo syscollector para confirmar la recolección de inventario.  
    - **Ejemplo:**  
      ```bash
      grep "syscollector" /var/ossec/logs/ossec.log
      ```

12. **Consultar Alertas Recientes del Sistema**  
    - **Explicación:** Muestra las alertas generadas recientemente, facilitando la revisión de incidentes.  
    - **Ejemplo:**  
      ```bash
      tail -n 50 /var/ossec/logs/alerts/alerts.json
      ```

13. **Obtener Información de la Versión y Compilación**  
    - **Explicación:** Muestra detalles de la versión instalada de Wazuh y parámetros de compilación para auditorías.  
    - **Ejemplo:**  
      ```bash
      wazuh-control version
      ```

14. **Recargar la Configuración sin Reiniciar el Manager**  
    - **Explicación:** Aplica cambios en la configuración de forma dinámica sin detener el servicio.  
    - **Ejemplo:**  
      ```bash
      wazuh-control reload
      ```

15. **Ejecutar el Módulo de Monitorización de Agentes**  
    - **Explicación:** Inicia el módulo que verifica la conectividad y el estado de todos los agentes en el entorno.  
    - **Ejemplo:**  
      ```bash
      wazuh-agentd -f
      ```

16. **Depurar Eventos con el Comando de Logtest en Modo Verbose**  
    - **Explicación:** Ejecuta una prueba de log con salida detallada para diagnosticar problemas en la interpretación de eventos.  
    - **Ejemplo:**  
      ```bash
      wazuh-logtest -v < /var/ossec/logs/test.log
      ```

17. **Consultar la Cola de Eventos del Manager**  
    - **Explicación:** Revisa la cola de procesamiento de eventos para identificar posibles cuellos de botella.  
    - **Ejemplo:**  
      ```bash
      cat /var/ossec/queue/events_queue.log
      ```

18. **Verificar Configuraciones de Integración con SIEM**  
    - **Explicación:** Revisa el archivo de configuración exportado para la integración con SIEM y asegurar la conectividad.  
    - **Ejemplo:**  
      ```bash
      cat /var/ossec/etc/integrations/siem.conf
      ```

19. **Ejecutar un Script de Prueba para Active Response**  
    - **Explicación:** Lanza manualmente un script de active response para validar la respuesta ante un incidente simulado.  
    - **Ejemplo:**  
      ```bash
      /var/ossec/active-response/bin/disable-account testuser
      ```

20. **Verificar el Estado del Módulo de Cluster**  
    - **Explicación:** Comprueba el estado del clúster del Manager para confirmar la sincronización entre nodos.  
    - **Ejemplo:**  
      ```bash
      wazuh-clusterd -s
      ```

21. **Obtener Estadísticas de Rendimiento del Manager**  
    - **Explicación:** Consulta métricas de rendimiento y uso de recursos del Manager para optimizar la configuración en entornos de alta carga.  
    - **Ejemplo:**  
      ```bash
      cat /var/ossec/stats/manager_stats.json
      ```

22. **Enviar un Evento de Prueba a través del Agente**  
    - **Explicación:** Genera y envía un evento de prueba desde un agente para verificar la correcta recepción en el Manager.  
    - **Ejemplo:**  
      ```bash
      wazuh-logtest -a /var/ossec/etc/agent.conf
      ```

23. **Consultar el Registro de Agentes Desconectados**  
    - **Explicación:** Lista los agentes que han perdido conectividad recientemente para tomar acciones correctivas.  
    - **Ejemplo:**  
      ```bash
      grep "disconnected" /var/ossec/logs/ossec.log
      ```

24. **Ejecutar Comando de Sincronización Manual del Agente**  
    - **Explicación:** Fuerza a un agente a sincronizar su configuración y recolección de datos con el Manager.  
    - **Ejemplo:**  
      ```bash
      agent-control -r <agent_id>
      ```

25. **Verificar la Integridad del Archivo de Configuración**  
    - **Explicación:** Utiliza una herramienta de validación para asegurarse de que el archivo ossec.conf esté correctamente formado.  
    - **Ejemplo:**  
      ```bash
      wazuh-logtest -a /var/ossec/etc/ossec.conf | grep "Valid configuration"
      ```

26. **Consultar la Lista de Decodificadores Cargados**  
    - **Explicación:** Muestra todos los decodificadores activos para confirmar la correcta interpretación de los logs.  
    - **Ejemplo:**  
      ```bash
      cat /var/ossec/etc/decoders/0010-wazuh_decoders.xml | grep "<decoder"
      ```

27. **Verificar la Configuración del Módulo Syscheck**  
    - **Explicación:** Revisa los parámetros de syscheck para confirmar la monitorización de integridad de archivos.  
    - **Ejemplo:**  
      ```bash
      grep "<syscheck>" /var/ossec/etc/ossec.conf
      ```

28. **Consultar el Registro de Decisiones de Active Response**  
    - **Explicación:** Muestra el log específico de respuestas activas para auditar acciones tomadas ante incidentes.  
    - **Ejemplo:**  
      ```bash
      tail -n 50 /var/ossec/logs/active-responses.log
      ```

29. **Ejecutar Prueba de Conectividad del Agente con el Manager**  
    - **Explicación:** Valida manualmente la conexión entre un agente y el Manager, asegurando la comunicación bidireccional.  
    - **Ejemplo:**  
      ```bash
      agent-auth -m 192.168.1.10 -p 1515 -A
      ```

30. **Revisar Configuraciones de Integración de Alertas Externas**  
    - **Explicación:** Consulta la configuración exportada para integraciones (como Slack o SIEM) y verifica parámetros críticos.  
    - **Ejemplo:**  
      ```bash
      cat /var/ossec/etc/integrations/slack.conf
      ```
