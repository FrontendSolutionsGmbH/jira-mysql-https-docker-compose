# jira-mysql-https-docker-compose

docker-compose for jira + mysql + https + letsencrypt-auto-renewal 

# Description 

With this project you can easily run jira-software inside one docker-compose including https and a mysql database

It is based on the docker image atlassian-jira-software:7.13.0 from cptactionhank jwilder/nginx-proxy and mysql:5  which can be changed easily by editing the [docker-compose.yaml](docker-compose.yaml)

[https://hub.docker.com/r/cptactionhank/atlassian-jira/](https://hub.docker.com/r/cptactionhank/atlassian-jira/)

[https://hub.docker.com/r/jwilder/nginx-proxy/](https://hub.docker.com/r/jwilder/nginx-proxy/)

[https://hub.docker.com/r/mysql/mysql-server](https://hub.docker.com/r/mysql/mysql-server)

# Prerequisite

* Docker needs to be installed
* Docker-Compose needs to be installed

# Usage

1. Set env variables

    ```
    export JIRA_MYSQL_ROOT_PASSWORD=supersecret
    export JIRA_MYSQL_DATABASE=jiradb
    export JIRA_MYSQL_USER=jiradbuser
    export JIRA_MYSQL_PASSWORD=12345
    export JIRA_HOST=jira.example.com
    export JIRA_EMAIL=admin@jira.example.com
    ```

2. Change dbconfig.xml

    Replace JIRA_MYSQL_USER and JIRA_MYSQL_PASSWORD with the correct username and password (same as env variables)

3. Variant A: Start it without importing data from another system

    docker-compose up

3. Variant B: Start it with importing data from another jira instance

    * export data from your old jira instance
        Something like ...
    
        * mysql: mysqldump -u jiradbuser -p --opt --single-transaction jiradb > "/home/server/jira-backup-sql.sql" 
        * data: tar -zcf /home/server/jira-back-var-application-data.tar.gz /var/atlassian/application-data/jira/data &>/dev/null

    * import 

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