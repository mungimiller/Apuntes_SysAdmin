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

## Comandos Avanzados de IPTABLES
Aquí tienes una lista de 30 comandos avanzados junto con una breve descripción de cada uno:

Listar reglas activas con detalles:
```
iptables -L -n -v
```

Muestra todas las reglas activas de forma numérica y con información detallada.

Vaciar todas las reglas:
```
iptables -F
```
Elimina todas las reglas en todas las cadenas.

Eliminar cadenas personalizadas:

bash
Copiar
iptables -X
Borra todas las cadenas definidas por el usuario.

Reiniciar contadores:

bash
Copiar
iptables -Z
Pone a cero los contadores de paquetes y bytes de todas las reglas.

Permitir conexiones establecidas y relacionadas:

bash
Copiar
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
Garantiza que las conexiones ya establecidas puedan continuar.

Permitir SSH (puerto 22):

bash
Copiar
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
Abre el puerto 22 para conexiones SSH.

Permitir tráfico HTTP (puerto 80):

bash
Copiar
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
Permite el acceso a servicios web.

Permitir tráfico HTTPS (puerto 443):

bash
Copiar
iptables -A INPUT -p tcp --dport 443 -j ACCEPT
Abre el puerto 443 para conexiones seguras.

Permitir mensajes ICMP (ping):

bash
Copiar
iptables -A INPUT -p icmp -j ACCEPT
Permite que los paquetes ICMP (como los pings) sean aceptados.

Permitir tráfico local (loopback):

bash
Copiar
iptables -A INPUT -i lo -j ACCEPT
Asegura que el tráfico de la interfaz de loopback no se bloquee.

Bloquear conexiones a MySQL (puerto 3306):

bash
Copiar
iptables -A INPUT -p tcp --dport 3306 -j DROP
Impide accesos no autorizados a bases de datos MySQL.

Permitir consultas DNS entrantes (UDP, puerto 53):

bash
Copiar
iptables -A INPUT -p udp --dport 53 -j ACCEPT
Permite que el servidor reciba solicitudes DNS.

Permitir respuestas DNS salientes (UDP, puerto 53):

bash
Copiar
iptables -A OUTPUT -p udp --dport 53 -j ACCEPT
Asegura que las respuestas DNS puedan salir del servidor.

Configurar NAT con masquerading:

bash
Copiar
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
Permite que los dispositivos internos compartan una única IP pública.

Redireccionar tráfico (port forwarding):

bash
Copiar
iptables -t nat -A PREROUTING -p tcp --dport 8080 -j DNAT --to-destination 192.168.1.100:80
Redirige el tráfico del puerto 8080 al puerto 80 de una IP interna.

Bloquear conexiones FTP (puerto 21):

bash
Copiar
iptables -A INPUT -p tcp --dport 21 -j DROP
Impide accesos no autorizados mediante FTP.

Bloquear conexiones SMTP (puerto 25):

bash
Copiar
iptables -A INPUT -p tcp --dport 25 -j DROP
Ayuda a prevenir el relé de spam bloqueando el puerto SMTP.

Limitar registros de paquetes rechazados:

bash
Copiar
iptables -A INPUT -m limit --limit 5/min -j LOG --log-prefix "IPTables denied: "
Reduce la cantidad de logs generados por paquetes denegados.

Control de tasa para nuevas conexiones TCP:

bash
Copiar
iptables -A INPUT -p tcp --syn -m limit --limit 10/s --limit-burst 20 -j ACCEPT
Permite controlar la cantidad de nuevas conexiones para evitar ataques de denegación.

Crear una cadena personalizada para logging:

bash
Copiar
iptables -N LOGGING
Crea una cadena llamada LOGGING para manejar registros.

Redirigir tráfico a la cadena de logging:

bash
Copiar
iptables -A INPUT -j LOGGING
Envía todo el tráfico de entrada a la cadena LOGGING.

Registrar con límite en la cadena LOGGING:

bash
Copiar
iptables -A LOGGING -m limit --limit 2/min -j LOG --log-prefix "IPTables Packet Dropped: "
Limita la cantidad de mensajes de log por minuto en la cadena LOGGING.

Descartar paquetes después de loguearlos:

bash
Copiar
iptables -A LOGGING -j DROP
Después de registrar, los paquetes se descartan para evitar inundar el sistema.

Configurar seguimiento de conexiones SSH (estableciendo un “recent”):

bash
Copiar
iptables -I INPUT 1 -p tcp --dport 22 -m state --state NEW -m recent --set
Marca nuevas conexiones SSH para monitorear intentos de acceso.

Bloquear múltiples intentos de conexión SSH:

bash
Copiar
iptables -I INPUT 2 -p tcp --dport 22 -m state --state NEW -m recent --update --seconds 60 --hitcount 4 -j DROP
Si hay más de 3 intentos en 60 segundos, la conexión se bloquea temporalmente.

Permitir reenvío de tráfico HTTP a red interna:

bash
Copiar
iptables -A FORWARD -p tcp -d 192.168.1.0/24 --dport 80 -j ACCEPT
Facilita el reenvío de tráfico web hacia una red interna específica.

Monitorear nuevas conexiones SMTP:

bash
Copiar
iptables -A INPUT -p tcp --dport 25 -m state --state NEW -m recent --set
Permite registrar intentos de conexión al puerto SMTP para detectar abusos.

Limitar la tasa de conexiones nuevas al puerto SMTP:

bash
Copiar
iptables -A INPUT -p tcp --dport 25 -m state --state NEW -m recent --update --seconds 60 --hitcount 3 -j DROP
Previene ataques o abusos limitando el número de conexiones SMTP.

Limitar conexiones simultáneas en HTTPS:

bash
Copiar
iptables -A INPUT -p tcp --dport 443 -m connlimit --connlimit-above 50 -j DROP
Restringe el número máximo de conexiones simultáneas al puerto 443 para evitar sobrecarga.

Guardar la configuración actual:

bash
Copiar
iptables-save > /etc/iptables/rules.v4
Exporta y guarda todas las reglas actuales para que persistan tras un reinicio.

