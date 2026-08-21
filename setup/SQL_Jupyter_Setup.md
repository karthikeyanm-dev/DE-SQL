# MySQL + Python + Jupyter Notebook Setup 

> **Purpose:** Set up a clean Python virtual environment, Jupyter Notebook, MySQL Server, and Python-to-MySQL connectivity for practicing SQL from a Jupyter Notebook.
>
> **Recommended for:** Windows 10/11. Linux/macOS commands are included where they differ.

---

## 1. What We Are Going to Install

By the end of this setup, you will have:

- Python
- A Python virtual environment (`venv`)
- Jupyter Notebook
- `ipython-sql` — lets you execute SQL directly inside Jupyter
- `mysql-connector-python` — Python driver for MySQL
- MySQL Server
- MySQL Workbench — optional GUI for MySQL
- A test database named `test`

### Final architecture

```text
Jupyter Notebook
       |
       |  %sql
       v
   ipython-sql
       |
       v
 SQLAlchemy
       |
       v
mysql-connector-python
       |
       v
 MySQL Server
```

---

# 2. Prerequisites

## 2.1 Check whether Python is installed

Open **Command Prompt** or **PowerShell**:

```powershell
python --version
```

Also check pip:

```powershell
python -m pip --version
```

You should see a Python version and a pip version.

If `python` is not recognized, install Python from the official website:

- https://www.python.org/downloads/

### Windows installation tip

During Python installation, enable:

```text
Add python.exe to PATH
```

Then close and reopen Command Prompt/PowerShell.

---

# 3. Install MySQL Server

If MySQL Server is already installed and running, skip to **Section 4**.

## 3.1 Download MySQL for Windows

Use the official MySQL download page:

https://dev.mysql.com/downloads/

For the traditional Windows installer, MySQL provides the MySQL Installer. Note that MySQL 8.0 is the final series distributed through the MySQL Installer; newer MySQL releases use their own MSI/ZIP packages and MySQL Configurator.

Official installer:

https://dev.mysql.com/downloads/installer/

---

## 3.2 Install MySQL Server

Run the MySQL installer and choose a setup suitable for development.

During configuration:

### Server configuration

Use the default development-friendly configuration unless you have a specific requirement.

### Port

Use:

```text
3306
```

This is the standard MySQL port.

### Authentication

Use the recommended/default authentication method offered by the installer.

### Root password

Create a password for the MySQL `root` account.

For example:

```text
YourStrongPassword
```

> **Important:** Do not use a simple password such as `root` for a real/shared system. For classroom practice on your own machine, you can use a simple password, but a stronger password is better.

Remember this password. You will need it when connecting Jupyter to MySQL.

---

# 4. Verify MySQL Server

After installation, open a new Command Prompt/PowerShell window.

Try:

```powershell
mysql --version
```

If the command is available, you should see the installed MySQL version.

If `mysql` is not recognized, MySQL may still be installed correctly but its `bin` directory may not be in your PATH.

You can also verify the server through **MySQL Workbench** or the Windows Services application.

---

# 5. Create the Practice Database

Open MySQL Workbench or the MySQL command-line client.

Log in as `root`.

Run:

```sql
CREATE DATABASE test;
```

Verify:

```sql
SHOW DATABASES;
```

You should see:

```text
test
```

Select the database:

```sql
USE test;
```

Create a sample table:

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    age INT,
    course VARCHAR(100)
);
```

Insert sample data:

```sql
INSERT INTO students (name, age, course)
VALUES
('Arun', 21, 'Data Engineering'),
('Priya', 22, 'Python'),
('Karthik', 23, 'Java'),
('Meena', 21, 'Data Analytics');
```

Check the data:

```sql
SELECT * FROM students;
```

---

# 6. Create a Project Folder

Create a dedicated folder for your SQL notebook.

For example:

```powershell
mkdir sql-notebook
cd sql-notebook
```

You should now be inside:

```text
sql-notebook
```

---

# 7. Create a Python Virtual Environment

A virtual environment keeps the project's Python packages isolated from your system Python installation.

Official Python documentation recommends `venv` for creating lightweight virtual environments.

Create the environment:

```powershell
python -m venv env
```

This creates:

```text
sql-notebook/
│
└── env/
```

---

# 8. Activate the Virtual Environment

## Windows Command Prompt

```cmd
env\Scripts\activate
```

## Windows PowerShell

```powershell
.\env\Scripts\Activate.ps1
```

After activation, your terminal should look similar to:

```text
(env) C:\...\sql-notebook>
```

The `(env)` confirms that the virtual environment is active.

### If PowerShell blocks activation

Run:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Then activate again:

```powershell
.\env\Scripts\Activate.ps1
```

---

# 9. Upgrade pip

With the virtual environment activated:

```powershell
python -m pip install --upgrade pip
```

Verify:

```powershell
pip --version
```

---

# 10. Install Jupyter Notebook

Install the classic Jupyter Notebook interface:

```powershell
pip install notebook
```

Jupyter's official documentation recommends installation through pip.

Verify:

```powershell
jupyter --version
```

---

# 11. Install SQL Support for Jupyter

Install `ipython-sql`:

```powershell
pip install ipython-sql
```

This package allows SQL commands such as:

```sql
%sql SELECT * FROM students;
```

to be executed directly from a Jupyter Notebook.

---

# 12. Install MySQL Connector for Python

Install the official MySQL Connector/Python package:

```powershell
pip install mysql-connector-python
```

This is the Python driver that allows Python applications to communicate with MySQL.

Verify:

```powershell
pip show mysql-connector-python
```

---

# 13. Optional: Install PrettyTable

`PrettyTable` can be useful for displaying tabular output.

Install the current compatible version:

```powershell
pip install prettytable
```

### About `prettytable==0.7.2`

Older classroom notes may contain:

```powershell
pip install prettytable==0.7.2
```

You generally do **not** need to pin this old version for a new setup. Use the current compatible package unless your course specifically requires version `0.7.2`.

---

# 14. Recommended One-Command Installation

After activating the virtual environment, you can install the required Python packages together:

```powershell
pip install notebook ipython-sql mysql-connector-python prettytable
```

This is the recommended setup for a fresh SQL notebook environment.

---

# 15. Check Installed Packages

Run:

```powershell
pip list
```

You should see packages including:

```text
ipython-sql
mysql-connector-python
notebook
prettytable
SQLAlchemy
```

`SQLAlchemy` is used by `ipython-sql` for database connections.

---

# 16. Start Jupyter Notebook

Make sure the virtual environment is active.

Run:

```powershell
jupyter notebook
```

Jupyter should open in your browser.

If it does not open automatically, the terminal will display a local URL. Open that URL in your browser.

---

# 17. Create the SQL Notebook

Inside Jupyter:

1. Open the `sql-notebook` folder.
2. Select **New**.
3. Select **Python 3**.
4. Rename the notebook to:

```text
SQL_MySQL_Practice.ipynb
```

---

# 18. Load the SQL Extension

In the first notebook cell, run:

```python
%load_ext sql
```

If there is no error, the SQL extension has loaded successfully.

---

# 19. Connect Jupyter to MySQL

The connection URL follows this structure:

```text
mysql+mysqlconnector://USERNAME:PASSWORD@HOST:PORT/DATABASE
```

For the local `root` user:

```text
mysql+mysqlconnector://root:YOUR_PASSWORD@localhost:3306/test
```

Replace `YOUR_PASSWORD` with the password you created during MySQL installation.

In Jupyter:

```python
%sql mysql+mysqlconnector://root:YOUR_PASSWORD@localhost:3306/test
```

Example, if your password is `root`:

```python
%sql mysql+mysqlconnector://root:root@localhost:3306/test
```

> **Important:** Do not put spaces in the connection URL.

---

# 20. Test the Connection

Run:

```python
%sql SELECT DATABASE();
```

Expected result:

```text
test
```

Now run:

```python
%sql SHOW TABLES;
```

You should see:

```text
students
```

---

# 21. Run SQL Queries in Jupyter

You can execute a single SQL statement using `%sql`:

```python
%sql SELECT * FROM students;
```

For multiple SQL statements, use `%%sql`:

```sql
%%sql

SELECT name, age, course
FROM students
WHERE age >= 21
ORDER BY name;
```

---

# 22. Example Practice Session

## Cell 1 — Load extension

```python
%load_ext sql
```

## Cell 2 — Connect to MySQL

```python
%sql mysql+mysqlconnector://root:YOUR_PASSWORD@localhost:3306/test
```

## Cell 3 — Check database

```sql
%sql SELECT DATABASE();
```

## Cell 4 — Show tables

```sql
%sql SHOW TABLES;
```

## Cell 5 — View data

```sql
%sql SELECT * FROM students;
```

## Cell 6 — Filter data

```sql
%%sql

SELECT *
FROM students
WHERE age > 21;
```

## Cell 7 — Aggregate

```sql
%%sql

SELECT course, COUNT(*) AS total_students
FROM students
GROUP BY course;
```

---

# 23. Better Connection Method for Teaching

For a classroom/practice notebook, the direct connection is fine:

```python
%sql mysql+mysqlconnector://root:YOUR_PASSWORD@localhost:3306/test
```

For a real project, avoid putting passwords directly inside notebooks.

A better approach is to use environment variables or a configuration mechanism so the password is not committed to Git.

---

# 24. Useful MySQL Commands

## Check server version

```sql
SELECT VERSION();
```

## Show databases

```sql
SHOW DATABASES;
```

## Select a database

```sql
USE test;
```

## Show tables

```sql
SHOW TABLES;
```

## Describe a table

```sql
DESC students;
```

or:

```sql
DESCRIBE students;
```

## View table data

```sql
SELECT * FROM students;
```

## Create a database

```sql
CREATE DATABASE database_name;
```

## Delete a database

```sql
DROP DATABASE database_name;
```

> Be careful with `DROP DATABASE` because it permanently removes the database and its tables.

---

# 25. Complete Installation Commands — Quick Version

If Python and MySQL Server are already installed:

```powershell
mkdir sql-notebook
cd sql-notebook

python -m venv env

.\env\Scripts\Activate.ps1

python -m pip install --upgrade pip

pip install notebook ipython-sql mysql-connector-python prettytable

jupyter notebook
```

Then, inside Jupyter:

```python
%load_ext sql
```

Connect:

```python
%sql mysql+mysqlconnector://root:YOUR_PASSWORD@localhost:3306/test
```

Test:

```python
%sql SELECT DATABASE();
```

Then:

```python
%sql SHOW TABLES;
```

---

# 26. Complete Folder Structure

After setup, your project can look like:

```text
sql-notebook/
│
├── env/
│   ├── Scripts/
│   ├── Lib/
│   └── ...
│
└── SQL_MySQL_Practice.ipynb
```

### Important

Do **not** upload the `env` folder to Git.

Add this to `.gitignore`:

```gitignore
env/
.venv/
__pycache__/
.ipynb_checkpoints/
```

---

# 27. Daily Workflow

Whenever you want to practice SQL:

## Step 1 — Open terminal

Go to the project:

```powershell
cd path\to\sql-notebook
```

## Step 2 — Activate environment

```powershell
.\env\Scripts\Activate.ps1
```

## Step 3 — Start Jupyter

```powershell
jupyter notebook
```

## Step 4 — Open your notebook

Open:

```text
SQL_MySQL_Practice.ipynb
```

## Step 5 — Connect

Run:

```python
%load_ext sql
```

Then:

```python
%sql mysql+mysqlconnector://root:YOUR_PASSWORD@localhost:3306/test
```

Now you can practice SQL.

---

# 28. How the Connection Works

When you execute:

```python
%sql mysql+mysqlconnector://root:YOUR_PASSWORD@localhost:3306/test
```

the components mean:

| Component | Meaning |
|---|---|
| `mysql` | Database type |
| `+mysqlconnector` | Python MySQL driver |
| `root` | MySQL username |
| `YOUR_PASSWORD` | MySQL password |
| `localhost` | MySQL is running on your computer |
| `3306` | MySQL port |
| `test` | Database name |

So:

```text
mysql+mysqlconnector://root:password@localhost:3306/test
```

means:

```text
Jupyter
   ↓
ipython-sql
   ↓
SQLAlchemy
   ↓
MySQL Connector/Python
   ↓
localhost:3306
   ↓
MySQL Server
   ↓
test database
```

---

# 29. Troubleshooting

## Problem 1 — `python is not recognized`

Check:

```powershell
python --version
```

If it fails, reinstall Python and enable:

```text
Add Python to PATH
```

Then reopen the terminal.

---

## Problem 2 — `pip is not recognized`

Use:

```powershell
python -m pip --version
```

Instead of:

```powershell
pip --version
```

You can also upgrade pip:

```powershell
python -m pip install --upgrade pip
```

---

## Problem 3 — PowerShell cannot activate `env`

Run:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

Then:

```powershell
.\env\Scripts\Activate.ps1
```

---

## Problem 4 — `jupyter is not recognized`

Make sure the virtual environment is active:

```powershell
.\env\Scripts\Activate.ps1
```

Then reinstall:

```powershell
pip install notebook
```

Try:

```powershell
jupyter notebook
```

---

## Problem 5 — `No module named sql`

Install:

```powershell
pip install ipython-sql
```

Then restart the Jupyter kernel.

Run:

```python
%load_ext sql
```

---

## Problem 6 — `No module named mysql`

Install:

```powershell
pip install mysql-connector-python
```

Verify:

```powershell
pip show mysql-connector-python
```

---

## Problem 7 — MySQL connection refused

Check whether MySQL Server is running.

The standard local connection should use:

```text
localhost:3306
```

Try connecting through MySQL Workbench first.

If Workbench cannot connect either, the problem is with the MySQL Server/configuration rather than Jupyter.

---

## Problem 8 — Access denied for `root`

Example error:

```text
Access denied for user 'root'@'localhost'
```

Check:

- Username
- Password
- MySQL server status
- Port
- Authentication configuration

Test the credentials directly through MySQL Workbench or the MySQL client.

---

## Problem 9 — Database does not exist

If you get an error similar to:

```text
Unknown database 'test'
```

Create it:

```sql
CREATE DATABASE test;
```

Then reconnect:

```python
%sql mysql+mysqlconnector://root:YOUR_PASSWORD@localhost:3306/test
```

---

# 30. Verification Checklist

Before starting your SQL classes/practice, confirm:

- [ ] Python is installed
- [ ] `python --version` works
- [ ] MySQL Server is installed
- [ ] MySQL Server is running
- [ ] MySQL port is `3306`
- [ ] MySQL `root` password is known
- [ ] `test` database exists
- [ ] Python virtual environment is created
- [ ] Virtual environment is activated
- [ ] pip is upgraded
- [ ] Jupyter Notebook is installed
- [ ] `ipython-sql` is installed
- [ ] `mysql-connector-python` is installed
- [ ] `prettytable` is installed
- [ ] Jupyter Notebook opens
- [ ] `%load_ext sql` works
- [ ] Jupyter connects to MySQL
- [ ] `SHOW TABLES;` works
- [ ] `SELECT * FROM students;` works

---

# 31. Final Working Setup

The essential commands are:

```powershell
mkdir sql-notebook
cd sql-notebook

python -m venv env

.\env\Scripts\Activate.ps1

python -m pip install --upgrade pip

pip install notebook ipython-sql mysql-connector-python prettytable

jupyter notebook
```

Inside Jupyter:

```python
%load_ext sql
```

Connect:

```python
%sql mysql+mysqlconnector://root:YOUR_PASSWORD@localhost:3306/test
```

Test:

```python
%sql SELECT DATABASE();
```

Then:

```python
%sql SHOW TABLES;
```

Finally:

```python
%sql SELECT * FROM students;
```

If the student records appear, the complete **MySQL → Python → Jupyter Notebook** setup is working.

---

# 32. Official Documentation

- [Python `venv` documentation](https://docs.python.org/3/library/venv.html)
- [Jupyter installation guide](https://jupyter.org/install)
- [MySQL Downloads](https://dev.mysql.com/downloads/)
- [MySQL Installer for Windows](https://dev.mysql.com/downloads/installer/)
- [MySQL Connector/Python installation](https://dev.mysql.com/doc/connector-python/en/quick-installation-guide.html)
- [MySQL Connector/Python documentation](https://dev.mysql.com/doc/connector-python/en/)
