# PHP Applications

We have tested Platform.sh extensively with Drupal and Symfony applications, but its 
versatility allows it to support any PHP application (people have been running 
Wordpress, CiviCrm, Laravel Applications, Magento and Prestashop on it as well as
custom built PHP applications), in this section we provide information specific
to the configuration and operation of specific Frameworks and Toolstacks.

It is noteworthy that for Drupal and Symfony Platform.sh provides specific 
tooling to help you be more productive (through built-in composer support or 
built-in drush support).

> **note**
> Find many examples for those configuration files on the [platformsh-examples repository on 
Github](https://github.com/platformsh/platformsh-examples).
> with many examples for Drupal, Symfony, Custom PHP Applications (with multi apps), Wordpress, Magento, Prestashop, Laravel...
Platform.sh supports custom PHP configurations.

## Configuring PHP

You can choose which version of PHP you want to run in your `.platform.app.yaml` file, currently
supported versions: PHP 5.4 (default), PHP 5.5 and PHP 5.6.

Example `.platform.app.yaml`:

```yaml
name: front
type: php:5.6
```

### PHP extensions

You can define the PHP extensions you want to enable or disable:

```yaml
# .platform.app.yaml
runtime:
    extensions:
        - http
        - redis
        - ssh2
    disabled_extensions:
        - sqlite3
```

The following extensions are enabled by default: *pdo, mysql, mysqli, mysqlnd,
pdo_mysql, pdo_sqlite, sqlite3, gd, curl, intl, mcrypt and
zendopcache/opcache*. You can disable those by adding them to the
`disabled_extensions` list.

This is the complete list of extensions that can be enabled: *apcu, blackfire,
curl, enchant, gd, gearman, geoip, gmp, http (5.4 and 5.5 only), imagick, imap,
intl, json (added in 5.6), ldap, mcrypt, memcache, memcached, mongo, msgpack
(added in 5.6), mysql, mysqli, mysqlnd, opcache (5.5 and up), pdo, pdo_mysql,
pdo_pgsql, pdo_sqlite, pecl-http (added in 5.6), pgsql, pinba, propro (added in
5.6), pspell, raphf (added in 5.6), readline, recode, redis, snmp, sqlite3,
ssh2, tidy, xdebug, xhprof, xmlrpc, xsl, and zendopcache (5.4 only)*.

> **note**
> You can check out the output of `ls /etc/php5/mods-available` to
> see the up-to-date complete list of extensions after you SSH into
> your environment.

### Use your own php.ini

You can also create and push a `php.ini` file that will be appended to
the configuration maintained by Platform.sh.

In your repository, the `php.ini` file should be added to the root of
the application (normally the repository root).

For example, if you need to increase the PHP memory limit:

```
; php.ini
; Increase PHP memory limit
memory_limit = 256M
```

After pushing your file, you can check that the custom PHP configuration
has been added to your environment by logging in via SSH and type:

```
web@kjh43kbobssae-master--php:~$ cat /etc/php5/fpm/php.ini

; =============== Custom configuration from `/app/php.ini`
; Increase PHP memory limit
memory_limit = 256M
```

------------------------------------------------------------------------

> **warning**
>
> We do not limit what you can put in your `php.ini` file, but many
> settings can break your application. This is a facility for advanced
> users.

