# Proyecto_ComeCat

Este repositorio contiene el ecosistema completo del proyecto **ComeCat**, que incluye varios contenedores Docker: API, Blazor, MQTT, N8N, MySQL y Portainer.  

Se incluyen las **imágenes Docker** y los **volúmenes** en formato `.tar` y `.tar.gz` respectivamente, listos para restaurar el entorno en cualquier máquina con Docker.

---

## Contenido del repositorio

- `ProyectoComeCat-imagenes.tar` → Contiene todas las imágenes Docker necesarias.
- `*.tar.gz` → Backups de los volúmenes Docker:
  - `mysql_data.tar.gz`
  - `api_data.tar.gz`
  - `n8n_data.tar.gz`
  - `mqtt_data.tar.gz`
  - `mqtt_log.tar.gz`
  - `mqtt_config.tar.gz`
  - `portainer_data.tar.gz`
- `docker-compose.yml` → Archivo para levantar todos los contenedores de manera coordinada.

---

## Requisitos

- [Docker](https://www.docker.com/get-started) instalado.
- [Docker Compose](https://docs.docker.com/compose/install/) (opcional si quieres usar `docker-compose.yml`).
- PowerShell o terminal Unix/Linux.

---

## 1️⃣ Cómo se crearon los archivos de respaldo

### Crear un backup de las imágenes Docker

Para exportar todas las imágenes Docker utilizadas en el proyecto a un archivo `.tar`:

```powershell
docker save -o ProyectoComeCat-imagenes.tar proyecto_comecat-api proyecto_comecat-blazor proyecto_comecat-mqtt proyecto_comecat-n8n mysql:8.0 phpmyadmin:latest portainer/portainer-ce:latest
```

### Crear backups de los volúmenes Docker

Para cada volumen Docker se creó un archivo `.tar.gz` usando un contenedor temporal de Ubuntu:

```powershell
docker run --rm -v nombre_volumen:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar czf /backup/archivo_backup.tar.gz ."
```

Ejemplo para el volumen `mysql_data`:

```powershell
docker run --rm -v proyecto_comecat_mysql_data:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar czf /backup/mysql_data.tar.gz ."
```

Esto se repitió para todos los volúmenes del proyecto.

---

## 2️⃣ Cargar las imágenes Docker

Primero, cargar las imágenes desde el archivo `.tar`:

```powershell
docker load -i ProyectoComeCat-imagenes.tar
```

Verificá que las imágenes se hayan cargado:

```powershell
docker images
```

Deberías ver todas las imágenes como:

```
proyecto_comecat-api
proyecto_comecat-blazor
proyecto_comecat-mqtt
proyecto_comecat-n8n
mysql:8.0
phpmyadmin:latest
portainer/portainer-ce:latest
```

---

## 3️⃣ Restaurar los volúmenes Docker

Cada volumen Docker se restaura desde su `.tar.gz` correspondiente usando un contenedor temporal de Ubuntu:

```powershell
docker run --rm -v nombre_volumen:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/archivo_backup.tar.gz --strip 1"
```

Ejemplos para este proyecto:

```powershell
docker run --rm -v proyecto_comecat_mysql_data:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/mysql_data.tar.gz --strip 1"

docker run --rm -v proyecto_comecat_api_data:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/api_data.tar.gz --strip 1"

docker run --rm -v proyecto_comecat_n8n_data:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/n8n_data.tar.gz --strip 1"

docker run --rm -v proyecto_comecat_mqtt_data:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/mqtt_data.tar.gz --strip 1"

docker run --rm -v proyecto_comecat_mqtt_log:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/mqtt_log.tar.gz --strip 1"

docker run --rm -v proyecto_comecat_mqtt_config:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/mqtt_config.tar.gz --strip 1"

docker run --rm -v proyecto_comecat_portainer_data:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/portainer_data.tar.gz --strip 1"
```

> ⚠️ Asegurate de ejecutar estos comandos desde la carpeta donde están los `.tar.gz`.

Podés verificar el contenido de un volumen con:

```powershell
docker run --rm -v proyecto_comecat_mysql_data:/data ubuntu ls /data
```

---

## 4️⃣ Levantar el ecosistema con Docker Compose

Si tenés el archivo `docker-compose.yml` en la misma carpeta:

```powershell
docker-compose up -d
```

Esto levantará todos los contenedores y montará los volúmenes restaurados automáticamente.

Para detener los contenedores:

```powershell
docker-compose down
```

---

## 5️⃣ Verificación

- Accedé a **Portainer** para administrar los contenedores: `http://localhost:9000` (si tu `docker-compose.yml` tiene este puerto).
- Accedé a **phpMyAdmin** para verificar la base de datos MySQL: `http://localhost:8080` (según tu configuración de puertos).
- Verificá que los servicios de Blazor, API, MQTT y N8N estén corriendo correctamente.

---

## 6️⃣ Notas importantes

- No es necesario reconstruir imágenes si ya están en el `.tar`.
- Siempre restaurá primero los volúmenes antes de levantar los contenedores, para evitar pérdida de datos.
- Si necesitás limpiar volúmenes antiguos antes de restaurar:

```powershell
docker volume rm nombre_volumen
```

- Los archivos `.tar.gz` contienen **todos los datos** de cada servicio, incluyendo configuraciones y bases de datos.

Con esto, tu ecosistema de **Proyecto_ComeCat** queda completamente restaurado y listo para usarse en cualquier máquina con Docker.

