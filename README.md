# Setup Ubuntu Server in Raspberry Pi 4
1. Download Raspberry Pi Imager: https://www.raspberrypi.com/software/
2. Insert SD card and run the Raspberry pi Imager.
3. Choose your SD card as your device.
4. Choose "Ubuntu Server (64bit)" under the "Other general purpose OS".
5. Continue to flash the OS to the SD card. Initial configuration setting for the server such as SSH can be used to remotely mange your server.
6. Insert SD card in Raspberry Pi and turn it on.
7. In your router, find the IP address of the Raspberry Pi.
8. Using Command prompt, remotely log in to your Ubuntu server using initial SSH credentials and Raspberry Pi IP address. Make sure that the computer you are using and the Raspberry Pi has the same network. In terminal you can type this:
   > ssh username@Raspberry Pi IP.
9. After logging-in update and upgrade your server.
   sudo apt-get update
   sudo apt-get upgrade

# Installing PostgreSQL and Creating Roles
1. Install PostgreSQL.
   sudo apt install postgresql postgresql-contrib
2. Login to PostgreSQL and go to postgres prompt.
   sudo su - postgres
   psql
3. Change the password of the default admin user "postgres".
   ALTER USER postgres WITH PASSWORD "yournewpassword";
4. Create user aside from 'postgres'.  It is advisable to use the admin user 'postgres' to management only.
   CREATE USER yourusername WITH PASSWORD 'yourpassword';
5. Create a database.
   CREATE DATABASE databasename;
6. Grant all to the new user the access to new database.
   GRANT ALL PRIVILEGES ON DATABASE databasename TO yourusername;
7. Exit the postgres prompt.
   \q

# Configure PostgreSQL for remote access
1. Access the PostgreSQL configuration file. The number '12' depends on the PostgreSQL version you have.
   sudo nano /etc/postgresql/12/main/postgresql.conf
2. Update the connection settings by uncommenting the line "#listen_address = 'localhost'" and changin the localhost to '*'.
   listen_address = '*'
3. Save and exit in the configuration file.
4. Verify the changes.
   sudo service postgresql restart
   ss -nlt | grep 5432
6. Access onother configuration file for remotely accessing the PostgreSQL.
   sudo nano /etc/postgresql/12/main/pg_hba.conf
7. On the bottom line add your user and database, IP address allowed to access the database and the encryption method. Use tab to separate inputs.
   TYPE: host
   DATABASE: databasename
   USER: yourusername
   ADDRESS: 0.0.0.0/0
   METHOD: md5
8. The configuration above means that you are allowing yourusername to access the databasename from any IP address with encryption method of md5. The IP address can be set depending on your allowable IP.  You can still allow all IP address here and deny IP addresses on firewall.
9. Restart PostgreSQL
   sudo service postgresql restart
10. Enable PostgreSQL to run after boot.
    sudo systemctl enable PostgreSQL

    
