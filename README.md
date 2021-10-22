# ldap21

**ldap21:base** Imatge base d'un servidor ldap

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



