Usage
-----

After deploying the compiled libraries, set the following parameters for using the Redmine database as authentication for other sites.

# mod_dbd configuration
[/etc/apache2/conf-available/mod_authn_dbd.conf]
```
DBDriver mysql
DBDParams "host=localhost port=3306 dbname=redmine user=redmine pass=xxxxxx"

DBDMin  4
DBDKeep 8
DBDMax  20  
DBDExptime 300 
```

[/etc/apache2/sites-available/xxxx.conf]
```
<Directory /usr/www/myhost/private>
  # core authentication and mod_auth_basic configuration
  # for mod_authn_dbd
  AuthType Basic
  AuthName "My Server"
  AuthBasicProvider dbd 

  # core authorization configuration
  Require valid-user

  # mod_authn_dbd_as SQL query to authenticate a user and password
  AuthDBDFullAuthQuery \
    "SELECT login FROM users WHERE login = %s AND hashed_password = SHA1(CONCAT(salt, SHA1(%s)))"
</Directory>
```

Supported Redmine versions
--------------------------

So far tested with: [3.2.1-2]
