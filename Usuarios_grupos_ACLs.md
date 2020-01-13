# Usuarios, grupos y ACLs en LDAP

## 1. Creación de unidades organizativas

#### Vamos a creear 2 unidades organizativas, una para los grupos 'ou=Group' y otra para los usuarios 'ou=People'

###### Vamos a crear un fichero '.ldif' para las unidades organizativas, este lo vamos a crear en el directorio /home/debian/2asir

###### Indicamos las Unidades Organizativas en 'organizativas.ldif':

~~~
dn: ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: organizationalUnit
ou: Group

dn: ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: organizationalUnit
ou: People
~~~

###### Añadimos con el siguiente comando las unidades organizarivas del fichero 'organizativas.ldif'

~~~
ldapadd -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" -f organizativas.ldif -W
  Enter LDAP Password: 
  adding new entry "ou=Group,dc=amorales,dc=gonzalonazareno,dc=org"

  adding new entry "ou=People,dc=amorales,dc=gonzalonazareno,dc=org"
~~~

###### Opciones:

ldapadd   Para añadir entradas en el servidor LDAP
-x        Para indicar la autentificación básica
-D        Para indicar el Distinguished Name
-f        Para indicar el fichero que queremos leer
-W        Para que nos pida la contraseña

## 2. Creación de usuarios

#### Crea 10 usuarios con los nombres que prefieras en LDAP, dentro de una unidad organizativa. Esos usuarios deben ser objetos de los tipos posixAccount e inetOrgPerson. Estos usuarios tendrán un atributo userPassword.

###### Vamos a crear el fichero '.ldif' para los usuarios, estos los vamos a crear en el directorio /home/debian/2asir/personas.ldif

###### Vamos a explicar varias cosas antes de crear el fichero

> Para el campo de 'userpassword' utilizaremos el comando 'slappasswd -v' para cifrar la contraseña del usuario.

~~~
sudo slappasswd -v
  New password:
  Re-enter new password:
  {SSHA}ui8cTZ0jQBYbVPTZ1vkQ5DgepcalgN1N
~~~

> Si tenemos un 'CN', 'SN' o 'givenName' con tildes tendremos que codificarlo en base64 para que no haya problemas.

~~~
echo Paloma del Rocío Garcia | base64
  QWxlamFuZHJvIE1vcmFsZXMgR3JhY2lhCg==
~~~

###### Indicamos los usuarios en 'personas.ldif':
> NOTA: si queremos poner un dato en base64 tenemos que indicarlo con '::'

~~~
dn: uid=paloma,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
cn:: UGFsb21hIGRlbCBSb2PDrW8gR2FyY2lhCg==
uid: paloma
uidNumber: 2001
gidNumber: 2500
homeDirectory: /home/paloma
loginShell: /bin/bash
userPassword: {SSHA}dAttFmPZKk5MPhKl01kkr4WkckiODKd2
sn: garcia
mail: paloma@gmail.com
givenName: paloma

dn: uid=fernando,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
cn: Fernando Tirado Bulnes
uid: fernando
uidNumber: 2002
gidNumber: 2000
homeDirectory: /home/fernando
loginShell: /bin/bash
userPassword: {SSHA}7GXVQd3eGtCRzEpxagvBrwrhOSftpBSd
sn: tirado
mail: fernando@gmail.com
givenName: fernando

dn: uid=luis,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
cn: Luis Vazquez Alejo
uid: luis
uidNumber: 2003
gidNumber: 2000
homeDirectory: /home/luis
loginShell: /bin/bash
userPassword: {SSHA}tsHLMyjWhMktwOnTK9b/qlV51HvB5QaH
sn: vazquez
mail: luis@gmail.com
givenName: luis

dn: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
cn: Alejandro Rodrigez Rojas
uid: alejandro
uidNumber: 2004
gidNumber: 2000
homeDirectory: /home/alejandro
loginShell: /bin/bash
userPassword: {SSHA}m8mWukTNHxs6XQvAZqFR4G2p8b+Ft/4l
sn: rodriguez
mail: alejandro@gmail.com
givenName: alejandro

dn: uid=francisco,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
cn: Francisco Huzon Villar
uid: francisco
uidNumber: 2005
gidNumber: 2000
homeDirectory: /home/francisco
loginShell: /bin/bash
userPassword: {SSHA}6VIEubbMSbx9CX4+LIQ4TacOuLQ6T1AZ
sn: huzon
mail: francisco@gmail.com
givenName: francisco

