```
vi /etc/my.cnf

### MySQL5.7 기준 ###

server-id = 3

#리플리케이션 대상 디비 설정.
replicate-do-db = testdb1
replicate-do-db = testdb2

#리플리케이션 제외 디비 설정.
replicate-ignore-db = information_schema
replicate-ignore-db = mysql
replicate-ignore-db = performance_schema
replicate-ignore-db = sys

master_info_repository = 'TABLE'
relay_log_info_repository = 'TABLE'
slave-skip-errors = all

#DB를 재시작하여 변경사항 적용.
systemctl restart mysqld
```