# How to Setup MySQL Master Slave Replication

## Install MySQL Server on Both Nodes
    # apt install mysql-server

### Configure MySQL on Master Node
  ## By default, MySQL listens on the localhost and doesn't allow connection from the remote system
    # vim /etc/mysql/mysql.conf.d/mysqld.cnf
      bind-address = Your-Master-IP
### Next, uncomment the following line:
    # server-id = 1
### Next, add the following lines at the end of the file:
    # log_bin = /var/log/mysql/mysql-bin.log
### Save and close the file then restart the MySQL service to apply the changes:
    # systemctl restart mysql
    
### Create a Replication User on Master Node
   ## Create a replication user with the following command:
    # mysql> CREATE USER 'replication_user'@'your-slave-ip' IDENTIFIED BY 'password';
### grant all privileges to replication slave with the following command:
    # mysql> GRANT REPLICATION SLAVE ON *.* TO 'replication_user'@'your-slave-ip';
### flush the privileges with the following command:
    # mysql> FLUSH PRIVILEGES;
### Next, verify the Master status with the following command:
    # mysql> SHOW MASTER STATUS\G 
    
    File: mysql-bin.000005
    Position: 157
    Binlog_Do_DB: mydb1
    
    note down the mysql-bin.000005 value and the Position ID 157. You will need both to set up a slave server.
### Configure the Slave Node
    uncomment and change the server-id value and follow same steps, whatever you followed while master configuration
    # server-id = 2

### Run the following command to allow the slave server to replicate the Master server:
    CHANGE REPLICATION SOURCE TO
    SOURCE_HOST='*.*.*.*',
    SOURCE_USER='username',
    MASTER_PORT=3306,
    SOURCE_PASSWORD='####',
    SOURCE_LOG_FILE='mysql-bin.******',
    SOURCE_LOG_POS=*****,
    GET_MASTER_PUBLIC_KEY=1;
### start the SLAVE with the following command:
    # mysql> START SLAVE;
    # mysql> show slave status\G;

  Create db in master and check they can replicate data or not.

    
