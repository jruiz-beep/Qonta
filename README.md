# Documentación Técnica de Qonta

Este repositorio contiene la documentación técnica del sistema Qonta, una solución enfocada en la automatización del acuse y causación de facturas electrónicas en Colombia.

La documentación está construida con MkDocs y es accesible como un sitio web estático.

## Acceso a la Documentación Publicada

Puedes acceder a la versión más reciente de la documentación aquí:
[`https://jruiz-beep.github.io/Qonta/`]

## Estructura del Proyecto

* `docs/`: Contiene todos los archivos fuente de la documentación en formato Markdown.
* `mkdocs.yml`: Archivo de configuración para MkDocs.

## Cómo Contribuir (Si usas Git para la documentación)

Para realizar cambios o adiciones a la documentación:

1.  Clona este repositorio: `git clone https://github.com/jruiz-beep/Qonta.git`
2.  Crea una nueva rama para tus cambios: `git checkout -b feature/tu-cambio`
3.  Modifica o añade archivos Markdown en la carpeta `docs/`.
4.  Si añades nuevas secciones, actualiza el archivo `mkdocs.yml` para incluirlas en la navegación.
5.  Puedes previsualizar los cambios localmente ejecutando `mkdocs serve` (requiere Python y MkDocs instalados).
6.  Realiza un commit con tus cambios: `git commit -m "docs: Descripción de tu cambio"`
7.  Sube la rama: `git push origin feature/tu-cambio`
8.  Crea una Pull Request en GitHub/GitLab/Bitbucket.

## Instalación y Ejecución Local de MkDocs

Si deseas construir y previsualizar la documentación localmente, asegúrate de tener Python instalado y sigue estos pasos:

1.  Instala MkDocs y el tema Material:
    ```bash
    pip install mkdocs mkdocs-material
    ```
2.  Navega a la raíz de este repositorio en tu terminal.
3.  Inicia el servidor de desarrollo de MkDocs:
    ```bash
    mkdocs serve
    ```
4.  Abre tu navegador y ve a `http://127.0.0.1:8000`.