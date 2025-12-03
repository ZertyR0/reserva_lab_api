# Despliegue en Railway - Guía Rápida

## Pasos para desplegar en Railway

### 1. Crear proyecto en Railway
1. Ve a [Railway.app](https://railway.app)
2. Haz clic en "New Project"
3. Selecciona "Deploy from GitHub repo"
4. Conecta tu repositorio `reserva_lab_api`

### 2. Agregar MySQL Database
1. En tu proyecto de Railway, haz clic en "+ New"
2. Selecciona "Database" → "Add MySQL"
3. Railway creará automáticamente las variables de entorno

### 3. Configurar Variables de Entorno
En la configuración de tu servicio, agrega estas variables:

```
SECRET_KEY=tu-secret-key-super-segura-y-larga
DEBUG=False
ALLOWED_HOSTS=.railway.app,.vercel.app
CORS_ORIGIN_ALLOW_ALL=False
CORS_ALLOWED_ORIGINS=https://reserva-lab-webappp-8603csxbl-zertyr0s-projects.vercel.app
```

**Nota:** Las variables de la base de datos (DB_NAME, DB_USER, DB_PASSWORD, DB_HOST, DB_PORT) se crean automáticamente cuando agregas MySQL.

### 4. Despliegue Automático
- Railway detectará automáticamente que es un proyecto Django
- Instalará las dependencias de `requirements.txt`
- Ejecutará las migraciones
- Iniciará el servidor con Gunicorn

### 5. Obtener la URL de Railway
1. Una vez desplegado, Railway te dará una URL como: `https://tu-proyecto.up.railway.app`
2. Copia esta URL y actualiza tu frontend en Vercel para que apunte a esta API

### 6. Actualizar Frontend (Vercel)
En tu proyecto de frontend en Vercel, actualiza la variable de entorno de la API:
```
API_URL=https://tu-proyecto.up.railway.app
```

## Comandos Útiles

### Ejecutar migraciones manualmente (si es necesario)
En Railway, puedes ejecutar comandos desde la terminal:
```bash
python manage.py migrate
```

### Crear superusuario
```bash
python manage.py createsuperuser
```

### Cargar datos de prueba
```bash
python load_sample_data.py
```

## Archivos importantes para Railway
- `Procfile` - Define el comando para iniciar la aplicación
- `railway.json` - Configuración específica de Railway
- `runtime.txt` - Versión de Python
- `requirements.txt` - Dependencias de Python

## Troubleshooting

### Error de CORS
Si tienes problemas de CORS, verifica que `CORS_ALLOWED_ORIGINS` incluya la URL exacta de tu frontend en Vercel.

### Error de base de datos
Asegúrate de que las variables de entorno de MySQL estén correctamente configuradas en Railway.

### Archivos estáticos no se cargan
Railway ejecutará automáticamente `collectstatic` gracias a WhiteNoise.
