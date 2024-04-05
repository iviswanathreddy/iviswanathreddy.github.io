---
title: SonarQube Installation in Linux 🐧
date: 2024-03-26 10:14 +0300
categories: [Blogging, Tutorial]
tags: [Linux, devops, sonarqube]
author: viswa
---

# PostgreSQL and SonarQube Installation and Integration in RHEL.
![Desktop View](/assets/posts/2024-03-26-SonarqubeInstallation/Sonarqube.png){: width="700"}
_This Story illustrates the components with the color of the above image._

SonarQube/Cloud Installation, exploration and Integration (Azure DevOps) has been illustrated in three stories simultaneously.

[Part 1](https://viswanathreddy.substack.com/p/postgresql-and-sonarqube-installation)

[Part 2](https://viswanathreddy.substack.com/p/exploring-sonarqube-or-sonarcloud)

[Part 3](https://viswanathreddy.substack.com/p/integrating-sonarcloud-with-azure)

This is Part 1 of 3.


## Installing PostgreSQL on Red Hat Enterprise Linux (RHEL):

1.  Add the PostgreSQL Yum repository to your system by creating a new file called `pgdg.repo` in the `/etc/yum.repos.d/` directory:

    ```shell
     sudo vi /etc/yum.repos.d/pgdg.repo
    ```

2. Add the following content to the `pgdg.repo` file:

    ```shell
    [pgdg13] name=PostgreSQL 13 for RHEL/CentOS 7 - x86_64 baseurl=https://download.postgresql.org/pub/repos/yum/13/redhat/rhel-7-x86_64 enabled=gpgcheck=01
    ```

    Note: You can replace `13` with the version of PostgreSQL you want to install, if it’s different.

3. Update the package list and install PostgreSQL:

    ```shell
    sudo yum update
    sudo yum install postgresql-server
    ```

4. Initialize the PostgreSQL database:

    ```shell
    sudo postgresql-setup initdb
    ```

5. Start the PostgreSQL service and enable it to start on boot:

    ```shell
    sudo systemctl start postgresql
    sudo systemctl enable postgresql
    ```

6. That’s it! PostgreSQL should now be installed and running on your RHEL system. You can log in to the PostgreSQL server by running the following command:

    ```shell
    sudo -i -u postgres
    psql
    ```

From here, you can start creating databases and tables.

## Installing SonarQube and integrating it with PostgreSQL.

1. Install Java 11 if it is not already installed:

    ```shell
    sudo yum install java-11-openjdk-devel
    ```

2. Download and install the latest version of SonarQube from the official website:

    ```shell
    sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.3.1.45539.zip
    sudo unzip sonarqube-9.3.1.45539.zip -d /opt
    ```

3. Create a new PostgreSQL database and user:
    
    ```shell
    sudo su - postgres
    create user sonar
    psql
    ALTER USER sonarqube WITH ENCRYPTED password 'password';
    create database sonarqube;
    GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;
    \q
    exit
    ```

    Note: Replace `password` with a strong password of your choice.

4. Configure SonarQube to use PostgreSQL by editing the file `/opt/sonarqube/conf/sonar.properties`:
   
    ```shell
    sonar.jdbc.username=sona
    sonar.jdbc.password=password
    sonar.jdbc.url=jdbc:postgresql://localhost/sonarquber
    ```

    Note: Replace `password` with the password you set in step 3.

5. Start SonarQube:
   
    ```shell
    sudo /opt/sonarqube/bin/linux-x86–64/sonar.sh start
    ```

6. Open the SonarQube web interface by visiting `http://your_server_ip:9000` in a web browser.

7. Log in with the default credentials (admin/admin) and follow the prompts to change the password.

    That’s it! SonarQube should now be up and running and integrated with your PostgreSQL database.

    You can now start analyzing your projects and improving your code quality.

**Consider subscribing my [substack](https://viswanathreddy.substack.com/).**