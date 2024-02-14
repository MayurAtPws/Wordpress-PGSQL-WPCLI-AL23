# Using WP CLI for tweaking WordPress
resource : https://wp-cli.org/

## Installation

- Download and give executable priviliges

        sudo curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar

        #check if its working or not
        sudo php wp-cli.phar --info

        sudo chmod +x wp-cli.phar

- Move to $PATH

        sudo mv wp-cli.phar /usr/local/bin/wp


- Update and check 

        wp cli update

        wp --info


## Installing a WP Theme via WP_CLI


resource : [click here ](https://www.inmotionhosting.com/support/edu/wordpress/wp-cli/install-a-theme-using-wp-cli/)

- navigate to the wordpress root dir

        cd /var/www/html

- Download the Theme zip file from web or Download via CLI itself

        wp theme search <theme name>
        # example :  wp theme search hestia

        -----------------------------------------

        Success: Showing 1 of 1 themes.
        +--------+--------+--------+
        | name   | slug   | rating |
        +--------+--------+--------+
        | Hestia | hestia | 96     |
        +--------+--------+--------+

        -----------------------------------------

- Use the **Slug value** from the search results and use 

        wp theme install activate <slug> 
        # example : wp theme activate install hestia 
        # remove the "activate to just install and not activate it"

(or)

- Download the zip to the wordpress root folder and us ehis command


        wp theme install activate <theme.zip>
        # example : wp theme install activate hestia.1.1.56.zip


- List all the themes 


         sudo -u apache wp theme list

        #example 
        +-------------------+----------+--------+---------+----------------+-------------+
        | name              | status   | update | version | update_version | auto_update |
        +-------------------+----------+--------+---------+----------------+-------------+
        | hestia            | active   | none   | 3.1.3   |                | off         |
        | twentytwentyfour  | inactive | none   | 1.0     |                | off         |
        | twentytwentythree | inactive | none   | 1.3     |                | off         |
        | twentytwentytwo   | inactive | none   | 1.6     |                | off         |
        +-------------------+----------+--------+---------+----------------+-------------+


- help command 

        wp help theme


