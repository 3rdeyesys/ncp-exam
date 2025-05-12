```
SELECT character_set_name
FROM information_schema.TABLES T, information_schema.COLLATION_CHARACTER_SET_APPLICABILITY CCSA
WHERE CCSA.collation_name = T.table_collation
AND TABLE_SCHEMA NOT IN ( 'information_schema', 'mysql', 'performance_schema', 'sys' )
AND CCSA.character_set_name NOT IN ( 'utf8', 'utf8mb4', 'euckr' );

SELECT DEFAULT_CHARACTER_SET_NAME
FROM information_schema.SCHEMATA T
WHERE SCHEMA_NAME NOT IN ( 'information_schema', 'mysql', 'performance_schema', 'sys' )
AND DEFAULT_CHARACTER_SET_NAME NOT IN ( 'utf8', 'utf8mb4', 'euckr');
```