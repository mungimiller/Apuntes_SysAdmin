
# Guía de Docker para SysAdmin

Esta guía cubre los fundamentos y herramientas esenciales para trabajar con Docker como un profesional del departamento de sistemas.

---

## **1. Introducción a Docker**
Docker es una plataforma de contenedores que permite empaquetar aplicaciones y sus dependencias en entornos aislados. 

### **1.1. Componentes principales:**
- **Imágenes:** Plantillas inmutables que contienen todo lo necesario para ejecutar una aplicación.
- **Contenedores:** Instancias en ejecución de imágenes.
- **Dockerfile:** Archivo para construir imágenes personalizadas.
- **Docker Engine:** Motor que gestiona imágenes, contenedores y redes.
- **Docker Hub:** Repositorio para compartir imágenes.

---

## **2. Instalación de Docker**
### **2.1. Instalación en Linux (Ubuntu/Debian):**
```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### **2.2. Verificar la instalación:**
```bash
docker --version
```

---

## **3. Comandos básicos**
### **3.1. Gestión de contenedores:**
- Listar contenedores activos:
  ```bash
  docker ps
  ```
- Listar todos los contenedores:
  ```bash
  docker ps -a
  ```
- Iniciar/Detener contenedores:
  ```bash
  docker start <container_name>
  docker stop <container_name>
  ```

### **3.2. Gestión de imágenes:**
- Listar imágenes locales:
  ```bash
  docker images
  ```
- Descargar una imagen:
  ```bash
  docker pull <image_name>
  ```
- Eliminar una imagen:
  ```bash
  docker rmi <image_name>
  ```

---

## **4. Crear un Dockerfile**
Un **Dockerfile** define el entorno para crear una imagen. Ejemplo para una app Node.js:
```Dockerfile
FROM node:14
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

Construir la imagen:
```bash
docker build -t my-node-app .
```

---

## **5. Redes en Docker**
Docker ofrece redes para la comunicación entre contenedores:
- Crear una red:
  ```bash
  docker network create my_network
  ```
- Conectar un contenedor a la red:
  ```bash
  docker network connect my_network <container_name>
  ```
- Listar redes:
  ```bash
  docker network ls
  ```

---

## **6. Docker Compose**
Herramienta para definir y ejecutar aplicaciones multicontenedor con un archivo `docker-compose.yml`.

### **Ejemplo básico:**
Archivo `docker-compose.yml` para una aplicación con Nginx y Node.js:
```yaml
version: "3"
services:
  web:
    image: nginx
    ports:
      - "80:80"
  app:
    build: .
    ports:
      - "3000:3000"
```

Iniciar la aplicación:
```bash
docker-compose up
```

---

## **7. Gestión de Volúmenes**
Los volúmenes permiten persistir datos entre reinicios de contenedores.
- Crear un volumen:
  ```bash
  docker volume create my_volume
  ```
- Montar un volumen en un contenedor:
  ```bash
  docker run -v my_volume:/data <image_name>
  ```

---

## **8. Seguridad con Docker**
- Ejecutar contenedores con usuarios no root (`USER` en Dockerfile).
- Restringir recursos (CPU/memoria):
  ```bash
  docker run --memory="512m" --cpus="1.0" <image_name>
  ```
- Escanear imágenes en busca de vulnerabilidades:
  ```bash
  docker scan <image_name>
  ```

---

## **9. Buenas prácticas**
- Mantén los Dockerfiles simples y evita instalar dependencias innecesarias.
- Usa versiones específicas de imágenes en lugar de `latest`.
- Limpia imágenes y contenedores no usados:
  ```bash
  docker system prune
  ```

---

## **10. Recursos adicionales**
- Documentación oficial: [https://docs.docker.com](https://docs.docker.com)
- Curso gratuito de Docker: [https://katacoda.com](https://katacoda.com)

---

Con esta guía estarás preparado para utilizar Docker en tareas de SysAdmin en CDmon. Si necesitas más ejemplos prácticos o ejercicios, no dudes en consultarlos.
