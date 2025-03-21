# 30 Comandos Linux Revisar para COBOL

Estos comandos complementan el proceso de evaluación, conversión, adaptación, automatización y optimización, permitiéndote analizar de forma profesional aspectos críticos durante la migración de aplicaciones COBOL legadas con herramientas Micro Focus.

### Evaluación y Análisis de la Aplicación
1. **ctags -R .**  
   - Genera un índice de símbolos para todo el código COBOL, facilitando la navegación y el análisis.
   - Ejemplo:  
     ```bash
     ctags -R .
     ```
2. **cscope -Rbq .**  
   - Construye una base de datos de código fuente para búsquedas interactivas y análisis de dependencias.
   - Ejemplo:  
     ```bash
     cscope -Rbq .
     ```
3. **find . -type f -name "*.cpy"**  
   - Lista todos los copybooks (archivos de inclusión) para revisar dependencias y estructuras compartidas.
   - Ejemplo:  
     ```bash
     find . -type f -name "*.cpy"
     ```
4. **grep -Ri "VSAM" .**  
   - Busca de forma recursiva menciones a VSAM para identificar partes del código que aún usan formatos legados.
   - Ejemplo:  
     ```bash
     grep -Ri "VSAM" .
     ```
5. **awk '{if(NF<5) print FILENAME,NR,$0}' *.cob**  
   - Revisa archivos COBOL para detectar líneas con muy pocos campos, lo que podría indicar problemas de formato o código incompleto.
   - Ejemplo:  
     ```bash
     awk '{if(NF<5) print FILENAME,NR,$0}' *.cob
     ```

### Uso de Herramientas de Conversión y Modernización
6. **dos2unix -c mac *.cob**  
   - Convierte archivos COBOL con terminadores de línea estilo Mac a formato Unix.
   - Ejemplo:  
     ```bash
     dos2unix -c mac *.cob
     ```
7. **enca -L es *.cob**  
   - Detecta y muestra la codificación de archivos, útil para confirmar si están en EBCDIC, ASCII, o UTF-8.
   - Ejemplo:  
     ```bash
     enca -L es *.cob
     ```
8. **file -b --mime-encoding *.cob**  
   - Muestra la codificación MIME de los archivos COBOL para asegurarse de que sean compatibles con el entorno moderno.
   - Ejemplo:  
     ```bash
     file -b --mime-encoding *.cob
     ```
9. **md5deep -r .**  
   - Calcula sumas MD5 recursivamente para verificar la integridad del código fuente migrado.
   - Ejemplo:  
     ```bash
     md5deep -r .
     ```
10. **fdupes -r .**  
    - Busca archivos duplicados en el repositorio, lo que puede ayudar a identificar redundancias o errores de migración.
    - Ejemplo:  
      ```bash
      fdupes -r .
      ```
11. **inotifywait -m .**  
    - Monitorea en tiempo real cambios en el directorio de código, para detectar modificaciones durante la conversión.
    - Ejemplo:  
      ```bash
      inotifywait -m .
      ```

### Adaptación de Datos y Entornos de Ejecución
12. **grep -R "COMP-5" .**  
    - Revisa el uso de variables compiladas en formato COMP-5, clave para evaluar la migración de datos binarios.
    - Ejemplo:  
      ```bash
      grep -R "COMP-5" .
      ```
13. **find . -type f -exec grep -H "TRUNC" {} \;**  
    - Busca menciones al manejo de truncamiento en variables, lo que puede afectar la integridad de datos.
    - Ejemplo:  
      ```bash
      find . -type f -exec grep -H "TRUNC" {} \;
      ```
14. **ldd ./cobol_app**  
    - Lista las dependencias de bibliotecas compartidas del ejecutable COBOL, asegurando que todas estén presentes.
    - Ejemplo:  
      ```bash
      ldd ./cobol_app
      ```
15. **nm -D ./cobol_app | grep main**  
    - Muestra los símbolos dinámicos del ejecutable para confirmar que se ha definido la función principal correctamente.
    - Ejemplo:  
      ```bash
      nm -D ./cobol_app | grep main
      ```
