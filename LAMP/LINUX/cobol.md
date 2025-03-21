# Comandos Avanzados de Linux para Diagnosticar Errores en la Migración de COBOL a Linux

Basándonos en la información disponible sobre migraciones de COBOL (especialmente en la conversión de codificación, manejo de archivos y diferencias en el entorno) y en los errores reportados en foros y documentación, a continuación se listan 50 comandos útiles para detectar, diagnosticar y solucionar problemas durante la migración:

1. **locale**  
   - **Descripción:** Muestra la configuración regional actual, importante para detectar problemas de codificación (ASCII vs. EBCDIC, UTF-8, etc.).  
   - **Ejemplo:**  
     ```bash
     locale
     ```

2. **file**  
   - **Descripción:** Identifica el tipo y codificación de un archivo; útil para confirmar si un archivo COBOL o de datos está en EBCDIC o ASCII.  
   - **Ejemplo:**  
     ```bash
     file my_cobol_source.cob
     ```

3. **iconv -l**  
   - **Descripción:** Lista las codificaciones disponibles en el sistema, para confirmar la presencia de EBCDIC o UTF-8.  
   - **Ejemplo:**  
     ```bash
     iconv -l | grep -i ebcdic
     ```

4. **iconv**  
   - **Descripción:** Convierte archivos entre diferentes codificaciones; esencial para transformar datos de EBCDIC a ASCII.  
   - **Ejemplo:**  
     ```bash
     iconv -f EBCDIC-US -t UTF-8 input.ebcdic > output.txt
     ```

5. **dos2unix**  
   - **Descripción:** Convierte archivos de formato DOS/Windows a Unix, eliminando caracteres de retorno de carro que pueden alterar el formato COBOL.  
   - **Ejemplo:**  
     ```bash
     dos2unix script.cob
     ```

6. **sed 's/\r//g'**  
   - **Descripción:** Elimina caracteres de retorno de carro (\r) en un archivo, útil para limpiar entradas con formato mixto.  
   - **Ejemplo:**  
     ```bash
     sed 's/\r//g' input_win.txt > input_unix.txt
     ```

7. **grep -E "ERROR|WARN|IGY"**  
   - **Descripción:** Busca patrones de error (como "ERROR", "WARN" o mensajes específicos de COBOL como "IGY") en archivos de log o código fuente.  
   - **Ejemplo:**  
     ```bash
     grep -E "ERROR|WARN|IGY" /var/log/cobol_migration.log
     ```

8. **tail -n 100**  
   - **Descripción:** Muestra las últimas 100 líneas de un archivo, para revisar los errores más recientes en logs de migración.  
   - **Ejemplo:**  
     ```bash
     tail -n 100 /var/log/cobol_migration.log
     ```

9. **head -n 50**  
   - **Descripción:** Visualiza las primeras 50 líneas de un archivo; útil para verificar encabezados o directivas (por ejemplo, "IDENTIFICATION DIVISION.").  
   - **Ejemplo:**  
     ```bash
     head -n 50 my_cobol_source.cob
     ```

10. **diff**  
    - **Descripción:** Compara dos archivos y resalta las diferencias, ayudando a identificar cambios no deseados en la conversión.  
    - **Ejemplo:**  
      ```bash
      diff original.cob migrated.cob
      ```

11. **cmp -l**  
    - **Descripción:** Compara dos archivos byte a byte para detectar discrepancias en archivos críticos.  
    - **Ejemplo:**  
      ```bash
      cmp -l original.cob migrated.cob
      ```

12. **strace -f -e trace=read,write**  
    - **Descripción:** Rastrear llamadas al sistema de una aplicación COBOL en ejecución para identificar fallos en I/O o acceso a archivos.  
    - **Ejemplo:**  
      ```bash
      strace -f -e trace=read,write -o cobol_strace.log ./cobol_app
      ```

13. **ltrace -f**  
    - **Descripción:** Rastrea las llamadas a funciones de bibliotecas, útil para identificar problemas con dependencias o funciones específicas.  
    - **Ejemplo:**  
      ```bash
      ltrace -f ./cobol_app
      ```

14. **gdb**  
    - **Descripción:** Depurador que permite examinar el estado interno de la aplicación COBOL para encontrar fallos en tiempo de ejecución.  
    - **Ejemplo:**  
      ```bash
      gdb ./cobol_app
      ```

15. **valgrind --leak-check=full**  
    - **Descripción:** Detecta fugas de memoria y errores en el manejo de recursos durante la ejecución de la aplicación.  
    - **Ejemplo:**  
      ```bash
      valgrind --leak-check=full ./cobol_app
      ```

16. **ps aux | grep cobol**  
    - **Descripción:** Lista los procesos relacionados con COBOL para verificar si la aplicación se está ejecutando correctamente.  
    - **Ejemplo:**  
      ```bash
      ps aux | grep cobol
      ```

