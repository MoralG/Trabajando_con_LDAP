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
~~~

###### Opciones:

ldapadd   Para añadir entradas en el servidor LDAP
-x        Para indicar la autentificación básica
-D        Para indicar el Distinguished Name
-f        Para indicar el fichero que queremos leer
-W        Para que nos pida la contraseña

## 2. Creación de grupos

#### Crea 3 grupos en LDAP, dentro de una unidad organizativa, que sean objetos del tipo groupOfNames. Estos grupos serán: comercial, almacen y admin

###### Vamos a crear un fichero '.ldif' para los grupos, este lo vamos a crear en el directorio /home/debian/2asir

#### Indicamos los Grupos en 'grupos.ldif':

~~~
dn: cn=comercial,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixGroup
gidNumber: 1001
cn: comercial

dn: cn=almacen,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixGroup
gidNumber: 1002
cn: almacen

dn: cn=admin,ou=Group,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixGroup
gidNumber: 1003
cn: admin
~~~

###### Añadimos con el siguiente comando los grupos del fichero 'grupos.ldif'

~~~
ldapadd -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" -f grupos.ldif -W
~~~

## 3. Creación de usuarios

#### Crea 10 usuarios con los nombres que prefieras en LDAP, dentro de una unidad organizativa diferente a los grupos. Esos usuarios deben ser objetos de los tipos posixAccount e inetOrgPerson. Estos usuarios tendrán un atributo userPassword.

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
dn: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
objectClass: person
cn:: UGFsb21hIGRlbCBSb2PDrW8gR2FyY2lhCg==
uid: paloma
uidNumber: 2001
gidNumber: 2000
homeDirectory: /home/paloma
loginShell: /bin/bash
userPassword: {SSHA}dAttFmPZKk5MPhKl01kkr4WkckiODKd2
sn: garcia
mail: paloma@gmail.com
givenName: paloma

dn: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
objectClass: person
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

dn: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
objectClass: person
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
objectClass: person
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

dn: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
objectClass: person
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

dn: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
objectClass: person
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

dn: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
objectClass: person
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

dn: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
objectClass: person
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

dn: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
objectClass: person
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

dn: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
objectClass: person
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

## 4. Añadiendo usuarios

#### Añade usuarios que pertenezcan a:

* Solo al grupo comercial



* Solo al grupo almacen



* Al grupo comercial y almacen



* Al grupo admin y comercial



* Solo al grupo admin



## 5. Modificar OpenLDAP con memberOf

#### Modifica OpenLDAP apropiadamente para que se pueda obtener los grupos a los que pertenece cada usuario a través del atributo "memberOf"



## 6. Creación de las ACLs 1

#### Crea las ACLs necesarias para que los usuarios del grupo almacen puedan ver todos los atributos de todos los usuarios pero solo puedan modificar las suyas



## 7. Creación de las ACLs 2

#### Crea las ACLs necesarias para que los usuarios del grupo admin puedan ver y modificar cualquier atributo de cualquier objeto












-----------------------------------------------------------------------------

###### Si queremos modificar un registro debemos de crear un fichero aparte con el mismo DN y las indicaciones de las modificaciones que se deben de realizar.

###### Creamos el fichero 'grupos-cp.ldif' y dentro le indicamos que vamos a modificar el 'cn: Usuarios' por 'cn: Grupos'

~~~
dn: cn=Grupos,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
changetype:modify
replace: cn
cn: Grupos
~~~

###### Luego Con el comando 'ldapmodify' modificamos el registro indicandole el fichero de modifiación.

~~~
ldapmodify -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" -f grupos-cp.ldif -W
  Enter LDAP Password: 
  modifying entry "cn=Grupos,ou=People,dc=amorales,dc=gonzalonazareno,dc=org"
~~~













~~~
ldapadd -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" -f organizativas.ldif -W
  Enter LDAP Password:
  adding new entry "ou=People,dc=amorales,dc=gonzalonazareno,dc=org"

ldapadd -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" -f grupos.ldif -W
  Enter LDAP Password:
  adding new entry "cn=Grupos,ou=People,dc=amorales,dc=gonzalonazareno,dc=org"
~~~