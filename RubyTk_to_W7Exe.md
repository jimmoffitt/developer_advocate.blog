###Deploying Ruby/Tk Applications as Windows 7 Executables

####Summary
This post discusses deploying Ruby Tk applications developed on Mac OS as Windows 7 executables with the [OCRA] (https://github.com/larsch/ocra) gem.  Discussion focuses on the prototyping of an application that automates downloading data files produced by Gnip’s Historical PowerTrack.
 
####Introduction
The user-story behind this cross-platform adventure came from working with one-time consumers of historical social media data. A common scenario is a researcher (from a non-computer field) who needs to import hundreds of thousands (if not millions) of tweets into some established data-store. Many of these data warehouses can readily import statically structured data so we often get asked whether we can package the data into comma-separated values (CSV). Often they work on the Windows platform and are not readily prepared to download thousands of files. On top of that is the fact that managing this many files on Windows can be painful. So the goal was to develop an application to automate these processes: file downloading, file consolidation, and data conversion. 

The first step was developing an application to download files produced by Gnip’s Historical PowerTrack. I started to prototype with Ruby on Mac OS since I am on that OS most of the time, and I was in the process of learning Ruby. Having experience with Python I had worked with a cross-platform user-interface (UI) package ([wxPython](http://wxpython.org/)) and had dabbled in creating Windows applications from python projects (with [py2exe](http://wxpython.org/)). So the initial steps were deciding on what Ruby UI package to build with, and seeing what support was available for building Windows executables.

I checked out several Ruby UI packages and at first gravitated toward [green-shoes](http://wxpython.org/), but ended up going with [RubyTk](http://wxpython.org/). This package is the Ruby implementation of the ubiquitous [Tk UI](http://wxpython.org/) toolkit. This UI package is ‘baked’ into most (all?) Ruby distributions and seemed readily portable to Windows. Initial Ui prototyping bore this out, while I hit snags with other packages attempting to install on Windows. Next I came across the [OCRA](http://wxpython.org/) (One-Click Ruby Application) gem.  With these tools a ‘hello world’ application was quickly written and deployed on Windows 7 and we were on our way.
An Example Ruby Tk Application
For this proof-of-concept application, the focus was on automating the downloading of Historical PowerTrack data files with these basic features:
Account and configuration management.
‘Look before leap’ logic for picking up where an incomplete download cycle left off.
Decompressing gzipped files.

Based on this set of features, the user-interface was designed using Ruby Tk. Figure 1 shows how Ruby Tk renders it on Mac OS. After developing this application on Mac OS, the next step was using OCRA to create a Windows executable. 

#####Figure 1 - Ruby Tk user-interface on Mac OS.


###Deploying Ruby Code as Windows Executable
With the help of the [OCRA gem](http://wxpython.org/) building a Windows executable was relatively straightforward. OCRA produces an executable that is self-extracting and self-running. Under the hood, the executable contains your Ruby source code, all required Ruby libraries and gems (and even Windows DLLs), as well as the Ruby interpreter to run it all. So with this tool you do not even need a Ruby environment to be pre-installed to run your Ruby application. The resulting dmApp.exe was 7.9 MB in size, which seemed reasonable. OCRA even provides [Inno Setup](http://wxpython.org/) support in case you need to create a more elaborate installation package. See http://ocra.rubyforge.org/ for more information.  

OCRA has many options for specifying what gems and code need to be packaged in the executable. The gem README outlines your options and suggests that if your application is complicated enough that a best-practice is to run your application during the OCRA build process. Apparently using ‘require_relative’ includes and/or the Tk user-interface toolkit makes the dmApp complicated enough to require this. Building without a download cycle resulted in an executable that would throw errors when downloading. Therefore, it is recommended to fully ‘exercise’ your application during the OCRA build process. Furthermore, I found that if your application is multithreaded it is critical that when you close your application that it gracefully and cleanly exits those threads. Otherwise, the underlying build process will likely abort. (See below for more details.) 

Even though the dmApp code does not use the Ruby ‘autoload’ mechanism (just-in-time gem loading), apparently something in the Tk toolkit requires use of the ‘--no-autoload’ parameter. Building without this option resulted in an executable that threw errors at start-up. Also, the ‘--windows’ parameter was used to invoke the rubyw.exe interpreter. Accordingly, the OCRA build command used for the dmApp.exe prototype was: 
     ocra --windows --no-autoload dmApp.rb

Below is the output from the build process. When the build process starts the user-interface is rendered. While the download cycle is performed there are some application status messages written. After that the actual executable is built, ending with the ‘Finished’ message. If you don’t see this then the application did not exit gracefully and the build process aborted. Figure 2 presents how the interface looks on Windows 7.

```
C:\work\dmApp>\Ruby193\bin\ocra --windows --no-autoload  dmApp.rb
=== Loading script to check dependencies
"Starting Download Manager application..."
"Historical PowerTrack job hk8m6mwxw2 has 36 data files "
"Starting downloads..."
"Took 50.224 seconds to download files.  "
=== Detected gem tk-win-0.2.2-x86-mingw32 (loaded, files)
===     47 files, 9912475 bytes
=== Including 52 encoding support files (2831360 bytes, use --no-enc to exclude)
=== Building dmApp.exe
=== Adding user-supplied source files
=== Adding ruby executable rubyw.exe
=== Adding library files
=== Compressing 18987735 bytes
=== Finished building dmApp.exe (7879008 bytes)
```

#####Figure 2 - Ruby Tk user-interface on Windows 7.


###Bumps on the Cross-Platform Road
Perhaps a naive goal was to have the exact code run on each operating system (OS). In the end there were a few areas where this ideal was not met. The good news is the UI code based on Ruby Tk was completely portable between Mac OS and Windows 7. As seen above some of the UI details look slightly different, but that is easy to live with given that the code is the exactly the same (well, actually, the size of the ‘Download Data’ button depends on OS-specific code, but that’s it).  

The bad news is that some fundamental details were not as straightforward.  
Secure Socket Layer (SSL) Support
After initial prototyping of the download process on MacOS, I immediately hit a problem with https downloading on Windows. I quickly learned that there is a fundamental issue with the standard Ruby Windows install and it knowing where to look for SSL certificate files (here and here are some example discussions of the issue).

```
OpenSSL::SSL::SSLError: SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed
```

Fortunately there are many discussion threads and workaround recipes around this issue, including gems (such as this one) dedicated to solving the problem. Since in an attempt to minimize cross-platform deployment headaches a general goal was to reduce the number of dependencies for this prototype. So a decision was made to do the work in native Ruby by first looking in a local directory for a certificate file, then to pull one down from a trusted source and create the file if needed. See here for a compilation of code written to implement this strategy. 
Event Programming and Background Tasks  
A general proof-of-concept goal was to develop a standalone application with a simple user-interface to enable users to manage long-running data processes. The user-interface needed to interact with these background processes, raising events to start them and monitoring their progress. And since it needed to run on both Windows and Mac OS/Linux, I set off to make it as simple (primitive is an appropriate description) and ‘native’ as possible. By native I mean I wanted to rely on Ruby native threads and minimize the use of ‘third-party’ gems that potentially have Windows installation issues. (This goal was driven by having attended a Rails course and witnessing the Windows students struggle to get their environments set-up.) A review of the project’s Gemfile reveals that the required gems are pretty standard for an application based on HTTP, parses JSON, manages GZipped files, and provides a Tk user-interface.  Getting both the Mac OS and Windows environments built up was easy.

Development started with a simple design of building the Tk user-interface, binding widgets to the download ‘worker’ class, and invoking the Tk main event loop. That results in essentially a single-threaded application, complete with unwanted event blocking during the downloading process. Instead a responsive user-interface was implemented by hosting both the Tk mainloop and background process in separate threads.  

```
threads << Thread.new {oDM.get_data}
t_ui = TkRoot.new.mainloop()
threads << Thread.new { t_ui }
```

See HERE for more details on how this simple design was implemented.
Cross-Platform Support Means Cross-Platform Development and Testing
If you target multiple operating systems, the level of effort to develop and test necessarily increases. Given the reality of differences in OS behavior, such as the issues outlined above, the effort to develop and test on Windows more than doubled the effort. Since the hope of ‘write once, run everywhere’ was not realized, that meant setting up Windows virtual machines and development environments (I used the Eclipse-based Aptana on Windows). 

###Source Code
All the Ruby source code for this prototype is available at https://github.com/jimmoffitt/dmApp.


