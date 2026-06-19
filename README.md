# Práctica: Pipeline CI/CD con Codespaces y GitHub Actions

Repositorio desarrollado para implementar un pipeline CI/CD que valida una aplicación base en Bash, construye una imagen Docker y verifica la generación del artefacto mediante GitHub Actions.

## Archivos principales

- `app.sh`: aplicación base que se ejecuta dentro del pipeline.
- `Dockerfile`: definición de la imagen Docker.
- `.github/workflows/pipeline.yml`: flujo CI/CD automatizado.

## Proceso automatizado

El pipeline se ejecuta en cada `push` y realiza las siguientes acciones:

1. Descarga el código del repositorio.
2. Lista los archivos disponibles.
3. Da permisos de ejecución al script.
4. Ejecuta la aplicación base.
5. Construye la imagen Docker `devops-app`.
6. Lista las imágenes Docker generadas.

## Relación con DevOps

- **CI:** validación automática del script.
- **CD parcial:** construcción de la imagen Docker como artefacto.
- **Docker:** empaquetamiento de la aplicación.
- **Cloud:** ejecución del pipeline en GitHub Actions.
