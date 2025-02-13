# Methodology

## Connect

```powershell
# Connect with credentials
psql -h $IP -U username
```

## Enumerate Database

```mysql
# List databases
\l

# Connect to a database
\c example_database

# List views and sequences for the connected database
\d

# Enumerate a table
SELECT * FROM "Users";
```
