## 02 Les prérequis
 - Trouver les prérequis

insite:mantisbt.org mantis server requisite
PHP extensions

MantisBT is designed to work in as many environments as possible. Hence the required extensions are minimal and many of them are optional affecting only one feature.
Mandatory extensions
- The extension for the RDBMS being used ( mysqli with mysqlnd, pgsql, oci8, sqlsrv )
- _mbstring_ - Required for Unicode (UTF-8) support.
- _ctype, filter, hash, json, session, tokenizer_ - Required to run MantisBT in general. These are bundled with PHP, and enabled by default. Note that _hash_ is a core extension since PHP 7.4.0, and _json_ is a core extension since PHP 8.0.0.


## Configuration
mariadb mysql_secure_installation :
 - on va se créer un user root de la base de donnée
 - 


CREATE DATABASE mantis_db;

CREATE USER 'mantis_db_admin'@'%' IDENTIFIED BY 'mot_de_passe_solide';

GRANT ALL PRIVILEGES ON mantis_db.* TO 'mantis_db_admin'@'%';

FLUSH PRIVILEGES;