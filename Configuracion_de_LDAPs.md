## Configuración de LDAPs

#### Configura el servidor LDAP de croqueta para que utilice el protocolo ldaps:// a la vez que el ldap:// utilizando el certificado x509 de la práctica de https o solicitando el correspondiente a través de gestiona.
#### Realiza las modificaciones adecuadas en el cliente ldap de croqueta para que todas las consultas se realicen por defecto utilizando ldaps://

> NOTA: Para la creación y el firmado del certificado [consulte aquí](https://github.com/MoralG/Servidores_CLOUD/blob/master/Configurar_HTTPS.md#creacion-del-certificado) 

#### Una vez que tenemos el certificado firmado, ya podemos configurar LDAPs, para hacer esto sigue estos pasos:

###### Creamos los directorios necesarios para ubicar el certificado 

###### Modificamos el fichero '/usr/lib/ssl/openssl.cnf'

###### En el apartado '[CA_default]' tenemos que comentar la siguiente linea:
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

###### Ahora vamos a crear un fichero '.ldif' de tipo modificación para modificar el objeto 'cn=config' y que acepte ssl

###### Creamos y añadimos las siguientes lineas un fichero, por ejemplo 'ldap-ssl.ldif'
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

###### Luego de tener ubicado los certificados y la clave privada, tendremos que dar permisos y asignarle el propietario 'openldap' a dichos ficheros con el comando 'setfacl'

###### Instalamos el paquete 'acl'

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


###### Para modificar esta entrada en LDAP, utilizamos 'ldapmodify'

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

###### Ahora tenemos modificar la ubicación del certificado de la Organizacion Certificadora del Gonzalo Nazareno del fichero '/etc/ldap/ldap.conf'

~~~
TLS_CACERT      /etc/ssl/openldap/certs/IESGonzaloNazareno.crt
~~~

###### Reiniciamos el servicio

~~~
sudo systemctl restart slapd
~~~
