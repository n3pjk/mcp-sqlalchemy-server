# Install SQLite3 ODBC Driver - macOS

[Home](/README.md)

To install the SQLite3 ODBC driver on macOS, the simplest method is to use the Homebrew package manager. The process involves installing Homebrew, the ODBC driver manager unixodbc, and the sqliteodbc driver itself, followed by configuring the driver.

## Step 1: Install Homebrew

If you don't have Homebrew, you need to install it first. Open your Terminal and run the following command:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## Step 2: Install unixodbc

The `unixodbc` package is the driver manager that macOS uses to handle ODBC connections.

```bash
brew install unixodbc
```

## Step 3: Install sqliteodbc

Next, install the `sqliteodbc` driver, which will also install SQLite as a dependency.

```bash
brew install sqliteodbc
```

## Step 4: Configure the odbcinst.ini file

After installation, you must tell `unixodbc` where to find the `sqliteodbc` driver. The configuration files are located in Homebrew's `etc` directory.
Find the configuration file location. Run the following command to determine the exact path of your `odbcinst.ini` file:

```bash
odbcinst -j
```

The output will tell you the path, which is commonly `/usr/local/etc/odbcinst.ini` on Intel Macs and `/opt/homebrew/etc/odbcinst.ini` on Apple Silicon Macs.
Edit the configuration file. Open the `odbcinst.ini` file with a text editor. Using `nano` is a good option:

```bash
nano /opt/homebrew/etc/odbcinst.ini
```

(Replace the path with the one from the previous step if it differs.)
Add the SQLite3 driver. Add the following block of text to the `odbcinst.ini` file, including the correct path to the driver library. To find the library path, you can run `brew test sqliteodbc`.

```ini
[SQLite3]
Description=SQLite3 ODBC Driver
Driver=/opt/homebrew/lib/libsqlite3odbc.dylib
```

For users with a different Homebrew prefix, the path to the driver might be `/usr/local/lib/libsqlite3odbc.dylib`.

## Step 5: (Optional) Create a Data Source Name (DSN)

If you want to use a DSN to simplify your connection strings, you can create or edit the `odbc.ini` file.
Create or edit the `odbc.ini` file. For a user-specific DSN, create the file in your home directory:

```bash
nano ~/.odbc.ini
```

For a system-wide DSN, edit the `odbc.ini` in Homebrew's etc directory (using sudo):

```bash
sudo nano /opt/homebrew/etc/odbc.ini
```

Add your DSN. Add an entry that specifies the driver and the location of your database file.

```ini
[your_dsn_name]
Driver=SQLite3
Database=/path/to/your/database.sqlite
```

## Step 6: Test the connection

You can use the `isql` command-line tool (part of `unixodbc`) to verify your connection.
To test without a DSN, specify the driver directly:

```bash
isql -v SQLite3 "Database=/path/to/your/database.sqlite"
```

To test with a DSN, reference the name you created in the `odbc.ini` file:

```bash
isql -v your_dsn_name
```

If the connection is successful, you will see a prompt like `SQL>`. To exit, type `quit`.

[Home](/README.md) --- [Top](#install-sqlite3-odbc-driver---macos)
