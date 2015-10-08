

`brew install openldap`

```
==> Installing openldap from homebrew/homebrew-dupes
==> Downloading ftp://ftp.openldap.org/pub/OpenLDAP/openldap-release/openldap-2.4.39.tgz
######################################################################## 100.0%
==> ./configure --prefix=/usr/local/Cellar/openldap/2.4.39 --sysconfdir=/usr/local/etc --localstatedir=/usr/local/var --enable-bdb=no --enable-hdb=no

...dihapus...

```



```
cd /private/etc/openldap/
cp ldap.conf.default ldap.conf
cp slapd.conf.default slapd.conf 
```

`slappasswd -s yourpassword`

edit slapd.conf dan ubah line 

```
rootpw			secret
```

menjadi

```
rootpw          {SSHA}0/QFDqWP7vigXWqeGx9LDmE4hP04qUm7
```


ldap.conf

```
BASE	dc=my-domain,dc=com
URI	ldap://localhost:389
```

```
cp /private/var/db/openldap/openldap-data/DB_CONFIG.example /private/var/db/openldap/openldap-data/DB_CONFIG
```

Jalankan slapd di terminal lain dengan perintah `slapd -d3 &`
opsi `-d` menunjukan level debug.

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
