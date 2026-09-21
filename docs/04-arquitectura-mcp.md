# Arquitectura de MCP

## Modelo host / cliente / servidor

MCP define tres roles que se organizan en capas:

**Host.** Es la aplicación que el usuario abre y con la que interactúa. El host contiene al modelo de lenguaje y decide qué servidores MCP puede usar. En esta actividad, el host es Claude Desktop.

**Cliente.** Es el componente dentro del host que se conecta a un servidor MCP específico. Un host puede tener varios clientes, cada uno conectado a un servidor distinto. El cliente se encarga de la comunicación JSON-RPC con el servidor. En mi instalación, el cliente es el componente interno de Claude Desktop que se conecta al servidor de filesystem.

**Servidor.** Es el programa que expone herramientas, recursos y plantillas de prompt. Corre como proceso separado (local o remoto) y responde a las peticiones del cliente. En mi caso, el servidor es @modelcontextprotocol/server-filesystem, que corre como proceso hijo en mi máquina.

El modelo no habla directamente con el servidor. El modelo le pide al host que ejecute una herramienta; el host usa al cliente para enviar la petición JSON-RPC al servidor; el servidor ejecuta la operación y devuelve el resultado; el cliente lo pasa de vuelta al host; y el host se lo muestra al modelo. En ningún momento el modelo toca el disco directamente.

## Primitivas del servidor

Un servidor MCP puede exponer tres tipos de primitivas:

**Herramientas (tools).** Son acciones que el modelo puede ejecutar. Cada herramienta tiene nombre, descripción y esquema de parámetros. Ejemplos del servidor de filesystem: read_file, write_file, list_directory, search_files.

**Recursos (resources).** Son datos que el servidor expone y que el cliente puede leer. A diferencia de las herramientas, los recursos no son acciones: son información. Por ejemplo, un servidor MCP que conecta con una base de datos podría exponer cada tabla como un recurso.

**Plantillas de prompt (prompts).** Son plantillas reutilizables que el usuario puede invocar. Por ejemplo, un servidor MCP de una wiki podría exponer una plantilla "resumir página" que el usuario elige desde el cliente y el servidor rellena con el contenido.

Es importante no quedarse solo con las herramientas: un servidor MCP bien diseñado puede exponer las tres primitivas. El servidor de filesystem, por su naturaleza, expone principalmente herramientas.

## Primitivas del cliente

Del lado del cliente también existen primitivas:

**Roots.** Son los directorios autorizados que el cliente le comunica al servidor. Le indican al servidor el alcance del sistema de archivos al que puede acceder. Esto es lo que limita al servidor de filesystem a un directorio específico.

**Elicitation.** Es el mecanismo por el cual el servidor puede solicitar información adicional al usuario a través del cliente. Por ejemplo, si una herramienta necesita una confirmación o un dato extra, el servidor puede pedirlo sin que el modelo tenga que adivinarlo.

## Transportes

MCP define dos transportes principales para la comunicación entre cliente y servidor:

**stdio.** El servidor corre como un proceso hijo del cliente y la comunicación ocurre por entrada/salida estándar. Es el transporte que se usa para servidores locales. En mi caso, el servidor de filesystem corre por stdio: Claude Desktop lanza el proceso y se comunica con él por stdin/stdout.

**Streamable HTTP.** El servidor corre de forma remota y la comunicación ocurre por HTTP. Se usa cuando el servidor está en otra máquina o cuando varios clientes necesitan conectarse al mismo servidor.

## Versión de la especificación consultada

La especificación que consulté es la versión 2025-06-18, publicada por el equipo de Model Context Protocol. La especificación se actualiza con frecuencia, así que este documento debe leerse como una descripción de esa versión específica.

## Referencias

Model Context Protocol. (2025). Specification (Version 2025-06-18). https://modelcontextprotocol.io/specification/2025-06-18

Model Context Protocol. (2025). Architecture. https://modelcontextprotocol.io/docs/concepts/architecture
