# Resumen de IPTABLES
IPTABLES es una herramienta de filtrado de paquetes muy potente en Linux que te permite definir políticas de seguridad para controlar el tráfico de red entrante, saliente y el tráfico que se reenvía. Con IPTABLES puedes:

- Definir cadenas: Las reglas se organizan en cadenas (INPUT, OUTPUT, FORWARD) que determinan cómo se procesan los paquetes.
- Filtrar tráfico: Puedes aceptar, rechazar o descartar paquetes en función de criterios como la dirección IP, puertos, protocolo y estado de la conexión.
- Implementar NAT: Con la tabla NAT se pueden crear reglas para enmascarar direcciones o redireccionar puertos, permitiendo la conexión de redes internas a Internet.
- Registrar eventos: Es posible configurar registros para monitorizar actividades o ataques, lo que facilita la auditoría y el análisis de incidentes.

## Instalación
La forma de instalar IPTABLES puede variar según la distribución:

Debian/Ubuntu:

Actualiza el índice de paquetes:
```
sudo apt-get update
```
Instala IPTABLES (aunque suele venir preinstalado):

```
sudo apt-get install iptables
```
Para guardar reglas de forma persistente, instala el paquete de persistencia:
```
sudo apt-get install iptables-persistent
```
Habilita y arranca el servicio:
```
sudo systemctl enable iptables
sudo systemctl start iptables
```

## Configuración Básica
Una vez instalado, la configuración se realiza mediante la adición o eliminación de reglas. Algunas reglas básicas incluyen:

Permitir conexiones ya establecidas:
```
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```
Permitir conexiones SSH:
```
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```
Bloquear todo lo demás:
```
iptables -A INPUT -j DROP
```

### Configuraciones Avanzadas de IPTABLES

1. **Política por Defecto Restringida**  
   - **Explicación:** Establece políticas por defecto de denegar el tráfico entrante y permitir el saliente, lo que fuerza a definir reglas explícitas para aceptar conexiones.  
   - **Ejemplo:**  
     ```bash
     iptables -P INPUT DROP
     iptables -P FORWARD DROP
     iptables -P OUTPUT ACCEPT
     ```

2. **Permitir Conexiones Establecidas y Relacionadas**  
   - **Explicación:** Acepta tráfico que ya fue autorizado previamente, reduciendo el riesgo de bloquear comunicaciones legítimas.  
   - **Ejemplo:**  
     ```bash
     iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
     ```

3. **Limitación de Conexiones SSH por IP**  
   - **Explicación:** Evita ataques de fuerza bruta en SSH limitando el número de nuevas conexiones por dirección IP.  
   - **Ejemplo:**  
     ```bash
     iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --set
     iptables -A INPUT -p tcp --dport 22 -m state --state NEW -m recent --update --seconds 60 --hitcount 4 -j DROP
     ```

4. **Protección Contra SYN Flood**  
   - **Explicación:** Mitiga ataques de inundación SYN usando límites en las solicitudes incompletas.  
   - **Ejemplo:**  
     ```bash
     iptables -A INPUT -p tcp --syn -m limit --limit 10/s --limit-burst 20 -j ACCEPT
     iptables -A INPUT -p tcp --syn -j DROP
     ```

5. **Descartar Paquetes Inválidos**  
   - **Explicación:** Elimina paquetes que no cumplan con los criterios normales de conexión, reduciendo el ruido y posibles ataques.  
   - **Ejemplo:**  
     ```bash
     iptables -A INPUT -m state --state INVALID -j DROP
     ```

6. **Protección contra Spoofing**  
   - **Explicación:** Bloquea paquetes entrantes que tengan direcciones IP privadas en interfaces públicas.  
   - **Ejemplo:**  
     ```bash
     iptables -A INPUT -i eth0 -s 10.0.0.0/8 -j DROP
     iptables -A INPUT -i eth0 -s 172.16.0.0/12 -j DROP
     iptables -A INPUT -i eth0 -s 192.168.0.0/16 -j DROP
     ```

7. **Rate Limiting para ICMP (Ping Flood)**  
   - **Explicación:** Limita el número de solicitudes ICMP para prevenir ataques de denegación de servicio por ping.  
   - **Ejemplo:**  
     ```bash
     iptables -A INPUT -p icmp --icmp-type echo-request -m limit --limit 1/s --limit-burst 5 -j ACCEPT
     iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
     ```

