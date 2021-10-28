# LDAP Server
## @edt ASIX M06-ASO 2021-2022
### Servidor LDAP (Debian 11)

Podeu trobar les imatges docker al Dockehub de [edtasixm06](https://hub.docker.com/u/edtasixm06/)

Podeu trobar la documentació del mòdul a [ASIX-M06](https://sites.google.com/site/asixm06edt/)

ASIX M06-ASO Escola del treball de barcelona


 * **algamg/ldap21:acl** Servidor LDAP amb la base de dades edt.org
   S'ha fet el següent:

   * Hem definit els següents grups amb el seus usuaris alumnes,professors,1asix,2asix,wheel,1wiam,2wiam,1hiaw.



1) access to * by * read 
- anonymous 
- read propi / altres 
- write propi 
- write altres 

## annonymous
ldapsearch -x -LLL | OK

## read propi / altres | anna
ldapsearch -x -LLL -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna 'uid=anna' | OK
ldapsearch -x -LLL -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna 'uid=pere' | OK

## write propi & altres | anna
```modify.ldif
dn: uid=anna,ou=usuaris,dc=edt,dc=org
changetype: modify
replace: description
description: JOHN CENA

dn: uid=pere,ou=usuaris,dc=edt,dc=org
changetype: modify
delete: homephone
```
ldapmodify -vxc -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna  -f modify.ldif | ERROR 

2) acces to * by * write 
- modificar propi 
- modificar altre 

## modificar propi | anna
```modify.ldif
dn: uid=anna,ou=usuaris,dc=edt,dc=org
changetype: modify
replace: description
description: JOHN CENA

ldapmodify -vxc -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna  -f modify.ldif | OK 
```

## modificar altre | anna
```modify.ldif
dn: uid=pere,ou=usuaris,dc=edt,dc=org
changetype: modify
delete: description

ldapmodify -vxc -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna  -f modify.ldif | OK 
```

3) acces to * by self write by * read 
- modicar el propi 
- modificar un altre 
- veure altre 

## modificar propi | anna
```modify.ldif
dn: uid=anna,ou=usuaris,dc=edt,dc=org
changetype: modify
replace: description
description: JOHN CENA

ldapmodify -vxc -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna  -f modify.ldif | OK 
```
## modificar altre | anna
```modify.ldif
dn: uid=pere,ou=usuaris,dc=edt,dc=org
changetype: modify
add: description
description: UNDERTAKER

ldapmodify -vxc -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna  -f modify.ldif | ERROR
```
## veure altre 
ldapsearch -x -LLL -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna 'uid=pere' | OK
