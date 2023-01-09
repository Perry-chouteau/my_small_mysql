# Description of mysql


# Create sql using docker

# Create and Connect to mysql/mariadb
`(mariadb is an edited fork of mysql cause mysql is deprecated under fedora)`
```sql
mysql -u ${user} -p
[Enter your password]: ${password}
--user with full privileges is by default root
```

# Create and Connect to mysql `using docker prompt under fedora`

## Create
- this line pl
```docker
docker run -d --name mysql_server_name -e MYSQL_ROOT_PASSWORD=password mysql
```

## Connect
```docker
docker exec -it mysql mysql -uroot -p
```

> :warning: **You may need Sudo mode to connect to it**: Be very careful here!

# Database

## Create
```sql
CREATE DATABASE database_name;
```

## Drop
```sql
DROP DATABASE database_name;
```

## BACKUP
```sql
BACKUP DATABASE databasename
TO DISK = 'filepath';
-- or with DIFFERENTIAL
BACKUP DATABASE databasename
TO DISK = 'filepath'
WITH DIFFERENTIAL;
```

## SHOW
```sql
SHOW DATABASES;
```

- Show database info
```sql
SHOW CREATE DATABASE database_name;
```



# TABLE

## Create
```sql
CREATE TABLE database_name.table_name (
    variable_name datatype optional_constraint, -- variable type(int=chiffre)
    variable_name2 datatype (optional_constraint)
);
--or
USE database_name;
CREATE TABLE table_name (
    variable_name datatype (optional_constraint), -- variable type(int=chiffre)
    variable_name2 datatype (optional_constraint)
);
```

## Drop
```sql
DROP TABLE table_name; -- delete table
-- or
TRUNCATE TABLE table_name; -- delete the data inside a table, but not the table itself.
```

## Alter
- ADD COLUMN
    ```sql
    ALTER TABLE table_name
    ADD column_name datatype;
    ```
- DELETE COLUMN
    ```sql
    ALTER TABLE table_name
    DROP COLUMN column_name;
    ```
- RENAME COLUMN
    ```sql
    ALTER TABLE table_name
    RENAME COLUMN old_name to new_name;
    ```
- EDIT COLUMN
    ```sql
    ALTER TABLE table_name
    ALTER COLUMN column_name datatype;
    -- or
    ALTER TABLE table_name
    MODIFY COLUMN column_name datatype;
    -- or
    ALTER TABLE table_name
    MODIFY column_name datatype;
    ```

## INSERT INTO
```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
-- or (if values folow the order of the table)
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

## SELECT
```sql
SELECT * -- to retrieve everything
FROM table_name;
-- or
SELECT column1, column2, ...
FROM table_name;
```

## Types
```sql
USE database_name;
CREATE TABLE table_name (

    -- StringDataType
    Mail CHAR(255), -- string(of size 255) size between (1-255), default (1);
    UserName VARCHAR(255), -- string(of size 255) size between (0-65535).
    UserType ENUM(val1, ...), -- A string object that can have only one value, chosen from a list of possible values. max enum list values is 65535. If a value is inserted that is not in the list, a blank value will be inserted. The values are sorted in the order you enter them
    UserSet SET(val1, ...), -- A string object that can have 0 or more values, chosen from a list of possible values. You can list up to 64 values in a SET list

    -- NumericalDataType
    Id INT,
    Male BOOLEAN,
    a FLOAT(2), -- If p is from 0 to 24, the data type becomes FLOAT(). If p is from 25 to 53, the data type becomes DOUBLE()

    -- DateDataTypes and TimeDataTypes
    birtday DATE,	-- A date. Format: YYYY-MM-DD. The supported range is from '1000-01-01' to '9999-12-31'
    birthTime TIME(fsp),	-- A time. Format: hh:mm:ss. The supported range is from '-838:59:59' to '838:59:59' The fsp value is the fractional seconds precision (default value is 0). If given, it must be in the range 0 to 6.
    lastUpdate DATETIME(), -- A date and time combination. Format: YYYY-MM-DD hh:mm:ss. The supported range is from '1000-01-01 00:00:00' to '9999-12-31 23:59:59'. Adding DEFAULT and ON UPDATE in the column definition to get automatic initialization and updating to the current date and time
);
```

## Constraint And Add-On
```sql
USE database_name;
CREATE TABLE table_name_two (
    -- UNIQUE
    Id INT UNIQUE, -- Ensures that all values in the (Id) column are different.

    -- AUTO_INCREMENT
    NumberId INT AUTO_INCREMENT=0, -- auto increment value (default value is 1). the default value has change to 0.
    -- or
    NumberId INT IDENTITY(0,5), -- start from 0 and evolve with a step of 5.

    -- NOT NULL
    UserName VARCHAR(255) NOT NULL, -- Ensures that a column cannot have a NULL value

    -- PRIMARY KEY
    UserId PRIMARY KEY, -- NOT NULL and UNIQUE combination, Only one column in a table can have the Constraint PRIMARY KEY.
    -- or
    ID int NOT NULL, PRIMARY KEY (UserName),
    -- or
    CONSTRAINT pk_combination PRIMARY KEY (ID, LastName),

    -- FOREIGN KEY
    UserName VARCHAR(255) FOREIGN KEY REFERENCES table_name(UserName), -- Prevents actions that would destroy links between tables
    -- or
    FOREIGN KEY UserName REFERENCES table_name(UserName),

    -- CHECK
    Age int,
    CHECK (Age>=18), --
    -- or
    Age int CHECK (Age>=18), --
    -- or
    Age int,
    birtday DATE,
    CONSTRAINT chk_combination CHECK (Age>=18 AND birtday >= '2000-01-01') --

    -- DEFAULT
    Description varchar(255) DEFAULT 'description not set' -- Sets a default value for a column if no value is specified
);
```

## Create INDEX -- Used to create and retrieve data from the database very quickly (users cannot see the  (UNIQUE) indexes, they are just used to speed up searches/queries.)
```sql
CREATE (UNIQUE) INDEX index_name
ON table_name (column_name, ...);
```

## Insert Data in Table;
- Insert info
```sql
INSERT INTO database_name.table_name (var_99, var1..) VALUES (value_99, value_1, ..);
--or
INSERT INTO database_name.table_name (value_1, value_2, ..);
```

## SHOW

- Show tables
```sql
SHOW TABLES;
```

- Show table info
```sql
SHOW CREATE TABLE table_name;
```

# USER

## PRIVILEGE

```sql
GRANT
REVOKE
```
>example
```sql
REVOKE INSERT ON *.* FROM 'jeffrey'@'localhost';
REVOKE 'role1', 'role2' FROM 'user1'@'localhost', 'user2'@'localhost';
REVOKE SELECT ON world.* FROM 'role3';
--or
CREATE USER 'jeffrey'@'localhost' IDENTIFIED BY 'password';
GRANT ALL ON db1.* TO 'jeffrey'@'localhost';
GRANT SELECT ON db2.invoice TO 'jeffrey'@'localhost';
ALTER USER 'jeffrey'@'localhost' WITH MAX_QUERIES_PER_HOUR 90;
```



## RESET USER PASSWORD (usefull for the review)

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MY_NEW_PASSWORD';
FLUSH PRIVILEGES;
```
