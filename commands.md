# commands of openldap

## Commands

* ldapsearch
    * dn is specified as reverse tree.
    * when introducing the dn, we don't write `dn`, we write it's value.

* ldapadd
    * the dn specified when creating the object is it's id, and location in the DIT.
    * the specifie the objectClass the know what schema are we gonna use to define the attributes.
* ldapmodif
    * changetype: can be modify, 
    * depending on changetype, we could do:
        * changetype: modify
            * add: attribute -> adding an attribute.
* OLC (online config)
    * 

## snipets
* to search smth:
`ldapsearch -h localhost -p 389 -D cn="Manager,dc=osema" -w secret -b dc=osema 'objectclass=*'`
> -D for the user defined in slapd.ldif.
> -w for the password in the same file.
> -b for the base of the search, we specify the node from which the search starts via a dn.
> 'objectclass=*' this is the filter of the search.

* for naming context:
`ldapsearch -x -b '' -s base '(objectclass=*)' namingContexts`
> -x not auth

* search all in branch
`ldapsearch -x -b dc=osema`
> show all of branch.

* example of modification
`ldapmodify -D cn="Manager,dc=osema" -w secret -f ./tests/ldifs/modifuser.ldif`
```ldif
dn: cn=commonname,ou=special_org_unit,dc=osema
# spedifing the change type.
changeType: modify
# specifiying the modification type (adding attribute).
add: telephoneNumber
telephoneNumber: +33 00990 82890
```