dn: uid=alejandra,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
cn: Alejandra Morales Gracia
uid: alejandra
uidNumber: 2006
gidNumber: 2000
homeDirectory: /home/alejandra
loginShell: /bin/bash
userPassword: {SSHA}HhVyF1xBo7V9EzdMKPUHHS0yjjTHNVly
sn: morales
mail: alejandra@gmail.com
givenName: alejandra

dn: uid=juanma,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
cn: Juanma Alcoba Dominguez
uid: juanma
uidNumber: 2007
gidNumber: 2000
homeDirectory: /home/juanma
loginShell: /bin/bash
userPassword: {SSHA}yiqcBSUlvBgqKgquVejJB1y0v9UqBZpN
sn: alcoba
mail: juanma@gmail.com
givenName: juanma

dn: uid=adrian,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
cn: Adrian Jaramillo Ramirez
uid: adrian
uidNumber: 2008
gidNumber: 2000
homeDirectory: /home/adrian
loginShell: /bin/bash
userPassword: {SSHA}4OeBiuXmvIKSPCU4EgyI/roLkMKFH+D0
sn: jaramillo
mail: adrian@gmail.com
givenName: adrian

dn: uid=jose,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
cn: Jose Garcia Tirado
uid: jose
uidNumber: 2009
gidNumber: 2000
homeDirectory: /home/jose
loginShell: /bin/bash
userPassword: {SSHA}SDncXCitT8WWTQ7oVww4MeynVpQ+ChDR
sn: garcia
mail: jose@gmail.com
givenName: jose

dn: uid=linda,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
cn: Linda Campon Morales
uid: linda
uidNumber: 2010
gidNumber: 2000
homeDirectory: /home/linda
loginShell: /bin/bash
userPassword: {SSHA}hVkrj//XbIR9BzXSrF38i1tac5o4icJ7
sn: campon
mail: linda@gmail.com
givenName: linda
~~~

###### Ahora introducimos los datos del fichero personas.ldif con el siguiente comando:

~~~
ldapadd -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" -f personas.ldif -W
~~~

## 3. Creación de grupos

#### Crea 3 grupos en LDAP, dentro de una unidad organizativa, que sean objetos del tipo groupOfNames. Estos grupos serán: comercial, almacen y admin

###### Vamos a crear un fichero '.ldif' para los grupos, este lo vamos a crear en el directorio /home/debian/2asir

#### Indicamos los Grupos en 'grupos.ldif':

~~~
dn: cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: groupOfNames
cn: comercial
member: 

dn: cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: groupOfNames
cn: almacen
member: 

dn: cn=admin,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: groupOfNames
cn: admin
member: 
~~~

###### Añadimos con el siguiente comando los grupos del fichero 'grupos.ldif'

~~~
ldapadd -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" -f grupos.ldif -W
  Enter LDAP Password: 
  adding new entry "cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org"

  adding new entry "cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org"

  adding new entry "cn=admin,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org"
~~~

## 4. Añadiendo usuarios

#### Añade usuarios que pertenezcan a:

###### Para realizar modificaciones y añadir los usuarios a los grupos correspondientes, tenemos que crear un fichero de modificación e indicarle que queremos modificar:

* Solo al grupo comercial

~~~
dn: cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
replace: member
member: uid=paloma,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
~~~

* Solo al grupo almacen

~~~
dn: cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
replace: member
member: uid=fernando,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
~~~

* Al grupo comercial y almacen

~~~
dn: cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
add: member
member: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org

dn: cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
add: member
member: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
~~~

* Al grupo admin y comercial

~~~
dn: cn=admin,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
replace: member
member: uid=francisco,ou=People,dc=amorales,dc=gonzalonazareno,dc=org

dn: cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
add: member
member: uid=francisco,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
~~~

* Solo al grupo admin


~~~
dn: cn=admin,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
add: member
member: uid=juanma,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
~~~

###### El fichero completo quedaría así:

~~~
dn: cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
replace: member
member: uid=paloma,ou=People,dc=amorales,dc=gonzalonazareno,dc=org

dn: cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
replace: member
member: uid=fernando,ou=People,dc=amorales,dc=gonzalonazareno,dc=org

