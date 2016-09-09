kinit-keytab
============

Ansible role to authenticate to a Windows domain and get a kerberos ticket using a kerberos keytab file. 

Requirements
------------

This role requires that you have a working kerberos client configuration. Please see the Ansible Windows guide to make sure that you have all of the libraries and configuration you need to connect to Windows hosts. Additionally you need to have WinRM enabled on your Windows host (see same Ansible Windows guide) and a kerberos keytab file for your Active Directory account. You can generate a keytab file in the following way:

```
### on linux
ktutil
addent -password -p username@YOURDOMAIN.LOCAL -k 1 -e rc4-hmac
# ENTER PASSWORD
wkt username.keytab
quit

### on OSX
ktutil -k username.keytab add -p username@YOURDOMAIN.LOCAL
# Enter "arcfour-hmac-md5" without quotes for Encryption type
# Enter "1" without quotes for Key version
# Enter and verify password
```

Treat the resultant file the same way you would treat an SSH key. It allows access to the AD domain as you without requiring your password.

Role Variables
--------------

```
keytab_file: Path to user's keytab file
username: Kerberos username including realm (i.e., user@name@DYOURDOMAIN.LOCAL)
krb_ticket_lifetime: Lifetime of the ticket in seconds (s), minutes (m) or hours (h) - default: 5m
```

Dependencies
------------

None

Example Playbook
----------------

```
    - hosts: localhost
      connection: local
      gather_facts: false
      roles:
        - { role: kinit-keytab,
            username: username@YOURDOMAIN.LOCAL,
            keytab_file: /home/username/username.keytab,
            krb_ticket_lifetime: 60s  }
```

License
-------

BSD
