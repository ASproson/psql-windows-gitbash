# psql-windows-gitbash
Welcome to the nightmare that is the PSQL setup on Windows. We will be running psql queries through the git-bash terminal in VSCode and more or less staying away from the PowerShell psql terminal

### Start up
Install git bash as your VSCode terminal, you can find instructions for that elsewhere

1. Install PSQL

Go to the main PSQL website and download the latest package, install it as you would install any other program until you get to password set up etc

2. Setting up the password and default

Set every pasword you come across as password, leave the defaults as they are

3. Connecting PSQL with VSCode

Download this [extension](https://marketplace.visualstudio.com/items?itemName=ms-ossdata.vscode-postgresql). You might be prompted to install other extensions in support of psql, I'd just take whatever VSCode recommends

4. Create a psql file

Go into any directory you want and create a new file with the .sql extension, such as: ```01_create-database.sql```

5. /c and creating the initial database host

At the top of the file type:

```DROP DATABASE IF EXISTS my_database;```

```CREATE DATABASE my_database;```

```\c my_database```

This creates the overall database host and connects (\c) you to that database. From there we can create new tables and execute psql code within VSCode, but first we have to trigger this database creation

6. Login into PSQL

Jump down into the git bash terminal and type:
```psql -U postgres``` then enter 'password'. Note that we're using the superuser version of postgres, not the username you set up on install. For whatever reason, if we don't use the superuser psql will refuse to run queries

7. Trigger database creation

```psql -U postgres --file=01-create-database.sql``` then enter 'password' again

From here on out, everytime we run a psql command in gitbash we will have to enter 'password' on each command execution. For some reason, I can't get my password to persist, if I find a way I'll update this section

8. Using multiple files

Now that we have the database as a whole set up we can now create tables and multiple files but note the priority here: each file MUST start with ```\c my_database```, obviously it doesn't have to be my_database but you must always refer to the host database in each and every file from here on

## DEMO

fileName: 02-create-table.sql

```
\c my_database;
DROP TABLE IF EXISTS employees;
CREATE TABLE employees (
  employee_id SERIAL PRIMARY KEY,
  first_name  VARCHAR(100)
);

SELECT * FROM employees;
```
Jump down into git-bash terminal: ```psql -U postgres --file=02-create-table.sql``` with 'password'

fileName: 03-insert-into-table.sql
```
\c my_database;

INSERT INTO employees (first_name)
VALUES ('atlas');

SELECT * FROM employees;
```
Jump down into git-bash terminal: ```psql -U postgres --file=03-insert-into-table.sql``` with 'password'