8. **Bloqueo de Paquetes Fragmentados**  
   - **Explicación:** Rechaza paquetes fragmentados que suelen usarse en ataques para evadir detección.  
   - **Ejemplo:**  
     ```bash
     iptables -A INPUT -f -j DROP
     ```

9. **Registro y Descarga de TCP con Banderas Sospechosas**  
   - **Explicación:** Detecta y registra intentos con combinaciones de banderas TCP anómalas antes de descartarlos.  
   - **Ejemplo:**  
     ```bash
     iptables -A INPUT -p tcp --tcp-flags ALL NONE -j LOG --log-prefix "Null Packet: "
     iptables -A INPUT -p tcp --tcp-flags ALL NONE -j DROP
     ```

10. **Prevención de Escaneo de Puertos**  
    - **Explicación:** Usa reglas para detectar patrones típicos de escaneo de puertos y bloquear el tráfico sospechoso.  
    - **Ejemplo:**  
      ```bash
      iptables -N PORTSCAN
      iptables -A PORTSCAN -p tcp --tcp-flags SYN,ACK,FIN,RST RST -m limit --limit 1/s --limit-burst 2 -j RETURN
      iptables -A PORTSCAN -j DROP
      iptables -A INPUT -p tcp -m tcp --dport 0:65535 -j PORTSCAN
      ```

11. **Limitación de Nuevas Conexiones por IP**  
    - **Explicación:** Restringe el número de conexiones nuevas desde una misma IP para evitar abusos.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp -m state --state NEW -m connlimit --connlimit-above 20 -j DROP
      ```

12. **Protección contra Ataques DoS en HTTP**  
    - **Explicación:** Limita la tasa de solicitudes nuevas a un servidor web para mitigar ataques DoS.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp --dport 80 -m state --state NEW -m recent --set --name http
      iptables -A INPUT -p tcp --dport 80 -m state --state NEW -m recent --update --seconds 60 --hitcount 50 --name http -j DROP
      ```

13. **Protección para SMTP Contra Spam**  
    - **Explicación:** Limita el número de conexiones nuevas al puerto SMTP para prevenir abusos y ataques de spam.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp --dport 25 -m state --state NEW -m recent --set --name smtp
      iptables -A INPUT -p tcp --dport 25 -m state --state NEW -m recent --update --seconds 60 --hitcount 10 --name smtp -j DROP
      ```

14. **Permitir Acceso a Administración desde IP Específica**  
    - **Explicación:** Restringe el acceso a servicios de administración (por ejemplo, webmin) solo a IPs confiables.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp --dport 10000 -s 203.0.113.100 -j ACCEPT
      iptables -A INPUT -p tcp --dport 10000 -j DROP
      ```

15. **Protección para Servidores DNS contra Amplificación**  
    - **Explicación:** Limita las respuestas DNS para prevenir ataques de amplificación.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p udp --dport 53 -m limit --limit 10/s --limit-burst 20 -j ACCEPT
      iptables -A INPUT -p udp --dport 53 -j DROP
      ```

16. **Registro Avanzado de Paquetes Rechazados**  
    - **Explicación:** Registra paquetes descartados con un prefijo distintivo para facilitar análisis de seguridad.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -j LOG --log-prefix "IPTABLES-DROP: " --log-level 4
      iptables -A INPUT -j DROP
      ```

17. **Uso de Módulo State para Tráfico de Aplicaciones**  
    - **Explicación:** Asegura que solo se acepten paquetes en estados válidos para aplicaciones críticas.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -m state --state NEW,ESTABLISHED,RELATED -p tcp --dport 443 -j ACCEPT
      ```

18. **Protección Avanzada para FTP con Multiport**  
    - **Explicación:** Permite conexiones FTP seguras limitando puertos y estados de conexión.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp -m multiport --dports 21,20,30000:31000 -m state --state NEW,ESTABLISHED -j ACCEPT
      ```

19. **Configuración NAT para Balanceo de Carga**  
    - **Explicación:** Distribuye tráfico entrante entre varios servidores backend usando reglas de NAT.  
    - **Ejemplo:**  
      ```bash
      iptables -t nat -A PREROUTING -p tcp --dport 80 -m statistic --mode random --probability 0.5 -j DNAT --to-destination 192.168.1.101:80
      iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.1.102:80
      ```

