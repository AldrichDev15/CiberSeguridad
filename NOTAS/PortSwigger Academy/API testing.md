Para empezar a hacer un test de API, debemos extraer la mayor cantidad de información del API posible para descubrir su superficie de ataque.
Para empezar, debemos identificar los endpoints de la API (ubicaciones donde la API recibe peticiones sobre un recurso concreto en su servidor).

---

**Ejemplo**
`GET /api/books HTTP/1.1 
`Host: example.com
En este caso el endpoint es `/api/books` 

---
Una vez identificados, debemos determinar cómo interactuar con ellos. Esto nos permitirá construir peticiones HTTP válidas para la API.
Debemos encontrar información sobre:
- Los datos de entrada que procesa la API, tanto parámetros obligatorios como opcionales.
- Los tipos de petición que acepta la API, incluyendo los métodos HTTP soportados y los formatos multimedia compatibles.
- Límites de acceso por políticas y mecanismos de autenticación.

La API suele estar documentada ya sea de forma legible para nosotros o para las máquinas (esta última forma está pensada para que sea procesada por software para la automatización de tareas como integraciones con la API o validaciones y está escrita en JSON o XML).
Normalmente esta documentación está abierta al público, y siempre deberíamos empezar nuestro análisis por ahí.
Si esta documentación no es pública, es posible que podamos acceder navegando por aplicaciones que usen la API. Para ello puedes usar <u>Burp Scanner</u>, <u>OpenAPI Parser BApp</u>, <u>Postman</u> o <u>SoapUI</u> para rastrear la API.
Si identificas un endpoint para un recurso, asegúrate de investigar la ruta base.
También puedes usar <u>Intruder</u> para encontrar una lista de rutas comunes para encontrar documentación.

Navegar las apps que usan la API es muy útil aunque poseas la documentación de la misma. También es útil identificar y estudiar ficheros JavaScript que puedan tener referencias a endpoints de la API. una vez identificados los endpoints interactuamos con ellos usando <u>Burp Repeater</u> o <u>Burp Intruder</u>. También estudiamos los mensajes de error y las respuestas para sacar información que nos permita construir peticiones HTTP válidas.

Un endpoint puede soportar varios métodos HTTP, con lo cual, es importante probar con todos los métodos que pueden potencialmente funcionar. Esto se traduce en descubrir nueva superficie de ataque. Puedes utilizar la lista integrada HTTP verbs en Burp Intruder para automáticamente cambiar de métodos.
**Nota**: Cuando pruebes métodos HTTP, ve a por objetos de baja prioridad. Esto evitará consecuencias no deseadas como alterar objetos críticos o crear excesivos logs.

Identifica también tipos de contenido soportados. Cambiar el tipo de contenido te puede permitir provocar errores que muestran información útil, traspasar defensas y aprovecharte de diferencias en la lógica de procesamiento (una API puede ser segura al manejar datos JSON, pero vulnerable a ataques de inyección al trabajar con XML).
Para ello, cambiamos la cabecera `Content-Type` (podemos usar <u>Content type converter BApp</u>)

---

**LAB: Finding and exploiting an unused API endpoint**
Hay que ver los métodos que acepta la API en `/api/products/X/price`, en este caso vemos con el <u>Burp Suite Repeater</u> (sirve para ver respuestas y editar peticiones) que acepta PATCH.

El método HTTP PUT únicamente permite reemplazar completamente un documento. A diferencia de `PUT`, el método `PATCH` no es idempotente, esto quiere decir que peticiones idénticas sucesivas pueden tener efectos diferentes. Sin embargo, es posible emitir peticiones `PATCH` de tal forma que sean idempotentes.

A esta petición le añadimos la cabecera que pide para funcionar (`Content-Type: application/json`) y le ponemos el objeto `{"price":0}`.

---

Una vez identificados algunos endpoints iniciales, puedes usar <u>Intruder</u>. Por ejemplo, si has encontrado un endpoint para actualizar la información de un usuario (`PUT /api/user/update`) puedes añadir un payload en la posición del `/update` con una lista de funciones comunes, como `delete` o `add` para ver si hay recursos extra.
Cuando busques endpoints, utiliza 3ici9hsydfl5n12t7phrinokfwk8togzlistas de palabras basadas en convenciones de nomenclatura para APIs y términos del sector o que sean relevantes para la aplicación basándote en tu reconocimiento inicial.

