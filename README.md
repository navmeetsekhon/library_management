# Frappe-Library-Management-Version-14--in-Ubuntu-22.04-LTS
A complete Guide to Install Frappe version 14 and create library management system in Ubuntu 22.04 LTS



### Pre-requisites 

      Python 3.6+
      Node.js 14+
      Redis 5                                       (caching and real time updates)
      MariaDB 10.3.x / Postgres 9.5.x               (to run database driven apps)
      yarn 1.12+                                    (js dependency manager)
      pip 20+                                       (py dependency manager)
      wkhtmltopdf (version 0.12.5 with patched qt)  (for pdf generation)
      cron                                          (bench's scheduled jobs: automated certificate renewal, scheduled backups)
      NGINX                                         (proxying multitenant sites in production)

![image](https://user-images.githubusercontent.com/64701624/228231508-e67437df-c27d-41c6-b524-faf9fd506299.png)


### STEP 1 Install git
    sudo apt-get install git

### STEP 2 install python-dev

    sudo apt-get install python3-dev

### STEP 3 Install setuptools and pip (Python's Package Manager).

    sudo apt-get install python3-setuptools python3-pip

### STEP 4 Install virtualenv
    
    sudo apt-get install virtualenv
    sudo apt install python3.10-venv
    

### STEP 5 Install MariaDB

    sudo apt-get install software-properties-common
    sudo apt install mariadb-server
    sudo mysql_secure_installation
    
    
      In order to log into MariaDB to secure it, we'll need the current
      password for the root user. If you've just installed MariaDB, and
      haven't set the root password yet, you should just press enter here.

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

 
    
    
    
### STEP 6  MySQL database development files

    sudo apt-get install libmysqlclient-dev

### STEP 7 Edit the mariadb configuration ( unicode character encoding )

    sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf

add this to the 50-server.cnf file

    
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
     
     [mysql]
     default-character-set = utf8mb4

Now press (Ctrl-X) to exit

    sudo service mysql restart

### STEP 8 install Redis
    
    sudo apt-get install redis-server

### STEP 9 install Node.js 14.X package

    sudo apt install curl 
    curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
    source ~/.profile
    nvm install 14

### STEP 10  install Yarn

    sudo apt-get install npm

    sudo npm install -g yarn

### STEP 11 install wkhtmltopdf

    sudo apt-get install xvfb libfontconfig wkhtmltopdf
    

### STEP 12 install frappe-bench

    sudo -H pip3 install frappe-bench==5.10.1
    
    bench --version
    
### STEP 13 initilise the frappe bench & install frappe latest version 

    bench init frappe-bench --frappe-branch version-14
    
    cd frappe-bench/
    bench start
    
### STEP 14 create an app in frappe bench 
    
    To create our Library Management app, run the new-app command:
    
    $ bench new-app library_management
      App Title (default: Library Management):
      App Description: Library Management System
      App Publisher: Faris Ansari
      App Email: faris@example.com
      App Icon (default 'octicon octicon-file-directory'):
      App Color (default 'grey'):
      App License (default 'MIT'):
      'library_management' created at /home/frappe/frappe-bench/apps/library_management

      Installing library_management
      $ ./env/bin/pip install -q -U -e ./apps/library_management
      $ bench build --app library_management
      yarn run v1.22.4
      $ FRAPPE_ENV=production node rollup/build.js --app library_management
      Production mode
      ✔ Built js/moment-bundle.min.js
      ✔ Built js/libs.min.js
      ✨  Done in 1.95s.
      
      You will be prompted with details of your app, fill them up and an app named library_management will be created in the apps folder.
    
### STEP 15 Create a new site 
    
    To create a new site, run the following command from the frappe-bench directory:
    
      $ bench new-site library.test
      MySQL root password:

      Installing frappe...
      Updating DocTypes for frappe        : [========================================] 100%
      Updating country info               : [========================================] 100%
      Set Administrator password:
      Re-enter Administrator password:
      *** Scheduler is disabled ***
      Current Site set to library.test
      
      Now, you will have a new folder named library.test in the sites directory.

      This command will create a new database, so you need to enter your MySQL root password. It will also ask to set the password for the                     Administrator user, just set a password that you won't forget. This will be useful later.

      Now, you will have a new folder named library.test in the sites directory.


  ### STEP 16 Access the site
      
      We need to tell bench to use our site which is library.test by using this command.
      
      $ bench use library.test
      
      Access it on http://localhost:8000 in our browser.
      
   ### STEP 17 Install app on site
   
     To install our Library Management app on our site, run the following command:

      $ bench --site library.test install-app library_management

      Installing library_management...
      
      
     To confirm if the app was installed, run the following command:
     
      $ bench --site library.test list-apps
      frappe
      library_management
     You should see frappe and library_management as installed apps on your site.

   ### STEP 18 Login to Desk 
      Go to http://library.test:8000 and it should show you a login page.

      Enter Administrator as the user and password that you set while creating the site.
      
   ### STEP 19 Creating a DocType
      Before we can create DocTypes, we need to enable developer mode on our bench.
    
