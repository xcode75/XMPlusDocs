# Basic Settings

#### Shared Account

As the name suggest, a shared account you want to display on user daehboard for free use.

You can add the account in the Accounts section.

#### Ideal time logout (s)

This is the time to automatically logout the user if inactive on the site. 0 (in seconds) is disabled for this option.

#### Enable Languages

The languages you want to translate the website into.

#### Adding other language

Go to your site localization directory  such as: /www/wwwroot/tld.com/localization

Make a copy of the en_US.php file to the language you want to translate.

Example fa_IR.php for Persian

Translate only the values and not the keys of the array.

Do not translate any word in %%, eg, %name%.

NB: eg, add in all the language files fa_IR

```
<?php
$i18n['fa_IR'] = array(
    'en_US'     => 	'English (US)',
    'zh_CN'     => 	'中文 (CN)',
    'fa_IR'     => 	'Persian (IR)',
);	
```

> `Enable Language` Go to admin settings > Basic settings and chose the language Persian and submit.

## General Settings

> `Site URL` : https://tld.com  this must be changed in /www/wwwroot/tld.com/config/config.php

> `Sub URL`: https://tld.com/link/

> `Password Salt`:  is a key added to user login password for security

> `Hashing`: method for generating user login password

> `Password Mode`:

- User defined + secured (must follow the password recommendation format to create password)
- User defined (whatever password the user chose)


### CheckIn Settings - Telegram

This is for users who bind their telegram account to the site can get free traffic and days added as set by the site admin.

#### Restrict Settings

For who can access your site based on their ip and location.


#### Fallback Config

Refere to Xray documentation [Fallback](https://xtls.github.io/Xray-docs-next/config/features/fallback.html)

### DNS Rules 

Refere to Xray documentation [DNS](https://xtls.github.io/Xray-docs-next/config/dns.html)


