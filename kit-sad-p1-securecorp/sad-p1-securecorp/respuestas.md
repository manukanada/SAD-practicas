# Práctica 1 de SAD · SecureCorp — Respuestas

**Nombre y apellidos:Manuel Cañada Castro**
**Usuario:mcanada**

Responde con tus palabras, en 1-3 líneas. En la defensa te preguntaré lo mismo en voz alta.

**Contraseñas que has usado** (solo porque es un laboratorio; en una empresa, jamás en un fichero):

- Tu usuario: manuel2005
- mtorres: marta2026

---

**1. (A1)** ¿Quién es el `issuer` de tu `ca.crt`? ¿Hasta qué fecha es válido? ¿Por qué el `subject`
y el `issuer` de la CA son iguales y los de `ldap.crt` no?
El issuer es Manuel Root CA - Manuel Canada. Es válido hasta el 5 de Octubre de 2036.
El subject y el issuer son el mismo el certificado ha sido firmado por mi mismo, en cambio, en ldap.crt
el subject es el servidor LDAP y el issuer es la CA que ha creado y firmado el certificado, es decir, yo.
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
Porque es la clave privada del servidor LDAP y contiene información sensible. El usuario openldap
necesita permisos 600 para que solo el owner pueda leer y escribir en el archivo.

**4. (A4)** ¿Qué valor has puesto en `SLAPD_SERVICES` y por qué?
He puesto SLAPD_SERVICES="ldaps:/// ldapi:///". Así LDAP funciona mediante LDAPS en el puerto 636 y se
mantiene ldapi:/// para la administración local.No usamos el puerto 389 porque las conexiones serían
sin cifrar.

**5. (A4)** Antes de añadir `TLS_CACERT` en el cliente, `ldaps://` no funcionaba. ¿Por qué?
Porque el cliente no conocía ni confiaba en la Autoridad Certificadora que había firmado el certificado 
de LDAP. Al añadir TLS_CACERT, el cliente puede verificar el certificado y establecer la conexión 
segura.

**6. (B3)** Pega la salida de `klist` con tus dos tickets. ¿Para qué sirve cada uno? ¿Ha viajado tu
contraseña por la red?

```

```

**7. (C)** En el `docker-compose.yml`, ¿qué diferencia hay entre `build:` e `image:`? ¿Qué
significa la línea `- "8081:80"` del servicio `phpldapadmin`?


**8. (C)** ¿Por qué en la máquina `web` no has tenido que escribir a mano `TLS_CACERT`, y en el
cliente sí? ¿Qué pasaría con esa línea del cliente si hicieras `./lab.sh reset`?