dn: cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
add: member
member: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org

dn: cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
add: member
member: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org

dn: cn=admin,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
replace: member
member: uid=francisco,ou=People,dc=amorales,dc=gonzalonazareno,dc=org

dn: cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
add: member
member: uid=francisco,ou=People,dc=amorales,dc=gonzalonazareno,dc=org

dn: cn=admin,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
add: member
member: uid=juanma,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
~~~

###### Ahora modificamos en LDAP los grupos:

~~~
ldapmodify -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" -f modificacionGrupos.ldif -W
  Enter LDAP Password: 
  modifying entry "cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org"

  modifying entry "cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org"

  modifying entry "cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org"

  modifying entry "cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org"

  modifying entry "cn=admin,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org"

  modifying entry "cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org"

  modifying entry "cn=admin,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org"
~~~

## 5. Modificar OpenLDAP con memberOf

#### Modifica OpenLDAP apropiadamente para que se pueda obtener los grupos a los que pertenece cada usuario a través del atributo "memberOf"


###### Tenemos que crear unos ficheros de configuración para añadir y poder utilizar el módulo de 'memberOf'

##### Carga del módulo

###### El primero que vamos a crear es 'memberOf_load_config.ldif', el cual cargamos el módulo en LDAP y lo configuramos: 
~~~
dn: cn=module,cn=config
cn: module
objectClass: olcModuleList
objectclass: top
olcModuleLoad: memberof.la
olcModulePath: /usr/lib/ldap

dn: olcOverlay={0}memberof,olcDatabase={1}mdb,cn=config
objectClass: olcConfig
objectClass: olcMemberOf
objectClass: olcOverlayConfig
objectClass: top
olcOverlay: memberof
olcMemberOfDangling: ignore
olcMemberOfRefInt: TRUE
olcMemberOfGroupOC: groupOfNames
olcMemberOfMemberAD: member
olcMemberOfMemberOfAD: memberOf
~~~

> Indicamos el nombre del módulo 'memberOf.la' y la dirección de este.

##### Carga de la integridad referencial

###### Ahora tenemos que añadir un fichero, el cuale, es para agregar la integridad referencial a la configuración de LDAP para tener una relación entre los objetos y no pierdan coherencia. 

###### Vamos a crear el fichero 'refint1.ldif' para cargar el módulo 'refint.la' y además lo configuramos.
~~~
dn: cn=module,cn=config
cn: module
objectclass: olcModuleList
objectclass: top
olcmoduleload: refint.la
olcmodulepath: /usr/lib/ldap

dn: olcOverlay={1}refint,olcDatabase={1}mdb,cn=config
objectClass: olcConfig
objectClass: olcOverlayConfig
objectClass: olcRefintConfig
objectClass: top
olcOverlay: {1}refint
olcRefintAttribute: memberof member manager owner
~~~

###### Cargamos los fichero creados

~~~
sudo ldapadd -Y EXTERNAL -H ldapi:/// -f memberof_config.ldif
  SASL/EXTERNAL authentication started
  SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
  SASL SSF: 0
  adding new entry "cn=module,cn=config"

  adding new entry "olcOverlay={0}memberof,olcDatabase={1}mdb,cn=config"
~~~

~~~
sudo ldapadd -Y EXTERNAL -H ldapi:/// -f refint1.ldif 
  SASL/EXTERNAL authentication started
  SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
  SASL SSF: 0
  adding new entry "cn=module,cn=config"

  adding new entry "olcOverlay={1}refint,olcDatabase={1}mdb,cn=config"
~~~

##### Borramos los grupos creados

###### Ahora tenemos que borrar los grupos creados porque los grupos creados antes de añadir los módulos anteriores no se le aplican el memberOf

###### Borramos los grupos

~~~
sudo ldapdelete -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" 'cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org' -W

sudo ldapdelete -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" 'cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org' -W

sudo ldapdelete -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" 'cn=admin,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org' -W
~~~

###### Y los agregamos los grupos como en el anterior ejercicio [VER AQUÍ](https://github.com/MoralG/Trabajando_con_LDAP/blob/master/Usuarios_grupos_ACLs.md#3-creaci%C3%B3n-de-grupos)

###### Y añadimos los usuarios a dichos grupos [VER AQUÍ](https://github.com/MoralG/Trabajando_con_LDAP/blob/master/Usuarios_grupos_ACLs.md#4-a%C3%B1adiendo-usuarios)

