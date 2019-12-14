# Usuarios, grupos y ACLs en LDAP

## 1. Creación de usuarios

#### Crea 10 usuarios con los nombres que prefieras en LDAP, esos usuarios deben ser objetos de los tipos posixAccount e inetOrgPerson. Estos usuarios tendrán un atributo userPassword.

##### Creación de usuarios

###### Ahora vamos a crear el fichero '.ldif' para los usuarios, estos los vamos a crear en el directorio /home/debian/2asir/personas.ldif

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

###### Ahora crearemos el fichero personas.ldif con estos datos:
> NOTA: si queremos poner un dato en base64 tenemos que indicarlo con '::'

~~~
dn: uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixAccount
objectClass: inetOrgPerson
objectClass: person
cn:: UGFsb21hIGRlbCBSb2PDrW8gR2FyY2lhCg==
uid: alejandro
uidNumber: 2001
gidNumber: 2000
homeDirectory: /home/paloma
loginShell: /bin/bash
userPassword: {SSHA}dAttFmPZKk5MPhKl01kkr4WkckiODKd2
sn: morales
mail: paloma@gmail.com
givenName: paloma
~~~

#### Creamos los Grupos 'grupos.ldif':

~~~
dn: cn=Grupos,ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: posixGroup
gidNumber: 2000
cn: Usuarios
~~~

#### Creamos las Unidades Organizativas 'organizativas.ldif':

~~~
dn: ou=People,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: top
objectClass: organizationalUnit
ou: People
~~~

###### Ahora introducimos los datos del fichero personas.ldif, grupos.ldif y organizativas.ldif con el siguiente comando:
~~~
ldapadd -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" -f <fichero.ldif> -W
~~~

###### Opciones:

ldapadd   Para añadir entradas en el servidor LDAP
-x        Para indicar la autentificación básica
-D        Para indicar el Distinguished Name
-f        Para indicar el fichero que queremos leer
-W        Para que nos pida la contraseña

~~~
ldapadd -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" -f organizativas.ldif -W
  Enter LDAP Password:
  adding new entry "ou=People,dc=amorales,dc=gonzalonazareno,dc=org"

ldapadd -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" -f grupos.ldif -W
  Enter LDAP Password:
  adding new entry "cn=Grupos,ou=People,dc=amorales,dc=gonzalonazareno,dc=org"

ldapadd -x -D "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" -f personas.ldif -W
  Enter LDAP Password:
  adding new entry "uid=alejandro,ou=People,dc=amorales,dc=gonzalonazareno,dc=org"
~~~

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

