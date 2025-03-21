### 30 Configuraciones Avanzadas de Docker

1. **Configuración de Remapeo de Namespace de Usuario**  
   - **Explicación:** Activa el remapeo de usuarios para aislar los contenedores y limitar el acceso al host, asignando un rango de UID/GID no privilegiado.  
   - **Ejemplo:**  
     ```json
     {
       "userns-remap": "default"
     }
     ```

2. **Daemon con Log Driver Personalizado y Opciones de Log**  
   - **Explicación:** Configura el daemon para usar un log-driver centralizado (por ejemplo, json-file o syslog) con opciones de tamaño y rotación para mejorar la gestión de logs.  
   - **Ejemplo:**  
     ```json
     {
       "log-driver": "json-file",
       "log-opts": {
         "max-size": "50m",
         "max-file": "5"
       }
     }
     ```

3. **Habilitar Live-Restore en el Daemon**  
   - **Explicación:** Permite que los contenedores continúen ejecutándose incluso si el daemon se reinicia, reduciendo el downtime en entornos críticos.  
   - **Ejemplo:**  
     ```json
     {
       "live-restore": true
     }
     ```

4. **Configuración de Seguridad con Seccomp Personalizado**  
   - **Explicación:** Aplica un perfil de seccomp personalizado para restringir llamadas al sistema no necesarias, aumentando la seguridad de los contenedores.  
   - **Ejemplo:**  
     ```json
     {
       "seccomp-profile": "/etc/docker/seccomp.json"
     }
     ```

5. **Habilitar Experimental Features**  
   - **Explicación:** Activa características experimentales para poder utilizar funcionalidades avanzadas y nuevas integraciones de Docker.  
   - **Ejemplo:**  
     ```json
     {
       "experimental": true
     }
     ```

6. **Configuración de Storage Driver Avanzado (Overlay2)**  
   - **Explicación:** Optimiza el rendimiento y la gestión del espacio configurando el driver de almacenamiento overlay2 con parámetros adicionales.  
   - **Ejemplo:**  
     ```json
     {
       "storage-driver": "overlay2",
       "storage-opts": [
         "overlay2.override_kernel_check=true"
       ]
     }
     ```

7. **Optimización de Cgroups para Límites de Recursos Globales**  
   - **Explicación:** Define límites de memoria y CPU a nivel de daemon para prevenir que contenedores consuman recursos excesivos.  
   - **Ejemplo:**  
     ```json
     {
       "default-runtime": "runc",
       "runtimes": {
         "runc": {
           "path": "runc"
         }
       },
       "cpu-period": 100000,
       "cpu-quota": 50000
     }
     ```

8. **Configuración de Registry Mirrors Privados**  
   - **Explicación:** Configura espejos privados para acelerar la descarga de imágenes y reducir la dependencia de Docker Hub.  
   - **Ejemplo:**  
     ```json
     {
       "registry-mirrors": ["https://mirror.mycompany.com"]
     }
     ```

9. **Habilitar la Autenticación y Autorización para el Daemon**  
   - **Explicación:** Configura TLS para el daemon, asegurando que solo clientes autenticados puedan interactuar con Docker.  
   - **Ejemplo:**  
     ```json
     {
       "tls": true,
       "tlsverify": true,
       "tlscacert": "/etc/docker/certs/ca.pem",
       "tlscert": "/etc/docker/certs/server-cert.pem",
       "tlskey": "/etc/docker/certs/server-key.pem"
     }
     ```

10. **Configuración de Plugin de Redes Avanzadas**  
    - **Explicación:** Activa y configura plugins de red (como Weave o Calico) para mejorar la conectividad y la seguridad de redes en contenedores.  
    - **Ejemplo:**  
      ```json
      {
        "cluster-store": "etcd://192.168.1.100:2379",
        "cluster-advertise": "eth0:2376"
      }
      ```

11. **Ajuste de Parámetros de Timeouts para Conexiones de API**  
    - **Explicación:** Optimiza la respuesta del daemon estableciendo timeouts adecuados para conexiones REST y operaciones de API.  
    - **Ejemplo:**  
      ```json
      {
        "api-cors-header": "https://mycompany.com",
        "shutdown-timeout": 30
      }
      ```

12. **Configuración de Recursos por Contenedor Predeterminados**  
    - **Explicación:** Define límites predeterminados de CPU, memoria y PIDs para contenedores nuevos, mejorando la estabilidad del host.  
    - **Ejemplo:**  
      ```json
      {
        "default-ulimits": {
          "nofile": {
            "Name": "nofile",
            "Hard": 64000,
            "Soft": 64000
          }
        }
      }
      ```

