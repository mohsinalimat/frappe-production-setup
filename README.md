# Frappe/ERPNext Production Setup Ubuntu 22.04

## Pre-requisites

    Python 3.10+
    Node.js 14+
    Redis 5                                       (caching and real time updates)
    MariaDB 10.3.x / Postgres 9.5.x               (to run database driven apps)
    yarn 1.12+                                    (js dependency manager)
    pip 20+                                       (py dependency manager)
    wkhtmltopdf (version 0.12.5 with patched qt)  (for pdf generation)
    cron                                          (bench's scheduled jobs: automated certificate renewal, scheduled backups)
    NGINX   
### STEP 1 Install git

*Git is the most commonly used version control system. Git tracks the changes you make to files, so you have a record of what has been done, and you can revert to specific versions should you ever need to. Git also makes collaboration easier, allowing changes by multiple people to all be merged into one source.*

    sudo apt-get install git
    
### STEP 2 install python-dev

*python-dev is the package that contains the header files for the Python C API, which is used by lxml because it includes Python C extensions for high performance.*
    
    sudo apt-get install python3-dev
    
### STEP 3 Install setuptools and pip (Python's Package Manager)

*Setuptools is a collection of enhancements to the Python distutils that allow developers to more easily build and distribute Python packages, especially ones that have dependencies on other packages. Packages built and distributed using setuptools look to the user like ordinary Python packages based on the distutils.*

*pip is a package manager for Python. It's a tool that allows you to install and manage additional libraries and dependencies that are not distributed as part of the standard library.*

    sudo apt-get install python3-setuptools python3-pip
 
### STEP 4 Install virtualenv

*virtualenv is a tool for creating isolated Python environments containing their own copy of python , pip , and their own place to keep libraries installed from PyPI. It's designed to allow you to work on multiple projects with different dependencies at the same time on the same machine.*

    sudo apt-get install virtualenv
    
CHECK PYTHON VERSION

    python3 -V
    
### For Python 3.10.x

    sudo apt install python3.10-venv
    
### STEP 5 Install MariaDB 10.3 stable package

    sudo apt-get install software-properties-common
    sudo apt install mariadb-server
    sudo mysql_secure_installation
 
#### Setting up DB Password

    Enter current password for root (enter for none): # PRESS ENTER
    OK, successfully used password, moving on...
  
  
    Switch to unix_socket authentication [Y/n] Y
    Enabled successfully!
    Reloading privilege tables..
    ... Success!

    Change the root password? [Y/n] Y
    New password: 
    Re-enter new password: 
    Password updated successfully!
    Reloading privilege tables..
    ... Success!

    Remove anonymous users? [Y/n] Y
    ... Success!

    Disallow root login remotely? [Y/n] Y
    ... Success!

    Remove test database and access to it? [Y/n] Y
    - Dropping test database...
    ... Success!
    - Removing privileges on test database...
    ... Success!

    Reload privilege tables now? [Y/n] Y
    ... Success!

### STEP 6 MySQL database development files

    sudo apt-get install libmysqlclient-dev
    
### STEP 7 Edit the mariadb configuration ( unicode character encoding )

    sudo nano /etc/mysql/my.cnf

add this to the my.cnf file after first 3 lines

     [server]
     user = mysql
     pid-file = /run/mysqld/mysqld.pid
     socket = /run/mysqld/mysqld.sock
     basedir = /usr
     datadir = /var/lib/mysql
     tmpdir = /tmp
     lc-messages-dir = /usr/share/mysql
     bind-address = 127.0.0.1
     query_cache_size = 16M
     log_error = /var/log/mysql/error.log

     [mysqld]
     innodb-file-format=barracuda
     innodb-file-per-table=1
     innodb-large-prefix=1
     character-set-client-handshake = FALSE
     character-set-server = utf8mb4
     collation-server = utf8mb4_unicode_ci
     
add this file to the bottom

    [mysql]
    default-character-set = utf8mb4
    
#### Now press (Ctrl-X) to exit

    sudo service mysql restart

























