# Useful commands

- I'll post some interesting commands I used in the past think that they are helpful at least for me :-)

## ldapsearch

``` { .sh }
# search for groups
ldapsearch -H "ldap://<LDAP-HOST-FQDN>" -x -W -D "<BINDDN_USED_FOR_SIMPLE_AUTHENTICATION>" -b "<BASEDN_USED_FOR_SEARCH>" "(objectClass=group)"
ldapsearch -H "ldap://corporation.lan" -x -W -D "CN=Service Acount LDAP read,OU=Service Accounts,OU=EXAMPLE,DC=corporation,DC=lan" -b "DC=corporation,DC=lan" "(objectClass=group)"

# search for user
ldapsearch -H "ldap://<LDAP-HOST-FQDN>" -x -W -D "<BINDDN_USED_FOR_SIMPLE_AUTHENTICATION>" -b "<BASEDN_USED_FOR_SEARCH>" "(objectClass=person)"
ldapsearch -H "ldap://corporation.lan" -x -W -D "CN=Service Acount LDAP read,OU=Service Accounts,OU=EXAMPLE,DC=corporation,DC=lan" -b "OU=Admin Accounts,OU=EXAMPLE,DC=corporation,DC=lan" "(objectClass=group)"

# search for specific user
ldapsearch -H "ldap://<LDAP-HOST-FQDN>" -x -W -D "<BINDDN_USED_FOR_SIMPLE_AUTHENTICATION>" -b "<BASEDN_USED_FOR_SEARCH>" "(CN=<CN_OF_SPECIFIC_USER>)"
ldapsearch -H "ldap://corporation.lan" -x -W -D "CN=Service Acount LDAP read,OU=Service Accounts,OU=EXAMPLE,DC=corporation,DC=lan" -b "OU=Admin Accounts,OU=EXAMPLE,DC=corporation,DC=lan" "(CN=Max Mustermann)"

# search for groups of a specific user
ldapsearch -H "ldap://<LDAP-HOST-FQDN>" -x -W -D "<BINDDN_USED_FOR_SIMPLE_AUTHENTICATION>" -b "<BASEDN_USED_FOR_SEARCH>" "(&(objectClass=group)(member=CN=<DN_OF_SPECIFIC_USER>)"
ldapsearch -H "ldap://corporation.lan" -x -W -D "CN=Service Acount LDAP read,OU=Service Accounts,OU=EXAMPLE,DC=corporation,DC=lan" -b "DC=corporation,DC=lan" "(&(objectClass=group)(member=CN=Max Mustermann,OU=Admin Accounts,OU=EXAMPLE,DC=corporation,DC=lan)"
```

## delete journal logs older than 5 days

``` { .sh }
sudo journalctl --vacuum-time=5days
```
