#Linux Commands on Windows 7 with Cygwin
##Introduction

If you ever want or need to work with Linux/Unix commands on your Windows machine you will need to install a Linux emulator. A Linux emulator brings the power of the Linux command-line and scripting to your Windows operating system.  

At Gnip we work and develop predominantly on the Mac OS, which is built on top of a Unix-like Open Source operating system called NetBSD. We can simply open the Terminal application and have a full-blown Linux-like experience. Because of this we offer simple command-line examples to quickly demonstrate how to exercise the many APIs that make up the foundation of the many data service products we provide. Most of these example commands are based on cURL. 

If you are reading this article it is highly likely that you were referred to it because you are a Windows user and have Historical PowerTrack data to download. Here is the example command provided to download Historical PowerTrack data files:

curl -sS -u<consoleuser>:<password> https://historical.gnip.com/accounts/<account_name>/publishers/twitter/historical/track/jobs/<job_uuid>/results.csv | xargs -P 8 -t -n2 curl -o
For customers on Linux or Mac OS, this command can readily be used to automate the file downloading. For Windows-users, this command will fail, even if you have cURL installed, unless you have a Linux emulator installed since it includes the Linux xargs command. If you try this command on Windows, the odds are very high you will receive the following error:

C:\work\dmApp>curl -sS -u<consoleuser>:<password> https://historical.gnip.com/accounts/<account_name>/publishers/twitter/historical/track/jobs/<job_uuid>/results.csv | xargs -P 8 -t -n2 curl -o

'xargs' is not recognized as an internal or external command, operable program or batch file.

Cygwin is a popular Linux emulator and below we discuss how to install it and selected packages.  We then demonstrate using the above command to download Historical PowerTrack data files.

##Installing Cygwin with Selected Packages
###Download and run the appropriate installation executable 

To install Cygwin go to the http://www.cygwin.com/ website and look for the Install Cygwin link in the upper left corner of the front page. On the “Installing and Updating Cygwin Packages” page your see links to both 32-bit Windows (setup-x86.exe) and 64-bit Windows (setup-x86_64.exe).
If you are not sure whether you have a 32-bit or 64-bit operating system, go to the Start button, select the ‘Computer’ link on the right panel, then select ‘System properties’ from the menu bar.

Select the install folder  
The Cygwin installation package will default to installing in the C:\cygwin64 folder. This recipe was based on that default, although Cygwin should work regardless of where you install it.  

Select Packages
By default the Cygwin installation deploys a set of ‘Base’ packages, which includes Cygwin itself along with common Unix-like utilities such as grep, gawk, gzip, sed, which and bash. The full list can be viewed under the ‘Base’ treeview of packages. A critical package for running most of Gnip example commands is the ‘curl’ package.  The ‘curl’ package is not deployed by default so you must select it under the ‘Web’ set of packages.  

Cygwin can deploy a wide variety of packages and utilities. If there is a certain type of work you commonly do, such as develop Python or Ruby software, or work with a database engine such as MySQL, there is likely a Cygwin package to install in support of that. Here are some examples:

Database
MySQL, ODBC. postgres, sqlite3
Python
python: Python language interpreter 2.7.5-3
python3: Py3K language interpreter 3.2.5-3
Ruby
ruby: Interpreted object-oriented scripting language 1.9.3-p448-1
ruby-json: Ruby JSON module 1.8.0-1
ruby-rake: Ruby build system 10.0.4-1
Web
Apache web services
Shell
bash-completion: Bash completion enhancements
Science 
R: R Statistical computing language 3.0.1-1
Math 
Tons of packages

Download and Install Packages
After making your selections, the installer will begin downloading the libraries and utilities, and any required dependencies. This stage of downloading and installing resources can take a while, potentially more than 30 minutes depending on what you selected.

After the download and installation, my install threw the following error: 

Postinstall script errors:
Package: bash
		bash.sh exit code 1

The error message stated that this error may not indicate an actual problem. I have not noticed any operational issues, so if you see such an error it may not mean much.

Downloading Historical PowerTrack Files

OK, now that we have Cygwin with the cURL package installed, let try again to download Historical PowerTrack file.

$ curl -sS -u<consoleuser>:<password> https://historical.gnip.com/accounts/<account_name>/publishers/twitter/historical/track/jobs/9n7q24hjjg/results.csv | xargs -P 8 -t -n2 curl -o

curl -o 20140225-20140225_9n7q24hjjg_2014_02_25_00_00_activities.json.gz https://s3-us-west-1.amazonaws.com/archive.replay.snapshots/snapshots/twitter/track/activity_streams/jim/2014/02/27/20140225-20140225_9n7q24hjjg/2014/02/25/00/00_activities.json.gz?AWSAccessKeyId=AKIAJ73RGAUYVJZCDPHA&Expires=1396130624&Signature=WTs8JsXZwuBtLn3eIp80LrNusvA%3D
curl -o 20140225-20140225_9n7q24hjjg_2014_02_25_00_10_activities.json.gz https://s3-us-west-1.amazonaws.com/archive.replay.snapshots/snapshots/twitter/track/activity_streams/jim/2014/02/27/20140225-20140225_9n7q24hjjg/2014/02/25/00/10_activities.json.gz?AWSAccessKeyId=AKIAJ73RGAUYVJZCDPHA&Expires=1396130624&Signature=qBLqCMAzSZ5B5ecWcM08NZpE0%2Bw%3D
curl -o 20140225-20140225_9n7q24hjjg_2014_02_25_00_20_activities.json.gz https://s3-us-west-1.amazonaws.com/archive.replay.snapshots/snapshots/twitter/track/activity_streams/jim/2014/02/27/20140225-20140225_9n7q24hjjg/2014/02/25/00/20_activities.json.gz?AWSAccessKeyId=AKIAJ73RGAUYVJZCDPHA&Expires=1396130624&Signature=GJ9%2BcP%2FZWjkb0JklK%2B6%2FdYdU7VI%3D
curl -o 20140225-20140225_9n7q24hjjg_2014_02_25_00_30_activities.json.gz https://s3-us-west-1.amazonaws.com/archive.replay.snapshots/snapshots/twitter/track/activity_streams/jim/2014/02/27/20140225-20140225_9n7q24hjjg/2014/02/25/00/30_activities.json.gz?AWSAccessKeyId=AKIAJ73RGAUYVJZCDPHA&Expires=1396130624&Signature=InMDEzkzwdzz7BWSjHeHmfwZkZI%3D
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0

Etc., Etc, Etc.

Success!
