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
- **CNAME**: Resuelven otro nombre de dominio. Por ejemplo, 