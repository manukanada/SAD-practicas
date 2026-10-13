# Práctica 1 de SAD · SecureCorp — Respuestas

**Nombre y apellidos:Manuel Cañada Castro**
**Usuario:mcanada**

Responde con tus palabras, en 1-3 líneas. En la defensa te preguntaré lo mismo en voz alta.

**Contraseñas que has usado** (solo porque es un laboratorio; en una empresa, jamás en un fichero):

- Tu usuario: manuel2005
- mtorres: marta2026
- lromero: Lucia2026
- contraseña kerberos master key: manuel
---

**1. (A1)** ¿Quién es el `issuer` de tu `ca.crt`? ¿Hasta qué fecha es válido? ¿Por qué el `subject`
y el `issuer` de la CA son iguales y los de `ldap.crt` no?
El issuer es Manuel Root CA - Manuel Canada. Es válido hasta el 5 de Octubre de 2036.
El subject y el issuer son el mismo porque el certificado ha sido firmado por mi mismo, en cambio,
en ldap.crt el subject es el servidor LDAP y el issuer es la CA que ha creado y firmado el
certificado, es decir, yo.
**2. (A3)** Pega el comando y el resultado de tus dos búsquedas:

```
a) miembros de rrhh:
ldapsearch -x -LLL -H ldap://ldap.securecorp.local \
  -b "ou=groups,dc=securecorp,dc=local" \
  "(cn=rrhh)" member
dn: cn=rrhh,ou=groups,dc=securecorp,dc=local
member: uid=lromero,ou=people,dc=securecorp,dc=local
member: uid=mtorres,ou=people,dc=securecorp,dc=local


b) cn y mail de todas las personas:
ldapsearch -x -LLL -H ldap://ldap.securecorp.local \
  -b "ou=people,dc=securecorp,dc=local" \
  "(objectClass=inetOrgPerson)" cn mail
dn: uid=lromero,ou=people,dc=securecorp,dc=local
cn: Lucia Romero
mail: lromero@securecorp.local

dn: uid=mcanada,ou=people,dc=securecorp,dc=local
cn: Manuel Canada
mail: manuel@securecorp.local

dn: uid=mtorres,ou=people,dc=securecorp,dc=local
cn: Marta Torres
mail: mtorres@securecorp.local


```

**3. (A4)** ¿Por qué la clave `ldap.key` tiene que ser de `openldap` y tener permisos 600?
Porque es la clave privada del servidor LDAP y contiene información sensible. La clave es privada 
por lo que necesita permisos 600 para que solo el owner pueda leer y escribir en el archivo.

**4. (A4)** ¿Qué valor has puesto en `SLAPD_SERVICES` y por qué?
He puesto SLAPD_SERVICES="ldaps:/// ldapi:///". Así LDAP funciona mediante LDAPS en el puerto
636 y se mantiene ldapi:/// para la administración local.No usamos el puerto 389 o "ldap:///"
porque las conexiones serían sin cifrar.

**5. (A4)** Antes de añadir `TLS_CACERT` en el cliente, `ldaps://` no funcionaba. ¿Por qué?
Porque el cliente no conocía ni confiaba en la Autoridad Certificadora que había firmado el certificado 
de LDAP. Al añadir TLS_CACERT, el cliente puede verificar el certificado y establecer la conexión 
segura.

**6. (B3)** Pega la salida de `klist` con tus dos tickets. ¿Para qué sirve cada uno? ¿Ha viajado tu
contraseña por la red?

```
Ticket cache: FILE:/tmp/krb5cc_0
Default principal: mcanada@SECURECORP.LOCAL

Valid starting     Expires            Service principal
10/13/26 08:53:39  10/13/26 18:53:39  krbtgt/SECURECORP.LOCAL@SECURECORP.LOCAL
	renew until 10/20/26 08:53:39
10/13/26 08:55:21  10/13/26 18:53:39  host/web.securecorp.local@SECURECORP.LOCAL
	renew until 10/20/26 08:53:39

El primer ticket te lo da el propio servidor Kerberos cuando te autenticas de manera correcta, 
este te permite solicitar tickets para otros servicios (sin autenticarte de nuevo)
El segundo ticket es un ticket de servicio en este caso para web.securecorp.local.
La contraseña no viaja por la red, solo se utiliza para el proceso de autenticación por Kerberos.
```

**7. (C)** En el `docker-compose.yml`, ¿qué diferencia hay entre `build:` e `image:`? ¿Qué
significa la línea `- "8081:80"` del servicio `phpldapadmin`?
La build es el directorio donde está el dockerfile, lo que permite construir una imagen e image 
basicamente indica que Docker se va a utilizar. La línea "8081:80" indica que php está en el puerto
80 y el servicio es accesible desde el navegador usando el puerto 8081.

**8. (C)** ¿Por qué en la máquina `web` no has tenido que escribir a mano `TLS_CACERT`, y en el
cliente sí? ¿Qué pasaría con esa línea del cliente si hicieras `./lab.sh reset`?
En web no lo he tenido que escribir porque ya está configurado en el propio Dockerfile, en cambio 
con el cliente hay que hacerlo de 0.
Si ejecuto ./lab.sh reset los contenedores se eliminan y se crean de nuevo por lo que la
configuración se pierde. 
