# Proyecto PHP con Docker

Este es un proyecto PHP básico containerizado con Docker, que utiliza PHP-FPM 8.1 para ejecutar aplicaciones PHP.

## 📋 Descripción

Este proyecto proporciona un entorno PHP containerizado utilizando Docker y Docker Compose. Está configurado para ejecutar PHP 8.1 con FPM (FastCGI Process Manager), lo que lo hace ideal para desarrollar y ejecutar aplicaciones PHP modernas.

## 🛠️ Tecnologías Utilizadas

- **PHP**: 8.1-fpm
- **Docker**: Para containerización
- **Docker Compose**: Para orquestación de contenedores

## 📁 Estructura del Proyecto

```
laravel/
├── .git/                    # Control de versiones Git
├── .gitignore              # Archivos ignorados por Git
├── .idea/                  # Configuración de IDE
├── demo.php                # Archivo PHP de demostración
├── docker-compose.yml      # Configuración de Docker Compose
├── Dockerfile              # Definición de la imagen Docker
├── html/                   # Carpeta para archivos HTML/PHP
└── README.md              # Este archivo
```

## 📦 Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (versión 20.10 o superior)
- [Docker Compose](https://docs.docker.com/compose/install/) (incluido en Docker Desktop para Windows)

### Verificar Instalación

Para verificar que Docker está correctamente instalado, ejecuta:

```powershell
docker --version
docker-compose --version
```

## 🚀 Construcción del Proyecto

### Paso 1: Clonar el Repositorio

Si aún no tienes el proyecto, clónalo:

```powershell
git clone https://github.com/HenryVegaAyala/ceti-php-laravel-g79
cd laravel
```

### Paso 2: Construir la Imagen Docker

Para construir la imagen Docker del proyecto, ejecuta:

```powershell
docker-compose build
```

Este comando:
- Lee el archivo `docker-compose.yml`
- Construye la imagen basándose en el `Dockerfile`
- Descarga la imagen base de PHP 8.1-fpm
- Configura el entorno de trabajo

### Paso 3: Iniciar los Contenedores

Una vez construida la imagen, inicia los servicios:

```powershell
docker-compose up -d
```

El flag `-d` ejecuta los contenedores en modo "detached" (segundo plano).

### Verificar que el Contenedor está Corriendo

```powershell
docker ps
```

Deberías ver un contenedor llamado `api` en ejecución.

## 🔧 Uso del Proyecto

### Acceder al Contenedor

Para acceder a la terminal del contenedor:

```powershell
docker exec -it api bash
```

### Ejecutar Scripts PHP

Desde dentro del contenedor, puedes ejecutar archivos PHP:

```bash
php /var/www/demo.php
```

O desde fuera del contenedor:

```powershell
docker exec api php /var/www/demo.php
```

### Ver los Logs

Para ver los logs del contenedor:

```powershell
docker-compose logs -f api
```

## 📝 Archivos de Configuración

### Dockerfile

El `Dockerfile` define la imagen del contenedor:

- **Imagen base**: `php:8.1-fpm`
- **Directorio de trabajo**: `/var/www`
- **Comando**: Inicia PHP-FPM

### docker-compose.yml

El archivo `docker-compose.yml` define:

- **Servicio**: `api`
- **Nombre del contenedor**: `api`
- **Volumen**: Monta el directorio actual en `/var/www` dentro del contenedor
- **Red**: `api_network` (bridge)
- **Restart policy**: `unless-stopped`

### demo.php

Archivo de demostración que imprime un mensaje simple. Puedes usarlo para verificar que PHP está funcionando correctamente.

## 🔄 Comandos Útiles

### Detener los Contenedores

```powershell
docker-compose down
```

### Reconstruir y Reiniciar

Si haces cambios en el Dockerfile:

```powershell
docker-compose down
docker-compose build --no-cache
docker-compose up -d
```

### Ver el Estado de los Contenedores

```powershell
docker-compose ps
```

### Eliminar Todo (contenedores, redes, volúmenes)

```powershell
docker-compose down -v
```

### Reiniciar el Contenedor

```powershell
docker-compose restart api
```

## 🌐 Acceso a la Aplicación

### Modo Actual

Actualmente, el proyecto está configurado solo con PHP-FPM, que no sirve archivos directamente a través de HTTP. Para acceder a los archivos PHP:

1. **Ejecutar scripts directamente**:
   ```powershell
   docker exec api php /var/www/demo.php
   ```

2. **Acceder al contenedor y trabajar**:
   ```powershell
   docker exec -it api bash
   cd /var/www
   php demo.php
   ```

### Para Agregar Acceso HTTP (Opcional)

Si deseas acceder a través del navegador, necesitarías agregar Nginx o Apache. Aquí está una guía rápida:

#### Opción 1: Agregar Nginx

Modifica el `docker-compose.yml` para agregar un servicio Nginx:

```yaml
services:
  api:
    # ... configuración existente ...

  nginx:
    image: nginx:alpine
    container_name: nginx
    ports:
      - "8080:80"
    volumes:
      - .:/var/www
      - ./nginx.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - api
    networks:
      - api_network
```

Y crear un archivo `nginx.conf` para conectar Nginx con PHP-FPM.

## 🛡️ Solución de Problemas

### El contenedor no inicia

1. Verifica que Docker Desktop esté corriendo
2. Revisa los logs: `docker-compose logs`
3. Verifica puertos en uso: `netstat -ano | findstr :<puerto>`

### Cambios en el código no se reflejan

Los cambios deberían reflejarse automáticamente gracias al volumen montado. Si no:

1. Verifica que el volumen esté correctamente montado: `docker inspect api`
2. Reinicia el contenedor: `docker-compose restart api`

### Error de permisos

En Windows con Docker Desktop, los permisos usualmente no son un problema. Si tienes problemas:

```powershell
docker exec api chown -R www-data:www-data /var/www
```

## 📚 Próximos Pasos

Para expandir este proyecto, podrías:

1. **Agregar un servidor web** (Nginx o Apache) para acceso HTTP
2. **Agregar una base de datos** (MySQL, PostgreSQL)
3. **Instalar Composer** para gestionar dependencias PHP
4. **Configurar Laravel** si deseas usar este framework
5. **Agregar Redis** para caché
6. **Configurar Xdebug** para debugging

## 📄 Licencia

[Especifica tu licencia aquí]

## 👥 Contribución

[Instrucciones para contribuir al proyecto]

## 📧 Contacto

[Tu información de contacto]

---

**Última actualización**: Enero 2026

