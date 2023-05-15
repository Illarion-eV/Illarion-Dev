# How to use the local Illarion development server

## 0. Preface

This server is a tool for Illarion script and map development. It allows you to
test your scripts and maps locally before uploading them to the Illarion server. 
It can also be used for website development as well as for client development.

## 1. Preparation

To use the local server you need
* to [download and install Docker](https://docs.docker.com/engine/install/).
**Carefully follow the installation instructions for your OS.**
* a local copy of this repository. You can find the
[official repository on GitHub](https://github.com/Illarion-eV/Illarion-Dev).

## 2. Setup

You _need_ both the script and the maps repository. Without
_both_ you cannot use the server. For website development you will also need the 
website repository.

1. Copy the file `.env.dist` and name that copy `.env`.
2. Using a text editor, fill in local repository paths in `.env`. **You need to use absolute paths.**
3. For unused repositories, paths can be left blank.

### Where to get your local repository from:

#### For testing only:

If you were asked to test a specific new feature, you have been given URLs to snapshots of both the scripts
(Illarion-Content) and maps (Illarion-Maps) repositories on GitHub. These will look like this:

* Scripts: https://github.com/some-user-name/Illarion-Content/tree/some-feature-name
* Maps: https://github.com/some-user-name/Illarion-Map/tree/some-feature-name

1. For each, click the green button labeled "Code" and select "Download ZIP".
2. Unzip each to a convenient location.
3. Enter the paths to these locations into your `.env` file.

Some tests also require changes to the database. In that case, you will have received a set of SQL commands.

You are now ready for [testing](#5-testing).

---

#### Development:

While you can clone the official GitHub repositories to your disk directly, it is **highly recommended** that you fork
them on GitHub first and clone from your own fork. This way you can push changes to your GitHub repository and then
issue a pull request against the official repository to have your changes included.

##### Official Illarion GitHub repositories:

* Scripts: https://github.com/Illarion-eV/Illarion-Content
* Maps: https://github.com/Illarion-eV/Illarion-Map
* Website: https://github.com/Illarion-eV/Illarion-Website

##### For Illarion developers with push access only:

* Scripts: ssh://git@illarion.org:1010/scripts
* Maps: ssh://git@illarion.org:1010/maps
* Website: ssh://git@illarion.org:1010/website

## 3. Running the server

* To start the game server, open a console and run `start` on Linux or `start.bat` on Windows.
* Press <kbd>ctrl</kbd>+<kbd>c</kbd> in that console to stop the server.
* To additionally run the webserver, run `start-with-webserver` instead.
* You can reload maps _while the server is running_ by executing `reload-maps`.

## 4. Connecting to the game

Fire up the [Illarion loader](http://illarion.org/illarion/us_java_download.php) as usual.

You want to use the development client, so in your Illarion loader you need to adjust the
update channel under "Options" accordingly.

After launching the client, select the **local** server. Now you can login with the default character:  
* Name: Testserver One
* Password: test

The character "Testserver Two" is also provided, if you need to test with multiple characters.
Of course you can also add new characters to the database.

## 5. Testing

Follow these steps every time you get new URLs or SQL statements for testing:

1. While the server is not running, run `reset-database` to get a clean database.
2. [Start](#3-running-the-server) the server by running `start`.
3. [Reload](#3-running-the-server) your maps by running `reload-maps` .
4. If you have received SQL commands:
    1. [connect to the database](#7-database-access).
    2. In the left column select `Local Illarion` > `illarion` > `illarionserver`.
    3. Click `SQL` in the very top right, paste your commands in the large text box and execute them.
5. [Connect](#4-connecting-to-the-game) to the game.
6. Run !fr inside the game to reload the database.
7. Conduct your tests.
8. Give a test report in the pull request you got the URLs from.

## 6. Script development

Reloading scripts on the local server works as it does on the official server. 
You need to use the reload command (!fr) in-game for the server to load all scripts.

This server reads scripts directly from your working copy of the
scripts repository. This means you do not need to commit them for testing.
You can commit whenever a logical step has been completed.

You need to commit and push your changes in order to allow others to see and test them.
If you are unfamiliar with Git, you could take a look at the [Pro Git book](https://git-scm.com/book/en/v2)
or other [tutorials](http://try.github.io/), depending on your preference.

Each feature you are working on should have its own dedicated branch.
* You can create a branch for working on magic with `git branch feature/magic`.
* Switching to that branch is done by `git checkout feature/magic`.
* You can even push your branches to your github account to share your work with others for collaboration and testing.
* To merge your work on magic into develop, simply checkout develop and then run `git merge feature/magic`.

## 7. Database access

While the server is running, access the database with [pgAdmin4](http://www.pgadmin.org) within your browser: http://localhost:8080/

Login information:  
* User: illarion  
* Password: illarion

If you prefer other tools installed locally, use the following connection details:
* Hostname: localhost
* Port: 15432

To connect with e.g. psql: `psql -W -U illarion -h localhost -p 15432 illarion`

## 8. Updating the server

You should keep your local copy of this repository up-to-date.

The database is initially created as a copy of the official development server database excluding
character data. You may update the database at any time to incorporate
changes made to the online database. However, be aware that
**_this will overwrite any changes you made to the local database_**.
To update the database run `reset-database` while the server is down.

## 9. Using the local web server

You can also use the local server for website development. For that you need to
set up the website folder as described in step 2 prior to starting the server. Also
you need to make your system believe that your server is actually illarion.org by
editing your [hosts config file](http://en.wikipedia.org/wiki/Hosts_(file)#Location_in_the_file_system).

Open the file with admin privileges and add a line at the end:
`127.0.0.1 illarion.org`

When the server is running you can now access your local copy of the website at
https://illarion.org

Website Login:
* User: test
* Password: test
