# Datos sensibles expuestos
Siempre que tengamos que evaluar una aplicación web en busca de problemas de seguridad, una de las primeras cosas que debemos hacer es revisar el código fuente de la página para ver si podemos encontrar credenciales de acceso expuestas o enlaces ocultos.
# Inyección HTML
La inyección HTML es una vulnerabilidad que ocurre cuando la entrada de un usuario no filtrada se muestra directamente en una página web. Si un sitio web no sanitiza la entrada del usuario, y esa entrada se usa en la página, un atacante puede inyectar código HTML en un sitio vulnerable.

La sanitización de entradas es fundamental para mantener un sitio seguro, ya que la información que un usuario introduce en un sitio web a menudo se utiliza en otras funcionalidades tanto del frontend como del backend.

Cuando un usuario controla cómo se muestra su entrada, puede enviar código HTML o JavaScript, y el navegador lo procesará en la página, permitiendo al usuario modificar la apariencia y funcionalidad de la misma. Esto puede incluir cambios visuales, la inserción de enlaces maliciosos, scripts que roban información de otros usuarios o incluso redirecciones no deseadas.