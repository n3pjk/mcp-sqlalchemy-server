# Install MySQL ODBC Driver - macOS

[Home](/README.md)

You can install the MySQL ODBC driver on macOS using the official `.dmg` installer or the Homebrew package manager. Both methods require an ODBC driver manager, and for macOS, the recommended manager is either iODBC or unixODBC.

## Method 1: Using the official DMG installer

This method is useful if you prefer a graphical installation process.

1. Install an ODBC driver manager

    You can install the iODBC driver manager, which was historically bundled with macOS and is required by some applications.
    1. Go to the iODBC downloads page and download the macOS installer.
    2. Follow the installation steps.
2. Download the MySQL Connector/ODBC
    1. Go to the MySQL Downloads page.
    2. Choose macOS as the operating system.
    3. Select the correct architecture for your Mac: x86, 64-bit (Intel) or ARM, 64-bit (Apple Silicon, e.g., M1/M2/M3).
    4. Download the `.dmg` archive.
3. Install the connector
    1. Open the downloaded `.dmg` file and double-click the installer package (`.pkg`).
    2. Follow the installation prompts, which will guide you through accepting the license agreement and entering your password to install the software.
4. Configure the DSN (Data Source Name)

    You can configure a DSN by manually editing the configuration files or by using the ODBC Administrator GUI.
    1. Locate your ODBC configuration files. You can find their paths by running `odbcinst -j` in the Terminal. For example, the files may be at `~/.odbc.ini` and `~/.odbcinst.ini`.
    2. Add a driver entry to `~/.odbcinst.ini`:

        ```ini
        [MySQL ODBC 8.0 Unicode Driver]
        Driver = /usr/local/mysql-connector-odbc-8.0/lib/libmyodbc8w.so
        ```

    3. Create or edit `~/.odbc.ini` to add your data source:

        ```ini
        [my_mysql_dsn]
        Driver = MySQL ODBC 8.0 Unicode Driver
        SERVER = your_server_host
        DATABASE = your_database_name
        USER = your_username
        PASSWORD = your_password
        PORT = 3306
        ```

    4. Replace the placeholder values with your own server and database credentials.

## Method 2: Using Homebrew

This method is recommended if you already use Homebrew, a popular package manager for macOS, as it handles dependencies automatically.

1. Install Homebrew (if you don't have it)
  Open Terminal and run the following command:

    ```bash
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```

2. Install unixODBC and MySQL Connector/ODBC
  Run the following commands in Terminal to install the ODBC driver manager and the MySQL driver:

    ```bash
    brew install unixodbc
    brew install mysql-connector-odbc
    ```

3. Configure the DSN
    1. After installation, use the `odbcinst -j` command to confirm the location of your `.ini` files.
    2. Create or edit the `~/.odbc.ini` and `~/.odbcinst.ini` files with your connection details as described in the previous section.

## How to test your connection

After installation and configuration, you can verify your connection using a command-line tool.
If you have the `isql` utility (included with unixODBC), you can test the DSN you created:

```bash
isql my_mysql_dsn
```

If the connection is successful, you will see a prompt where you can enter SQL queries. 

[Home](/README.md) --- [Top](#install-mysql-odbc-driver---macos)