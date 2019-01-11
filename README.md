# jira-docker

docker-compose for jira including letsencrypt auto renewal https 

# Description 

With this project you can easily run jira-software inside one docker-compose including https. 

# Prerequisite

* Docker needs to be installed

# Usage

## Set env variables

```
export JIRA_MYSQL_ROOT_PASSWORD=__ROOT_PASSWORD__  # e.g. supersecret
export JIRA_MYSQL_DATABASE=jiradb
export JIRA_MYSQL_USER=jiradbuser
export JIRA_MYSQL_PASSWORD=__JIRAUSER_PASSWORD__ # e.g. 12345
export JIRA_HOST=__YOUR_HOSTNAME__     # e.g. jira.example.com
export JIRA_EMAIL=__EMAIL___       # e.g. admin@jira.example.com

```

## Update dbconfig.xml

Replace 
* JIRA_MYSQL_USER and
* JIRA_MYSQL_PASSWORD 
with the correct username and password like in the env variables

## Start it

* docker-compose up

# Contact us

If you need more information feel free to contact us 

[https://froso.de](https://froso.de)