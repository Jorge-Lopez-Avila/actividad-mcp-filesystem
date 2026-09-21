# MCP frente a una API

## ¿Qué es una API?

Una API (Application Programming Interface) es un contrato entre dos programas. Define un conjunto de endpoints a los que un cliente puede enviar peticiones, qué parámetros acepta cada uno, y qué formato tiene la respuesta.

El flujo típico es este: el desarrollador lee la documentación de la API, elige el endpoint que necesita, arma la petición HTTP con el método correcto, los headers y el cuerpo, la envía, y escribe el código que interpreta la respuesta. Toda esa lógica —qué se llama, cuándo, con qué parámetros— está escrita de antemano en el código del cliente.

Esto tiene una consecuencia importante: el cliente está acoplado al servicio. Si la API cambia, hay que actualizar el código. Si quieres agregar una capacidad nueva, hay que modificar el cliente y volver a desplegarlo.

## ¿Qué es MCP?

MCP (Model Context Protocol) es un protocolo abierto, basado en JSON-RPC 2.0, que permite a un modelo de lenguaje descubrir y usar herramientas expuestas por un servidor. Fue publicado por Anthropic en noviembre de 2024 y ha sido adoptado por múltiples proveedores (OpenAI, Google, etc.), por lo que no es propietario de una sola empresa.

La diferencia clave con una API es el descubrimiento dinámico. En MCP, el servidor publica un catálogo de herramientas. Cada herramienta tiene:

- Un nombre (por ejemplo, read_file)
- Una descripción en lenguaje natural (por ejemplo, "lee el contenido de un archivo")
- Un esquema de parámetros (por ejemplo, que read_file recibe una ruta de archivo como string)

El modelo lee ese catálogo en tiempo de ejecución y decide cuál herramienta invocar según lo que el usuario pidió. No hay endpoints hardcodeados en el cliente: el cliente descubre lo que el servidor ofrece en el momento en que se conecta.

Además de las herramientas, MCP define otras dos primitivas del lado del servidor: los recursos (datos que el servidor expone, como archivos o registros) y las plantillas de prompt (plantillas reutilizables que el usuario puede invocar). Del lado del cliente existen los roots (directorios autorizados) y la elicitation (solicitar información adicional al usuario).

## Tabla comparativa

| Criterio | API tradicional | MCP |
|---|---|---|
| Quién decide qué se invoca | El desarrollador, en el código | El modelo, en tiempo de ejecución |
| Descubrimiento de capacidades | Leyendo documentación, endpoints fijos | Catálogo dinámico que el modelo consulta |
| Acoplamiento cliente-servicio | Alto: cada endpoint está escrito de antemano | Bajo: el cliente descubre las herramientas |
| Formato de mensajes | Varía (REST, JSON, gRPC, SOAP) | JSON-RPC 2.0 estandarizado |
| Autenticación y consentimiento | Definida por el desarrollador | Gestionada por el host MCP |
| Reutilización entre aplicaciones | Baja: cada app integra su propia API | Alta: un servidor sirve a varios clientes |
| Rol del servidor | Expone endpoints | Expone herramientas, recursos y plantillas de prompt |

## MCP no sustituye a las APIs

Es importante aclarar esto porque es un error frecuente: MCP no reemplaza a las APIs ni las vuelve obsoletas. Un servidor MCP casi siempre envuelve una API o un recurso que ya existe.

Por ejemplo, el servidor MCP de GitHub no reinventa la API de GitHub. La envuelve: internamente hace peticiones HTTP a los endpoints de GitHub, pero expone esas capacidades como herramientas descubribles por un modelo. Lo mismo pasa con servidores MCP de Slack, Notion, bases de datos, sistemas de archivos, etc.

MCP es una capa por encima que hace que las capacidades ya existentes sean utilizables por un modelo de lenguaje sin que el desarrollador tenga que escribir código específico para cada integración. Las APIs siguen ahí, debajo. Sin ellas, la mayoría de los servidores MCP no tendrían nada que exponer.

## Referencias

Model Context Protocol. (2025). Specification (Version 2025-06-18). https://modelcontextprotocol.io/specification/2025-06-18

Anthropic. (2024). Introducing the Model Context Protocol. https://www.anthropic.com/news/model-context-protocol
