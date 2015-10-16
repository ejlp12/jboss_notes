

`brew install openldap`

```
==> Installing openldap from homebrew/homebrew-dupes
==> Downloading ftp://ftp.openldap.org/pub/OpenLDAP/openldap-release/openldap-2.4.39.tgz
######################################################################## 100.0%
==> ./configure --prefix=/usr/local/Cellar/openldap/2.4.39 --sysconfdir=/usr/local/etc --localstatedir=/usr/local/var --enable-bdb=no --enable-hdb=no

...dihapus...

```

Ubah beberapa default configuration file:

```
cd /private/etc/openldap/
cp ldap.conf.default ldap.conf
cp slapd.conf.default slapd.conf 
```

Generate password yang dienkripsi: `slappasswd -s yourpassword`, lalu copy hasil keluaran command tersebut.

edit `/private/etc/openldap/slapd.conf` dan ubah line 

```
rootpw			secret
```

menjadi

```
rootpw          {SSHA}0/QFDqWP7vigXWqeGx9LDmE4hP04qUm7
```


Edit file `/private/etc/openldap/ldap.conf`, ubah line

```
BASE	dc=my-domain,dc=com
URI	ldap://localhost:389
```

Buat file konfigurasi untuk penyimpanan data (OpenLDAP menggunakan Bakerley Database untuk meyimpan data):

```
cp /private/var/db/openldap/openldap-data/DB_CONFIG.example /private/var/db/openldap/openldap-data/DB_CONFIG
```

Jalankan slapd di terminal lain dengan perintah `/usr/libexec/slapd -d3 &`
opsi `-d` menunjukan level debug.

Output:
```
561fff4e slapd startup: initiated.
561fff4e backend_startup_one: starting "cn=config"
561fff4e config_back_db_open
561fff4e config_build_entry: "cn=config"
561fff4e config_build_entry: "cn=schema"
561fff4e >>> dnNormalize: <cn={0}core>
561fff4e <<< dnNormalize: <cn={0}core>
561fff4e config_build_entry: "cn={0}core"
561fff4e config_build_entry: "olcDatabase={-1}frontend"
561fff4e config_build_entry: "olcDatabase={0}config"
561fff4e config_build_entry: "olcDatabase={1}bdb"
561fff4e backend_startup_one: starting "dc=somedomain,dc=org"
561fff4e bdb_db_open: database "dc=somedomain,dc=org": dbenv_open(/private/var/db/openldap/openldap-data).
561fff4e bdb_monitor_db_open: monitoring disabled; configure monitor database to enable
561fff4e slapd starting
561fff4e daemon: posting com.apple.slapd.startup notification
```

buat file `root-ou.ldif`

```
dn:dc=my-domain,dc=com
objectClass:dcObject
objectClass:organizationalUnit
dc:my-domain
ou:MyDomain
```

buat file `people-ou.ldif`

```
dn: ou=people,dc=my-domain,dc=com
objectClass: organizationalUnit
ou: people
```

```
$ ldapadd -D "cn=Manager,dc=my-domain,dc=com" -W -x -f root-ou.ldif
Enter LDAP Password:
adding new entry "dc=my-domain,dc=com"

$ ldapadd -D "cn=Manager,dc=my-domain,dc=com" -W -x -f people-ou.ldif
Enter LDAP Password:
adding new entry "ou=people,dc=my-domain,dc=com"
```

-W :password perlu diinput setelah perintah tersebut, 
    gunakan -w jika ingin menuliskan password langsung
-x :otentikasi sederhana

Gunakan opsi `-h` untuk medefinisikan hostname/IP dan `-p` untuk port jika anda melakukan perintah tersebut dari mesin yang berbeda.

Test ldapsearch -x -b "dc=my-domain,dc=info"