20. **Redirección de Tráfico a Proxy Interno**  
    - **Explicación:** Redirige tráfico HTTP a un proxy interno para inspección o cacheo.  
    - **Ejemplo:**  
      ```bash
      iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.2.5:3128
      ```

21. **Filtrado por Coincidencia de Cadena (String Match)**  
    - **Explicación:** Detecta patrones específicos en el contenido de los paquetes para bloquear tráfico malicioso.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp --dport 80 -m string --algo bm --string "malicious_payload" -j DROP
      ```

22. **Bloqueo de Paquetes con Valores MSS Sospechosos**  
    - **Explicación:** Rechaza paquetes TCP que presentan valores MSS fuera del rango esperado, señal de posibles ataques.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp --tcp-flags SYN,RST SYN -m tcpmss --mss 500:600 -j DROP
      ```

23. **Bloqueo de Rango de IPs Maliciosas**  
    - **Explicación:** Utiliza listas negras para bloquear rangos de IP conocidos por actividades maliciosas.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -s 198.51.100.0/24 -j DROP
      ```

24. **Filtro Basado en Interfaz para Tráfico No Autorizado**  
    - **Explicación:** Asegura que solo el tráfico de interfaces autorizadas sea aceptado, bloqueando tráfico de interfaces desconocidas.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -i eth1 -j DROP
      ```

25. **Limitación de Flooding UDP**  
    - **Explicación:** Restringe la tasa de paquetes UDP para prevenir inundaciones y abusos en servicios basados en UDP.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p udp -m limit --limit 20/s --limit-burst 50 -j ACCEPT
      iptables -A INPUT -p udp -j DROP
      ```

26. **Descartar Paquetes TCP con Combinaciones Ilegales de Banderas**  
    - **Explicación:** Detecta y descarta paquetes TCP que usan combinaciones de banderas que no son válidas en tráfico normal.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp --tcp-flags ALL ALL -j DROP
      iptables -A INPUT -p tcp --tcp-flags ALL NONE -j DROP
      ```

27. **Cadena Personalizada para Registro de Tráfico Sospechoso**  
    - **Explicación:** Crea una cadena dedicada para registrar y posteriormente descartar paquetes sospechosos, permitiendo un análisis posterior.  
    - **Ejemplo:**  
      ```bash
      iptables -N SUSPICIOUS
      iptables -A SUSPICIOUS -j LOG --log-prefix "SUSPICIOUS: " --log-level 4
      iptables -A SUSPICIOUS -j DROP
      iptables -A INPUT -p tcp -m string --string "attack_pattern" -j SUSPICIOUS
      ```

28. **Rate Limiting para ICMP Avanzado**  
    - **Explicación:** Aplica límites estrictos a las solicitudes ICMP para prevenir abusos sin afectar la funcionalidad normal.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p icmp --icmp-type echo-request -m limit --limit 1/s --limit-burst 10 -j ACCEPT
      iptables -A INPUT -p icmp --icmp-type echo-request -j DROP
      ```

29. **Protección contra Escaneos con Múltiples Puertos UDP**  
    - **Explicación:** Limita la tasa de nuevos paquetes UDP a múltiples puertos para mitigar escaneos y ataques de inundación.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p udp -m multiport --dports 53,123,161 -m limit --limit 15/s --limit-burst 30 -j ACCEPT
      iptables -A INPUT -p udp -m multiport --dports 53,123,161 -j DROP
      ```

