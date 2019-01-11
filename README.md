# jira-docker

docker-compose for jira including mysql db + https with letsencrypt-auto-renewal 

# Description 

With this project you can easily run jira-software inside one docker-compose including https and a mysql database

It is based on the docker image atlassian-jira-software:7.13.0 from cptactionhank and mysql:5

[https://hub.docker.com/r/cptactionhank/atlassian-jira/](https://hub.docker.com/r/cptactionhank/atlassian-jira/)

* Note: If you want to use another version of jira or to use jira and not jira-software, just change it inside the docker-compose.yaml


# Prerequisite

* Docker needs to be installed
* Docker-Compose needs to be installed

# Usage

## Set env variables

```
export JIRA_MYSQL_ROOT_PASSWORD=supersecret
export JIRA_MYSQL_DATABASE=jiradb
export JIRA_MYSQL_USER=jiradbuser
export JIRA_MYSQL_PASSWORD=12345
export JIRA_HOST=jira.example.com
export JIRA_EMAIL=admin@jira.example.com
```

## Update dbconfig.xml

Replace 
* JIRA_MYSQL_USER 

and

* JIRA_MYSQL_PASSWORD

with the correct username and password like in the env variables

## Variant A: Start it without importing data from another system

* docker-compose up

## Open it and configure

* http://jira.example.com
* https://jira.example.com

or if you want to try it on your local machine

* http://localhost:80 - nginx
* http://localhost:8080 - jira

## Variant B: Start it with importing data from another jira instance

### export 


login to your current jira and backup the mysql data and the jira data

Something like ...

* mysql: mysqldump -u jiradbuser -p --opt --single-transaction jiradb > "/home/server/jira-backup-sql.sql" 
* data: tar -zcf /home/server/jira-back-var-application-data.tar.gz /var/atlassian/application-data/jira/data &>/dev/null

### import 

start the mysql container only and import data there:

* docker-compose up mysql
* docker cp ./jira-backup-sql.sql mysql:/
* docker exec -it mysql bash
* mysql -u root -p jiradb < /jira-backup-sql.sql. # replace jiradb with the name of the jira databasename

then you start jira and import data there

* docker-compose up jira
* docker cp jira-backup-data.zip jira:/
* docker exec -it jira bash
* unzip /jira-backup-data.zip -d /var/atlassian/jira/

then you need to reconfigure some settings like look and feel and plugins as they were not exported (the data for the plugins were stored in the mysql export before so they are not lost)

* install add-ons "Tempo"
* configure look and feel again

# Contact us

If you need more information feel free to contact us 

[https://froso.de](https://froso.de)