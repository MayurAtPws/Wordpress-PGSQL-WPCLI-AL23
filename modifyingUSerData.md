

# Changing user's Password / uname / email in database 

- Login as postgres

        su - postgres

- Login to psql as owner of wpdb

        psql -U wpuser -d wpdb

- Set the new password

        UPDATE wp_users SET user_pass = MD5('new_password') WHERE user_login = 'username' OR user_email = 'username';

- Search for the uname /email

        SELECT * FROM wp_users WHERE user_login = 'username';

        SELECT * FROM wp_users WHERE user_email = 'email@example.com';


- Note the user ID and change it based on that 

        UPDATE wp_users SET user_login = 'new_username' WHERE ID = user_id;

        UPDATE wp_users SET user_email = 'new_email@example.com' WHERE ID = user_id;

