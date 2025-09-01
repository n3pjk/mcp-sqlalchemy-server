# Mac OS Install PostgreSQL ODBC Driver

The simplest way to install the PostgreSQL ODBC driver on macOS is with Homebrew. The process involves three main steps: installing the ODBC driver manager (UnixODBC), installing the PostgreSQL ODBC driver (psqlodbc), and configuring the DSN (Data Source Name) to connect to your PostgreSQL database. 

## Prerequisites

**Homebrew**: If you don't have Homebrew installed, you can install it by running the following command in your Terminal:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## Step 1: Install the ODBC driver manager

The PostgreSQL ODBC driver requires a driver manager to function. UnixODBC is a common and reliable choice. 
Open the Terminal app.
Run the following command to install unixodbc:

```bash
brew install unixodbc
```

## Step 2: Install the PostgreSQL ODBC driver

Next, install the psqlodbc driver, which is the official PostgreSQL ODBC driver. 
Run the following command in your Terminal:

```bash
brew install psqlodbc
```

## Step 3: Configure the DSN in odbc.ini

To connect to a specific database, you need to set up a Data Source Name (DSN). This involves editing or creating the odbc.ini configuration file.
Find the configuration file location: Run odbcinst -j to find the correct location for your odbc.ini file. This is often /opt/homebrew/etc/odbc.ini.

```bash
odbcinst -j
```

Edit the odbc.ini file: Open the odbc.ini file in a text editor.
For a system-wide DSN, use sudo vim /opt/homebrew/etc/odbc.ini (or your preferred editor).
For a user-specific DSN, use vim ~/.odbc.ini.
Add a DSN entry for your PostgreSQL database. Fill in the required connection details, such as the server, database name, and port.Example odbc.ini entry:

```ini
[MyPostgresDSN]
Description = PostgreSQL DSN via psqlodbc
Driver = /opt/homebrew/lib/psqlodbcw.so
Servername = localhost
Port = 5432
Database = your_database_name
Username = your_username
```

**[MyPostgresDSN]**: This is the name you will use to reference your DSN. You can choose any name you like.
Driver: Confirm the path to your driver file by running brew info psqlodbc. The path may vary.
Servername, Port, Database, Username: Replace these with your actual PostgreSQL connection details.

## Step 4: Verify the installation

You can test the ODBC connection using the isql tool, which is part of UnixODBC.
Run the command with your DSN name and credentials.

```bash
isql -v MyPostgresDSN your_username your_password
```

If the connection is successful, you will see a Connected! message and a SQL prompt. You can run a test query like SELECT 1; and then type quit to exit.
