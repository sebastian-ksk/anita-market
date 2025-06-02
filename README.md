# N8N Docker Compose Setup

Este proyecto contiene la configuración de Docker Compose para ejecutar N8N con PostgreSQL.

## Características

- ✅ N8N con autenticación básica
- ✅ Base de datos PostgreSQL para persistencia
- ✅ Volúmenes para persistir datos
- ✅ Red interna para comunicación entre servicios
- ✅ Variables de entorno configurables

## Requisitos Previos

- Docker y Docker Compose instalados
- Puertos 5678 disponible en tu sistema

## Configuración

### 1. Variables de Entorno (Opcional)

Puedes crear un archivo `.env` en el directorio raíz para personalizar las configuraciones:

```env
# Configuración de N8N
N8N_BASIC_AUTH_USER=tu_usuario
N8N_BASIC_AUTH_PASSWORD=tu_password_seguro

# Configuración de Base de Datos
POSTGRES_DB=n8n
POSTGRES_USER=n8n_user
POSTGRES_PASSWORD=tu_password_db_seguro

# Configuración de Zona Horaria
TIMEZONE=America/Bogota

# URL Base
WEBHOOK_URL=http://localhost:5678/
```

### 2. Directorio para Archivos Locales (Opcional)

Si planeas usar archivos locales en tus workflows, crea el directorio:

```bash
mkdir n8n-local-files
```

## Uso

### Iniciar los Servicios

```bash
docker-compose up -d
```

### Ver los Logs

```bash
# Ver todos los logs
docker-compose logs -f

# Ver solo los logs de N8N
docker-compose logs -f n8n

# Ver solo los logs de PostgreSQL
docker-compose logs -f postgres
```

### Detener los Servicios

```bash
docker-compose down
```

### Detener y Eliminar Volúmenes (⚠️ Esto eliminará todos los datos)

```bash
docker-compose down -v
```

## Acceso a N8N

Una vez que los servicios estén ejecutándose:

1. Abre tu navegador web
2. Ve a: `http://localhost:5678`
3. Usa las credenciales configuradas:
   - Usuario: `admin` (o el que hayas configurado)
   - Contraseña: `cambiar_password` (o la que hayas configurado)

## Estructura de Archivos

```
.
├── docker-compose.yml    # Configuración principal
├── .env                 # Variables de entorno (opcional)
├── n8n-local-files/     # Archivos locales para workflows (opcional)
└── README.md           # Este archivo
```

## Volúmenes de Datos

- `n8n_data`: Contiene workflows, credenciales y configuraciones de N8N
- `postgres_data`: Contiene los datos de la base de datos PostgreSQL

## Solución de Problemas

### Error de Conexión a la Base de Datos

Si N8N no puede conectarse a PostgreSQL:

1. Verifica que ambos contenedores estén ejecutándose:

   ```bash
   docker-compose ps
   ```

2. Revisa los logs de PostgreSQL:
   ```bash
   docker-compose logs postgres
   ```

### Cambiar Contraseñas

Para cambiar las contraseñas después de la primera ejecución:

1. Detén los servicios: `docker-compose down`
2. Modifica las variables de entorno en `.env` o `docker-compose.yml`
3. Reinicia los servicios: `docker-compose up -d`

### Backup de Datos

Para hacer backup de tus workflows y datos:

```bash
# Backup de N8N
docker run --rm -v n8n_n8n_data:/data -v $(pwd):/backup alpine tar czf /backup/n8n_backup.tar.gz -C /data .

# Backup de PostgreSQL
docker-compose exec postgres pg_dump -U n8n n8n > n8n_db_backup.sql
```

## Configuración Avanzada

### Usar HTTPS

Para usar HTTPS, necesitarás configurar un proxy reverso como Nginx o Traefik, o modificar las variables de entorno de N8N para usar certificados SSL.

### Configuración de Producción

Para un entorno de producción, considera:

1. Usar contraseñas más seguras
2. Configurar un proxy reverso con SSL
3. Limitar el acceso a la base de datos
4. Configurar backups automáticos
5. Usar un dominio personalizado

## Soporte

Para más información sobre N8N, visita:

- [Documentación oficial de N8N](https://docs.n8n.io/)
- [Repositorio de N8N en GitHub](https://github.com/n8n-io/n8n)
