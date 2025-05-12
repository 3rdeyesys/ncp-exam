```xml
vi /bin/db_backup.sh

#!/bin/bash
DATE=$(date +%Y%m%d%H%M%S)
BACKUP_DIR=/data_backup/db/

# 전체 DB를 백업할 경우
mysqldump -u root -p디비패스워드 --all-databases > $BACKUP_DIR"backup_"$DATE.sql

# 특정 DB를 백업할 경우
# mysqldump -u root -p디비패스워드 --databases DB명  > $BACKUP_DIR"backup_"$DATE.sql



find $BACKUP_DIR -ctime +7 -exec rm -f {} \;
```