13. **Habilitar el Uso de Overlay Networks en Swarm Mode**  
    - **Explicación:** Configura parámetros avanzados para redes overlay en clusters Docker Swarm, mejorando la resiliencia y el aislamiento.  
    - **Ejemplo:**  
      ```json
      {
        "swarm-default-advertise-addr": "192.168.1.10",
        "cluster-store-opts": {
          "kv.cacertfile": "/etc/docker/certs/ca.pem"
        }
      }
      ```

14. **Configuración de Garbage Collection de Imágenes y Volúmenes**  
    - **Explicación:** Establece políticas de recolección de basura para liberar espacio de imágenes y volúmenes no utilizados en producción.  
    - **Ejemplo:**  
      ```json
      {
        "experimental": true,
        "features": { "buildkit": true },
        "image-gc": {
          "enabled": true,
          "interval": "30m",
          "min-usage": "5GB"
        }
      }
      ```

15. **Configuración de Swarm Mode con Etiquetas y Reglas de Colocación**  
    - **Explicación:** Define reglas de colocación basadas en etiquetas de nodos para desplegar servicios críticos en hardware específico.  
    - **Ejemplo:**  
      ```json
      {
        "task-history-limit": 5,
        "default-address-pools": [
          {"base": "10.10.0.0/16", "size": 24}
        ]
      }
      ```

16. **Configuración de Logging Centralizado para Swarm**  
    - **Explicación:** Configura el envío de logs de servicios en Swarm a un sistema centralizado (por ejemplo, ELK o Fluentd).  
    - **Ejemplo:**  
      ```json
      {
        "log-driver": "fluentd",
        "log-opts": {
          "fluentd-address": "192.168.1.150:24224",
          "tag": "{{.Name}}/{{.ID}}"
        }
      }
      ```

17. **Configuración de Healthcheck Global para Contenedores**  
    - **Explicación:** Define parámetros de salud globales que se apliquen por defecto a todos los contenedores para facilitar la monitorización.  
    - **Ejemplo:**  
      ```json
      {
        "healthcheck": {
          "interval": "30s",
          "timeout": "5s",
          "retries": 3,
          "start_period": "10s"
        }
      }
      ```

18. **Configuración de Redes Multi-Tenant con MACVLAN**  
    - **Explicación:** Implementa redes MACVLAN para aislar contenedores en entornos multi-tenant, permitiendo direcciones MAC únicas.  
    - **Ejemplo:**  
      ```bash
      docker network create -d macvlan --subnet=192.168.100.0/24 --gateway=192.168.100.1 -o parent=eth0 my_macvlan_net
      ```

19. **Configuración de Resource Quotas en Docker Swarm**  
    - **Explicación:** Establece límites de CPU y memoria para servicios en un cluster Swarm para evitar la sobrecarga de nodos.  
    - **Ejemplo:**  
      ```yaml
      version: '3.7'
      services:
        web:
          image: mycompany/webapp:latest
          deploy:
            resources:
              limits:
                cpus: '0.50'
                memory: 512M
      ```

20. **Configuración de Volúmenes con Control de Acceso y Cifrado**  
    - **Explicación:** Define volúmenes con opciones de control de acceso y, en sistemas compatibles, habilita el cifrado en disco para datos sensibles.  
    - **Ejemplo:**  
      ```json
      {
        "data-root": "/var/lib/docker",
        "storage-opts": [
          "dm.no_warn_on_loop_devices=true"
        ]
      }
      ```

21. **Configuración de Plugins de Autenticación para Registro Privado**  
    - **Explicación:** Integra plugins de autenticación para el acceso seguro a registros privados, forzando el uso de certificados o tokens.  
    - **Ejemplo:**  
      ```json
      {
        "registry-mirrors": ["https://registry.mycompany.com"],
        "insecure-registries": [],
        "disable-legacy-registry": true
      }
      ```

22. **Configuración de Actualización Automática de Imágenes en Swarm**  
    - **Explicación:** Define políticas para actualizar automáticamente servicios en Swarm al detectar nuevas versiones de imágenes.  
    - **Ejemplo:**  
      ```yaml
      version: '3.7'
      services:
        api:
          image: mycompany/api:latest
          deploy:
            update_config:
              parallelism: 2
              delay: 10s
              failure_action: rollback
      ```

