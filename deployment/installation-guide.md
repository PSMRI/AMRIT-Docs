# Installation guide

## Software Installation Guide

### 1. Java Installation (JDK 17.0.3)

#### Steps:

1. Download the JDK installer from the following link:\
   [JDK 17.0.3 for Windows (64-bit)](https://download.oracle.com/java/17/archive/jdk-17.0.3.1_windows-x64_bin.exe)
2. Run the downloaded `.exe` file.
3. Follow the on-screen instructions to complete the installation.
4.  Verify installation by running the command in the terminal:

    ```
    java -version
    ```

***

### 2. MySQL Installation (8.0.34)

#### Steps:

1. Download the MySQL installer from the following link:\
   [MySQL Installer](https://dev.mysql.com/downloads/installer/)
2. Run the installer and select the desired setup type (e.g., Developer Default).
3. Follow the configuration steps for server and client settings.
4.  Complete the installation and verify by connecting to MySQL using the command-line client:

    ```
    mysql -u root -p
    ```
5. [Important] Configure Table Name Consistency
   To ensure consistent behavior of table name handling across different operating systems (especially when deploying between Windows and Linux), update the MySQL configuration to set `lower_case_table_names = 1`.
   This setting should be added under the [mysqld] section of your MySQL configuration file (my.ini or my.cnf):

   [mysqld]
   lower_case_table_names=1
   
   Verify the current setting of **lower_case_table_names** in your MySQL server by running the following SQL query:

   ```
   SHOW VARIABLES LIKE 'lower_case_table_names';
   ```
   Note: This must be set before creating any databases or tables. Changing it afterward can cause issues with case sensitivity in table names.

***

### 3. WildFly Installation (30.0.0.Final)

#### Steps:

1. Download the WildFly zip file from the following link:\
   [WildFly 30.0.0.Final](https://github.com/wildfly/wildfly/releases/download/30.0.0.Final/wildfly-30.0.0.Final.zip)
2. Extract the downloaded `.zip` file to your desired directory.
3. Navigate to the `bin` folder inside the extracted WildFly directory.
4.  Start the server using the command:

    ```
    standalone.bat
    ```

    (For Linux/Mac, use `standalone.sh` instead.)
5. Access the WildFly management console at:\
   `http://localhost:9990`

***

### 4. Redis Installation (7.2.5)

#### Steps:

1. Visit the Redis documentation for getting started:\
   [Redis Installation Guide](https://redis.io/docs/latest/get-started/)
2. Follow the platform-specific instructions to install Redis.
   * For Windows: Use the available Redis binaries.
   * For Linux/Mac: Install via package manager or compile from source.
3.  Start the Redis server using the command:

    ```
    redis-server
    ```
4.  Verify the installation by connecting to the Redis CLI:

    ```
    redis-cli
    ```

***

### 5. OpenKM Installation

#### Steps:

1. Download the OpenKM software from the following link:\
   [OpenKM Downloads](https://www.openkm.com/en/download.html)
2. Follow the installation guide specific to your platform (Windows, Linux, or MacOS) provided on the OpenKM website.
3. Configure the application by editing the necessary configuration files as per your requirements.
4. Start the OpenKM server and access the application through your web browser at the specified URL (e.g., `http://localhost:8080/OpenKM`).
5. Refer to the official OpenKM documentation for advanced configurations and usage.

***

Ensure all installations are correctly configured for your system environment and project needs.
