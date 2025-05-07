README file for enhanced CONFERENCE application project

In the folder you will find 6 files:
	place these files in the same working folder:
		infra.lbs
		CONFERENCE.lbs
		CONFERENCE.libini
	
	these are backed up postgres v16 databases to restore:
		conf_obf.backup
		stb.backup
	
	this is a script with the group roles that the database objects need 
		roles.sql
	

If you see a directory called omniscient-logs, ignore it.  

Here are your setup instructions:
1.	connect to your Postgres instance and run the roles.sql script
2.	create a database called conf_obf
3.	restore the backup conf_obf into it (you may get an error reporting transaction_timeout but it can be ignored)
4.	create a database called stb
5.	restore the backup stb.backup into it
6.	assign role _developer to your own login id to access everything in stb and conf_obf

conf_obf is an obfuscated version of the proper application database.

infra.lbs is our infrastructure library, common to all of our applications.  It has schema and table and UI classes to provide functionality for user access (logins), permissions and roles to assign to users, "reference" tables (a trio of them which you can ignore for now - if you want an explanation just ask) amongst other things.  Of note it includes infra.tkSuper and infra.rtSuper which contain most of the common code to all of our applications.  tkSuper (for fat UI) and rtSuper (for remote tasks) are pretty much identical.  There is also infra.tMaster, our super class containing all our generic code for table classes.

infra must be open in Omnis before CONFERENCE is opened.  You must open CONFERENCE.Startup_Task for the remote forms to work.  If you have any difficulty connecting to the database stop execution and use the menu "Connection configuration" and select "db connections" to inspect the sqllite database.  Make sure the connection parameters are correct (port is the biggest culprit).

CONFERENCE.libini is a sqllite database / file where the database connection and other variables are stored.  Very briefly the Startup_Task in CONFERENCE.lbs will read CONFERENCE.libini to get the Host, database name, etc to automatically login to stb (to get stringtable entries) and conf_obf.

There are two faces to the application: the administration of the conference and the public face.  The public face is the primary concern of the project.  To identify the public face classes look for classes beginning rjs... and ending in ...ForSite.  There is no login required when using these classes.



** NOT RECOMMENDED  **
To begin using the remote form application locate CONFERENCE.rjsBusinessApp and test that form.  This is to access the administration part of the application.  As the mission is to improve the public face of the application (not the administration of the application), you may not need to use this at all.  A User record has been setup for user devuser with password euromnis.  If you do not have marquee installed (from Arts Management) there will be an error message after login that you can ignore.
