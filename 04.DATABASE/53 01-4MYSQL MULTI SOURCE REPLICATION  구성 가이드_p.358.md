```
vi /etc/my.cnf

### MySQL 8 기준 ###

server-id = 3

# 리플리케이션 대상 디비 설정 (replicate-do-db = 채널명:DB명)
replicate-do-db = ch_testdb1:testdb1
replicate-do-db = ch_testdb1:testdb2

#리플리케이션 제외 디비 설정.
replicate-ignore-db = information_schema
replicate-ignore-db = mysql
replicate-ignore-db = performance_schema
replicate-ignore-db = sys

slave-skip-errors = all

#DB를 재시작하여 변경사항 적용.
systemctl restart mysqld
```