Cuando hagas reconocimiento de APIs puedes encontrarte con parámetros no documentados que la API soporta, y puedes intentar utilizarlos para cambiar el comportamiento de la aplicación.
Para identificar estos parámetros puedes usar <u>Burp Intruder</u>, <u>Param miner BApp</u> o la <u>herramienta de descubrimiento de contenido</u>

La **asignación masiva** (o enlace automático, mass alignment en inglés), es una vulnerabilidad que ocurre cuando los frameworks vinculan automáticamente parámetros de solicitudes a campos de objetos internos. Esto resulta en que la aplicación admita parámetros que no estaban pensados por el desarrollador.
Como la asignación masiva crea parámetros a partir de campos de objetos, normalmente podemos identificar dichos parámetros al examinar los objetos devueltos por la API.

A considerar cuando diseñemos APIs:
- Asegura tu documentación si no la vas a hacer pública.
- Asegúrate de que la documentación está actualizada.
- Aplica una lista de métodos permitidos.
- Valida que el tipo de contenido es el esperado en cada petición/respuesta
- Utiliza mensajes de error genéricos para evitar dar información.
- Utiliza medidas de protección en todas las versiones de tu API, no solo la más reciente.

Algunos sistemas tienen APIs internas que no son accesibles por Internet. La **contaminación de parámetros del servidor (o contaminación de parámetros HTTP)** ocurre cuando una web incluye la entrada del usuario en una petición del servidor a una API interna sin la codificación adecuada. Un atacante puede inyectar/manipular parámetros, lo que permitiría sobrescribir parámetros existentes, modificar el comportamiento de la aplicación o acceder a datos no autorizados.
Puedes usar todo tipo de input para buscar cualquier tipo de contaminación de parámetros: campos de formulario, parámetros de URLs... todo es susceptible de ser vulnerable.
Para los parámetros en la query string de una URL, ponemos caracteres sintácticos como `#`(indica el inicio de una sección dentro de la página), `&`(separa cadenas clave-valor) y `=`(indica el inicio del valor de una clave).  Por ejemplo, puedes usar el carácter `#` codificado en URL (%23) para intentar truncar la petición del servidor a la API interna. Es esencial que esté codificado en URL, porque sino se interpreta como parte del fragmento identificador.
Revisa las respuestas para encontrar pistas sobre las modificaciones de la query y como afecta esto a los demás parámetros de la query string o si se ha conseguido inyectar algún parámetro válido de los que hemos hablado anteriormente. 
También puedes intentar sobrescribir parámetros poniendo uno adicional en la query string con el mismo nombre. El comportamiento varía según la tecnología utilizada. PHP analiza solo el segundo parámetro, ASP.NET combina ambos valores separados por una coma y Node.js analiza solo el primer parámetro.

Las APIs RESTful pueden poner los parámetros y valores en la ruta URL en lugar de en la query string.
Para explotar esto, podemos añadir secuencias de recorrido de rutas para ver cómo responde la aplicación.

Un atacante podría manipular parámetros para aprovechar vulnerabilidades en el procesamiento de otros formatos de datos estructurados como JSON o XML por parte del servidor. Para comprobarlo, introducir datos estructurados inesperados en las entradas del usuario y ver cómo responde la aplicación.
Estas inyecciones de formato estructurado también pueden producirse en las respuestas. Por ejemplo, si la entrada de usuario se almacena de forma segura en una BD y luego se inserta en una respuesta JSON de una API de backend sin la codificación adecuada. Generalmente, la inyección de formato estructurado en las respuestas se puede detectar y explotar del mismo modo que en las solicitudes.
Para estos ataques puedes usar <u>Burp Scanner</u> o <u>Backslash Powered Scanner BApp</u>.
Para prevenir estas vulnerabilidades, utiliza una lista blanca para definir caracteres que no necesiten codificación y asegúrate de que el resto de la entrada del usuario está codificada antes de incluirla en una solicitud al servidor. Además, asegurarse de verificar que la entrada cumpla con el formato y la estructura esperada.