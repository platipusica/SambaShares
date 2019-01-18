# Jam.py Application for Samba Shares (compatible with Python3)

[![Build Status](https://api.travis-ci.org/platipusica/jam-py.png?branch=master)](http://travis-ci.org/platipusica/jam-py)
![Python versions](https://img.shields.io/pypi/pyversions/python3-saml.svg)

The Problem
=============
Some *nix environments might have a large number of servers with Samba shared folders and reoccurring access groups/users for particular shared folders.
The core Samba configuration ([global] section in smb.conf) is rarely changing, but the shares might, the problem is how to maintain the smb.conf file for all servers in controlled fashion over the Web?



The Solution
=================

The Jam.py Samba Shares Application is my take on solving the problem of:

* The Application Access in an secure and monitored way over the Web
* The Complete Actions History on any Samba Share (or any other part of App) 
* Custom Comments (ie Incident in Service Ticketing Software) 
* Search on any records
* Reports and Dashboard (wip)


How does it work?
=================

Please visit Heroku App:

https://sambashares.herokuapp.com

Here, you're presented with the Samaba Shares Application. On the Catalogs Menu is everything what User shouldn't access, Samba Server, Users, Veto Files and Valid Groups. The Samba Server here is just an example for each Server in question. We need a Header to Warn the server console users who might manually update the smb.conf file, and to store all other information for Samba functioning (warning, updating the system might update the smb.conf).
We need Path to write the content of generated smb.conf somewhere where the Web server can read/write.

After the server is updated with a share, the new entry added, deleted or shares disabled, by click on "Save", the Application automatically creates the smb.conf  file (overwrites the old one).


The file smb.conf would look like:
```
#WARNING GENERATED FILE
# Global parameters
[global]
	server string = %h server (Samba, Ubuntu)
	server role = standalone server
	map to guest = Bad User
	obey pam restrictions = Yes
	pam password change = Yes
	passwd program = /usr/bin/passwd %u
	passwd chat = *Enter\snew\s*\spassword:* %n\n *Retype\snew\s*\spassword:* %n\n *password\supdated\ssuccessfully* .
	unix password sync = Yes
	syslog = 0
	log file = /var/log/samba/log.%m
	max log size = 1000
	dns proxy = No
	usershare allow guests = Yes
	panic action = /usr/share/samba/panic-action %d
	idmap config * : backend = tdb


[printers]
	comment = All Printers
	path = /var/spool/samba
	create mask = 0700
	printable = Yes
	browseable = No


[print$]
	comment = Printer Drivers
	path = /var/lib/samba/printers

[Test_Share_1]
	create mask = 0660
	read only = no
  .
  .
	valid users = @group1 @group2
[Test_Share_2]
	create mask = 0664
	veto files = /file 1/file 2/file 3/
  .
  .
	valid users = @group3 @group4
#WARNING GENERATED FILE

```

How to run in *your* environment?
==================================

Download this repo, and run it. The App will be in read only mode. Change the file jam/server_classes.py and remove lines 208-211 to remove read only. If you like the App, completely remove the jam folder, which is here only for r/o App, install the latest Jam.py (with Python virtenv as a preference), and try the App again. To try the user access, open Application builder, click on Parameters/Safe Mode. The users are admin/111 as seen in Users Catalog.

If all good, add some server details (like file location plus the server name DOT conf), and the rest on Catalogs menu. After that, add some server shares. Click on "Save" will create or overwrite server.conf file. Copy the file to the server with cron or similar.

The above is based on assumption that the App runs as root. If the App is running with mod_wsgi and Apache, the OS permissions is a problem as Apache usually runs as non root user. One option is to change the permissions on files/folders to match the Apache user. For sure all Apache files can be overwritten with this way. The other option might include writing files somewhere, and picking the files by cron.

One thing to remember, nothing is really deleted in this App. Jam.py is using a flag deleted=1 in the table for deleted records. Plus, there is a History for any record, hence audited. For even higher protection, one could Export/Import data into a more secure database, like Oracle, Postgres, MySQL or MSSQL. Simple.

Further Enhancements 
=================
One can add any Samba smb.conf *name = value*. To create a *name*, Open Application builder/Project/Task/Group/Journals/Samba Share, and add it there as a *value* which is a Lookup value created on Project/Task/Lookup lists.
This App was based on my other Jam.py app, http://jampy-aliases.herokuapp.com, hence some table names will reference email aliases. This can be refactored.

About Jam.py
=================

With Jam.py you can create, customise, test and share awesome, fast, event-driven applications for SQLite, Oracle, MySQL, PostgreSQL and Firebird. All of that for free and no vendor lock-in!

How was this Demo published on Heroku?
------------------------------------
The Samba Shares App you see on Heroku is just the Jam.py Project with two files added: requirements.txt and Procfile.

Then the Heroku account was open, jampy App created, Git repo linked and deployed. In 10 seconds it magically appeared as a live Web site. 

My second Jam.py Demo lives here: http://jampy-aliases.herokuapp.com
The same principles apply.

Enjoy.


Why using Jam.py?
------------------------------------
DRY principle! Don't repeat yourself, do it once, do it well.

With Jam.py Application Builder, you can resolve a specific business problem. Out of the box Jam is providing: fast access to underlying databases, security, authentication, validation, calendars, multi languages, all of that with minimum of coding needed. Being Python framework, it is extensible and flexible.

**“All in the browser” framework**
* Internet Browser IDE
* Code Editor with Syntax Highlighting and Code Completion
* No declarative options, you are in charge.
* Instant WYSIWYG
* Application lifecycle tracking.
* SQL and stored procedures supported for major vendors.
* Integrate any Python library with no contract lock-in and reduce cost instantly.
* Bootstrap, JQuery, JS, all in here. Use it with no fuss and learning this massive libraries.
* Create rich, informative reports, due to band-oriented report generation based on LibreOffice templates.

**Event driven grids**

* Event driven grids enable you to easily manipulate data simply by clicking on a cell and editing its value.
* Event driven data-aware visual interface controls makes the framework flexible and powerful.
* Edit your data in the grid, as you would in any Desktop spreadsheet application.
* Create the master-detail table with breeze, utilising templates for displaying, which is no more than a copy/paste. Easy. * Again, no declarative methods, the control is with you as it should be.

**jsCharts or any charting libraries**

* Locked-in with a vendor charting capabilities? Never again. Use free libraries as jsChart, at.al.
* Use the same charting capabilities on your mobile devices, once for all.
* Visualise charts immediately after you create/import tables, with a few lines of code. Simple and effective.
* Analyzing/displaying BIG data? Add free Python lib's, build a Jam Web Form with parameters, execute on the server. 
* Profit.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.


Jam.py Installation
------------

### Dependencies

 * python 2.7 // python 3.6
 * For MySQL database access: mysqlclient, libmysqlclient-dev
 * For Oracle database access: cx_oracle
 * For Firebird database access: fdb
 * For Jam.py Reports editing/creation: LibreOffice

## Installing an official release with pip


The easiest is to use the standalone pip installer.

If you’re using Linux, Mac OS X or some other flavor of Unix, enter the command:
```
sudo pip install jam.py 
```
at the shell prompt. If you’re using Windows, start a command shell with administrator privileges and run the command:
```
pip install jam.py
```
This will install Jam.py in your Python installation’s site-packages directory.


## Installing an official release manually

Download the package archive from https://github.com/jam-py/jam-py/tree/master

Create a new directory and unzip the archive there.

From the above directory, enter the command:

```
sudo python setup.py install
```

This will install Jam.py in your Python installation’s site-packages directory.

## Running the Demo App

Navigate to jam.py installation demo folder, enter the command:
```
python server.py
```

You'll have the Demo App running at http://localhost:8080

## Create a new App

```
mkdir newapp
cd newapp
jam-project.py
python server.py
```
The new and empty App will run at http://localhost:8080

Please visit http://jam-py.com/docs/intro/index.html for complete Getting Started Introduction.


## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.



## Authors

* **Andrew Yushev** - [Jam-py.com](https://github.com/jam-py)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the BSD License.

## Acknowledgments

* Hat tip to anyone who's code was used
* Inspiration
* etc
# SambaShares
