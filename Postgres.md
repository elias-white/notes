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
A schema contains a collection of tables. It can also include data types and functions.
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