17. **top**  
    - **Descripción:** Monitorea en tiempo real el uso de CPU y memoria, permitiendo detectar cuellos de botella o sobrecargas durante la migración.  
    - **Ejemplo:**  
      ```bash
      top
      ```

18. **htop**  
    - **Descripción:** Versión interactiva y colorida de `top` para un monitoreo más detallado.  
    - **Ejemplo:**  
      ```bash
      htop
      ```

19. **free -m**  
    - **Descripción:** Muestra el uso de memoria en megabytes, para evaluar recursos del sistema durante pruebas de rendimiento.  
    - **Ejemplo:**  
      ```bash
      free -m
      ```

20. **df -h**  
    - **Descripción:** Verifica el espacio disponible en disco, ayudando a detectar posibles errores por falta de espacio durante la migración o ejecución.  
    - **Ejemplo:**  
      ```bash
      df -h
      ```

21. **du -sh**  
    - **Descripción:** Calcula el uso de espacio de archivos o directorios, identificando posibles acumulaciones de datos o logs anómalos.  
    - **Ejemplo:**  
      ```bash
      du -sh /var/log/cobol_logs/*
      ```

22. **ls -l**  
    - **Descripción:** Lista archivos con detalles de permisos y propietarios; importante para problemas de acceso y ejecución.  
    - **Ejemplo:**  
      ```bash
      ls -l /opt/cobol/bin/
      ```

23. **stat**  
    - **Descripción:** Muestra información detallada (fecha de modificación, tamaño, permisos) de un archivo específico.  
    - **Ejemplo:**  
      ```bash
      stat cobol_app
      ```

24. **chmod**  
    - **Descripción:** Cambia los permisos de un archivo o directorio, resolviendo problemas de acceso.  
    - **Ejemplo:**  
      ```bash
      chmod 755 cobol_app
      ```

25. **chown**  
    - **Descripción:** Cambia el propietario y grupo de archivos o directorios, útil si los errores se deben a permisos.  
    - **Ejemplo:**  
      ```bash
      chown usuario:grupo cobol_app
      ```

26. **journalctl -xe**  
    - **Descripción:** Revisa los logs del sistema en sistemas con systemd, permitiendo detectar errores relacionados con la ejecución o servicios.  
    - **Ejemplo:**  
      ```bash
      journalctl -xe | grep cobol
      ```

27. **tail -f /var/log/syslog**  
    - **Descripción:** Monitorea en tiempo real el log del sistema para observar eventos y errores que ocurran durante la migración o ejecución.  
    - **Ejemplo:**  
      ```bash
      tail -f /var/log/syslog
      ```

28. **watch -n 5 'ls -l /ruta/al/directorio'**  
    - **Descripción:** Observa cambios en un directorio cada 5 segundos, útil para detectar archivos que se generan o modifican inesperadamente.  
    - **Ejemplo:**  
      ```bash
      watch -n 5 'ls -l /opt/cobol/logs'
      ```

29. **nc -zv <host> <puerto>**  
    - **Descripción:** Verifica la conectividad de red en un puerto específico, especialmente si la aplicación COBOL interactúa con servicios externos.  
    - **Ejemplo:**  
      ```bash
      nc -zv localhost 8080
      ```

30. **curl -I**  
    - **Descripción:** Consulta los encabezados HTTP de un servicio web, útil para probar la conectividad y respuesta de APIs integradas.  
    - **Ejemplo:**  
      ```bash
      curl -I http://localhost:8080
      ```

31. **netstat -tulpen**  
    - **Descripción:** Muestra conexiones de red y puertos abiertos, ayudando a identificar problemas de comunicación.  
    - **Ejemplo:**  
      ```bash
      netstat -tulpen | grep cobol
      ```

32. **ss -s**  
    - **Descripción:** Proporciona un resumen de las conexiones de red, útil para detectar cuellos de botella o conexiones inesperadas.  
    - **Ejemplo:**  
      ```bash
      ss -s
      ```

33. **lsof -i :8080**  
    - **Descripción:** Lista procesos que usan un puerto específico, ideal para resolver conflictos en servicios que usan puertos predeterminados.  
    - **Ejemplo:**  
      ```bash
      lsof -i :8080
      ```

34. **grep -R "IDENTIFICATION DIVISION" .**  
    - **Descripción:** Busca en recursión en el directorio actual para confirmar que los archivos COBOL contienen la declaración obligatoria.  
    - **Ejemplo:**  
      ```bash
      grep -R "IDENTIFICATION DIVISION" .
      ```

35. **grep -R "program-id" .**  
    - **Descripción:** Verifica que los archivos COBOL tengan la declaración de programa, fundamental para compilar correctamente.  
    - **Ejemplo:**  
      ```bash
      grep -R "program-id" .
      ```

