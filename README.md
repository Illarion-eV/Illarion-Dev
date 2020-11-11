# How to use the local Illarion development server

## 0. Preface

This server is a tool for Illarion script and map development. It allows you to
test your scripts and maps locally before uploading them to the Illarion server. 
It can also be used for website development as well as for client development.

## 1. Preparation

To use the local server you need to [download and install Docker](https://docs.docker.com/engine/install/).  
On Linux you might want to add your user to the `docker` group, so that you can run commands without `sudo`.

You also need a local copy of this repository. You can find the [official repository on GitHub](https://github.com/Illarion-eV/Illarion-Dev).

## 2. Setup

You _need_ both the script and the maps repository. Without
_both_ you cannot use the server. For website development you will also need the 
website repository.
**Copy the file `.env.dist` and name that copy `.env`. Using a text editor, fill in local repository paths in `.env`**.
For unused repositories paths can be left blank.

##### For Illarion developers with push access:

* Script repository: ssh://git@illarion.org:1010/scripts
* Map repository: ssh://git@illarion.org:1010/maps
* Website repository: ssh://git@illarion.org:1010/website

##### For everyone else:

* Script repository: https://github.com/Illarion-eV/Illarion-Content
* Map repository: https://github.com/Illarion-eV/Illarion-Map
* Website repository: https://github.com/Illarion-eV/Illarion-Website

## 3. Running the server

To start the game server run `start`. Press <kbd>ctrl</kbd>+<kbd>c</kbd> to stop it.  
To additionally run the webserver, run `start-with-webserver` instead.  
You can reload maps while the server is running by executing `reload-maps`.

## 4. Updating the server

You should keep your local copy of this repository up-to-date.

The database is initially created as a copy of the official development server database excluding
character data. You may update the database at any time to incorporate
changes made to the online database. However, be aware that
**_this will overwrite any changes you made to the local database_**.
To update the database run `reset-database` while the server is down.

## 5. Script development

Reloading scripts on the local server works as it does on the official server. 
You need to use the reload command (!fr) in-game for the server to load all scripts.

This server reads scripts directly from your working copy of the
scripts repository. This means you do not need to commit them for testing.
You can commit whenever a logical step has been completed, but
push working states _only_!

You need to commit and push your changes in order to allow others to see and test them.
If you are unfamiliar with Git, you could take a look at the [Pro Git book](https://git-scm.com/book/en/v2).

Each feature you are working on should have its own dedicated branch.
You can create a branch for working on magic with `git branch feature/magic`.
Switching to that branch is done by `git checkout feature/magic`.
You can even push your branches to your github account to share your work with others for collaboration and testing.
To merge your work on magic into develop, simply checkout develop and then run `git merge feature/magic`.

## 6. Database access

Use e.g. [pgAdmin](http://www.pgadmin.org) or `psql` to access the database from your OS.  
Hostname: localhost  
Port: 15432

You can also use phpPgAdmin within your browser, this is the same method we use
for development on the official server: http://localhost:8080/

Login information:  
User: illarion  
Password: illarion

## 7. Connecting to the game

Fire up the [Illarion loader](http://illarion.org/illarion/us_java_download.php) as usual.

You want to use the development client, so you need to adjust the
update channel under "Options" accordingly.

After launching the client, hit "Options" and select the server category.

Server address: localhost  
Server port: 13000  
Account login: disabled

Save and go back to the login screen.
Select the user-defined server.
Now you can login with the default character:  
Name: Testserver One  
Password: test

Of course you can add new characters to the database.

## 8. Using the local web server

You can also use the local server for website development. For that you need to
set up the website folder as described in step 2 prior to starting the server. Also
you need to make your system believe that your server is actually illarion.org by
editing your hosts config file. If you have no idea where that might be, here is
an overview where you can find the location for your operating system:
http://en.wikipedia.org/wiki/Hosts_(file)#Location_in_the_file_system

Open the file with admin privileges and add a line at the end:
127.0.0.1 illarion.org

When the server is running you can now access your local copy of the website at
https://illarion.org

Log into the website with user test and password test.
