# django-docker-bootstrap
[![Requirements Status](https://requires.io/github/legios89/django-docker-bootstrap/requirements.svg?branch=master)](https://requires.io/github/legios89/django-docker-bootstrap/requirements/?branch=master)
[![Build Status](https://travis-ci.org/legios89/django-docker-bootstrap.svg?branch=master)](https://travis-ci.org/legios89/django-docker-bootstrap)
[![Dependency Status](https://david-dm.org/legios89/django-docker-bootstrap/master.svg?path={{cookiecutter.repo_name}}/react/)](https://david-dm.org/legios89/django-docker-bootstrap/?path={{cookiecutter.repo_name}}/react/)
![postgres](https://img.shields.io/badge/postgres-9.4-brightgreen.svg)
![nginx](https://img.shields.io/badge/nginx-1.8.1-brightgreen.svg)

## Concept
A [`cookiecutter`](https://github.com/audreyr/cookiecutter) template for Django/Nodejs(React)/Nginx/Postgres. The main idea behind this project to create an easily configurable and easy to use django development/production environment for any project.

## Installation
* Docker requires a 64-bit OS.
* Install the docker(1.12.1) - https://docs.docker.com/engine/installation/
* docker-compose(1.8.0) - ```pip install docker-compose```
* Requirements for cookiecutter: ```apt-get install python-dev```
* Then get cookiecutter: ```pip install Markdown cookiecutter```
* Finally enter the directory where you want to store your project and enter the following:
      ```cookiecutter https://github.com/Elienvalleau/django-docker-bootstrap.git```

## Cookiecutter params
* project_name: The name of the project.
* repo_name: The directory name(automtically generated from the project_name).
* db_password: The postgres user password for the database.
* use_translation: Translation integration(Rosetta, ModelTransalation, admin integration).
* use_react: React, NodeJS, React-Router to replace the django frontend.
* admin_name: The name of the admin user who will receive the error messages.
* admin_email: The email address of the admin user who will receive the error messages.
* email_host_user: The gmail email address what the system can use to send emails.
* email_host_password: The gmail email address password what the system can use to send emails.

## Usage
* Build the images: ```bash buildall.sh```
* Start the project: ```docker-compose up ```
* You can set every secret variable in the  ```.secret``` in the root
* If you want to run the project in production mode you need to set the following environment variable:         
    * ```COMPOSE_FILE="production-docker-compose.yml"```
    * https://docs.docker.com/compose/reference/overview/#compose-file

## Tips & Tricks
* Every image has a container_shared directory linked as a volume, so if you want to put something inside the container, or
you want to get something from the containers like a backup file you just need to copy everything into this directory.
* Create a bash alias for for the docker-compose by edit the ```.bash_aliases``` file ```alias dc='docker-compose'```
* Enter the container as root: ```dc run --rm postgres shell root```
* You can use vim in every container.
* https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04
* better prompt: https://github.com/vertisfinance/gitprompt

## Volumes
- The data volume, automatically created on the first start, which will contains every data.
- You can find it in every container in here: ```/data/```

## Images
1. postgres
 * postgresql-9.4
 * The 5433 port is open by default if you want to connect the db with a client
 * Commands:
    * start - start the database
    * shell - start a bash shell ```dc run --rm postgres shell```
    * backup - create a backup (```/data/backup/<backup_name>```) ```dc run --rm postgres backup```
    * restore - restore from a backup (```/data/backup/<backup_name>```) ```dc run --rm postgres restore```
2. django-python3
 * The projects can be found under the /src/ directory
 * Installed Apps:
    * Django: 1.10.1
    * uWSGI: 2.0.13.1
    * psycopg2: 2.6.2
    * django-debug-toolbar: 1.5
    * djangorestframework: 3.4.6 + optional packages
    * django-cleanup: 0.4.2
    * django-extensions: 1.7.4
    * django-compressor: 2.1
    * django-rosetta: 0.7.12 [optional]
    * django-modeltranslation: 0.11 [optional]
 * Commands:
    * shell -start a bash shell ```dc run --rm django shell```
    * start_runserver - in development mode this will start django runserver
    * start_uwsgi - in production mode this will start the uwsgi
3. nginx
 * Commands:
    * shell -start a bash shell ```dc run --rm nginx shell```
 * Installed Apps:
    * Nginx: 1.8.1
4. nodejs [optional]
 * Commands:
      * shell -start a bash shell ```dc run --rm nodejs shell```
      * start_build - start the react build process than exit ```dc run --rm nodejs start_build```
      * start_watchify - start the watchify process for the development mode
   * Installed Apps:
      * nodejs: 4.x.x
      * npm: 2.x.x
   * Installed Packages: [automatically installed]
      * react: 15.3.1,
      * react-dom: 15.3.1,
      * babelify: 7.3.0,
      * babel-preset-react: 6.11.1,
      * browserify: 13.1.0,
      * watchify: 3.7.0,
      * react-router: 2.8.0
