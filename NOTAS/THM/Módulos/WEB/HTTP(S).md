Conjunto de reglas usadas para comunicarte con servidores web para la transmisión de datos de la página web, ya sea HTML, imágenes, videos,...
Si utilizamos HTTPS, los datos estarán encriptados, lo que impide que se vea la información que enviamos y recibimos y nos aseguramos que estamos tratando con el servidor web adecuado y no otro haciéndose pasar por él.
# Métodos
Indican la acción a realizar para un recurso determinado.
- **GET**: Obtener información de un servidor web.
- **POST**: Añadir información a un servidor web.
- **PUT**: Actualizar información.
- **DELETE**: Borrar información.
# Códigos de estado
La primera línea de la respuesta HTTP siempre contiene un estado informando del resultado de la petición:
- **100-199**: Para indicar al cliente que la primera parte de su petición se ha aceptado y que deberían continuar mandando el resto de la petición.
- **200-299**: Petición aceptada.
	- **200**: OK.
	- **201**: Created.
- **300-399**: Usado para redireccionar a otro recurso.
	- **301**: Moved permanently - Movido permanentemente a otra URL.
	- **302**: Found - Temporalmente en otra URL.
- **400-499**: Errores en la petición del cliente.
	- **400**: Bad request - Algo falla o falta en la petición.
	- **401**: Not authorized - Tienes que ser autorizado (normalmente con usuario y contraseña).
	- **403**: Forbidden - Sin permisos estés o no logueado.
	- **404**: Page not found.
- **500-599**: Errores en el servidor.
	- **500**: Internal server error.
	- **503**: Service unavailable.
# Cabeceras HTTP
Cabeceras en la solicitud:
- **Host**: Web que se visita.
- **User-Agent**: Software del navegador y versión. Ayuda a dar formato a las páginas web específicamente para el software de navegación, mostrar distintas versiones para móvil o escritorio...
- **Content-Length**: Al enviar datos a un servidor web, como en un formulario, le indica al servidor cuánto contenido debe esperar en la petición. Así el servidor puede asegurarse de que no falta ningún dato.
- **Accept-Encoding**: Le dice al servidor web los métodos de compresión que acepta para que los datos viajen de forma óptima.
- **Cookie**: Datos enviados para que el servidor pueda recordar nuestra información.
Cabeceras de respuesta:
- **Set-Cookie**: Información a almacenar que regresa en cada petición al servidor web.
- **Cache-Control**: Tiempo que almacenar los contenidos de la respuesta en la caché antes de tener que solicitarlos de nuevo.
- **Content-Type**: Tipo de dato devuelto.
- **Content-Encoding**: Método usado para comprimir los datos.
# Cookies
Pequeñas porciones de datos almacenadas en el ordenador. Se guardan al recibir un "Set-Cookie" de parte de un servidor web. Después, por cada petición que hagas, se manda esta cookie de vuelta al servidor. Como HTTP es stateless (no guarda información de tus peticiones previas), las cookies se usan para recordar al servidor quién eres, ajustes personales de la página o si has visitado o no la página.
Se utilizan mucho en la autenticación web. Se pueden ver utilizando las developer tools en la parte de Network.
