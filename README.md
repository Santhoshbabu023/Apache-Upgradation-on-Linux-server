# Apache-Upgradation-on-Linux-server
This document outlines the steps required to upgrade Apache HTTP Server on a Linux system. It assumes that you have a working Apache installation and sufficient permissions to carry out the upgrade.

## Prerequisites
- A Linux server (physical or virtual) with an existing Apache installation.
- Root user access.
- Basic familiarity with the Linux command line interface.

## Steps to Upgrade Apache Services

### Step 1: Download the Latest Apache Version
1. Visit the official [Apache HTTP Server download page](https://httpd.apache.org/download.cgi) to get the latest version of Apache.
2. Download and extract the following dependencies:
   - [PCRE2 (Perl Compatible Regular Expressions)](https://www.pcre.org/).
   - [Apache Portable Runtime (APR)](https://apr.apache.org/).
   - [APR Utilities](https://apr.apache.org/).
   - [Expat](https://libexpat.github.io/).

   Ensure that you download the `tar.gz` files for each package.

### Step 2: Upload Files to the Server
Copy the downloaded files to the `/tmp` directory of your server.

### Step 3: Log in as Root User
Log into the server as the root user:

```bash
sudo su
```

### Step 4: Locate the Current Apache Installation
Run the following command to check the path of the current Apache installation:

```bash
ps aux | grep httpd
```

### Step 5: Navigate to the Apache Installation Path
Use the path from the previous step to navigate to the current Apache installation directory.

```bash
cd /path/to/apache/installation
```

### Step 6: Create Installation Folder
Create a new folder where you will install the upgraded Apache version:

```bash
mkdir /path/to/apache_installfiles
```

### Step 7: Extract the Downloaded Files
Copy the downloaded `tar.gz` files to the newly created folder and extract them:

```bash
cp /tmp/* /path/to/apache_installfiles
cd /path/to/apache_installfiles
tar -xvzf apache-*.tar.gz
```

### Step 8: Install PCRE2
Navigate to the extracted PCRE2 directory and configure the installation:

```bash
cd pcre2-10.42
./configure --prefix=/apps/apache2.4.59_installfiles/pcre2-10.42
make
make install
```

### Step 9: Install APR (Apache Portable Runtime)
Navigate to the APR directory and configure the installation:

```bash
cd APR-xxxx
./configure --prefix=/user/ecs/apps/apache2.4.62_installfiles/apr-1.7.4
make
make install
```

### Step 10: Install APR Utilities
Navigate to the APR Utilities directory and configure the installation:

```bash
cd apr-utilities-xxxx
./configure --prefix=/user/ecs/apps/apache2.4.62_installfiles/apr-util-1.6.3 --with-apr= 
make
make install
```

### Step 11: Install APR Iconv
Navigate to the APR Iconv directory and configure the installation:

```bash
cd apr-iconv-xxxx
./configure --prefix=/storage/apr-iconv --with-apr=/storage/apr/ --with-apr-util=/storage/apr-util/ --with-pcre=/storage/pcre
make
make install
```

### Step 12: Install Apache HTTP Server
Navigate to the Apache source directory and configure the installation:

```bash
cd httpd-2.4
./configure --prefix=/storage/apache2.4 --enable-mods-shared=all --with-mpm=prefork --enable-ssl --with-apr-util=/apps/apr-util --with-apr=/apps/apr --with-apr=/apps/pcre --with-ssl --enable-so
make
make install
```

### Step 13: Verify Apache Folder Creation
Once the installation completes, you should see a new folder named `/storage/apache2.4`.

### Step 14: Update File Ownership
Change the ownership of the newly created Apache files:

```bash
chown -R sisladm:www /storage/apache2.4
```

### Step 15: Set File Permissions
Ensure that the correct permissions are applied to the files:

```bash
chmod -R 775 /storage/apache2.4
```

### Step 16: Update Apache Configuration Path
Update the `httpd.conf` file to point to the new Apache directory. Use `sed` to replace the old paths with the new ones:

```bash
sed -i "s/old_path/new_path/g" /path/to/httpd.conf
```

### Step 17: Test Apache Configuration
Navigate to the `bin` directory and test the Apache configuration:

```bash
cd /storage/apache2.4/bin
./apachectl -t
```

The output should show `Syntax Ok` if the configuration is correct.

### Step 18: Stop Apache Services
Stop the running Apache service before making further changes:

```bash
./apachectl -k stop
```

### Step 19: Update Service Configuration
Reconfigure the `httpd.service` file to reflect the new Apache version path. Work with your Linux team to update and restart the system daemon.

### Step 20: Start Apache Services
Start the Apache service again:

```bash
./apachectl -k start
```

### Step 21: Verify Apache Version
Check the Apache version to ensure the upgrade was successful:

```bash
./apachectl -v
```

---

## Apache Upgrade README

### Overview
This document provides the steps to upgrade Apache HTTP Server on a Linux server. It includes instructions to install dependencies such as PCRE2, APR, APR utilities, and Expat, as well as the Apache HTTP Server itself. The upgrade process also includes configuration and permission adjustments to ensure that the server runs the new version.

### Requirements
- Root or sudo access to the server.
- The following software dependencies: PCRE2, APR, APR Utilities, Expat, and the latest Apache HTTP Server package.
- Basic command-line knowledge and tools such as `tar`, `sed`, and `make`.

### Installation Procedure
1. Download the required packages from the respective websites.
2. Upload the tarball files to your server and extract them.
3. Install dependencies (PCRE2, APR, APR Utilities, Expat) following the provided `./configure`, `make`, and `make install` commands.
4. Install the latest Apache HTTP Server and configure it with the necessary options.
5. Adjust file ownership and permissions, and update the `httpd.conf` configuration.
6. Test and verify the new Apache installation by restarting the service and checking the version.

### Notes
- Always ensure that backups of configuration files are taken before making any modifications.
- Consult the Linux team for assistance with service file modifications and system daemon restarts.
- The exact directory paths may vary based on your specific server setup.

---

By following these steps, you will successfully upgrade Apache on your Linux server to the latest version.
