# Usuarios, grupos y ACLs en LDAP

## 1. Creación de usuarios

#### Crea 10 usuarios con los nombres que prefieras en LDAP, esos usuarios deben ser objetos de los tipos posixAccount e inetOrgPerson. Estos usuarios tendrán un atributo userPassword.

###### Antes de realizar esta tarea, hay que intalar el servidor de LDAP y configurarlo

##### Instalacion y configuracion de LDAP

###### Instalamos el paquete *sldap*

~~~
sudo apt install slapd
~~~

###### Nos pedira la contraseña del Administrador
~~~
               ┌─────────────────────────┤ Configuring slapd ├──────────────────────────┐
               │ Please enter the password for the admin entry in your LDAP directory.  │ 
               │                                                                        │ 
               │ Administrator password:                                                │ 
               │                                                                        │ 
               │ ************__________________________________________________________ │ 
               │                                                                        │ 
               │                                 <Ok>                                   │ 
               │                                                                        │ 
               └────────────────────────────────────────────────────────────────────────┘ 
~~~

###### Instalamos el paquete 'ldap-utils' necesarios para tener varias utilidades necesarias
~~~
sudo apt install ldap-utils
~~~

###### Con el siguiente comando vemos todas las entradas creadas por defecto durante la instalación:
~~~
sudo slapcat
  dn: dc=nodomain
  objectClass: top
  objectClass: dcObject
  objectClass: organization
  o: nodomain
  dc: nodomain
  structuralObjectClass: organization
  entryUUID: 2b7ddc84-afc6-1039-9240-7d36fc884949
  creatorsName: cn=admin,dc=nodomain
  createTimestamp: 20191210182521Z
  entryCSN: 20191210182521.556081Z#000000#000#000000
  modifiersName: cn=admin,dc=nodomain
  modifyTimestamp: 20191210182521Z

  dn: cn=admin,dc=nodomain
  objectClass: simpleSecurityObject
  objectClass: organizationalRole
  cn: admin
  description: LDAP administrator-
  userPassword:: e1NTSEF9Vi9UcGFkK3VPaGNyV054RFF0bm5acmxsa01mclVRTWc=
  structuralObjectClass: organizationalRole
  entryUUID: 2b7dfab6-afc6-1039-9241-7d36fc884949
  creatorsName: cn=admin,dc=nodomain
  createTimestamp: 20191210182521Z
  entryCSN: 20191210182521.556919Z#000000#000#000000
  modifiersName: cn=admin,dc=nodomain
  modifyTimestamp: 20191210182521Z
~~~

> Como se puede ver, tenemos 2 registros y uno de ellos es el del Administrador.

###### Con el siguiente comando podemos vemos el número de entrada existentes:

~~~
sudo ldapsearch -x -h localhost
  # extended LDIF
  #
  # LDAPv3
  # base <> (default) with scope subtree
  # filter: (objectclass=*)
  # requesting: ALL
  #

  # search result
  search: 2
  result: 32 No such object

  # numResponses: 1
~~~

###### Ahora nosotros queremos modificar la entrada del administrador para que tenga nuestros datos, como el 'Distinguished Name' que lo queremo dejar como a continuación:

~~~
dn: cn=admin,dc=amorales,dc=gonzalonazareno,dc=org
~~~

###### Con el siguiente comando podremos configurar nuestro LDAP

~~~
sudo dpkg-reconfigure -plow slapd
~~~

###### Ahora nos irá saliendo distintas ventanas de configuración, te muestro como hacerlo:

> Selecciona 'No' para que no se omita la configuración inicial
~~~
      ┌───────────────────────────────────┤ Configuring slapd ├───────────────────────────────────┐
      │                                                                                           │ 
      │ If you enable this option, no initial configuration or database will be created for you.  │ 
      │                                                                                           │ 
      │ Omit OpenLDAP server configuration?                                                       │ 
      │                                                                                           │ 
      │                          <Yes>                             <No>                           │ 
      │                                                                                           │ 
      └───────────────────────────────────────────────────────────────────────────────────────────┘ 
~~~
> Introduce 'amorales.gonzalonazareno.org' para indicar el dominio del DNS
~~~
     ┌───────────────────────────────────┤ Configuring slapd ├────────────────────────────────────┐
     │ The DNS domain name is used to construct the base DN of the LDAP directory. For example,   │ 
     │ 'foo.example.org' will create the directory with 'dc=foo, dc=example, dc=org' as base DN.  │ 
     │                                                                                            │ 
     │ DNS domain name:                                                                           │ 
     │                                                                                            │ 
     │ amorales.gonzalonazareno.org______________________________________________________________ │ 
     │                                                                                            │ 
     │                                           <Ok>                                             │ 
     │                                                                                            │ 
     └────────────────────────────────────────────────────────────────────────────────────────────┘ 
~~~
> Introduce 'amorales.gonzalonazareno.org' para indicar nuestra Organización
~~~
      ┌──────────────────────────────────┤ Configuring slapd ├───────────────────────────────────┐
      │ Please enter the name of the organization to use in the base DN of your LDAP directory.  │ 
      │                                                                                          │ 
      │ Organization name:                                                                       │ 
      │                                                                                          │ 
      │ amorales.gonzalonazareno.org____________________________________________________________ │ 
      │                                                                                          │ 
      │                                          <Ok>                                            │ 
      │                                                                                          │ 
      └──────────────────────────────────────────────────────────────────────────────────────────┘ 
