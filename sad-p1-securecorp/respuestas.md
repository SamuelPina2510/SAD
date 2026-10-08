# Práctica 1 de SAD · SecureCorp — Respuestas

**Nombre y apellidos:** Samuel Pina Sánchez
**Usuario:** samuelpina2510

Responde con tus palabras, en 1-3 líneas. En la defensa te preguntaré lo mismo en voz alta.

**Contraseñas que has usado** (solo porque es un laboratorio; en una empresa, jamás en un fichero):

- Tu usuario: Samuel2026
- mtorres: Marta2026

---

**1. (A1)** ¿Quién es el `issuer` de tu `ca.crt`? ¿Hasta qué fecha es válido? ¿Por qué el `subject`
y el `issuer` de la CA son iguales y los de `ldap.crt` no?

El issuer es CN=SecureCorp Root CA - Tu Nombre Apellido y es válido hasta la fecha indicada en Not After. 
En la CA coinciden porque se autofirma a sí misma; en ldap.crt difieren porque está firmado por la CA.

**2. (A3)** Pega el comando y el resultado de tus dos búsquedas:

```
a) miembros de rrhh:

Comando: ldapsearch -x -LLL -H ldap://ldap.securecorp.local -b "ou=groups,dc=securecorp,dc=local" "(cn=rrhh)" member
Resultado:
dn: cn=rrhh,ou=groups,dc=securecorp,dc=local
member: uid=lromero,ou=people,dc=securecorp,dc=local
member: uid=mtorres,ou=people,dc=securecorp,dc=local

b) cn y mail de todas las personas:

Comando: ldapsearch -x -LLL -H ldap://ldap.securecorp.local -b "ou=people,dc=securecorp,dc=local" "(objectClass=inetOrgPerson)" cn mail
Resultado:
dn: uid=lromero,ou=people,dc=securecorp,dc=local
cn: Lucia Romero
mail: lromero@securecorp.local

dn: uid=mtorres,ou=people,dc=securecorp,dc=local
cn: Marta Torres
mail: mtorres@securecorp.local

dn: uid=TUUSUARIO,ou=people,dc=securecorp,dc=local
cn: Tu Nombre Apellido
mail: TUUSUARIO@securecorp.local

```

**3. (A4)** ¿Por qué la clave `ldap.key` tiene que ser de `openldap` y tener permisos 600?

Porque slapd ejecuta como openldap y necesita leerla, mientras que los permisos 600 impiden que otros usuarios la lean.

**4. (A4)** ¿Qué valor has puesto en `SLAPD_SERVICES` y por qué?

SLAPD_SERVICES="ldaps:/// ldapi:///" para forzar conexiones cifradas por red y dejar el acceso local sin cifrar por socket.

**5. (A4)** Antes de añadir `TLS_CACERT` en el cliente, `ldaps://` no funcionaba. ¿Por qué?

Porque el cliente no tenía la CA para verificar la identidad del servidor ni validar su certificado TLS.

**6. (B3)** Pega la salida de `klist` con tus dos tickets. ¿Para qué sirve cada uno? ¿Ha viajado tu
contraseña por la red?

```
Ticket grant: krbtgt/SECURECORP.LOCAL@SECURECORP.LOCAL
Service ticket: host/web.securecorp.local@SECURECORP.LOCAL

```

El krbtgt sirve para pedir otros tickets y el de host/web para acceder al servicio web. La contraseña no viaja por la red.


**7. (C)** En el `docker-compose.yml`, ¿qué diferencia hay entre `build:` e `image:`? ¿Qué
significa la línea `- "8081:80"` del servicio `phpldapadmin`?

build: construye la imagen desde un Dockerfile local e image: la descarga ya hecha de Docker Hub. "8081:80" mapea el puerto 8081 del host al 80 del contenedor.

**8. (C)** ¿Por qué en la máquina `web` no has tenido que escribir a mano `TLS_CACERT`, y en el
cliente sí? ¿Qué pasaría con esa línea del cliente si hicieras `./lab.sh reset`?

En web se incluyó al construir la imagen con el Dockerfile. En el cliente se hizo manualmente, por lo que con ./lab.sh reset se perdería.
