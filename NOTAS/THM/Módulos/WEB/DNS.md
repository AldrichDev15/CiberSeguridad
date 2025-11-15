# Jerarquía de dominios
## Top-Level Domain (TLD)
Es la parte más a la derecha del dominio. Hay dos tipos de TLD:
- **gTLD** (Generic Top Level Domain): Genérico, históricamente se ha usado para indicar el propósito (.com comercio, .edu educación, .org organización...)
- **ccTLD** (Country Code Top Level Domain): Propósito geográfico
Lista de TLDs: https://data.iana.org/TLD/tlds-alpha-by-doma.txt
## Second-Level Domain (SLD)
 Limitado a 63 caracteres más el TLD y solo se pueden usar caracteres alfanuméricos y guiones cortos (no puede empezar ni acabar por uno o tener varios consecutivos).
## Subdomain
A la izquierda del todo y tiene las mismas restricciones que un SLD. Puedes usar múltiples subdomains separados por puntos y no hay límite de subdomains que puedes crear, pero el número de caracteres debe ser menos que 253.

# Entradas DNS
- **A**: Resuelven direcciones IPv4.
- **AAAA**: Resuelven direcciones IPv6.
- **CNAME**: Resuelven otro nombre de dominio, actúan como un alias. Por ejemplo, cuando alguien escribe "blog.ejemplo.com" en el navegador, el servidor DNS primario busca el registro CNAME de "blog.ejemplo.com" y encuentra que apunta a "ejemplo.com". Entonces, realiza otra búsqueda para obtener la IP de "ejemplo.com" y se conecta al sitio
- **MX**: Estos registros resuelven a las direcciones de los servidores de correo del dominio que consultas. Dirigen el correo electrónico a dichos servidores. Estos registros tienen una flag de prioridad que indica en que orden intentar establecer conexión con los servidores: a menor valor más prioridad.
- **TXT**: Campos de texto donde cualquier tipo de dato basado en texto puede ser almacenado. Algunos usos comunes son: verificación de propiedad del nombre de dominio al registrarse en servicios de terceros o listar servidores que tienen la autoridad para enviar mails en nombre de un dominio.
# Peticiones DNS
1. Al hacer una petición a un nombre de dominio, el ordenador comprueba si está en la memoria caché local. Si no lo está, se hace una petición a tu servidor DNS Recursivo.
2. Un servidor DNS Recursivo normalmente viene dado por tu ISP (Internet Service Provider), pero puedes elegir el tuyo propio. Este servidor también tiene su propia caché. Si la petición no se encuentra en la caché local, empieza a buscar, empezando por los servidores DNS raíz de Internet.
3. Estos servidores actúan como la espina dorsal de Internet y te redirigen a los servidores TLD correctos según tu petición.
4. Los servidores TLD mantienen registros de donde encontrar los servidores autoritativos (o nameserver) para resolver esa petición DNS. Normalmente encontrarás varios nameservers por si acaso alguno se cae.
5. Un servidor DNS autoritativo se encarga de almacenar los registros DNS de un dominio en particular y donde se realizan las actualizaciones de dichos registros. Según el tipo de registro, éste se envía al servidor DNS Recursivo, donde se almacena una copia en la caché para futuras solicitudes y luego se reenvía al cliente que realizó la solicitud original. Estos registros tienen un TTL en segundos que marca cuanto tiempo se almacenará este registro en la caché.

# NSLOOKUP
Es un programa para hacer query a servidores DNS. 
Tiene dos modos:
1. **Interactivo**: Permite preguntar a nameservers por información de varios hosts y dominios o listar los hosts de un dominio. Se activa si no se proporcionan argumentos o cuando el primer argumento es un guión y el segundo es el nombre de host o la IP de un nameserver. 
2. **No interactivo**: Muestra el nombre y la información de un host o dominio. Se activa al proporcionar la IP o el nombre de host a buscar como primer argumento. El segundo argumento (opcional) indica la dirección IP de un servidor de nombres.

