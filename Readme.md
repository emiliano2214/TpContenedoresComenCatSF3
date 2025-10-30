# 🐾 Proyecto_ComeCat

Ecosistema completo del proyecto **ComeCat**, implementado con contenedores Docker que incluyen los servicios **API**, **Blazor**, **MQTT**, **N8N**, **MySQL**, **phpMyAdmin** y **Portainer**.  
El entorno está completamente preparado para ejecutarse localmente **sin conexión a Internet** y sin necesidad de descargar imágenes desde Docker Hub.

---

## 📦 Estructura esperada

```
Proyecto_ComeCat/
├── docker-compose.yml
├── mysql_data.tar.gz
├── api_data.tar.gz
├── n8n_data.tar.gz
├── mqtt_data.tar.gz
├── mqtt_log.tar.gz
├── mqtt_config.tar.gz
├── portainer_data.tar.gz
└── ProyectoComeCat-imagenes.tar  ← se descarga desde Release
```

> ⚠️ El archivo `ProyectoComeCat-imagenes.tar` (≈1.13 GB) no está incluido en el repositorio por el límite de GitHub.  
> Debe descargarse desde la sección **[Releases](https://github.com/emiliano2214/TpContenedoresComenCatSF3/releases)** y colocarse en la misma carpeta clonada.

---

## 🧰 Requisitos

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)
- PowerShell, CMD o Terminal Linux  

Verificá la instalación:
```bash
docker --version
docker compose version
```

---

## 🚀 Uso paso a paso

### 1️⃣ Clonar el repositorio
```bash
git clone https://github.com/emiliano2214/TpContenedoresComenCatSF3.git
cd TpContenedoresComenCatSF3
```

### 2️⃣ Descargar el Release
1. Ir a 👉 [Releases del proyecto](https://github.com/emiliano2214/TpContenedoresComenCatSF3/releases)  
2. Descargar **ProyectoComeCat-imagenes.tar**  
3. Colocar el archivo en la raíz del repositorio (junto a `docker-compose.yml`)

---

### 3️⃣ Cargar imágenes Docker locales
```bash
docker load -i ProyectoComeCat-imagenes.tar
docker images
```
Deberías ver las imágenes:
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

### 4️⃣ Restaurar los volúmenes Docker
Ejecutar desde la carpeta clonada:
```bash
docker run --rm -v proyecto_comecat_mysql_data:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/mysql_data.tar.gz --strip 1"
docker run --rm -v proyecto_comecat_api_data:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/api_data.tar.gz --strip 1"
docker run --rm -v proyecto_comecat_n8n_data:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/n8n_data.tar.gz --strip 1"
docker run --rm -v proyecto_comecat_mqtt_data:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/mqtt_data.tar.gz --strip 1"
docker run --rm -v proyecto_comecat_mqtt_log:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/mqtt_log.tar.gz --strip 1"
docker run --rm -v proyecto_comecat_mqtt_config:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/mqtt_config.tar.gz --strip 1"
docker run --rm -v proyecto_comecat_portainer_data:/data -v ${PWD}:/backup ubuntu bash -c "cd /data && tar xzf /backup/portainer_data.tar.gz --strip 1"
```

---

### 5️⃣ Levantar el ecosistema
Desde la carpeta raíz del proyecto:
```bash
docker compose up -d
```
Esto levantará todos los contenedores usando las imágenes y volúmenes locales.

Para detenerlos:
```bash
docker compose down
```

---

## 🌐 Puertos y Servicios

| Servicio | URL / Puerto | Imagen | Descripción |
|-----------|--------------|---------|--------------|
| **Blazor** | [http://localhost:5003](http://localhost:5003) | `proyecto_comecat-blazor` | Interfaz web del sistema |
| **N8N** | [http://localhost:5678](http://localhost:5678) | `proyecto_comecat-n8n` | Automatización de flujos |
| **API** | [http://localhost:5000](http://localhost:5000) | `proyecto_comecat-api` | Backend del sistema |
| **MQTT Broker** | `1883` | `proyecto_comecat-mqtt` | Comunicación IoT |
| **phpMyAdmin** | [http://localhost:5011](http://localhost:5011) | `phpmyadmin:latest` | Administración MySQL |
| **Portainer** | [http://localhost:8000](http://localhost:8000) / [http://localhost:9000](http://localhost:9000) | `portainer/portainer-ce` | Panel de gestión Docker |
| **MySQL** | `3306` (interno) | `mysql:8.0` | Base de datos del sistema |

---

## 🔍 Verificación rápida

Ver contenedores activos:
```bash
docker ps
```
Deberías ver los contenedores:
```
blazor, n8n, api, mqtt, phpmyadmin, portainer, mysql
```

---

## 🧱 Concepto de funcionamiento
El proyecto restaura un entorno completo de contenedores locales.  
- Las imágenes se cargan desde el archivo `.tar`.  
- Los volúmenes `.tar.gz` contienen todos los datos persistentes.  
- `docker-compose.yml` orquesta el arranque sin requerir conexión a Docker Hub.  

Ideal para **entregas académicas**, **presentaciones offline** o **demostraciones técnicas**.

---

## ✨ Créditos
**Autor:** Abate Emiliano  
**Instituto Tecnológico de Educación Superior – ITES**  
**Materia:** *Software Factory III*  
**Año:** 2025  
