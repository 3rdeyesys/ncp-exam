MySQL5.7
```
mysql> CHANGE MASTER TO MASTER_HOST='마스터IP', MASTER_PORT=포트번호, MASTER_USER='생성한 리플리케이션 계정명', MASTER_PASSWORD='패스워드', MASTER_LOG_FILE='위에서 확인된 File명', MASTER_LOG_POS=위에서 확인된 Position 번호 FOR CHANNEL '채널 이름';
```

MySQL 8.0 ~ 8.0.22
```
# MySQL 8.0 ~ 8.0.22
mysql> CHANGE MASTER TO MASTER_HOST='마스터IP', MASTER_PORT=포트번호, MASTER_USER='생성한 리플리케이션 계정명', MASTER_PASSWORD='패스워드', GET_MASTER_PUBLIC_KEY=1, MASTER_LOG_FILE='위에서 확인된 File명', MASTER_LOG_POS=위에서 확인된 Position 번호 FOR CHANNEL '채널 이름';
```

MySQL 8.0.23~
```
# MySQL 8.0.23 이후 버전
mysql> CHANGE REPLICATION SOURCE TO SOURCE_HOST='마스터IP', SOURCE_PORT=포트번호, SOURCE_USER='생성한 리플리케이션 계정명', SOURCE_PASSWORD='패스워드', GET_SOURCE_PUBLIC_KEY=1, SOURCE_LOG_FILE='위에서 확인된 File명', SOURCE_LOG_POS=위에서 확인된 Position 번호 FOR CHANNEL '채널 이름';
```
