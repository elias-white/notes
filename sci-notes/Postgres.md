### Creating a database
`CREATE DATABASE database_name;`

### Creating a Table
Naming Restrictions:
- Maximum 31 characters
- Must begin with letter or underscore
```postgresql
CREATE TABLE table_name (
	column1_name column1_datatype [col1_constraints],
	column2_name column2_datatype [col2_constraints],
	...
	columnN_name columnN_datatype [colN_constraints],
);
```
e.g.
```postgresql
CREATE TABLE school (
	id serial PRIMARY KEY,
	name TEXT NOT NULL,
	mascot_name TEXT,
);
```

#### Adding relationships between two tables
```postgresql
CREATE TABLE business_type (
	id serial PRIMARY KEY,
	description TEXT NOT NULL
);

CREATE TABLE applicant (
	id serial PRIMARY KEY,
	name TEXT NOT NULL,
	zip_code CHAR(5) NOT NULL,
	business_type_id INTEGER references business_type(id)
);
```

### Creating Schemas
A schema contains a collection of tables. It can also include data types and functions. A postgres schema is analagous to MySQL databases.

Use cases:
- Provided database users with separate environments
- Organise components of a database
- The `public` schema is the default schema in PostgreSQL

```postgresql
-- CREATE SCHEMA schema_name;
CREATE SCHEMA division1;
CREATE TABLE division1.school (
	id serial PRIMARY KEY,
	name TEXT NOT NULL,
	mascot_name TEXT,
	num_scholarships INTEGER DEFAULT 0
)
```

### Data Types
#### Data Categories
The four main data categories are:
- Text
- Numeric
- Temporal
- Boolean

There are also geometric and array-based data types.

#### Text
The three available `TEXT` data types are:
1. `TEXT`
These strings can be of any length. Good for text values of unknown length.
3. `VARCHAR(N)`
These strings can be of any length, but allows for restrictions to be placed. `VARCHAR` without any *N* given is equivalent to `TEXT`.
5. `CHAR(N)`
`CHAR(N)` values consist of exactly `N` characters. If a value that is less than `N` is provided, the value will be right-padded with spaces. An additional option is `NOT NULL`.



#### Numeric
There are 
`SMALLINT` (small-range integer): -32768 to +32768
`INTEGER` (typical choice for integer): -2147483648 to +2147483648
`BIGINT` (large-range integer): -9223372036854775808 to +9223372036854775808
`SERIAL` (autoincrementing integer): 1 to 2147483647 
`BIGSERIAL` (autoincrementing integer): 1 to 9223372036854775807

`DECIMAL(PRECISION, SCALE)`: *Precision* is total number of digits and scale is number of digits reserved for *after* the decimal point.


#### BOOLEAN 
`BOOLEAN` can be `TRUE`, `FALSE` or `NULL`.
Can be defined with either `BOOL` or `BOOLEAN`.

#### TEMPORAL
`TIMESTAMP`: Represents date & time.
`DATE`: Represents a date.
`TIME`: Reprents a time.

### Access Control
A default installation creates `postgres` user that has the `superuser` role. They can create, delete, insert, drop databases and tables.

#### Creating New Users and Managing Passwords
`CREATE USER user_name`  creates a new user account.
`CREATE USER user_name WITH PASSWORD 'secret'` to create a user account with a password.
`ALTER USER user_name WITH PASSWORD 'new_password'`  to change passwords.

#### Roles and Privileges
Basic syntax:
`GRANT p ON obj TO grantee`
i.e. To allow user `ewhite` to update records on the `account` table, run the following:
`GRANT INSERT ON account TO ewhite` 

Some privileges cannot be granted. Those commands must be run by the table owner.
For example -- modifying tables requires ownership. To transfer ownership of a table, run `ALTER TABLE table_name OWNER TO new_owner`.

#### Heirarchal Access Control
You can provide access across schemas via:
- `GRANT USAGE ON SCHEMA schema_name TO user_name`.
- `GRANT [SELECT, INSERT, ...] ON ALL TABLES IN SCHEMA schema_nme TO user_name` .

#### Groups
You can create groups to group roles. For example, all developers in a software company require the same privileges on the test DB. Instead of applying them individually, you can:
```postgreSQL
CREATE GROUP developers;
GRANT USAGE ON SCHEMA public TO developers;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO developers;
-- To add users to the group
ALTER GROUP developers ADD USER ewhite, sshafiq, ...;
```

To remove access:
```postgreSQL
REVOKE p ON table FROM user/group;
-- REVOKE ALL PRIVILEGES
REVOKE ALL ON table FROM user/group;
```