~~~
> Introduce la contraseña del administrador
~~~
               ┌─────────────────────────┤ Configuring slapd ├──────────────────────────┐
               │ Please enter the password for the admin entry in your LDAP directory.  │ 
               │                                                                        │ 
               │ Administrator password:                                                │ 
               │                                                                        │ 
               │ ************__________________________________________________________ │ 
               │                                                                        │ 
               │                                 <Ok>                                   │ 
               │                                                                        │ 
               └────────────────────────────────────────────────────────────────────────┘ 
~~~
> Selecciona 'MDB' para seleccionar el motor de base de datos por defecto
~~~
  ┌───────────────────────────────────────┤ Configuring slapd ├───────────────────────────────────────┐
  │ HDB and BDB use similar storage formats, but HDB adds support for subtree renames. Both support   │ 
  │ the same configuration options.                                                                   │ 
  │                                                                                                   │ 
  │ The MDB backend is recommended. MDB uses a new storage format and requires less configuration     │ 
  │ than BDB or HDB.                                                                                  │ 
  │                                                                                                   │ 
  │ In any case, you should review the resulting database configuration for your needs. See           │ 
  │ /usr/share/doc/slapd/README.Debian.gz for more details.                                           │ 
  │                                                                                                   │ 
  │ Database backend to use:                                                                          │ 
  │                                                                                                   │ 
  │                                               BDB                                                 │ 
  │                                               HDB                                                 │ 
  │                                               MDB                                                 │ 
  │                                                                                                   │ 
  │                                                                                                   │ 
  │                                              <Ok>                                                 │ 
  │                                                                                                   │ 
  └───────────────────────────────────────────────────────────────────────────────────────────────────┘ 
~~~
> Selecciona 'No' para que no elimine el directorio slapd cuando el paquete sea eliminado
~~~
                    ┌─────────────────────┤ Configuring slapd ├─────────────────────┐
                    │                                                               │ 
                    │                                                               │ 
                    │                                                               │ 
                    │ Do you want the database to be removed when slapd is purged?  │ 
                    │                                                               │ 
                    │                <Yes>                   <No>                   │ 
                    │                                                               │ 
                    └───────────────────────────────────────────────────────────────┘ 

~~~
> Selecciona 'Yes' para dejarlo por defecto
~~~
  ┌──────────────────────────────────────┤ Configuring slapd ├───────────────────────────────────────┐
  │                                                                                                  │ 
  │ There are still files in /var/lib/ldap which will probably break the configuration process. If   │ 
  │ you enable this option, the maintainer scripts will move the old database files out of the way   │ 
  │ before creating a new database.                                                                  │ 
  │                                                                                                  │ 
  │ Move old database?                                                                               │ 
  │                                                                                                  │ 
  │                            <Yes>                               <No>                              │ 
  │                                                                                                  │ 
  └──────────────────────────────────────────────────────────────────────────────────────────────────┘ 

~~~

###### Si todo ha salido bien, nos saldrá lo siguiente.
~~~
  Backing up /etc/ldap/slapd.d in /var/backups/slapd-2.4.47+dfsg-3+deb10u1... done.
  Moving old database directory to /var/backups:
  - directory unknown... done.
  Creating initial configuration... done.
  Creating LDAP directory... done.
~~~

###### Podemos comprobar que se ha modificado el administrador realizando de nuevo el siguiente comando:

~~~
sudo slapcat
  dn: dc=amorales,dc=gonzalonazareno,dc=org
  objectClass: top
  objectClass: dcObject
  objectClass: organization
  o: amorales.gonzalonazareno.org
  dc: amorales
  structuralObjectClass: organization
  entryUUID: 8901d092-afc9-1039-90af-af573535533d
  creatorsName: cn=admin,dc=amorales,dc=gonzalonazareno,dc=org
  createTimestamp: 20191210184926Z
  entryCSN: 20191210184926.939173Z#000000#000#000000
  modifiersName: cn=admin,dc=amorales,dc=gonzalonazareno,dc=org
  modifyTimestamp: 20191210184926Z

  dn: cn=admin,dc=amorales,dc=gonzalonazareno,dc=org
  objectClass: simpleSecurityObject
  objectClass: organizationalRole
  cn: admin
  description: LDAP administrator
  userPassword:: e1NTSEF9a0h3QlNaeWxBRnphRmlyRmdubFdYM2V2MUJmVDhSZ3c=
  structuralObjectClass: organizationalRole
  entryUUID: 8901f90a-afc9-1039-90b0-af573535533d
  creatorsName: cn=admin,dc=amorales,dc=gonzalonazareno,dc=org
  createTimestamp: 20191210184926Z
  entryCSN: 20191210184926.940253Z#000000#000#000000
  modifiersName: cn=admin,dc=amorales,dc=gonzalonazareno,dc=org
  modifyTimestamp: 20191210184926Z
~~~

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

