# Installing & Configuring Wordpress + PostgreSQL on AL23

## Table of Contents

- [Installing WP & PGSQL](#installing-wp-and-pgsql)
- [Change User Details using DB](./modifyingUSerData.md)
- [Using WP_CLI to configure wordpress](./wpCli.md)


## Installing WP and PGSQL

resource (wp + mysql , but we need pgsql): [CLICK HERE](https://docs.aws.amazon.com/linux/al2023/ug/hosting-wordpress-aml-2023.html#hosting-wordpress-prereqs-2023)

- Install necessary packages

            dnf install -y php8.2-{8.2.9-1.amzn2023.0.3.x86_64,common,gd,devel,xml,mbstring,zip,pdo,pgsql,fpm} php-mysqlnd php-pear vim httpd git wget patch libzip git unzip postgresql15.x86_64 postgresql15-server postgresql-devel

- Start the Web Server


        systemctl start httpd
        systemctl enable httpd


- Download Latest Wordpress

        wget https://wordpress.org/latest.tar.gz

    (or)

    Download Specfic Version

        wget https://wordpress.org/wordpress-6.4.2.tar.gz

- Extract it 

        tar -xzf latest.tar.gz

- Move it to your web server root 

        mv wordpress/* /var/www/html/

        sudo chown -R apache:apache /var/www/html/

## Configuring PGSQL

resource : https://hbayraktar.medium.com/how-to-install-postgresql-15-on-amazon-linux-2023-a-step-by-step-guide-57eebb7ad9fc

PG4WP : https://github.com/PostgreSQL-For-Wordpress/postgresql-for-wordpress

        postgresql-setup --initdb
        sudo systemctl start postgresql
        sudo systemctl enable postgresql

- Changing Password

        # Change the ssh user password:
        sudo passwd postgres

        # Log in using the Postgres system account:
        su - postgres

        # Now, change the admin database password:
        psql -c "ALTER USER postgres WITH PASSWORD 'your-password';"

        exit

Create a User & Database

        # Connect to the PostgreSQL server as the Postgres user:
        sudo -i -u postgres psql

        # Create a new database user:
        CREATE USER yourusername WITH PASSWORD 'password';

        # Create a new database:
        CREATE DATABASE database_name;

        # Grant all privileges on the database to the user:
        GRANT ALL PRIVILEGES ON DATABASE <DBNAME> TO <UNAME>;

        GRANT ALL PRIVILEGES ON SCHEMA public TO <UNAME>;

        ALTER SCHEMA public OWNER TO <UNAME>;

        ALTER DATABASE <DBNAME> OWNER TO <UNAME>;


        # To list all available PostgreSQL users and databases:
        \l

        # To quit from pqsql
        \q


## Configuring PG4WP

resource : https://github.com/PostgreSQL-For-Wordpress/postgresql-for-wordpress

                cd /var/www/html

                cd wp-content // go to wp-content

                git clone https://github.com/PostgreSQL-For-Wordpress/postgresql-for-wordpress.git

                mv postgresql-for-wordpress/pg4wp pg4wp

                cp pg4wp/db.php db.php

                rm -rf postgresql-for-wordpress

                
## Configuring Db Details 

        cd /var/www/html

        cp wp-config-sample.php wp-config.php

        nano wp-config.php

        -------------------------------------------

        /** The name of the database for WordPress */
        define('DB_NAME', 'wpdb');

        /** MySQL database username */
        define('DB_USER', 'wpuser');

        /** MySQL database password */
        define('DB_PASSWORD', 'Wp@123#');

        -------------------------------------------

- Optional - Enable dev mode if you are in dev env , this will help in debugging

        -------------------------------------------

        define( 'WP_DEBUG', true );
        
        -------------------------------------------
## Configuring PGSQL auth 

- login to gqsql
  
        sudo -u postgres psql

- find the Config File

        SHOW config_file;

        #/var/lib/pgsql/data/postgresql.conf

- Exit

        \q


- Go to that folder 

         cd /var/lib/pgsql/data

- Edit the file

        nano pg_hba.conf

- Go to the end of the file and change these (ident to md5, peer to md5)

        -------------------------------------------

        # "local" is for Unix domain socket connections only

        local   all             all                                     md5
        # IPv4 local connections:
        host    all             all             127.0.0.1/32            md5
        # IPv6 local connections:
        host    all             all             ::1/128                 md5

        -------------------------------------------
- (optional ) if u want to allow remote db connections
- 
        -------------------------------------------
        # IPv4 remote connections from any IP address
        host    all             all             0.0.0.0/0               md5
        -------------------------------------------

- Reload pqsql

        sudo systemctl reload postgresql


### Change User Details : [Click here](./modifyingUSerData.md)

### Using WP_CLI : [Click here](./wpCli.md)
