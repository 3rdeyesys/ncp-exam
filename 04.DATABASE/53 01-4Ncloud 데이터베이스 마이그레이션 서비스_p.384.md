```
SELECT DEFINER FROM information_schema.ROUTINES
WHERE ROUTINE_SCHEMA NOT IN ( 'information_schema', 'mysql', 'performance_schema', 'sys' ) AND SECURITY_TYPE = 'DEFINER';
SELECT DEFINER FROM information_schema.VIEWS WHERE table_schema NOT IN ( 'information_schema', 'mysql', 'performance_schema’, ‘sys’ ) AND SECURITY_TYPE = ‘DEFINER’ ;
```