VFF Demo Project using Angular Material
-----------------

### Requirements:
- Build with AngularJS 1.
- Use Angular Material.
- Grid responsive should be handled by Angular Material. (Please observe Code Pen examples provided on Angular Material site.)
- Create Mouse-over behavior shown in mockups.
- Menu responsive should be handled by Angular Material. (Please observe Code Pen examples provided on Angular Material site or reference other Angular Material Admin panel templates.)
- Create Sub-menus according to mockups.
- Login pop-up and form should work. (Submitting login form will close the login pop-up.)

### Utilities:
[Angular Material](https://material.angularjs.org/latest/)
 
### Animation References:
[Mikiya Kobayashi](http://www.mikiyakobayashi.com)
 
### Fonts:
**Grid Text:** Helvetica Neue roman.

**Drop Menu and Footer:** Helvetica Neue Lt light.



Magento Basics
===

[Magento Commands](#mage-commands)
----------------------------------------
- php bin/magento [module:status](http://devdocs.magento.com/guides/v2.0/install-gde/install/cli/install-cli-subcommands-enable.html)
- php bin/magento [indexer:reindex](http://devdocs.magento.com/guides/v2.1/config-guide/cli/config-cli-subcommands-index.html)
- php bin/magento setup:update
- php bin/magento setup:static-content:deploy
- php bin/magento setup:di:compile
- php bin/magento [maintenance:enable](http://devdocs.magento.com/guides/v2.0/install-gde/install/cli/install-cli-subcommands-maint.html)

[How to Backup and Restore](#mage-backup)
----------------------------------------
## Backup
Terminal command (use Putty):

```php bin/magento setup:backup [--code] [--media] [--db]```

1. Puts the store in maintenance mode.
2. Executes one of the following command options:

| Option  | Meaning	                                            | Filename/Location                            |
| ------- |-----------------------------------------------------| ---------------------------------------------|
| --code  | Backs up Mage files (excluding var and pub/static). | var/backups/<timestamp>_filesystem.tgz       |
| --media | Backs up pub/media.                                 | var/backups/<timestamp>_filesystem_media.tgz |
| --db    | Backs up Mage db.                                   | var/backups/<timestamp>_db.sql               |


**Best Practices:**

Flush cache and disable prior to starting backup!

### Config and Run Cron

In cPanel, go to Cron Jobs. Set time. Add command:

```/usr/local/bin/php -c /opt/cpanel/ea-php70/root/etc/php.ini /home/site/public_html/bin/magento setup:backup --db >> /home/site/public_html/var/log/backup.cron.log```

## Rollback (Restore)
Open Putty and login. Terminal command:

```php bin/magento setup:rollback [-c|--code-file="<name>"] [-m|--media-file="<name>"] [-d|--db-file="<name>"]```

**Best Practices:**

1. Save [app/etc/env.php]() _prior_ to rollback.

2. Rollback database _first_.

	```php bin/magento setup:rollback -d 1234567890_db.gz]```

	Or:

	```php bin/magento setup:rollback --db-file="1234567890_db.gz"]```


3. Rollback media next.

	```php bin/magento setup:rollback -m 1234567890_filesystem_media.tgz```

	Or:

	```php bin/magento setup:rollback --media-file="1234567890_filesystem_media.tgz"```

4. Rollback code last.

	```php bin/magento setup:rollback -c 1234567890_filesystem_code.tgz```

	Or:

	```php bin/magento setup:rollback --code-file="1234567890_filesystem_code.tgz"```

**Note:** Restore will overwrite ENV.php file, which stores the db login configuration. Save locally prior to restore.


[Permissions](#mage-permissions)
------------------------------------------
- **All directories have 770 permissions.**
	Gives full control (read/write/execute) to owner and group, and no permissions to anyone else.

- **All files have 660 permissions.**
	Owner and group can read and write, but other users have no permissions.

- The Mage admin must have full control (read/write/execute) of all files and directories.

- Users must have write access to the following files and directories:
	- [var]()
	- [app/etc]()
	- [pub]()

```cd <Magento install dir>```

```find . -type f -exec chmod 644 {} \;```

```find . -type d -exec chmod 755 {} \;```

```find ./var -type d -exec chmod 777 {} \;```

```find ./pub/media -type d -exec chmod 777 {} \;```

```find ./pub/static -type d -exec chmod 777 {} \;```

```chmod 777 ./app/etc```

```chmod 644 ./app/etc/*.xml```


[phpMyAdmin Database Tables](#mage-db)
------------------------------------------
Open: *Hosting / WebHost Manage / cPanel / Databases / phpMyAdmin*.

1. ```core_config_data```
	- Change to dev. for http and https (col 2 and 3)
	- ```php bin/magento setup:upgrade```
2. ```setup_module```
	- Delete module causing issue.
