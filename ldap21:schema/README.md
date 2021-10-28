# LDAP @isx25633105 ASIX
# Curs 2021-2022

* **isx25633105/ldap21:schema** Servidor LDAP amb la base de dades edt.org
 S'ha fet el següent:
 * futbolistaA.schema Derivat de inetorgPerson, structural, injectat dades de data-futbolistaA.ldif.

```
docker network create 2hisx
docker build -t isx25633105/ldap21:schema
docker run --rm --name ldap.edt.org -h ldap.edt.org --net 2hisx -p 389:389 -it isx25633105/ldap21:schema /bin/bash

docker run --rm --name phpldapadmin.edt.org -h phpldapadmin.edt.org --net 2hisx -p 80:80 -d edtasixm06/phpldapadmin:20
```
 

