## create the battacorp db
* add the following snippet to the slapd.ldif to create the db (the naming context batta-corp)
```ldif
dn: olcDatabase=mdb,cn=config
objectClass: olcDatabaseConfig
objectClass: olcMdbConfig
olcDatabase: mdb
olcDbMaxSize: 1073741824
olcSuffix: dc=batta-corp
# olcSuffix: dc=my-domain,dc=com
olcRootDN: cn=admin,dc=batta-corp
# Cleartext passwords, especially for the rootdn, should
# be avoided.  See slappasswd(8) and slapd-config(5) for details.
# Use of strong authentication encouraged.
olcRootPW: secret
# The database directory MUST exist prior to running slapd AND 
# should only be accessible by the slapd and slap tools.
# Mode 700 recommended.
olcDbDirectory:	/usr/local/var/batta-corp-data
# Indices to maintain
olcDbIndex: objectClass eq
```
* be sure to kill the process of slapd if it's already on
`ps aux | grep slapd` then `kill <pid>`
* delete the content of /usr/local/etc/slapd.d
* then you need to import the config to slapd.d, execute the command:
`sudo /usr/local/sbin/slapadd -n 0 -F /usr/local/etc/slapd.d -l /usr/local/etc/openldap/slapd.ldif`
* run the ldap server using the previous config
`sudo /usr/local/libexec/slapd -F /usr/local/etc/slapd.d`
* check if the server started
`ps aux | grep slapd`
* check that the db is created, this command will show the databases or the naming contexts.
`ldapsearch -x -b '' -s base '(objectclass=*)' namingContexts`

## adding the organization and the users to the batta corp
* checking the acces in slapd.conf, this user will be used to tweak the config.
```slapd.conf
database config
# Uncomment the rootpw line to allow binding as the cn=config
# rootdn so that temporary modifications to the configuration can be made
# while slapd is running. They will not persist across a restart.
rootdn "cn=admin,cn=config"
rootpw secret
```

* add the schemas we defined in battaSchema.ldif
    * it's possible 
`ldapadd -D cn=admin,cn=config -w secret -f battaSchema.ldif`
> some error is preventing the execution of this schema... :'(

* add the the organization defined in organization.ldif