16. **objdump -d ./cobol_app | less**  
    - Desensambla el ejecutable para examinar la generación de código y detectar posibles anomalías.
    - Ejemplo:  
      ```bash
      objdump -d ./cobol_app | less
      ```
17. **perf stat ./cobol_app**  
    - Ejecuta la aplicación COBOL y muestra métricas de rendimiento para evaluar la optimización.
    - Ejemplo:  
      ```bash
      perf stat ./cobol_app
      ```

### Automatización y Pruebas
18. **crontab -l**  
    - Lista las tareas programadas, verificando la automatización de compilaciones y pruebas.
    - Ejemplo:  
      ```bash
      crontab -l
      ```
19. **make -n**  
    - Realiza un "dry-run" de la compilación para detectar errores de compilación sin ejecutar cambios.
    - Ejemplo:  
      ```bash
      make -n
      ```
20. **cmake --build . --target test -- -j4**  
    - Ejecuta las pruebas unitarias/integración en paralelo, garantizando que la lógica de negocio se mantenga intacta.
    - Ejemplo:  
      ```bash
      cmake --build . --target test -- -j4
      ```
21. **find . -name "*.log" -exec tail -n 20 {} \;**  
    - Revisa los últimos 20 renglones de cada archivo de log para detectar errores durante la automatización.
    - Ejemplo:  
      ```bash
      find . -name "*.log" -exec tail -n 20 {} \;
      ```
22. **awk '{print NR, $0}' /var/log/cobol_conversion.log | less**  
    - Numera y muestra el contenido de los logs de conversión para facilitar su análisis.
    - Ejemplo:  
      ```bash
      awk '{print NR, $0}' /var/log/cobol_conversion.log | less
      ```

### Optimización y Monitoreo
23. **systemctl list-units --type=service | grep microfocus**  
    - Lista los servicios relacionados con Micro Focus para confirmar que se están ejecutando correctamente.
    - Ejemplo:  
      ```bash
      systemctl list-units --type=service | grep microfocus
      ```
24. **journalctl -u microfocus-cobol.service --since "1 hour ago"**  
    - Consulta el log del servicio Micro Focus COBOL para detectar problemas recientes.
    - Ejemplo:  
      ```bash
      journalctl -u microfocus-cobol.service --since "1 hour ago"
      ```
25. **watch -n 5 'ls -l /opt/cobol/logs'**  
    - Monitorea cada 5 segundos los cambios en el directorio de logs para detectar picos o errores.
    - Ejemplo:  
      ```bash
      watch -n 5 'ls -l /opt/cobol/logs'
      ```
26. **inotifywait -m -r /opt/cobol/bin**  
    - Vigila en tiempo real todos los cambios en el directorio de ejecutables de COBOL.
    - Ejemplo:  
      ```bash
      inotifywait -m -r /opt/cobol/bin
      ```
27. **diff -rq original_dir/ migrated_dir/**  
    - Compara recursivamente dos directorios (por ejemplo, antes y después de la migración) para asegurar que todos los archivos se han transferido correctamente.
    - Ejemplo:  
      ```bash
      diff -rq original_dir/ migrated_dir/
      ```
28. **rsync -av --dry-run /data/cobol/ /backup/cobol/**  
    - Realiza una simulación de sincronización para validar que los datos se copian correctamente sin modificar nada.
    - Ejemplo:  
      ```bash
      rsync -av --dry-run /data/cobol/ /backup/cobol/
      ```
29. **iperf3 -c localhost -t 10**  
    - Ejecuta una prueba de rendimiento de red para asegurar que la comunicación entre componentes (por ejemplo, con bases de datos o servicios web) es óptima.
    - Ejemplo:  
      ```bash
      iperf3 -c localhost -t 10
      ```
30. **watch -n 2 'grep -R "error" /opt/cobol/logs'**  
    - Monitorea cada 2 segundos los logs para detectar rápidamente la aparición de nuevos errores.
    - Ejemplo:  
      ```bash
      watch -n 2 'grep -R "error" /opt/cobol/logs'
      ```
