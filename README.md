# Documentación del proyecto

Documentación técnica del despliegue de un entorno de gestión del conocimiento (PKM) con **Trilium Notes** y **Obsidian**, realizada para la práctica PI-1 del Proyecto Intermodular (2º ASIR) con la filosofía *Docs as Code*.

## Qué se documenta
Un portátil con Windows 11 que ejecuta:
- **Trilium Notes** en un contenedor Docker (Docker Desktop con WSL 2), usado como bitácora del proyecto.
- **Obsidian** sobre este repositorio, sincronizado con GitHub mediante el plugin Git.

## Índice

| Documento | Para quién | Contenido |
|---|---|---|
| [architecture.md](architecture.md) | Quien necesite entender cómo está montado | Componentes, diagramas Mermaid, persistencia y decisiones |
| [deployment.md](deployment.md) | Administrador que quiera replicar el despliegue | Guía paso a paso desde cero, con comandos comprobados |
| [user_manual.md](user_manual.md) | Usuario del día a día | Cómo usar y operar el entorno y qué hacer si falla algo |

## Tecnologías

| Componente | Versión usada |
|---|---|
| Windows 11 con WSL 2 | WSL 3.0.5, Ubuntu |
| Docker Desktop | 4.99.0 (Docker 29.9.3, Compose v5.7.0) |
| Trilium Notes | Imagen `triliumnext/trilium:latest` |
| Git para Windows | 2.53.0 |
| Obsidian | 1.15.1, con el plugin Git 2.43.0 |

## Cómo se organiza este repositorio
- `docs/`: esta documentación, escrita en Markdown con diagramas Mermaid.
- Las decisiones (ADR), el registro de errores, las actas y el reparto de roles se llevan en la bitácora de Trilium. Su resumen está en `architecture.md`.

## Autoría
Daniel Castilla Pérez, 2º CFGS ASIR, IES Politécnico Jesús Marín.
