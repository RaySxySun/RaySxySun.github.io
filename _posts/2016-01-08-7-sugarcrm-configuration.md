---
layout: post
title: 7. php & sugarcrm  configuration
date: 2016-01-14
categories: sugarcrm
tags: [php, composer]
description: To understand python class and metaclass
---

# 1. Adding submodule to Mango/xxxxxx/xxxxxx

    #cd submodule folder
    git submodule add git@github.com:[username]/xxxx.git Mango/xxxxxx/xxxxxx
    git submodule init
    git submodule sync
    git submodule update


# 2. Install module mcrypt required by composer install

>   Problem 1
    - Installation request for onelogin/php-saml 2.5.0 -> satisfiable by onelogin/php-saml[2.5.0].
    - onelogin/php-saml 2.5.0 requires ext-mcrypt * -> the requested PHP extension mcrypt is missing from your system.


    sudo apt-get install mcrypt php5-mcrypt
    sudo php5enmod mcrypt
    php -i | grep mcrypt
    sudo php -m

    #In oder to use command composer globally

    # 1 put composer.phar into /usr/local/bin/
    # 2 rename it
    # 3 sudo mv composer.phar composer

>    #downloading composer could be slow because of the wall
     #visit [official wormhole](https://getcomposer.org/download/) [mirror wormhole](http://wenda.golaravel.com/question/18)

command|description
---|---
php -m| show enabled modules
php -i| show all info
php5enmod| enable module (location: /etc/php5/mods-available/)
sudo install pear| add packages manager for php (EX. installing ibm_db2)
sudo apt-get install -y php5-dev| before install ibm_db2 by pecl (dependency: phpsize)





******************** [ Repairing... ] ********************
PHP Warning:  require_once(/home/ohraymaster/www/sugarcrm/config.php): failed to open stream: Permission denied in /home/ohraymaster/www/sugarcrm/include/entryPoint.php on line 82
PHP Fatal error:  require_once(): Failed opening required 'config.php' (include_path='/home/ohraymaster/www/sugarcrm:/home/ohraymaster/www/sugarcrm/vendor:.:/usr/share/php:/usr/share/pear') in /home/ohraymaster/www/sugarcrm/include/entryPoint.php on line 82


******************** [ Rebuilding js code... ] ********************
PHP Warning:  require_once(/home/ohraymaster/www/sugarcrm/config.php): failed to open stream: Permission denied in /home/ohraymaster/www/sugarcrm/include/entryPoint.php on line 82
PHP Fatal error:  require_once(): Failed opening required 'config.php' (include_path='/home/ohraymaster/www/sugarcrm:/home/ohraymaster/www/sugarcrm/vendor:.:/usr/share/php:/usr/share/pear') in /home/ohraymaster/www/sugarcrm/include/entryPoint.php on line 82
[install_hook after_run_install] error. Exit code: [255]

> ==no ibm_db2 cause PHP Fatal error==
******************** [ Repairing... ] ********************
PHP Warning:  Illegal string offset 'ibm_ClientRestricted' in /home/ohraymaster/www/sugarcrm/config_override.php on line 44
PHP Warning:  Illegal string offset 'default' in /home/ohraymaster/www/sugarcrm/config_override.php on line 45
PHP Fatal error:  Call to undefined function db2_pconnect() in /home/ohraymaster/www/sugarcrm/include/database/IBMDB2Manager.php on line 567
PHP Fatal error:  Call to undefined function db2_pconnect() in /home/ohraymaster/www/sugarcrm/include/database/IBMDB2Manager.php on line 567


******************** [ Rebuilding js code... ] ********************
PHP Warning:  Illegal string offset 'ibm_ClientRestricted' in /home/ohraymaster/www/sugarcrm/config_override.php on line 44
PHP Warning:  Illegal string offset 'default' in /home/ohraymaster/www/sugarcrm/config_override.php on line 45
PHP Fatal error:  Call to undefined function db2_pconnect() in /home/ohraymaster/www/sugarcrm/include/database/IBMDB2Manager.php on line 567
PHP Fatal error:  Call to undefined function db2_pconnect() in /home/ohraymaster/www/sugarcrm/include/database/IBMDB2Manager.php on line 567
[install_hook after_run_install] error. Exit code: [255]

Process finished with exit code 1


# 3. steps of CI

Step|description
--|--
'init'|
'prepare_code_for_pr'|
'codeStyle_check'|
'before_install'|
'after_run_install'|
'after_install'|
