# Casos de uso

## Google Antigravity

Google Antigravity es un entorno de desarrollo agéntico presentado por Google a finales de 2025. Está construido sobre Visual Studio Code y usa modelos de la familia Gemini, aunque también puede usar Claude y otros. Soporta MCP de forma nativa para que el agente pueda acceder a archivos del proyecto, ejecutar comandos en la terminal, controlar el navegador y realizar otras operaciones. Lo relevante es que el agente descubre las herramientas vía MCP en lugar de tenerlas cableadas en el código.

## Cursor

Cursor es un editor de código con IA construido también sobre VS Code. Soporta MCP para que el agente pueda operar sobre archivos del proyecto, consultar bases de datos, ejecutar comandos y conectarse a otros servicios. Cuando el usuario le pide a Cursor que modifique un archivo, el agente usa las herramientas MCP del servidor de filesystem para leer, editar y guardar el archivo directamente en disco, sin que el usuario tenga que subirlo manualmente.

## Claude Code

Claude Code es la CLI de Anthropic para tareas de desarrollo. Soporta servidores MCP locales y remotos, lo que le permite al agente leer y modificar archivos de un repositorio, ejecutar tests, hacer commits y otras operaciones. Al igual que Cursor, usa el servidor de filesystem vía MCP para operar sobre el proyecto.

## Qwen Code (aclaración importante)

Qwen es una familia de modelos de lenguaje desarrollada por Alibaba Cloud. No es una plataforma de desarrollo ni una herramienta de edición. La herramienta concreta que implementa MCP es Qwen Code, una CLI basada en Qwen-Coder que soporta servidores MCP y permite al modelo operar sobre archivos y terminal. Es importante no confundir la familia de modelos con la herramienta que la usa.

## Cómo editan repos completos sin subir archivos manualmente

Estas herramientas editan repositorios completos sin que el usuario tenga que subir archivos porque el servidor MCP de filesystem corre localmente y expone herramientas para listar, leer y escribir archivos del proyecto. El flujo es el siguiente:

1. El usuario pide al agente algo como "agrega validación al formulario de login".
2. El agente, vía MCP, lista el contenido del directorio del proyecto.
3. El agente lee los archivos relevantes (por ejemplo, login.tsx y validators.ts).
4. El agente edita los archivos directamente en disco con la herramienta write_file o edit_file.
5. El agente puede ejecutar tests o comandos con otra herramienta MCP (por ejemplo, un servidor de terminal).
6. El agente repite el ciclo hasta terminar la tarea.

En ningún momento el usuario sube archivos manualmente. El servidor MCP corre en la misma máquina que el proyecto y opera sobre el disco local. El modelo nunca ve el disco directamente: pide la operación, el servidor la ejecuta y le devuelve el resultado. El usuario solo autoriza el directorio de trabajo al configurar el servidor.

## Referencias

Google. (2025). Antigravity. https://antigravity.google/

Cursor. (2025). Cursor - The AI Code Editor. https://cursor.com/

Anthropic. (2025). Claude Code. https://www.anthropic.com/claude-code

Alibaba Cloud. (2025). Qwen Code. https://github.com/QwenLM/qwen-code
