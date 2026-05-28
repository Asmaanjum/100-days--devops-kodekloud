# Day 17 : install and configure postgreSQL

## Task : postgreSQL database server is already installed on the nautilus database server
a.create a database user kodekloud_pop and set its password to B4zNgHA7Ya
b.createa database kodekloud_db8 and grant full permissions to user kodekloud_pop on this database

## step 1 : SSH to the database server
'''ssh peter@stdb01'''

WHY:postgreSQL is installed on the database server (stdb01), not the jump host we must connect to the correct server first
RESULT:logged into the database server as user peter

## step 2 : switch to postgres user shell
'''sudo -i -u postgres'''

WHY: postgreSQL commands can only be run by the postgres system user (the DB admin) regular users dont have permission
RESULT:switch to the postgres user shell

## step 3 : open psql
'''psql'''

WHY : opens the postgreSQL interactive terminal where we can run SQL commands
RESULT:entered the postgres=#prompt

## step 4 : creating new user
'''CREATE USER kodekloud_pop WITH PASSWORD 'B4zNgHA7Ya';'''

WHY:the application needs a dedicated database user to connect securely--never use the default postgres superuser for apps
RESULT:CREATE ROLE--new user created

## step 5 : new database created
'''CREATE DATABASE kodekloud_db8;

WHY:the application needs its own isolated database to store its data
RESULT:CREATE DATABASE-- new database created

## step 6 : grant permission
'''GRANT ALL PRIVILEGES ON DATABASE kodekloud_db8 TO kodekloud_pop'''

WHY:the user kodekloud_pop needs full access (read,write,delete) to that database to function properly
RESULT:GRANT--permissions assigned

## step 7 : exit
'''\q'''

WHY:exit psql cleanly after work is done
RESULT: returned to postgres shell


