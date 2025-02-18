![image](https://github.com/user-attachments/assets/116d0272-e661-4bfb-8979-c1ee6ac20fca)

# **¿Cómo Funciona IPTABLES?**
**IPTABLES** es una herramienta que forma parte del framework Netfilter del kernel de Linux. Su función principal es el filtrado y manipulación de paquetes de red en tiempo real. Su arquitectura se basa en varios componentes clave:

## 1. Tablas y Cadenas
- **Tablas**: IPTABLES organiza sus reglas en diferentes tablas, cada una diseñada para tareas específicas:

  - `filter`: Es la tabla por defecto y se encarga de filtrar el tráfico. Contiene las cadenas INPUT (paquetes entrantes), OUTPUT (paquetes salientes) y FORWARD (paquetes que pasan a través del equipo).
  - `nat`: Se utiliza para la traducción de direcciones (Network Address Translation). Posee cadenas como PREROUTING (antes de que se determine el destino del paquete) y POSTROUTING (después de que se ha   determinado el destino).
  - `mangle`: Permite modificar o marcar paquetes para su procesamiento posterior. Se usa en situaciones avanzadas como la configuración de calidad de servicio (QoS).
  - `raw y security`: Son menos utilizadas, pero permiten configuraciones específicas, como excepciones a la conexión tracking o políticas de seguridad más finas.

- **Cadenas y Reglas**:
Las reglas se agrupan en cadenas dentro de cada tabla. Cada regla tiene dos partes principales:

  - **Condiciones (matches)**: Se definen utilizando módulos (como `state`, `conntrack`, `multiport`, etc.) que permiten evaluar atributos del paquete (dirección IP, puerto, protocolo, etc.).
  - **Acciones (targets)**: Una vez que se cumple la condición, se especifica qué acción realizar (por ejemplo, `ACCEPT`, `DROP`, `LOG`, `MASQUERADE`, etc.).

## 2. Flujo de Procesamiento
Cuando un paquete llega al equipo, sigue un flujo definido:

- **Entrada (INPUT):** Los paquetes dirigidos al equipo son evaluados contra la cadena INPUT.
- **Salida (OUTPUT):** Los paquetes que salen del equipo pasan por la cadena OUTPUT.
- **Reenvío (FORWARD):** Los paquetes que atraviesan el sistema (en equipos que actúan como routers) se evalúan en la cadena FORWARD.

El proceso de evaluación se realiza en orden secuencial: IPTABLES evalúa cada regla de una cadena hasta que encuentra una coincidencia. Si no se encuentra ninguna regla aplicable, se aplica la política predeterminada de la cadena (generalmente `DROP` o `ACCEPT`).


