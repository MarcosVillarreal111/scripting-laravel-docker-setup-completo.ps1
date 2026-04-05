# Laravel Docker Setup

Stack completo de Laravel con Docker en Windows, incluyendo MySQL, Redis, Nginx y phpMyAdmin.

## Requisitos

- [Docker Desktop](https://www.docker.com/products/docker-desktop) instalado y corriendo
- Windows 10/11

## Servicios incluidos

| Servicio | URL / Puerto |
|---|---|
| Laravel | http://localhost |
| phpMyAdmin | http://localhost:8080 |
| MySQL | puerto 3306 |
| Redis | puerto 6379 |

## Instalación

### 1. Ejecutar el script

Abrí PowerShell y corré:

```powershell
powershell -ExecutionPolicy Bypass -File setup-completo.ps1
```

El script crea toda la estructura, construye los contenedores y espera en el **PASO 5**.

---

### 2. PASO 5 — Instalar Laravel (manual)

Cuando el script se detenga en el PASO 5, **abrí una terminal nueva**, navegá hasta la carpeta `laravel-docker` y ejecutá:

```bash
docker compose exec app sh -c "composer create-project laravel/laravel /tmp/laravel --remove-vcs --no-interaction && cp -rT /tmp/laravel /var/www/html && rm -rf /tmp/laravel"
```

> ⚠️ **IMPORTANTE: Esperá a que este comando termine completamente antes de volver al script.**
> Si presionás Enter en el script antes de que Composer termine, el paso de `key:generate` fallará porque los archivos todavía no están en su lugar.
> Sabés que terminó cuando volvés a ver el prompt `PS C:\...>` en la terminal.

---

### 3. Continuar el script

Una vez que Composer terminó, volvé a la terminal del script y presioná **Enter**. El script completará automáticamente:

- Configuración del `.env`
- Ajuste de permisos
- Generación de `APP_KEY`
- Migraciones de base de datos

---

## Si algo salió mal

Si por error presionaste Enter antes de que Composer terminara, corré estos comandos manualmente:

```powershell
docker compose exec app php artisan key:generate --force
docker compose exec app php artisan config:clear
docker compose exec app php artisan migrate --force
```

---

## Credenciales de base de datos

| Campo | Valor |
|---|---|
| Host | db |
| Base de datos | my_database |
| Usuario | root |
| Contraseña | root_password |

---

## Comandos útiles

```powershell
# Ver logs del contenedor PHP
docker compose logs -f app

# Consola de Laravel
docker compose exec app php artisan tinker

# Correr migraciones
docker compose exec app php artisan migrate

# Detener todos los contenedores
docker compose down

# Reconstruir los contenedores
docker compose up -d --build
```
