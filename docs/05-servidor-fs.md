# El servidor de sistema de archivos

## Aclaración importante

El servidor de sistema de archivos, o "FS", no es parte del protocolo MCP. Es uno de los servidores de referencia que Anthropic publicó como ejemplo, entre muchos otros posibles. MCP define cómo se comunican cliente y servidor; qué herramientas expone cada servidor depende de quién lo programe.

Existen otros servidores de referencia oficiales, como el de GitHub, el de Git, el de Slack, el de Google Drive, el de bases de datos SQLite, etc. Cada uno expone herramientas distintas según el recurso que envuelven.

## Herramientas que expone

El servidor de filesystem expone las siguientes herramientas (entre otras):

- list_directory: lista el contenido de un directorio.
- list_directory_with_sizes: igual que la anterior pero incluye el tamaño de cada archivo.
- read_file: lee el contenido de un archivo.
- read_text_file: lee un archivo de texto (variante especializada).
- read_media_file: lee un archivo de imagen o audio.
- read_multiple_files: lee varios archivos a la vez.
- write_file: escribe contenido en un archivo (lo sobrescribe si existe).
- edit_file: modifica un archivo existente con un conjunto de cambios.
- create_directory: crea un directorio.
- move_file: mueve o renombra un archivo.
- search_files: busca archivos por nombre o patrón.
- get_file_info: devuelve metadatos de un archivo.
- directory_tree: devuelve la estructura completa del directorio como árbol.

Es importante notar que el servidor de filesystem no tiene una herramienta de búsqueda por contenido. Solo busca por nombre o patrón. Si el usuario pide buscar por contenido, el modelo tiene que leer los archivos y buscarlos manualmente, como pasó en mi operación de búsqueda.

## Alcance mediante directorios permitidos

El servidor de filesystem se limita a un conjunto de directorios que se declaran al configurarlo. En mi caso, solo tiene acceso a C:\mcp-tarea\workspace. Cualquier ruta fuera de ese directorio es rechazada, como se puede ver en la prueba del límite de seguridad.

Esta delimitación se hace en el momento de configurar el servidor. En mi manifiesto de extensión, el directorio permitido se pasa como argumento:

"args": [
  "C:\\mcp-tarea\\workspace"
]

## Por qué existe ese límite

Sin ese límite, el servidor de filesystem tendría acceso a todo el disco: documentos personales, archivos del sistema, credenciales guardadas en el navegador, configuraciones, etc. Un modelo con ese nivel de acceso podría, en principio, leer cualquier archivo o modificar cualquier cosa.

Además, el límite protege contra inyección de instrucciones. Si el modelo lee un archivo que contiene texto malicioso y ese archivo está dentro del directorio autorizado, el daño está contenido. Si el modelo tuviera acceso a todo el disco, un atacante podría inyectar instrucciones para que el modelo borrara archivos críticos del sistema.

El límite es la mitigación más importante del servidor, y por eso no se debe usar la raíz del disco ni la carpeta de usuario completa como directorio autorizado.

## Referencias

Model Context Protocol. (2025). Filesystem MCP Server. GitHub. https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem
