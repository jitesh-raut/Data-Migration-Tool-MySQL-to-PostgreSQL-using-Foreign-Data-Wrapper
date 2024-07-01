# Data-Migration-Tool-MySQL-to-PostgreSQL-using-Foreign-Data-Wrapper
This repository provides tools and instructions for migrating data from MySQL to PostgreSQL using Foreign Data Wrapper (FDW). Ideal for seamless database migration and integration projects.

### Installing PostgreSQL

To install PostgreSQL 14 on Ubuntu, you can follow these steps:

1. **Update Package List**: First, update the package list to ensure you get the latest versions of packages.
   
   ```bash
   sudo apt update
   ```

2. **Install PostgreSQL**: Install PostgreSQL 14 and its contrib package (which provides additional features and extensions).

   ```bash
   sudo apt install postgresql-14 postgresql-contrib
   ```

3. **Verify Installation**: After installation, PostgreSQL should start automatically. You can verify its status with:

   ```bash
   systemctl status postgresql@14-main
   ```

   This command checks the status of the PostgreSQL service running under version 14 (`@14-main`).

4. **Access PostgreSQL**: By default, PostgreSQL creates a `postgres` user and a system account. To access the PostgreSQL prompt as the `postgres` user, use:

   ```bash
   sudo -u postgres psql
   ```

   This command opens the PostgreSQL interactive terminal (`psql`) as the `postgres` user.

5. **Set a Password for the `postgres` User**: If you haven't set a password during installation, you can set it now from within the `psql` prompt:

   ```sql
   ALTER USER postgres PASSWORD 'new_password';
   ```

   Replace `'new_password'` with your desired password.

6. **Create a New PostgreSQL User (Optional)**: You might want to create a new PostgreSQL user for your applications or personal use. While in the `psql` prompt, you can create a new user with:

   ```sql
   CREATE USER username WITH PASSWORD 'password';
   ```

   Replace `username` and `'password'` with your desired credentials.

7. **Create a New Database (Optional)**: To create a new database, still within `psql`:

   ```sql
   CREATE DATABASE dbname;
   ```

   Replace `dbname` with the name of your new database.

8. **Exit `psql`**: When you're done, exit the PostgreSQL prompt with:

   ```sql
   \q
   ```


### Installation of MySQL

1. **Install MySQL Server:**
   ```sh
   sudo apt-get install mysql-server
   ```

2. **Secure MySQL Installation (optional but recommended):**
   ```sh
   sudo mysql_secure_installation
   ```

3. **Check MySQL Service Status:**
   ```sh
   sudo service mysql status
   ```

4. **Start MySQL Service:**
   ```sh
   sudo service mysql start
   ```

5. **Verify MySQL Process:**
   ```sh
   ps aux | grep mysql
   ```

6. **Access MySQL Command-Line Interface:**
   ```sh
   mysql -u root -p
   ```

7. **Create Directory and Adjust Permissions (if needed):**
   ```sh
   sudo mkdir -p /var/run/mysqld
   sudo chown mysql:mysql /var/run/mysqld
   sudo chmod 755 /var/run/mysqld
   ```

8. **Restart MySQL Service:**
   ```sh
   sudo service mysql restart
   ```

### Installation Foreign Data Wrapper (FDW)

9. **Check PostgreSQL Version:**
   ```sh
   psql --version
   ```

10. **Install PostgreSQL FDW Extensions (Example commands):**
    - Install `postgres_fdw`:
      ```sh
      sudo apt install postgresql-14-postgres-fdw
      ```
    - Install `mysql_fdw` (example):
      ```sh
      sudo apt install postgresql-14-mysql-fdw
      ```

11. **Access PostgreSQL as Superuser (postgres):**
    ```sh
    sudo -u postgres psql
    ```

12. **Check PostgreSQL Service Status:**
    ```sh
    sudo service postgresql status
    ```

13. **Start PostgreSQL Service:**
    ```sh
    sudo service postgresql start
    ```

14. **Switch to PostgreSQL User (optional):**
    ```sh
    sudo -i -u postgres
    ```

### Create test tables in MySQL

Steps to create a database named `userdb`, create tables related to users, and insert data into those tables in MySQL

### Steps to Create Database, Tables, and Insert Data