##### Busqueda con memberOf

###### Algunos ejemplos indicandole que nos muestre el memberOf:

~~~
ldapsearch -LL -Y EXTERNAL -H ldapi:/// "(uid=paloma)" -b dc=amorales,dc=gonzalonazareno,dc=org memberOf
  SASL/EXTERNAL authentication started
  SASL username: gidNumber=1000+uidNumber=1000,cn=peercred,cn=external,cn=auth
  SASL SSF: 0
  version: 1

  dn: uid=paloma,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
  memberOf: cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org

ldapsearch -LL -Y EXTERNAL -H ldapi:/// "(uid=francisco)" -b dc=amorales,dc=gonzalonazareno,dc=org memberOf
  SASL/EXTERNAL authentication started
  SASL username: gidNumber=1000+uidNumber=1000,cn=peercred,cn=external,cn=auth
  SASL SSF: 0
  version: 1

  dn: uid=francisco,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
  memberOf: cn=admin,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
  memberOf: cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org

ldapsearch -LL -Y EXTERNAL -H ldapi:/// "(uid=alejandro)" -b dc=amorales,dc=gonzalonazareno,dc=org  uidNumber cn homeDirectory loginShell mail memberOf
  SASL/EXTERNAL authentication started
  SASL username: gidNumber=1000+uidNumber=1000,cn=peercred,cn=external,cn=auth
  SASL SSF: 0
  version: 1

  dn: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
  cn: Alejandro Rodrigez Rojas
  uidNumber: 2004
  homeDirectory: /home/alejandro
  loginShell: /bin/bash
  mail: alejandro@gmail.com
  memberOf: cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
  memberOf: cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
~~~

## 6. Creación de las ACLs 1

#### Crea las ACLs necesarias para que los usuarios del grupo almacen puedan ver todos los atributos de todos los usuarios pero solo puedan modificar las suyas

##### Si quieres saber mas sobre ACLs consulte [AQUÍ](https://github.com/MoralG/Trabajando_con_LDAP/blob/master/ACLs.md#gesti%C3%B3n-de-acceso-con-acls)

###### Todos los usuarios del grupo almacen tienen permiso de lectura de todos los atributos de los usuarios de People.
~~~
access to dn.regex="uid=[a-zA-z0-9]*,ou=People,dc=amorales,dc=gonzalonazareno,dc=org"
    by dn.member="cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org" read
~~~

###### Los usuarios del grupo almacen pueden modificar solo sus atributos.
~~~
access to dn.member="cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org" by self write
~~~

###### Realizamos el `ldapmodify`.
~~~
cat ACL1.ldif 
  dn: olcDatabase={1}mdb,cn=config
  changetype: modify
  add: olcAccess
  olcAccess: {3}to dn.regex="uid=[a-zA-z0-9]*,ou=People,dc=amorales,dc=gonzalonazareno, dc=org"
    by self write

cat ACL2.ldif 
  dn: olcDatabase={1}mdb,cn=config
  changetype: modify
  add: olcAccess
  olcAccess: {}to dn.member="cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org" 
    by self write
~~~

~~~
sudo ldapmodify  -Y EXTERNAL -H ldapi:/// -f ACL1.ldif
~~~

###### Denegamos todos los permisos de lectura y escritura del grupo comercial.
~~~

~~~

###### Comprobación:

~~~
ldapsearch -x -D "uid=francisco,ou=People,dc=amorales,dc=gonzalonazareno,dc=org" -b 'ou=People,dc=amorales,dc=gonzalonazareno,dc=org' -W
~~~


## 7. Creación de las ACLs 2

#### Crea las ACLs necesarias para que los usuarios del grupo admin puedan ver y modificar cualquier atributo de cualquier objeto

###### Otorgamos todos lo permisos de escritura y lectura a los usuarios del grupo admin.

~~~
access *
    by dn.member="cn=admin,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org" write
~~~

###### Comprobación:
~~~
ldapsearch -x -D "uid=francisco,ou=People,dc=amorales,dc=gonzalonazareno,dc=org" -b 'ou=People,dc=amorales,dc=gonzalonazareno,dc=org' -W
~~~

###### Cambiar contraseña del usuario 

~~~
ldappasswd "uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org"
~~~