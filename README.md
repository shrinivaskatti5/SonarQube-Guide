# sonarqube-installation-guide

## Step 1: Install java 17.

```bash
yum install java-17-amazon-corretto -y
```

## Step 2: Login as root and execute the following commands.

```bash
sysctl -w vm.max_map_count=262144
sysctl fs.file-max
ulimit -n
ulimit -u
```

## Setup PostgreSQL15 Database For SonarQube.

```bash
yum install postgresql15 postgresql15-server -y
postgresql-setup --initdb
```

Need to change config file as shown in below

```bash
vi /var/lib/pgsql/data/pg_hba.conf
```

Replace Method name "ident" to "md5"

![image](https://user-images.githubusercontent.com/68885738/90953619-aef2f800-e48a-11ea-9b50-489183e9b0c1.png)

Enable  postgresql:

```bash    
systemctl enable postgresql
```
Start postgresql:

```bash
systemctl start postgresql
````

Login into Database

```bash
su - postgres
psql
```

Create a sonarqubedb database, username and provide access to user

```bash
create database sonarqubedb;
create user sonarqube with encrypted password 'Naresh#240';
grant all privileges on database sonarqubedb to sonarqube;
\c sonarqubedb;
GRANT ALL ON SCHEMA public TO sonarqube;
GRANT USAGE ON SCHEMA public TO sonarqube;
\q
exit
```

## Setup Sonarqube Web Server.
Download the latest sonarqube installation file to /opt folder. You can get the latest download link from here. http://www.sonarqube.org/downloads/

 ```bash
cd /opt
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.0.65466.zip
unzip sonarqube-9.9.0.65466.zip
mv sonarqube-9.9.0.65466 sonarqube
chown -R ec2-user:ec2-user sonarqube
```
	
Open /opt/sonarqube/conf/sonar.properties file

```
sudo vi /opt/sonarqube/conf/sonar.properties
```
![image](https://user-images.githubusercontent.com/68885738/90953687-7acc0700-e48b-11ea-94f9-4b32f8f170b0.png)
![image](https://user-images.githubusercontent.com/68885738/90953736-c1b9fc80-e48b-11ea-88f9-2629c85fdf56.png)
![image](https://user-images.githubusercontent.com/68885738/90953772-05146b00-e48c-11ea-8dab-143be09d878b.png)

Switch to ec2-user and navigate to the start script directory

```bash
su - ec2-user
cd /opt/sonarqube/bin/linux-x86-64
./sonar.sh start
./sonar.sh status
```

Troubleshooting Sonarqube:
All the logs of sonarqube are present in the /opt/sonarqube/logs directory

```bash
cd /opt/sonarqube/logs
```
