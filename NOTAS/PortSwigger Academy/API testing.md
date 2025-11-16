Para empezar a hacer un test de API, debemos extraer la mayor cantidad de información del API posible para descubrir su superficie de ataque.
Para empezar, debemos identificar los endpoints de la API (ubicaciones donde la API recibe peticiones sobre un recurso concreto en su servidor).

**Ejemplo**
`GET /api/books HTTP/1.1 
`Host: example.com
En este caso el endpoint es `/api/books` 

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