36. **awk 'length($0) > 80' archivo.cob**  
    - **Descripción:** Identifica líneas que exceden 80 columnas, un posible problema en formatos fijos heredados.  
    - **Ejemplo:**  
      ```bash
      awk 'length($0) > 80' testdoc.cob
      ```

37. **xxd archivo.cob | less**  
    - **Descripción:** Muestra una representación hexadecimal del archivo, permitiendo detectar caracteres invisibles o no imprimibles.  
    - **Ejemplo:**  
      ```bash
      xxd testdoc.cob | less
      ```

38. **perl -ne 'print if /[^\x20-\x7E]/' archivo.cob**  
    - **Descripción:** Detecta caracteres no imprimibles en el archivo, que pueden causar errores en la compilación.  
    - **Ejemplo:**  
      ```bash
      perl -ne 'print if /[^\x20-\x7E]/' testdoc.cob
      ```

39. **tr -d '\r' < archivo.txt > archivo_clean.txt**  
    - **Descripción:** Elimina caracteres de retorno de carro para asegurar un formato Unix limpio.  
    - **Ejemplo:**  
      ```bash
      tr -d '\r' < input.txt > clean.txt
      ```

40. **export LC_ALL=C**  
    - **Descripción:** Fuerza la configuración de la localidad a “C”, lo que puede ayudar a evitar problemas de interpretación de caracteres en scripts.  
    - **Ejemplo:**  
      ```bash
      export LC_ALL=C
      ```

41. **env | grep -i COBOL**  
    - **Descripción:** Revisa las variables de entorno relacionadas con COBOL para confirmar configuraciones de compilación y ejecución.  
    - **Ejemplo:**  
      ```bash
      env | grep -i COBOL
      ```

42. **which cobc**  
    - **Descripción:** Verifica la ruta del compilador COBOL (por ejemplo, GnuCOBOL) para asegurarse de que se está usando el correcto.  
    - **Ejemplo:**  
      ```bash
      which cobc
      ```

43. **cobc --version**  
    - **Descripción:** Muestra la versión del compilador COBOL, clave para diagnosticar problemas de compatibilidad.  
    - **Ejemplo:**  
      ```bash
      cobc --version
      ```

44. **grep "^$" archivo.cob**  
    - **Descripción:** Busca líneas vacías que puedan estar afectando la estructura del código COBOL en formato fijo.  
    - **Ejemplo:**  
      ```bash
      grep "^$" testdoc.cob
      ```

45. **sed -n '1,20p' archivo.cob**  
    - **Descripción:** Muestra las primeras 20 líneas del código para revisar la correcta ubicación de divisiones y declaraciones.  
    - **Ejemplo:**  
      ```bash
      sed -n '1,20p' testdoc.cob
      ```

46. **find . -type f -name "*.cob" -exec grep -H "IDENTIFICATION DIVISION" {} \;**  
    - **Descripción:** Busca archivos COBOL válidos en el directorio actual, comprobando la presencia de la división de identificación.  
    - **Ejemplo:**  
      ```bash
      find . -type f -name "*.cob" -exec grep -H "IDENTIFICATION DIVISION" {} \;
      ```

47. **cut -d" " -f1,2 /var/log/cobol_migration.log**  
    - **Descripción:** Extrae campos específicos de logs para analizar códigos de error o mensajes clave.  
    - **Ejemplo:**  
      ```bash
      cut -d" " -f1,2 /var/log/cobol_migration.log
      ```

48. **awk '{print $NF}' /var/log/cobol_migration.log**  
    - **Descripción:** Extrae la última columna de un log, ayudando a aislar códigos o mensajes finales de error.  
    - **Ejemplo:**  
      ```bash
      awk '{print $NF}' /var/log/cobol_migration.log
      ```

49. **logrotate -d /etc/logrotate.conf**  
    - **Descripción:** Simula la rotación de logs para diagnosticar problemas relacionados con el manejo y almacenamiento de logs en el sistema.  
    - **Ejemplo:**  
      ```bash
      logrotate -d /etc/logrotate.conf
      ```

50. **watch -n 5 'date'**  
    - **Descripción:** Permite monitorear en intervalos regulares (por ejemplo, cada 5 segundos) para sincronizar la revisión de logs y eventos durante pruebas o migración.  
    - **Ejemplo:**  
      ```bash
      watch -n 5 'date'
      ```

# Notas Finales
- **Combina estos comandos en scripts:** Puedes automatizar la detección de errores integrando varios de estos comandos en scripts de Bash.  
- **Prueba en entornos de desarrollo:** Antes de aplicarlos en producción, ejecuta estos diagnósticos en entornos de pruebas para validar la migración.  
- **Documenta cada hallazgo:** Registra los errores detectados y sus condiciones para facilitar la corrección y futuras migraciones.