23. **Configuración de Pruning Automático en el Daemon**  
    - **Explicación:** Programa la eliminación automática de contenedores, imágenes y volúmenes no utilizados para mantener limpio el entorno.  
    - **Ejemplo:**  
      ```bash
      # Programar en cron: docker system prune -af --volumes
      ```

24. **Configuración de Perfiles de Seguridad AppArmor/SELinux Personalizados**  
    - **Explicación:** Aplica perfiles de AppArmor o SELinux personalizados para contenedores, restringiendo permisos de manera granular.  
    - **Ejemplo:**  
      ```bash
      # En el contenedor: docker run --security-opt "apparmor=custom_profile" my_image
      ```

25. **Configuración de Redes Virtuales con IPv6**  
    - **Explicación:** Habilita y configura el soporte IPv6 en el daemon para redes dual stack en entornos modernos.  
    - **Ejemplo:**  
      ```json
      {
        "ipv6": true,
        "fixed-cidr-v6": "2001:db8:1::/64"
      }
      ```

26. **Configuración de Gestión de Certificados Automática**  
    - **Explicación:** Integra herramientas de renovación automática de certificados para la comunicación segura entre nodos y agentes Docker.  
    - **Ejemplo:**  
      ```json
      {
        "tls": true,
        "tlscacert": "/etc/docker/certs/ca.pem",
        "tlscert": "/etc/docker/certs/server-cert.pem",
        "tlskey": "/etc/docker/certs/server-key.pem",
        "tlsverify": true
      }
      ```

27. **Configuración de Etiquetado y Metadatos para Contenedores**  
    - **Explicación:** Establece reglas para inyectar etiquetas y metadatos en contenedores, facilitando la auditoría y la orquestación.  
    - **Ejemplo:**  
      ```json
      {
        "labels": ["env=production", "team=devops"]
      }
      ```

28. **Configuración de Sandbox de Red para Aislamiento de Contenedores**  
    - **Explicación:** Utiliza opciones avanzadas de red para crear entornos aislados con sandboxes que eviten interferencias entre contenedores.  
    - **Ejemplo:**  
      ```bash
      docker network create --driver bridge --internal sandbox_net
      ```

29. **Configuración de Recursos de Almacenamiento con I/O Throttling**  
    - **Explicación:** Limita la velocidad de I/O de contenedores críticos para evitar que saturen el disco, utilizando parámetros de block IO.  
    - **Ejemplo:**  
      ```bash
      docker run --device-read-bps /dev/sda:1mb --device-write-bps /dev/sda:1mb my_image
      ```

30. **Configuración de Orquestación Avanzada en Docker Compose**  
    - **Explicación:** Define archivos Compose con dependencias, estrategias de reinicio y recursos reservados para servicios críticos.  
    - **Ejemplo:**  
      ```yaml
      version: '3.8'
      services:
        web:
          image: mycompany/webapp:latest
          deploy:
            restart_policy:
              condition: on-failure
            resources:
              limits:
                cpus: '1.0'
                memory: 1G
          depends_on:
            - db
        db:
          image: mysql:8
          deploy:
            resources:
              reservations:
                memory: 512M
      ```

---

### 30 Comandos Avanzados de Docker en Entornos de Producción

1. **Crear Servicio en Swarm con Actualización Continua**  
   - **Explicación:** Despliega un servicio en Docker Swarm con políticas de actualización definidas para minimizar el downtime.  
   - **Ejemplo:**  
     ```bash
     docker service create \
       --name webapp \
       --update-delay 10s \
       --update-parallelism 2 \
       --limit-cpu 1.0 \
       --limit-memory 1G \
       mycompany/webapp:latest
     ```

2. **Ejecutar Contenedor con Capabilities Restringidas**  
   - **Explicación:** Inicia un contenedor limitando capacidades del kernel para aumentar la seguridad.  
   - **Ejemplo:**  
     ```bash
     docker run -d --name secure_app \
       --cap-drop=ALL \
       --cap-add=NET_BIND_SERVICE \
       mycompany/secureapp:latest
     ```

3. **Iniciar Contenedor con Perfil Seccomp Personalizado**  
   - **Explicación:** Utiliza un perfil seccomp personalizado para limitar llamadas al sistema no necesarias.  
   - **Ejemplo:**  
     ```bash
     docker run -d --name app_seccomp \
       --security-opt seccomp=/etc/docker/seccomp.json \
       mycompany/app:latest
     ```

