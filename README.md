# PRÁCTICA DE LABORATORIO

## Pipeline CI/CD con Codespaces, GitHub Actions y Docker

### Objetivo

- Implementar un pipeline CI/CD completo.
- Automatizar validación y despliegue.
- Ejecutar todo en la nube usando Codespaces y GitHub Actions.

### Entorno

- GitHub Codespaces.
- GitHub Actions.
- Docker.

### Escenario

Un equipo DevOps necesita automatizar:

- Validación del código.
- Construcción de la aplicación.
- Generación de una imagen Docker.

---

## PARTE 1 - CREAR ENTORNO

### Paso 1 - Crear repositorio

En GitHub se creó el repositorio:

```bash
devops-ci-cd-docker_7193
```

El repositorio fue creado con archivo `README.md`.

### Paso 2 - Abrir Codespaces

Desde GitHub:

```text
Code -> Codespaces -> Create codespace on main
```

Codespaces crea una máquina Linux en la nube lista para trabajar con DevOps.

---

## PARTE 2 - CREAR APLICACIÓN BASE

### Paso 3 - Crear aplicación

Comando para crear el archivo:

```bash
nano app.sh
```

Contenido del archivo:

```bash
#!/bin/bash
echo "Aplicacion ejecutada correctamente"
```

### Paso 4 - Dar permisos de ejecución

```bash
chmod +x app.sh
```

Esta será la aplicación que el pipeline validará automáticamente.

---

## PARTE 3 - CREAR DOCKERFILE

### Paso 5 - Crear Dockerfile

```bash
nano Dockerfile
```

Contenido del archivo:

```dockerfile
FROM ubuntu:latest
WORKDIR /app
COPY app.sh .
RUN chmod +x app.sh
CMD ["./app.sh"]
```

### Explicación del Dockerfile

| Línea | Qué hace |
|---|---|
| `FROM ubuntu:latest` | Usa Ubuntu como imagen base. |
| `WORKDIR /app` | Define la carpeta de trabajo dentro del contenedor. |
| `COPY app.sh .` | Copia el script al contenedor. |
| `RUN chmod +x app.sh` | Asigna permisos de ejecución. |
| `CMD ["./app.sh"]` | Ejecuta la aplicación al iniciar el contenedor. |

Aquí se define la aplicación como contenedor.

---

## PARTE 4 - CREAR PIPELINE CI/CD

### Paso 6 - Crear estructura de carpetas

```bash
mkdir -p .github/workflows
```

### Paso 7 - Crear pipeline

```bash
nano .github/workflows/pipeline.yml
```

---

## PARTE 5 - DEFINIR PIPELINE

Contenido del archivo `.github/workflows/pipeline.yml`:

```yaml
name: CI-CD Pipeline

on:
  push:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout del repositorio
        uses: actions/checkout@v3

      - name: Listar archivos
        run: ls -l

      - name: Dar permisos al script
        run: chmod +x app.sh

      - name: Ejecutar script
        run: ./app.sh

      - name: Construir imagen Docker
        run: docker build -t devops-app .

      - name: Listar imagenes Docker
        run: docker images
```

### Explicación paso a paso

| Elemento | Explicación |
|---|---|
| `on: push` | Ejecuta el pipeline automáticamente cuando se suben cambios. |
| `actions/checkout@v3` | Descarga el código del repositorio en el runner. |
| `run: ls -l` | Lista los archivos para verificar que están disponibles. |
| `chmod +x app.sh` | Da permisos al script. |
| `./app.sh` | Ejecuta la aplicación y valida su funcionamiento. |
| `docker build` | Construye la imagen Docker como artefacto. |
| `docker images` | Verifica que la imagen fue creada. |

---

## PARTE 6 - EJECUTAR PIPELINE

### Paso 8 - Subir cambios

```bash
git add .
git commit -m "pipeline completo"
git push
```

### Paso 9 - Ver ejecución

En GitHub:

```text
Actions -> CI-CD Pipeline
```

Resultado esperado:

- Script ejecutado correctamente.
- Imagen Docker creada.
- Pipeline exitoso.

---

## PARTE 7 - VALIDAR RESULTADO

Revisar los logs del pipeline y verificar cada paso:

- Ejecución del script.
- Construcción de la imagen Docker.
- Listado de imágenes Docker.

Interpretación:

El pipeline automatiza:

1. Validación.
2. Construcción.
3. Generación de artefacto.

---

## PARTE 8 - SIMULAR ERROR

### Paso 10 - Modificar `app.sh`

```bash
nano app.sh
```

Contenido para provocar error:

```bash
#!/bin/bash
echo "Error intencional"
exit 1
```

Subir cambios:

```bash
git add .
git commit -m "error para probar pipeline"
git push
```

Resultado esperado:

```text
Pipeline falla
```

Explicación:

CI detecta errores automáticamente cuando el script termina con `exit 1`.

---

## PARTE 9 - CORRECCIÓN

Revertir el cambio en el archivo:

```bash
nano app.sh
```

Contenido corregido:

```bash
#!/bin/bash
echo "Aplicacion ejecutada correctamente"
exit 0
```

Subir cambios:

```bash
git add .
git commit -m "corregir error del pipeline"
git push
```

Resultado esperado:

```text
Pipeline vuelve a funcionar
```

---

## PARTE 10 - RELACIÓN CON DEVOPS

| Concepto | Aplicación |
|---|---|
| CI | Ejecución automática del script para validar cambios. |
| CD | Construcción de imagen Docker como artefacto. |
| Docker | Empaquetamiento de la aplicación. |
| Cloud | Ejecución del pipeline en GitHub Actions. |

---

## Comandos útiles para verificar en Codespaces

Actualizar el Codespace si no aparecen los archivos:

```bash
git pull origin main
```

Listar archivos del repositorio:

```bash
ls -la
```

Verificar workflow:

```bash
ls -la .github/workflows
```

Ejecutar la app manualmente:

```bash
chmod +x app.sh
./app.sh
```

Construir imagen Docker manualmente:

```bash
docker build -t devops-app .
```

Listar imágenes Docker:

```bash
docker images
```