1. **Access MySQL:**
   Open your MySQL command-line client:
   ```sh
   mysql -u your_username -p
   ```
   Replace `your_username` with your MySQL username.

2. **Create Database:**
   Create the `userdb` database:
   ```sql
   CREATE DATABASE userdb;
   ```

3. **Use the Database:**
   Switch to the `userdb` database to perform operations within it:
   ```sql
   USE userdb;
   ```

4. **Create Table:**
   Create a table for users. Here’s an example schema for a basic user table:
   ```sql
   CREATE TABLE users (
       id INT AUTO_INCREMENT PRIMARY KEY,
       username VARCHAR(50) NOT NULL,
       email VARCHAR(100) NOT NULL,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );
   ```

5. **Insert Data:**
   Insert some sample data into the `users` table:
   ```sql
   INSERT INTO users (username, email) VALUES
   ('john_doe', 'john.doe@example.com'),
   ('jane_smith', 'jane.smith@example.com');
   ```

6. **Verify Data:**
   Optionally, verify that the data has been inserted correctly:
   ```sql
   SELECT * FROM users;
   ```

7. **Create `orders` Table:**
   Create the `orders` table with a foreign key constraint referencing the `users` table:
   ```sql
   CREATE TABLE orders (
       id INT AUTO_INCREMENT PRIMARY KEY,
       user_id INT NOT NULL,
       order_date DATE NOT NULL,
       total_amount DECIMAL(10, 2) NOT NULL,
       FOREIGN KEY (user_id) REFERENCES users(id)
   );
   ```

8. **Insert Data into `orders` Table:**
   Insert some sample data into the `orders` table:
   ```sql
   INSERT INTO orders (user_id, order_date, total_amount) VALUES
   (1, '2024-07-01', 100.50),
   (2, '2024-06-30', 75.25);
   ```

9. **Verify Data:**
   Verify that the data has been inserted correctly:
   ```sql
   SELECT * FROM orders;
   ```

10. **Exit MySQL:**
   Exit the MySQL command-line client when you're done:
   ```sql
   EXIT;
   ```

### Steps to Create `userdb_postgres` Database in PostgreSQL

1. **Access PostgreSQL:**
   Open your PostgreSQL prompt. If you haven't installed `psql`, you can do so using:
   ```sh
   sudo -u postgres psql
   ```

2. **Create Database:**
   Use the `CREATE DATABASE` statement to create the `userdb_postgres` database:
   ```sql
   CREATE DATABASE userdb_postgres;
   ```

3. **Verify Database Creation:**
   You can list all databases to verify that `userdb_postgres` was created:
   ```sql
   \l
   ```

   Alternatively, you can query the `pg_database` catalog:
   ```sql
   SELECT datname FROM pg_database WHERE datistemplate = false;
   ```

### Importing Foreign Schema from MySQL into PostgreSQL

15. **Import Foreign Schema in PostgreSQL:**
    ```sql
    IMPORT FOREIGN SCHEMA userdb
    FROM SERVER mysql_server
    INTO userdb_postgres;
    ```


To test if data has been successfully transferred from MySQL to PostgreSQL using Foreign Data Wrapper (FDW), you can execute the following SQL queries in your PostgreSQL environment. These queries assume that you have imported the foreign schema `userdb` from MySQL into the `userdb_postgres` schema in PostgreSQL:

### Steps to Test Data Transfer

1. **Access PostgreSQL:**
   Open your PostgreSQL
   ```sh
   sudo -u postgres psql
   ```

2. **Query `users` Table:**
   Check if data from the `users` table has been transferred:
   ```sql
   SELECT * FROM userdb_postgres.users;
   ```

3. **Query `orders` Table:**
   Check if data from the `orders` table has been transferred:
   ```sql
   SELECT * FROM userdb_postgres.orders;
   ```

4. **View Data:**
   - Execute the above queries and review the output to see if the data appears as expected.
   - Ensure that the schema (`userdb_postgres`) and table names (`users`, `orders`) match exactly as they were imported.

5. **Exit PostgreSQL:**
   When you're done querying, exit the PostgreSQL prompt:
   ```sql
   \q
   ```

### Notes:

- Adjust the version numbers (`postgresql-14`, `mysql-server`) based on your specific system and PostgreSQL/MySQL versions installed.