4. **Ejecutar Contenedor con Remapeo de Usuario**  
   - **Explicación:** Lanza un contenedor que use remapeo de usuario para mayor aislamiento del host.  
   - **Ejemplo:**  
     ```bash
     docker run -d --name userns_app \
       --userns=host \
       mycompany/app:latest
     ```

5. **Desplegar Servicio en Swarm con Montaje de Volúmenes en NFS**  
   - **Explicación:** Crea un servicio que monte volúmenes NFS para almacenamiento compartido entre réplicas.  
   - **Ejemplo:**  
     ```bash
     docker service create \
       --name nfs_service \
       --mount type=volume,source=nfs_vol,target=/data,volume-driver=local,volume-opt=type=nfs,volume-opt=o=addr=192.168.1.50,rw,volume-opt=device=:/exported/path \
       mycompany/app:latest
     ```

6. **Ejecutar Contenedor en Modo Privilegiado con Dispositivos Específicos**  
   - **Explicación:** Inicia un contenedor en modo privilegiado para acceder a dispositivos del host, usado en entornos de IoT o diagnósticos avanzados.  
   - **Ejemplo:**  
     ```bash
     docker run -d --name privileged_app \
       --privileged \
       --device=/dev/snd \
       mycompany/audioapp:latest
     ```

7. **Inspeccionar Configuración Completa de un Servicio en Swarm**  
   - **Explicación:** Muestra la configuración detallada y el estado de un servicio desplegado en Swarm.  
   - **Ejemplo:**  
     ```bash
     docker service inspect --pretty webapp
     ```

8. **Monitorizar Logs en Tiempo Real con Filtros Avanzados**  
   - **Explicación:** Visualiza los logs de un contenedor aplicando filtros y formatos específicos para diagnóstico en producción.  
   - **Ejemplo:**  
     ```bash
     docker logs -f --tail 100 secure_app | grep "ERROR"
     ```

9. **Ejecutar un Contenedor en Modo Interactivo para Debug**  
   - **Explicación:** Inicia un contenedor con un shell interactivo para depurar problemas en tiempo real.  
   - **Ejemplo:**  
     ```bash
     docker run --rm -it --entrypoint /bin/bash mycompany/app:latest
     ```

10. **Utilizar Docker Exec con Sesión TTY para Diagnóstico**  
    - **Explicación:** Conecta a un contenedor en ejecución con una sesión TTY para inspección y corrección de incidencias.  
    - **Ejemplo:**  
      ```bash
      docker exec -it secure_app /bin/sh
      ```

11. **Extraer Variables de Entorno y Configuración de un Contenedor**  
    - **Explicación:** Inspecciona un contenedor para revisar las variables de entorno y configuraciones establecidas.  
    - **Ejemplo:**  
      ```bash
      docker inspect -f '{{json .Config.Env}}' webapp
      ```

12. **Actualizar un Servicio en Swarm sin Downtime**  
    - **Explicación:** Realiza una actualización en vivo de un servicio, escalando gradualmente y sin interrumpir el servicio.  
    - **Ejemplo:**  
      ```bash
      docker service update --image mycompany/webapp:newtag --update-parallelism 2 --update-delay 10s webapp
      ```

13. **Eliminar Imágenes Huérfanas y Optimizar Almacenamiento**  
    - **Explicación:** Ejecuta la limpieza de imágenes y volúmenes no utilizados para liberar espacio en el host.  
    - **Ejemplo:**  
      ```bash
      docker system prune -af --volumes
      ```

14. **Inspeccionar Información de Recursos y Limitaciones de un Contenedor**  
    - **Explicación:** Revisa el uso de CPU, memoria y otras métricas de un contenedor para análisis de rendimiento.  
    - **Ejemplo:**  
      ```bash
      docker stats secure_app --no-stream
      ```

15. **Exportar la Configuración de un Contenedor para Migración**  
    - **Explicación:** Guarda la configuración de un contenedor en un archivo para replicarla en otro host o entorno.  
    - **Ejemplo:**  
      ```bash
      docker export secure_app > /backup/secure_app_$(date +%F).tar
      ```

16. **Importar una Imagen desde un Archivo Exportado**  
    - **Explicación:** Restaura un contenedor exportado como imagen para su despliegue en un nuevo entorno.  
    - **Ejemplo:**  
      ```bash
      docker import /backup/secure_app_2025-03-20.tar mycompany/secure_app:restored
      ```

