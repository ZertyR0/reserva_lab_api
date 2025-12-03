# 🚀 PASOS PARA DESPLEGAR EN RAILWAY

## ✅ Paso 1: Acceder a Railway
1. Ve a https://railway.app
2. Inicia sesión con tu cuenta de GitHub

## ✅ Paso 2: Crear Nuevo Proyecto
1. Haz clic en **"New Project"**
2. Selecciona **"Deploy from GitHub repo"**
3. Busca y selecciona **`ZertyR0/reserva_lab_api`**

## ✅ Paso 3: Agregar Base de Datos MySQL
1. En tu proyecto, haz clic en **"+ New"**
2. Selecciona **"Database"**
3. Elige **"Add MySQL"**
4. ⚠️ **IMPORTANTE:** Railway creará automáticamente estas variables:
   - `MYSQLHOST`
   - `MYSQLPORT`
   - `MYSQLDATABASE`
   - `MYSQLUSER`
   - `MYSQLPASSWORD`

## ✅ Paso 4: Configurar Variables de Entorno del Backend
1. Haz clic en tu servicio **reserva_lab_api**
2. Ve a la pestaña **"Variables"**
3. Agrega estas variables manualmente:

```bash
SECRET_KEY=django-insecure-TU_CLAVE_SECRETA_AQUI_MUY_LARGA
DEBUG=False
ALLOWED_HOSTS=.railway.app,.vercel.app

# Mapeo de variables de MySQL (usa las de Railway)
DB_NAME=${{MySQL.MYSQLDATABASE}}
DB_USER=${{MySQL.MYSQLUSER}}
DB_PASSWORD=${{MySQL.MYSQLPASSWORD}}
DB_HOST=${{MySQL.MYSQLHOST}}
DB_PORT=${{MySQL.MYSQLPORT}}

# CORS - Agrega tu URL de Vercel
CORS_ORIGIN_ALLOW_ALL=False
CORS_ALLOWED_ORIGINS=https://reserva-lab-webappp-8603csxbl-zertyr0s-projects.vercel.app,http://localhost:4200
```

### 🔑 Para generar un SECRET_KEY seguro:
Ejecuta esto en tu terminal local:
```python
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

## ✅ Paso 5: Esperar el Despliegue
- Railway automáticamente:
  - ✅ Instalará las dependencias
  - ✅ Ejecutará las migraciones
  - ✅ Iniciará el servidor
- Espera unos 2-3 minutos

## ✅ Paso 6: Obtener la URL del Backend
1. Una vez desplegado, ve a **"Settings"** en tu servicio
2. Copia la URL que se ve como: `https://reserva-lab-api-production-XXXX.up.railway.app`
3. **Prueba la API:** Abre `https://TU-URL.railway.app/api/` en el navegador

## ✅ Paso 7: Actualizar Frontend en Vercel
1. Ve a tu proyecto en Vercel
2. Settings → Environment Variables
3. Agrega o actualiza:
```bash
API_URL=https://TU-URL-DE-RAILWAY.up.railway.app
NEXT_PUBLIC_API_URL=https://TU-URL-DE-RAILWAY.up.railway.app
```
4. Redespliega el frontend

## ✅ Paso 8: Ejecutar Comandos Iniciales (Opcional)
Si necesitas crear un superusuario o cargar datos:

1. En Railway, ve a tu servicio
2. Haz clic en la pestaña **"Deployments"**
3. Selecciona el último deployment
4. Haz clic en **"View Logs"** y luego en el ícono de terminal
5. Ejecuta:
```bash
python manage.py createsuperuser
python load_sample_data.py
```

## ⚠️ IMPORTANTE: Actualizar CORS
Después de obtener tu URL de Railway, actualiza la variable de entorno:
```bash
CORS_ALLOWED_ORIGINS=https://TU-URL-RAILWAY.railway.app,https://reserva-lab-webappp-8603csxbl-zertyr0s-projects.vercel.app
```

## 🔍 Verificación Final
Prueba estos endpoints:
- `https://TU-URL.railway.app/api/` - Debe mostrar la API
- `https://TU-URL.railway.app/admin/` - Debe mostrar el admin de Django
- Desde tu frontend en Vercel, intenta hacer login

## 🐛 Solución de Problemas

### Error 500
- Revisa los logs en Railway
- Verifica que todas las variables de entorno estén configuradas

### Error de CORS
- Asegúrate que `CORS_ALLOWED_ORIGINS` incluya la URL exacta de Vercel (con https://)

### Error de Base de Datos
- Verifica que las variables `DB_*` estén mapeadas correctamente a las variables de MySQL

### Las migraciones no se ejecutan
- Ve a la terminal de Railway y ejecuta manualmente:
```bash
python manage.py migrate
```

## 📝 Notas
- Railway tiene un tier gratuito con $5 de crédito mensual
- Si se agota, el servicio se pausará hasta el próximo mes
- Puedes monitorear el uso en el dashboard de Railway
