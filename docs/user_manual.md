# Manual de usuario

**Dirigido a:** quien use el entorno en el día a día, sin necesidad de saber cómo se instaló. Para la instalación, ver [deployment.md](deployment.md).

## 1. Acceder a Trilium
1. Abrir **Docker Desktop** y esperar a que aparezca **Engine running**.
2. Abrir http://localhost:8080 en el navegador e introducir la contraseña de Trilium.

Si la página no carga tras encender el equipo, casi siempre es porque Docker Desktop no está iniciado. Ver el apartado 5.

## 2. La bitácora en Trilium
La bitácora del proyecto ("Bitácora PI-1") se organiza en cuatro bloques:

| Bloque | Contenido |
|---|---|
| 1. Gestión y planificación | Objetivos y alcance, reparto de roles, actas de reuniones |
| 2. Arquitectura y diseño | Arquitectura del despliegue y ADRs (decisiones) |
| 3. Operaciones | Despliegue de Trilium y arranque tras reiniciar |
| 4. Incidencias | Registro de errores y su solución |

**Crear una nota nueva**
1. Clic derecho sobre la nota padre → **Insertar subnota**.
2. En una nota vacía, cambiar el tipo de **Texto** a **Markdown**.
3. Escribir el contenido. Los diagramas Mermaid y las tablas se ven con la **vista previa** (icono del ojo en la barra de la nota). El modo de solo código muestra el texto sin dibujar.

## 3. Editar la documentación con Obsidian
La documentación de `/docs` se escribe en Obsidian, sobre el repositorio clonado en `C:\PI\DOCUMENTACION-PI-ASIR`.

1. Abrir Obsidian con la bóveda `DOCUMENTACION-PI-ASIR`.
2. Crear o editar notas dentro de la carpeta `docs`.
3. Guardar los cambios en el repositorio: **Ctrl + P** → **Git: Commit all changes**.
4. Subirlos a GitHub: **Ctrl + P** → **Git: Push**.
5. Comprobar en GitHub, dentro de `/docs`, que el cambio aparece.

> Si se trabaja desde más de un equipo, hacer **Git: Pull** antes de empezar para traer los últimos cambios.

## 4. Operar el servicio de Trilium
Estos comandos se ejecutan en PowerShell, desde `C:\stacks\trilium`:

| Acción | Comando |
|---|---|
| Ver el estado del contenedor | `docker compose ps` |
| Ver los registros en directo | `docker compose logs -f` |
| Parar el servicio | `docker compose stop` |
| Arrancar el servicio | `docker compose up -d` |

### Copia de seguridad
1. Parar el servicio: `docker compose stop`.
2. Copiar la carpeta `C:\stacks\trilium\trilium-data` a otra ubicación, por ejemplo un disco externo.
3. Volver a arrancar: `docker compose up -d`.

Para restaurar, parar el servicio, sustituir la carpeta `trilium-data` por la copia y arrancar de nuevo.

## 5. Problemas frecuentes

| Síntoma | Causa probable | Solución |
|---|---|---|
| `localhost:8080` no carga tras encender el equipo | Docker Desktop no está iniciado | Iniciar Docker Desktop y esperar a *Engine running* |
| Docker Desktop arranca, pero `docker ps` no muestra `trilium` | El contenedor no se ha levantado | En `C:\stacks\trilium`, ejecutar `docker compose up -d` |
| Docker Desktop no arranca | WSL no funciona | Ejecutar `wsl -l -v` y comprobar que Ubuntu aparece en versión 2 |
| Los diagramas de una nota de Trilium aparecen como texto | La nota está en modo solo código | Activar la vista previa (icono del ojo) |
| El push de Obsidian falla | Falta iniciar sesión en GitHub o no hay conexión | Comprobar la conexión y las credenciales de GitHub, y repetir *Git: Push* |
| Los cambios de Obsidian no aparecen en GitHub | Se hizo el commit pero no el push | Ejecutar *Git: Push* |