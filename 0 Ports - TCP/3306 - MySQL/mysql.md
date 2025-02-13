---
tags:
  - tool
  - mysql
---
# mysql

Interact with `mysql` databases

## Capabilities

```powershell
# Connect locally
mysql -u $USER -p'$PASSWD'

# Connect remotely
mysql -u '$USER' -p'$PASSWD' -h $IP
```

### Session Commands

```mysql
# Check version
select version();

# Check current database user
select system_user();

# Select database to enumerate
show databases;
use example_db;

# Enumerate tables
show tables;
SELECT * FROM example_table;
SELECT example_val1, example_val2 FROM example_table WHERE example_val1 = 'username';
```
