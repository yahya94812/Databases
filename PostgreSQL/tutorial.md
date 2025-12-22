# PostgreSQL
* PostgreSQL = official name
* Postgres = accepted short name
* all other names = wrong
## setup
* sudo apt update
* sudo apt install postgresql postgresql-contrib
* sudo systemctl start postgresql // Start PostgreSQL service
* sudo su - postgres // Switch user and load postgres's environment and shell
* why "-"?
    * Starts a login shell
    * Loads the target user’s environment:
        * .bash_profile
        * PATH
        * HOME directory
✅ Important for PostgreSQL tools to work correctly

* psql // Enter PostgreSQL shell

## meta commands
* \l // List all databases
* \c <dbname> // Connect to a specific database
* \dt // List all tables in the current database
* \d <tablename> // Describe a specific table
* \q // Exit PostgreSQL shell

## SQL Categories
| Category | Full Form                    | Purpose                                   | Example Commands                  |
| -------- | ---------------------------- | ----------------------------------------- | --------------------------------- |
| **DDL**  | Data Definition Language     | Define database structures (tables, etc.) | `CREATE`, `ALTER`, `DROP`         |
| **DML**  | Data Manipulation Language   | Manipulate data inside tables             | `INSERT`, `UPDATE`, `DELETE`      |
| **DQL**  | Data Query Language          | Query data from the database              | `SELECT`                          |
| **DCL**  | Data Control Language        | Control access to data                    | `GRANT`, `REVOKE`                 |
| **TCL**  | Transaction Control Language | Manage transactions                       | `COMMIT`, `ROLLBACK`, `SAVEPOINT` |


## DDL (Data Definition Language)
### database
* create database mydb;
* \l
* drop database mydb;
* \c mydb // Connect to database

### table
* create table mytable (
    id serial primary key,
    name text not null,
    age int not null,
    email text unique not null,
);
* \dt // List tables
* \d mytable // Describe table structure
* drop table mytable;
* alter table mytable add column address text; // Add a new column

## DML (Data Manipulation Language)
### CRUD operations
* insert into mytable (name, age, email) values ('Alice', 30, 'abc@gmail.com');
* Single quotes ' → used for string values
* select * from mytable; // not in DML
* update mytable set age = 31 where name = 'Alice';
* delete from mytable where name = 'Alice';

## DQL (Data Query Language)
* select * from mytable; // Select all columns

## DCL (Data Control Language)

## TCL (Transaction Control Language)
* begin; // Start a transaction
* select * from mytable; // Perform operations
* commit; // Commit the transaction

* rollback; // Rollback the transaction
