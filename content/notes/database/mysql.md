+++
title = 'MySQL'
date = 2024-02-11

+++

### MySQL administration

- `sudo mysql_secure_installation utility` : this utility prompts to define the mysql root password and other security-related options.
- If iptables is enabled and want to connect to the MySQL db from another machine, open a port in the server's firewall. Run the following command:
  - `sudo ufw enable`
  - `sudo ufw allow mysql`
- `sudo systemctl start mysql` : Start the MySQL service
- `sudo systemctl enable mysql` : To ensure that the database server launches after a reboot, run the following command
- `mysql -u root -p`
- Set the root password:
  - `UPDATE mysql.user SET authentication_string = PASSWORD('password') WHERE User = 'root';`
- To make the change take effect, reload the stored user information with the following command: `FLUSH PRIVILEGES`
- If application connects remotely, the host entry that MySQL looks for is the IP address or DNS hostname of the remote computer.
- Give the newly created user full permissions for a database by running the following command: `GRANT ALL PRIVILEGES ON demodb.* to demouser@localhost`
- To verify that those privileges are set, run the following command: `SHOW GRANTS FOR 'demouser'@'localhost';`
- By default MySQL's configuration file resides in `/etc/mysql`
- Default options are read from the following files in the given order: `/etc/my.cnf` `/etc/mysql/my.cnf` `/usr/etc/my.cnf` `~/.my.cnf`
- By default MySQL stores its log files in the directory: `/var/log/mysql`
- Behind the scenes there are actually two versions of the MySQL server, `mysqld` and `mysqld_safe`.
- `mysqld_safe` launches with a few more safety features enabled to make it easier to recover from a crash.
- `mysqladmin` tool lets perform some administrative functions from the command line.
- Two main options for backups are copying the database files and using `mysqldump`
- `service mysql start` : for starting the service.
- `mysql -V`
- `mysql -u root -p --host=127.0.0.1 --port=3310` : connecting to the docker instance
- `sudo docker run --name mysql-55-container -p 127.0.0.1:3310:3306 -e MYSQL_ROOT_PASSWORD=rootpassword -d mysql:5.5`

### File copy

- By default MySQL creates a directory for each database in its data directory `/var/lib/mysql`
- Approach to take is to lock the database as read-only for the duration of the copy so that applications can still read the data while backing up the files.
- Lock the database to read-only by running : `mysql -u root -p -e "FLUSH TABLES WITH READ LOCK;"`
- To unlock the db : `mysql -u -root -p -e "UNLOCK TABLES;"`

#### mysqldump

- `mysqldump` generates a text file that represents the database. By default the text file contains a list of SQL statements used to recreate a database.
- `mysqldump -u root -p demodb > dbbackup.sql`
- We can include the password directly after "-p" in a script like -p"password"
- for restoring a mysqldumped database : `mysql -u root -p demodb < dbbackup.sql`
- by using the `-add-drop-table` option with the command that creates the mysqldump causes it to add a command to the backup files it writes that will drop tables before recreating them.

#### Database engine

- The engine is the process that's churning away behind the scenes, writing to and reading data from files.
- To see the engine used by your database's tables run in MySQL shell : `SHOW TABLE STATUS FROM demodb;`
- InnoDB is more fault-tolerant that MyISAM.
- certain types of searches perform better on MyISAM than InnoDB