17. **Utilizar Docker Diff para Auditar Cambios en un Contenedor**  
    - **Explicación:** Muestra los cambios realizados en el sistema de archivos de un contenedor en ejecución.  
    - **Ejemplo:**  
      ```bash
      docker diff secure_app
      ```

18. **Crear y Administrar Redes Personalizadas Avanzadas**  
    - **Explicación:** Configura una red personalizada con parámetros de segmentación y aislamiento avanzados para servicios críticos.  
    - **Ejemplo:**  
      ```bash
      docker network create --driver overlay --subnet 10.0.9.0/24 secure_net
      ```

19. **Listar Volúmenes con Detalles y Usos**  
    - **Explicación:** Muestra una lista detallada de volúmenes, facilitando la auditoría y el mantenimiento de datos persistentes.  
    - **Ejemplo:**  
      ```bash
      docker volume ls -q | xargs docker volume inspect
      ```

20. **Montar un Volumen con Opciones Avanzadas de Lectura/Escritura**  
    - **Explicación:** Inicia un contenedor con volúmenes montados y opciones específicas para optimizar I/O y seguridad.  
    - **Ejemplo:**  
      ```bash
      docker run -d --name db_app \
        -v db_data:/var/lib/mysql:rw \
        mycompany/mysql:8
      ```

21. **Ejecutar Pruebas de Rendimiento con Docker Bench**  
    - **Explicación:** Utiliza la herramienta Docker Bench for Security para auditar la configuración de seguridad y rendimiento.  
    - **Ejemplo:**  
      ```bash
      docker run -it --net host --pid host --cap-add audit_control \
        --label docker_bench_security \
        docker/docker-bench-security
      ```

22. **Ejecutar Contenedor con Políticas de Reinicio Avanzadas**  
    - **Explicación:** Define políticas de reinicio personalizadas para servicios críticos en entornos de alta disponibilidad.  
    - **Ejemplo:**  
      ```bash
      docker run -d --restart on-failure:5 --name resilient_app mycompany/app:latest
      ```

23. **Correlacionar Logs de Contenedores Usando Docker Events**  
    - **Explicación:** Escucha y filtra eventos del daemon Docker para correlacionar incidencias en producción.  
    - **Ejemplo:**  
      ```bash
      docker events --filter 'event=die' --filter 'container=webapp'
      ```

24. **Inspeccionar la Configuración de una Imagen Base para Optimización**  
    - **Explicación:** Examina la configuración interna de una imagen para identificar oportunidades de optimización.  
    - **Ejemplo:**  
      ```bash
      docker history --no-trunc mycompany/app:latest
      ```

25. **Utilizar Docker Compose para Despliegues Multi-Servicio con Variables de Entorno**  
    - **Explicación:** Despliega un stack completo definiendo variables de entorno seguras y opciones avanzadas de orquestación.  
    - **Ejemplo:**  
      ```bash
      docker-compose -f docker-compose.prod.yml up -d
      ```

26. **Ejecutar Comando de Inspección con Formato Personalizado**  
    - **Explicación:** Extrae información específica de un contenedor usando formatos de salida personalizados para auditorías.  
    - **Ejemplo:**  
      ```bash
      docker inspect -f '{{.State.Status}} - {{.HostConfig.Memory}}' secure_app
      ```

27. **Generar un Volcado Completo de la Configuración del Daemon**  
    - **Explicación:** Exporta la configuración actual del daemon para revisarla o migrarla a otro entorno.  
    - **Ejemplo:**  
      ```bash
      cat /etc/docker/daemon.json
      ```

28. **Aplicar Etiquetado Masivo a Contenedores en Ejecución**  
    - **Explicación:** Actualiza las etiquetas de múltiples contenedores para una mejor organización y búsqueda en entornos dinámicos.  
    - **Ejemplo:**  
      ```bash
      docker ps -q | xargs -I {} docker container update --label-add env=production {}
      ```

29. **Revisar Estadísticas Avanzadas de Red de un Contenedor**  
    - **Explicación:** Consulta estadísticas detalladas de red (bytes, paquetes, errores) para un contenedor específico.  
    - **Ejemplo:**  
      ```bash
      docker stats secure_app --no-stream --format "table {{.Name}}\t{{.NetIO}}"
      ```

30. **Eliminar Servicios Obsoletos en un Cluster Swarm**  
    - **Explicación:** Limpia servicios antiguos o inactivos en el clúster para optimizar recursos y mantener el entorno organizado.  
    - **Ejemplo:**  
      ```bash
      docker service ls --filter "desired-state=shutdown" -q | xargs docker service rm
      ```