30. **Bloqueo de Tráfico de IP Privadas en Interfaces Externas**  
    - **Explicación:** Evita que tráfico con direcciones IP privadas entre por interfaces externas, previniendo rutas de evasión.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -i eth0 -s 10.0.0.0/8 -j DROP
      iptables -A INPUT -i eth0 -s 192.168.0.0/16 -j DROP
      ```

---

### Comandos Avanzados de IPTABLES en Entornos de Producción

1. **Listar Reglas con Contadores y Detalles**  
   - **Explicación:** Visualiza todas las reglas activas con contadores de paquetes y bytes para análisis de tráfico.  
   - **Ejemplo:**  
     ```bash
     iptables -L -v -n
     ```

2. **Insertar Regla para Bloquear una IP Específica**  
   - **Explicación:** Añade una regla para bloquear todo el tráfico entrante desde una dirección IP maliciosa.  
   - **Ejemplo:**  
     ```bash
     iptables -I INPUT -s 203.0.113.10 -j DROP
     ```

3. **Agregar Regla de Rate Limiting para Nuevas Conexiones**  
   - **Explicación:** Controla la tasa de nuevas conexiones en un puerto específico para mitigar intentos de DoS.  
   - **Ejemplo:**  
     ```bash
     iptables -A INPUT -p tcp --dport 80 -m state --state NEW -m limit --limit 25/minute --limit-burst 100 -j ACCEPT
     ```

4. **Insertar Regla de Registro para Paquetes Rechazados**  
   - **Explicación:** Agrega una regla que registre los paquetes descartados para auditoría sin afectar el rendimiento.  
   - **Ejemplo:**  
     ```bash
     iptables -A INPUT -m limit --limit 5/minute -j LOG --log-prefix "DROP: " --log-level 4
     ```

5. **Verificar Contadores de Paquetes en una Cadena Específica**  
   - **Explicación:** Muestra los contadores de una cadena particular para identificar reglas con alto impacto.  
   - **Ejemplo:**  
     ```bash
     iptables -L INPUT -v -n --line-numbers
     ```

6. **Guardar la Configuración Actual en un Archivo**  
   - **Explicación:** Exporta las reglas actuales para respaldos o migración a otros servidores.  
   - **Ejemplo:**  
     ```bash
     iptables-save > /etc/iptables/rules.v4
     ```

7. **Restaurar Reglas desde un Archivo de Configuración**  
   - **Explicación:** Restaura las reglas previamente guardadas para reestablecer la configuración de seguridad.  
   - **Ejemplo:**  
     ```bash
     iptables-restore < /etc/iptables/rules.v4
     ```

8. **Eliminar una Regla por Número de Línea**  
   - **Explicación:** Borra una regla específica identificada por su posición en la cadena.  
   - **Ejemplo:**  
     ```bash
     iptables -D INPUT 3
     ```

9. **Insertar Regla para Permitir Acceso desde una IP de Confianza**  
   - **Explicación:** Asegura que una IP específica tenga acceso sin restricciones en un puerto crítico.  
   - **Ejemplo:**  
     ```bash
     iptables -I INPUT -p tcp --dport 443 -s 198.51.100.20 -j ACCEPT
     ```

10. **Listar Reglas de la Tabla NAT con Detalles**  
    - **Explicación:** Visualiza las reglas de NAT para diagnosticar problemas de red y redirección de puertos.  
    - **Ejemplo:**  
      ```bash
      iptables -t nat -L -v -n
      ```

11. **Restablecer Contadores de Todas las Reglas**  
    - **Explicación:** Reinicia los contadores de paquetes y bytes en todas las reglas para iniciar un nuevo periodo de monitoreo.  
    - **Ejemplo:**  
      ```bash
      iptables -Z
      ```

12. **Agregar Comentarios a una Regla para Auditoría**  
    - **Explicación:** Inserta una regla con un comentario descriptivo para futuras referencias y análisis de seguridad.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp --dport 22 -j ACCEPT -m comment --comment "Permitir SSH desde red interna"
      ```

13. **Uso de la Opción Multiport para Múltiples Puertos**  
    - **Explicación:** Permite agregar reglas que afectan varios puertos simultáneamente, optimizando la administración.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp -m multiport --dports 80,443,8080 -j ACCEPT
      ```

14. **Insertar Regla para Bloquear Nuevas Conexiones TCP con Flags Específicos**  
    - **Explicación:** Bloquea intentos de conexión que no siguen el handshake TCP estándar.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp --tcp-flags SYN,ACK,FIN,RST RST -j DROP
      ```

15. **Añadir Regla con Coincidencia de Estado y Conexión Tracking**  
    - **Explicación:** Usa el módulo state para asegurar que solo se permitan conexiones legítimas basadas en el seguimiento de estado.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -m state --state NEW -p tcp --dport 25 -j ACCEPT
      ```

16. **Uso de la Extensión de String Match para Detección de Ataques**  
    - **Explicación:** Agrega una regla que inspecciona el contenido del paquete para detectar patrones de ataque conocidos.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp --dport 80 -m string --algo bm --string "bad_request" -j DROP
      ```

