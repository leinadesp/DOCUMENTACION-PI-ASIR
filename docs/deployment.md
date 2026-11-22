# Guía de despliegue

**Dirigido a:** administrador que quiera replicar el entorno desde cero en un Windows 11. Los comandos se ejecutan en **PowerShell como administrador**. Para entender cómo encajan las piezas, ver [architecture.md](architecture.md).

## Requisitos
- Windows 11 con virtualización activada en la BIOS.
- Conexión a internet (se descargan Docker Desktop, la imagen de Trilium y Obsidian).
- Cuenta de GitHub con un repositorio creado para la documentación.

## 1. Instalar WSL 2
Docker Desktop usa WSL 2 como motor. Comprobar si está instalado:

```powershell
wsl --version
```

Si indica que no está instalado, instalar con la distribución por defecto (Ubuntu), crear el usuario de Linux y reiniciar si lo pide:

```powershell
wsl --install
```

Verificar que la distribución usa la versión 2:

```powershell
wsl -l -v
```

## 2. Instalar Docker Desktop
1. Descargar Docker Desktop desde la web oficial de Docker.
2. Ejecutar el instalador y dejar marcada la opción de usar **WSL 2**.
3. Abrir Docker Desktop, aceptar los términos y esperar a **Engine running**.
4. Comprobar las versiones:

```powershell
docker --version
docker compose version
```

## 3. Desplegar Trilium con Docker Compose
Cada servicio Docker vive en su propio directorio dentro de `stacks`:

```powershell
mkdir C:\stacks\trilium
cd C:\stacks\trilium
notepad docker-compose.yml
```

Pegar este contenido en el Bloc de notas y guardar:

```yaml
services:
  trilium:
    image: triliumnext/trilium:latest
    container_name: trilium
    restart: unless-stopped
    ports:
      - "8080:8080"
    environment:
      TRILIUM_DATA_DIR: /home/node/trilium-data
    volumes:
      - ./trilium-data:/home/node/trilium-data
```

> El archivo se crea con el Bloc de notas porque pegar bloques de varias líneas directamente en la consola de PowerShell los desordena.

Arrancar el servicio y comprobarlo:

```powershell
docker compose up -d
docker ps
```

El contenedor `trilium` debe aparecer con estado `Up` y, pasado un rato, `healthy`.

Abrir http://localhost:8080 y completar el asistente: idioma, **Nueva base de conocimientos**, **Vacía** y una contraseña de acceso.

## 4. Clonar el repositorio
Git ya viene instalado en muchos equipos. Comprobarlo:

```powershell
git --version
```

El repositorio se clona en `C:\PI`, no en `stacks` (esa carpeta es solo para servicios Docker):

```powershell
mkdir C:\PI
cd C:\PI
git clone https://github.com/leinadesp/DOCUMENTACION-PI-ASIR.git
```

Excluir la configuración local de Obsidian:

```powershell
Set-Content C:\PI\DOCUMENTACION-PI-ASIR\.gitignore ".obsidian/"
```

## 5. Instalar Obsidian y el plugin Git
Instalar Obsidian (aceptar los términos del origen con `Y`):

```powershell
winget install --id Obsidian.Obsidian -e
```

En Obsidian:
1. En la pantalla de inicio, **Abrir una carpeta como bóveda** y elegir `C:\PI\DOCUMENTACION-PI-ASIR`.
2. **Configuración → Complementos de la comunidad → Salir del modo restringido**.
3. **Buscar**, escribir `git`, abrir el complemento **Git** de Vinzent, **Instalar** y después **Activar**.

## 6. Verificar el despliegue

| Comprobación | Resultado esperado |
|---|---|
| `docker ps` | Contenedor `trilium` en estado `Up` |
| http://localhost:8080 | Interfaz de Trilium |
| Crear una nota en `docs/`, **Ctrl + P** → *Git: Commit all changes* → *Git: Push* | La nota aparece en GitHub dentro de `/docs` |

## Resolución de problemas

| Síntoma | Causa | Solución |
|---|---|---|
| `wsl --version` dice que WSL no está instalado, y `docker` no se reconoce | Falta WSL y Docker Desktop | `wsl --install`, reiniciar e instalar Docker Desktop |
| Las líneas del `docker-compose.yml` salen desordenadas al pegar en la consola | PowerShell no gestiona bien el pegado multilínea | Crear el archivo con `notepad docker-compose.yml` |
| `localhost:8080` no responde tras reiniciar el equipo | Docker Desktop no está iniciado | Iniciar Docker Desktop y esperar a *Engine running*; si el contenedor no aparece en `docker ps`, ejecutar `docker compose up -d` en `C:\stacks\trilium` |