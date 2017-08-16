Ansible Role: OpenLDAP Test Server
=========

This role install OpenLDAP server and put some data for test purpose.

Requirements
------------
None

Role Variables
--------------

| Name                      | Default value                         |        Requird       | Description                                                                 |
|---------------------------|---------------------------------------|----------------------|-----------------------------------------------------------------------------|
| temp_dir                  | /tmp/test-openldap-server             |         no           | Temp directory                                                              |
| ldap_http_port            | 389                                   |         no           | LDAP HTTP Port                                                              |
| ldap_https_port           | 636                                   |         no           | If ssl set true, LDAP HTTPS Port will be set                                |
| clean_all                 | true                                  |         no           | LDAP Data reset                                                             |
| ssl                       | false                                 |         no           | Enable SSL for LDAP Server                                                  |
| ssl_ca_cert               | ''                                    |         no           | CA Certificate. If ssl set true, this value must be set                     |
| ssl_cert                  | ''                                    |         no           | Server Certificate. If ssl set true, this value must be set                 |
| ssl_private_key           | ''                                    |         no           | Server Private Key. If ssl set true, this value must be set                 |


Dependencies
------------

None



Example Playbook
----------------
~~~
- name: Example Playbook
  hosts: ldap.example.com
  gather_facts: false

  roles:
    - { role: Jooho.openldap-test-server }
~~~

Information
-----------
- LDAP Password: redhat

- LDAP Bind DN: cn=read-only-admin,dc=example,dc=com

- LDAP Base DN: dc=example,dc=com

**LDAP Test Data**

|       Group     |      CN     |    OU    |    PW    |                  CN raw                    |
|-----------------|-------------|----------|----------|--------------------------------------------|
|  Administrators | Sue Jacobs  |  People  |  redhat  | cn=Sue Jacobs,ou=People,dc=example,dc=com  | 
|  Administrators | Pete Minsky |  People  |  redhat  | cn=Pete Minsky,ou=People,dc=example,dc=com | 
|  Developers     | Jooho Lee   |  People  |  redhat  | cn=Jooho Lee,ou=People,dc=example,dc=com   |


Client Configuration
--------------------
The root-ca.cert.pem file will be found on ldap server vm

```
TLS_CACERTDIR /etc/openldap/cacerts
TLS_CACERT    /etc/openldap/certs/root-ca.cert.pem
TLS_REQCERT allow
```


Useful Commands
----------------
```

ldapadd -x -w redhat -D "cn=read-only-admin,dc=example,dc=com" -f base.ldif

ldapsearch -v -H ldaps://ldap.example.com -D "cn=read-only-admin,dc=example,dc=com" -w "redhat" -b "dc=example,dc=com" -o ldif-wrap=no   -vvvv

ldapmodify -h ldap.example.com -p 389 -D "cn=read-only-admin,dc=example,dc=com" -f user-passwd.ldif -w redhat

ldapdelete -H ldaps://ldap.example.com -D "cn=read-only-admin,dc=example,dc=com" "cn=Sue Jacobs,ou=People,dc=example,dc=com" -w redhat -vvv

```



References
----------
- [Install OpenLDAP on CentOS7](http://www.itzgeek.com/how-tos/linux/centos-how-tos/step-step-openldap-server-configuration-centos-7-rhel-7.html)

- [External LDAP Test Server](http://www.forumsys.com/tutorials/integration-how-to/ldap/online-ldap-test-server/)



License
-------

BSD/MIT

Author Information
------------------

This role was created in 2017 by [Jooho Lee](http://github.com/jooho).