17. **Insertar Regla para Bloquear Fragmentación de Paquetes**  
    - **Explicación:** Previene ataques que usan fragmentación manipulada, descartando paquetes fragmentados.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -f -j DROP
      ```

18. **Insertar Regla para Mitigar Flooding UDP**  
    - **Explicación:** Limita la tasa de paquetes UDP en puertos críticos para evitar saturación del servidor.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p udp -m limit --limit 30/s --limit-burst 50 -j ACCEPT
      iptables -A INPUT -p udp -j DROP
      ```

19. **Monitorear la Cantidad de Paquetes por Regla (Hit Count)**  
    - **Explicación:** Verifica en tiempo real cuántos paquetes han coincidido con una regla específica, útil para análisis de incidentes.  
    - **Ejemplo:**  
      ```bash
      iptables -L INPUT -v -n --line-numbers
      ```

20. **Utilizar iptables-save para Exportar Configuración Completa**  
    - **Explicación:** Exporta toda la configuración de iptables para auditoría o respaldo, generando un volcado completo.  
    - **Ejemplo:**  
      ```bash
      iptables-save > /root/iptables-backup-$(date +%F).conf
      ```

21. **Aplicar iptables-restore para Cargar una Configuración Predefinida**  
    - **Explicación:** Restaura una configuración previamente exportada, permitiendo una rápida reconfiguración del firewall.  
    - **Ejemplo:**  
      ```bash
      iptables-restore < /root/iptables-backup-2025-03-20.conf
      ```

22. **Listar Reglas con Salida en Formato Numérico y Ordenado**  
    - **Explicación:** Muestra reglas en formato numérico y ordenado para facilitar la edición y eliminación manual.  
    - **Ejemplo:**  
      ```bash
      iptables -L -n --line-numbers | sort -n
      ```

23. **Insertar Regla para Limitar el Tráfico ICMP por Fuente**  
    - **Explicación:** Controla la tasa de solicitudes ICMP desde una IP para mitigar abusos sin bloquear comunicaciones legítimas.  
    - **Ejemplo:**  
      ```bash
      iptables -I INPUT -p icmp --icmp-type echo-request -m recent --set
      iptables -I INPUT -p icmp --icmp-type echo-request -m recent --update --seconds 1 --hitcount 5 -j DROP
      ```

24. **Eliminar Regla Específica por Coincidencia de Parámetros**  
    - **Explicación:** Remueve una regla que coincida exactamente con los parámetros especificados, facilitando ajustes finos.  
    - **Ejemplo:**  
      ```bash
      iptables -D INPUT -p tcp --dport 22 -s 203.0.113.10 -j DROP
      ```

25. **Ajustar Opciones de Log para Mayor Detalle**  
    - **Explicación:** Modifica una regla de log para incluir información adicional y así facilitar el análisis de incidentes.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp --dport 443 -j LOG --log-prefix "HTTPS Access: " --log-tcp-sequence --log-tcp-options
      ```

26. **Insertar Regla para Bloquear Paquetes con TTL Bajo**  
    - **Explicación:** Descarta paquetes con valores de TTL demasiado bajos, lo que puede indicar intentos de manipulación o rutas anómalas.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp -m ttl --ttl-lt 64 -j DROP
      ```

27. **Utilizar el Módulo “connlimit” para Controlar Conexiones Concurrentes**  
    - **Explicación:** Restringe el número de conexiones simultáneas por IP para un puerto crítico, evitando saturaciones.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp --dport 443 -m connlimit --connlimit-above 50 -j DROP
      ```

28. **Insertar Regla para Bloquear Tráfico con Flags TCP Ilegales**  
    - **Explicación:** Detecta y descarta paquetes que usen combinaciones de flags TCP no válidas en tráfico normal.  
    - **Ejemplo:**  
      ```bash
      iptables -A INPUT -p tcp --tcp-flags ALL FIN,PSH,URG -j DROP
      ```

29. **Listar Reglas NAT con Contadores y Salida Detallada**  
    - **Explicación:** Muestra todas las reglas en la tabla NAT para identificar rápidamente el tráfico redirigido y balanceado.  
    - **Ejemplo:**  
      ```bash
      iptables -t nat -L -v -n
      ```

30. **Ajustar TTL en Paquetes Salientes para Optimización de Red**  
    - **Explicación:** Modifica el valor TTL de paquetes salientes para mejorar la propagación o cumplir políticas de red específicas.  
    - **Ejemplo:**  
      ```bash
      iptables -t mangle -A POSTROUTING -o eth0 -j TTL --ttl-set 64
      ```
