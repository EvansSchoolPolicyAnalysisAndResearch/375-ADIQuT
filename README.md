#   [Evans School Policy Analysis and Research Group][epar]
##  Data Distribution Project

This repository was created to distribute the source code behind 
EPAR's [AgQuery][agq]. It was created using the  Flask Web Framework 
and PostgreSQL 10. It was designed to be easily adaptable to similar projects
created by research teams around the world. Currently it is undergoing rapid
development as we strive to make it more user friendly and powerful. 

##  Getting Started: Setting Up A Dev Environment

If you are new to this project you can follow along with instructions below to
get set up with a working version of the website on your personal computer. If 
you are trying to set up a server using this code please make sure to follow
best industry practices. Setting up a web-accessible server in this manner is insecure. If you wish to deploy this software on your server please consult a 
professional.

These instructions assume you have some familiarity with the command line. 
While some of these steps may be completed using a graphic interface, many of 
them cannot. 
```
    Any instructions like this should be typed/pasted into your local terminal
```

### Steps:

1. Download a local copy of the code (clone the repository)
   
   Open up your terminal of choice and use the git command line tool to clone
   the repository. While you can use the GitHub desktop tool for this step, 
   later steps will require you to use the command line. 

   ```sh 
   git clone git@github.com:EvansSchoolPolicyAnalysisAndResearch/375-AgQuery.git
   ```

2. Install/Setup Postgresql

   You will need to setup and install PostgreSQL. However, this is beyond 
   the scope of  this tutorial. Please see the following pages for operating
   system specific instructions. 

   1. Debian (including Ubuntu, Mint, etc.) - [Ubuntu Wiki][ubuntu]

   2. Fedora - [Fedora Wiki][fedora]

   3. Mac OSX - [Postgress.app][osx]

   4. Windows - Still looking for a good tutorial (Web development on windows
      is notoriously difficult)

3. Setup the epardata Database within PostgreSQL.

   Once you have setup the PostgreSQL connect to it using the command line tool
   `psql`. How you do this will depend on your operating system but it should
   have been explained in the provided tutorial. 

   Once you have connected to the database run the following commands within
   the sql console to prepare the database for later. 

   > NOTE: lines starting with -- should not be typed in. They are comments and
   > are for guidance purposes only. 

   ```sql
   -- Create the epardata user
   CREATE USER epardata PASSWORD '<password>';
   -- Create the 'epardata' database with the 'epardata' user as the owner.
   CREATE DATABASE epardata OWNER epardata;
   -- You're done, disconnect from the database
   \quit
   ```
   Once you set the password you will want to update the .env file in the
   repository. Replace `<password>` with the password you created when setting
   up your epardata user. Once  you have changed the .env file be sure not to 
   commit it to your git repository.



4. Setup Python Virtual Environment.
  
   > __NOTE:__ Please make sure you have python 3.7 installed before you 
   > continue.


   Once you have the database set up you will need to set up the python virtual
   environment and install the python modules required to run the 
   python app server. 
 
   > __NOTE:__ lines starting with # should not be typed in. They are comments 
   > and are for guidance purposes only. 

   ```sh
   # Replace <your_repository_directory> with the location where you cloned this
   # repository
   cd <directory_with_the_repository>
   # Next we create the python virtual environment
   python3 -m venv env
   # Next start up the virtual environment you just created
   source env/bin/activate
   # Finally use pip to install the python dependencies
   pip install -r requirements.txt
   ```

5. Populate the Database

   Once the Python requirements have been installed updating the database is
   easy. 

   > __NOTE:__ lines starting with # should not be typed in. They are comments 
   > and are for guidance purposes only. 
   ```sh
   # Set the data directory as your working directory
   cd data
   # Run the db_updater
   ./db_updater.py
   ```

   Once the data is downloaded and prepared for upload to your database you will
   be asked for your SQL password. You will need the password you created in 
   step 3. Type it in, press enter and the rest should be automatic.

   > __NOTE:__ The first time you run this script it will state that there was
   > an error on dropping tables. This can safely be ignored.

6. Start the Server
   
   At this point you are pretty much ready to go. There are a couple more 
   commands you will need to run in order to start the server. After which 
   you should be able to view the site in your web browser.

   > __NOTE:__ lines starting with # should not be typed in. They are comments 
   > and are for guidance purposes only. 

   ```sh
   # Load the environment variables into the local system
   source .env
   # Start the flask app server
   flask run
   ```

   Now you can type [http://localhost:5000][local] into your browser's URL bar
   to view your 

7. Cleaning up 

   When you are done testing your the website you can quit flask by pressing 
   `Ctrl+c` while your terminal is selected. This will end the currently running
   process. You may want to stop PostgreSQL from running as well. The tutorials
   linked earlier should provide you with direction on how to do that as well.

   If you intend to continue using your open terminal for other purposes you
   will want to deactivate the python virtual environment. To do so simply type
   `deactivate` into your shell and press enter.

8. Re-starting the Server
   
   When you are ready to come back to testing your server after some period of time you will need to run a small subset of the above commands to view your
   website again. The first step is to start the PostgreSQL. The method for 
   doing so will depend on your operating system and should be in the tutorials
   linked above. Once that is done 4 simple commands will get your web server
   running again.

   ```sh
   # Change to the directory where you downloaded the repository
   cd <directory_with_the_repository>
   # Start the python virtual environment
   source env/bin/activate
   # Source your environment variables
   source .env
   # start the server
   flask run
   ```
   Now you can view your [local copy][local] of the website once again.


[epar]:     https://evans.uw.edu/policy-impact/epar
[agq]:      https://www.agquery.org
[ubuntu]:   https://help.ubuntu.com/lts/serverguide/postgresql.html
[fedora]:   https://fedoraproject.org/wiki/PostgreSQL
[osx]:      https://postgresapp.com/
[win]:      https://www.postgresql.org/download/windows/
[data]:     https://evans.uw.edu/policy-impact/epar/agricultural-development-data-curation
[sheet]:    https://github.com/EvansSchoolPolicyAnalysisAndResearch/335_Data-Dissemination/raw/master/EPAR_UW_335_AgDev_Indicator_Estimates.xlsx 
[local]:    http://localhost:5000
