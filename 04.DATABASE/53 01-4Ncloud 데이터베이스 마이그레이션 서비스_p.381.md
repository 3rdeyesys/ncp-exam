 8.0.19	이하
```
CREATE USER 'migration_test'@'%' IDENTIFIED WITH 'mysql_native_password' BY '[패스워드]';
GRANT RELOAD, PROCESS, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'migration_test'@'%';
GRANT SELECT ON mysql.* TO 'migration_test'@'%';
GRANT SELECT, SHOW VIEW, LOCK TABLES, TRIGGER ON [사용자 생성 DB].* TO 'migration_test'@'%';
FLUSH PRIVILEGES;
 ```

 8.0.20	이상
 ```
CREATE USER 'migration_test'@'%' IDENTIFIED WITH 'mysql_native_password’ BY '[패스워드]';
GRANT SHOW_ROUTINE, RELOAD, PROCESS, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'migration_test'@'%';
GRANT SELECT ON mysql.* TO ‘migration_test'@'%';
GRANT SELECT, SHOW VIEW, LOCK TABLES, TRIGGER ON [사용자 생성 DB].* TO 'migration_test'@'%';
FLUSH PRIVILEGES;
 ```