[Apache Directory Studio](http://directory.apache.org/studio/)



```java
import javax.naming.Context;
import javax.naming.NamingEnumeration;
import javax.naming.NamingException;
import javax.naming.directory.Attributes;
import javax.naming.directory.DirContext;
import javax.naming.directory.InitialDirContext;
import javax.naming.directory.SearchControls;
import javax.naming.directory.SearchResult;
import java.util.Hashtable;

public class OpenLdapSearchAuthenticate
{
    private final Hashtable<String, String> env = new Hashtable<String, String>();

    private final SearchControls controls = new SearchControls();

    public static void main(String[] args) throws NamingException
    {
        final User user = new OpenLdapSearchAuthenticate().searchForUser("aeells");
        System.out.println("found user: " + user.getName());

        final boolean authenticated = new OpenLdapSearchAuthenticate().authenticateUser(user);
        System.out.println("user " + user.getName() + " authenticated: " + authenticated);
    }

    public OpenLdapSearchAuthenticate()
    {
        env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
        env.put(Context.PROVIDER_URL, "ldap://localhost:389/dc=my-domain,dc=com");

        controls.setSearchScope(SearchControls.SUBTREE_SCOPE);
        controls.setTimeLimit(5000);
        controls.setCountLimit(1);
    }

    public User searchForUser(final String userName) throws NamingException
    {
        // Needed for the Bind (User Authorized to Query the LDAP server)
        env.put(Context.SECURITY_AUTHENTICATION, "simple");
        env.put(Context.SECURITY_PRINCIPAL, "cn=Manager,dc=my-domain,dc=com");
        env.put(Context.SECURITY_CREDENTIALS, "*******");

        final DirContext context = new InitialDirContext(env);
        final NamingEnumeration result = context.search("", "(&(objectclass=person)(cn=" + userName + "))", controls);

        if (result.hasMore())
        {
            final SearchResult searchResult = (SearchResult) result.next();
            final Attributes attributes = searchResult.getAttributes();
            return new User(attributes.get("cn").toString(), attributes.get("userPassword").toString());
        }
        else
        {
            throw new IllegalArgumentException("user " + userName + " not found!");
        }
    }

    public boolean authenticateUser(final User user)
    {
        env.put(Context.SECURITY_AUTHENTICATION, "none");
        env.put(Context.SECURITY_PRINCIPAL, "cn=" + user.getName() + ",ou=people,dc=my-domain,dc=com");
        env.put(Context.SECURITY_CREDENTIALS, user.getPassword());

        try
        {
            new InitialDirContext(env);

            return true;
        }
        catch (NamingException e)
        {
            throw new IllegalArgumentException("user " + user.getName() + " not authenticated!");
        }
    }
}

public final class User
{
    private final String name;

    private final String password;

    public User(String name, String password)
    {
        this.name = name;
        this.password = password;
    }

    public String getName()
    {
        return name;
    }

    public String getPassword()
    {
        return password;
    }
}

```


Link: http://www.andrew-eells.com/2014/06/20/mac-os-x-10-9-openldap-install-search-and-authentication/

Error saat commnad `ldapadd`, **ldap_bind: Invalid credentials (49)**


Berhasil setelah menambahkan baris berikut di file

```
access to *
        by dn.exact="gidNumber=0+uidNumber=0,cn=peercred,cn=external,cn=auth" read
        by dn.exact="cn=manager,dc=somedomain,dc=org" read
        by * none
```

selain itu beberapa line juga ditambahakan:

```
include         /private/etc/openldap/schema/cosine.schema
include         /private/etc/openldap/schema/nis.schema
include         /private/etc/openldap/schema/inetorgperson.schema


modulepath     /usr/libexec/openldap
moduleload     back_bdb.la
 
```

Hasilnya... masin error:

```
> ldapadd -x -D "cn=manager,dc=somedomain,dc=org" -f initial2.ldif -W
Enter LDAP Password:
ldapadd: attributeDescription "dn": (possible missing newline after line 20, entry "dc=somedomain,dc=org"?)
ldapadd: attributeDescription "dn": (possible missing newline after line 21, entry "dc=somedomain,dc=org"?)
ldapadd: attributeDescription "dn": (possible missing newline after line 22, entry "dc=somedomain,dc=org"?)
ldapadd: attributeDescription "dn": (possible missing newline after line 23, entry "dc=somedomain,dc=org"?)
ldapadd: attributeDescription "dn": (possible missing newline after line 24, entry "dc=somedomain,dc=org"?)
ldapadd: attributeDescription "dn": (possible missing newline after line 25, entry "dc=somedomain,dc=org"?)
adding new entry "dc=somedomain,dc=org"
ldap_add: Type or value exists (20)
	additional info: objectClass: value #0 provided more than once
```

Setelah mengubah file LDIF dengan menambahkan beberapa line kosong agar mudah dibaca, suprisingly... berhasil:

```
> ldapadd -x -D "cn=manager,dc=somedomain,dc=org" -f initial2.ldif -W
Enter LDAP Password:
adding new entry "dc=somedomain,dc=org"

adding new entry "ou=people,dc=somedomain,dc=org"

adding new entry "cn=John Smith,ou=people,dc=somedomain,dc=org"

adding new entry "cn=Susan Adams,ou=people,dc=somedomain,dc=org"

adding new entry "cn=Bob Adams,ou=people,dc=somedomain,dc=org"

adding new entry "ou=groups,dc=somedomain,dc=org"

adding new entry "cn=registered_users,ou=groups,dc=somedomain,dc=org"
ldap_add: Object class violation (65)
	additional info: object class 'groupOfNames' requires attribute 'member'
```

```
> ldapsearch -x -b 'dc=somedomain,dc=org' '(objectclass=*)'
# extended LDIF
#
# LDAPv3
# base <dc=somedomain,dc=org> with scope subtree
# filter: (objectclass=*)
# requesting: ALL
#

# somedomain.org
dn: dc=somedomain,dc=org
objectClass: top
objectClass: dcObject
objectClass: organization
dc: somedomain
o: Some Org
description: A sample domain

# people, somedomain.org
dn: ou=people,dc=somedomain,dc=org
objectClass: top
objectClass: organizationalUnit
ou: people

# John Smith, people, somedomain.org
dn: cn=John Smith,ou=people,dc=somedomain,dc=org
objectClass: inetOrgPerson
cn: John Smith
sn: Smith
givenName: John
uid: jsmith
userPassword:: MDk4ZjZiY2Q0NjIxZDM3M2NhZGU0ZTgzMjYyN2I0ZjY=
mail: jsmith@somedomain.org
description: This is John

# Susan Adams, people, somedomain.org
dn: cn=Susan Adams,ou=people,dc=somedomain,dc=org
objectClass: inetOrgPerson
cn: Susan Adams
sn: Adams
givenName: Susan
uid: sadams
userPassword:: MDk4ZjZiY2Q0NjIxZDM3M2NhZGU0ZTgzMjYyN2I0ZjY=
mail: sadams@somedomain.org
description: This is Sue

# Bob Adams, people, somedomain.org
dn: cn=Bob Adams,ou=people,dc=somedomain,dc=org
objectClass: inetOrgPerson
cn: Bob Adams
sn: Adams
givenName: Bob
uid: badams
userPassword:: MDk4ZjZiY2Q0NjIxZDM3M2NhZGU0ZTgzMjYyN2I0ZjY=
mail: badams@somedomain.org
description: This is Bob

# groups, somedomain.org
dn: ou=groups,dc=somedomain,dc=org
objectClass: top
objectClass: organizationalUnit
ou: groups

# search result
search: 2
result: 0 Success

# numResponses: 7
# numEntries: 6
```


```
> ldapsearch -x -b 'cn=Susan Adams,ou=people,dc=somedomain,dc=org' -h localhost -p 389 '(objectclass=*)'
# extended LDIF
#
# LDAPv3
# base <cn=Susan Adams,ou=people,dc=somedomain,dc=org> with scope subtree
# filter: (objectclass=*)
# requesting: ALL
#

# Susan Adams, people, somedomain.org
dn: cn=Susan Adams,ou=people,dc=somedomain,dc=org
objectClass: inetOrgPerson
cn: Susan Adams
sn: Adams
givenName: Susan
uid: sadams
userPassword:: MDk4ZjZiY2Q0NjIxZDM3M2NhZGU0ZTgzMjYyN2I0ZjY=
mail: sadams@somedomain.org
description: This is Sue

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1
```
