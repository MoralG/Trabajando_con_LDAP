# Gestión de acceso con ACLs

#### Las ACLs son autorizaciones y mecanismos de control de acceso.

### Sintaxis base de las ACLs:
~~~
access to <what> [ by <who> [ <access> ] [ <control> ] ]+
~~~

### Tabla de niveles de acceso:
Nivel de acceso	     |  Privilegios	   |                     Descripción
:-------------------:|:---------------:|-----------------------------------------------------------
none	               |0	               |sin acceso
disclose	           |d	               |necesario para información en caso de error
auth	               |dx	             |necesario para autenticación (bind)
compare	             |cdx	             |necesario para comparar
search	             |scdx	           |necesario para aplicar los filtros de búsqueda
read	               |rscdx	           |necesario para leer los resultados de la búsqueda
add ó delete	       |wrscdx	         |necesario para modificar o renombrar
manage	             |mwrscdx	         |necesario para gestionar

> NOTA: Hay que tener en cuenta que las ACL se van leyendo por el orden que estan dispuestas en la configuración y al leer una específica, las siguientes de dicha especificación no se tendrán en cuenta. 

#### Las ACL se gestionan mediante ficheros de configuración o mediante la directiva 'olcAccess' para modificar.

##### Fichero de Configuración
###### Ejemplo:
~~~
access to attrs=userPassword
         by dn="cn=ldapreader,dc=genfic,dc=org" read
         by self read
         by anonymous auth
         by * none
  
access to dn.base="cn=Subschema" by users read
access to dn.base="" by * read
~~~

##### Fichero de Modificación con la directiva 'olcAccess'
###### Ejemplo:
~~~
dn: olcDatabase={-1}frontend,cn=config
changetype: modify
add: olcAccess
olcAccess: {0}to dn.base="cn=subschema"  by users read
olcAccess: {1}to dn.base="" by * read
~~~

###### Ejemplo para insertarla en la parte superior:
~~~
dn: olcDatabase={-1}frontend,cn=config
changetype: modify
add: olcAccess
olcAccess: {0}to attrs=userPassword  by dn="cn=ldapreader,dc=genfic,dc=org" read by self read by anonymous auth by * none
~~~

###### Ejemplo para elimnar:
~~~
dn: olcDatabase={-1}frontend,cn=config
changetype: modify
delete: olcAccess
olcAccess: {1}
~~~