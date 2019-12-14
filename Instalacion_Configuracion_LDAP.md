# Instalacion y configuracion de LDAP

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
