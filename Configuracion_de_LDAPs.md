## Configuración de LDAPs

#### Configura el servidor LDAP de croqueta para que utilice el protocolo ldaps:// a la vez que el ldap:// utilizando el certificado x509 de la práctica de https o solicitando el correspondiente a través de gestiona.
#### Realiza las modificaciones adecuadas en el cliente ldap de croqueta para que todas las consultas se realicen por defecto utilizando ldaps://

> NOTA: Para la creación y el firmado del certificado [consulte aquí](https://github.com/MoralG/Servidores_CLOUD/blob/master/Configurar_HTTPS.md#creacion-del-certificado) 

#### Una vez que tenemos el certificado firmado, ya podemos configurar LDAPs, para hacer esto sigue estos pasos:

###### Creamos los directorios necesarios para ubicar el certificado 

###### Modificamos el fichero `/usr/lib/ssl/openssl.cnf`

###### En el apartado `[CA_default]` tenemos que comentar la siguiente linea:
~~~
#dir            = ./demoCA              # Where everything is kept
~~~

###### y añadir la siguiente linea:
~~~
dir             = /etc/ssl/openldap
~~~

###### Quedaría como se muestra a contiuación:
~~~
[ CA_default ]

#dir            = ./demoCA              # Where everything is kept
dir             = /etc/ssl/openldap
certs           = $dir/certs            # Where the issued certs are kept
crl_dir         = $dir/crl              # Where the issued crl are kept
database        = $dir/index.txt        # database index file.
~~~

###### Ahora vamos a crear un fichero `.ldif` de tipo modificación para modificar el objeto `cn=config` y que acepte ssl

###### Creamos y añadimos las siguientes lineas un fichero, por ejemplo `ldap-ssl.ldif`
~~~
dn: cn=config
changetype: modify
add: olcTLSCACertificateFile
olcTLSCACertificateFile: /etc/ssl/openldap/certs/IESGonzaloNazareno.crt
-
replace: olcTLSCertificateFile
olcTLSCertificateFile: /etc/ssl/openldap/certs/amorales.gonzalonazareno.org.crt
-
replace: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: /etc/ssl/openldap/private/gonzalonazareno.pem
~~~

###### Luego de tener ubicado los certificados y la clave privada, tendremos que dar permisos y asignarle el propietario `openldap` a dichos ficheros con el comando `setfacl`

###### Instalamos el paquete `acl`

~~~
sudo apt install acl
~~~

###### Cambiamos los permisos y propietario

~~~
sudo setfacl -m u:openldap:r-x /etc/ssl/openldap/certs/
sudo setfacl -m u:openldap:r-x /etc/ssl/openldap/certs/amorales.gonzalonazareno.org.crt
sudo setfacl -m u:openldap:r-x /etc/ssl/openldap/certs/IESGonzaloNazareno.crt 
sudo setfacl -m u:openldap:r-x /etc/ssl/openldap/private/
sudo setfacl -m u:openldap:r-x /etc/ssl/openldap/private/gonzalonazareno.pem 
~~~


###### Para modificar esta entrada en LDAP, utilizamos `ldapmodify`

~~~
sudo ldapmodify -Y EXTERNAL -H ldapi:/// -f ldap-ssl.ldif
    SASL/EXTERNAL authentication started
    SASL username: gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth
    SASL SSF: 0
    modifying entry "cn=config"
~~~

###### Podemos comprobar que los ficheros estan modificados correctamente:

~~~
sudo slapcat -b "cn=config" | grep -E "olcTLS"
    olcTLSCACertificateFile: /etc/ssl/openldap/certs/IESGonzaloNazareno.crt
    olcTLSCertificateFile: /etc/ssl/openldap/certs/amorales.gonzalonazareno.org.
    olcTLSCertificateKeyFile: /etc/ssl/openldap/private/gonzalonazareno.pem
~~~

###### Y otra comprobación es la de la validez de la configuración de LDAPs

~~~
sudo slaptest -u
    config file testing succeeded
~~~

###### Ahora tenemos modificar la ubicación del certificado de la Organizacion Certificadora del Gonzalo Nazareno del fichero `/etc/ldap/ldap.conf`

~~~
TLS_CACERT      /etc/ssl/openldap/certs/IESGonzaloNazareno.crt
~~~

###### Añadimos al fichero `/etc/default/slapd`, en el apartado `SLAPD_SERVICES` lo siguiente:

~~~
ldaps:///
~~~

###### Reiniciamos el servicio

~~~
sudo systemctl restart slapd
~~~

###### Comprobamos que podemos realizar una consulta utilizando `ldaps`

~~~
sudo ldapsearch -x -H ldaps://croqueta.amorales.gonzalonazareno.org:636 -b "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" 
# extended LDIF
#
# LDAPv3
# base <cn=admin,dc=amorales,dc=gonzalonazareno,dc=org> with scope subtree
# filter: (objectclass=*)
# requesting: ALL
#

# admin, amorales.gonzalonazareno.org
dn: cn=admin,dc=amorales,dc=gonzalonazareno,dc=org
objectClass: simpleSecurityObject
objectClass: organizationalRole
cn: admin
description: LDAP administrator

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
~~~

###### Si ahora queremos realizar la misma consulta pero con `ldap` no podremos.
~~~
debian@croqueta:~$ sudo ldapsearch -x -H ldap://croqueta.amorales.gonzalonazareno.org:636 -b "cn=admin,dc=amorales,dc=gonzalonazareno,dc=org" 
ldap_result: Can't contact LDAP server (-1)
~~~