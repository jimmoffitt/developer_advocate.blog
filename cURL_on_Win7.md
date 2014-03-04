#cURL on Windows

cURL is a handy command-line utility for making HTTP requests.  cURL is so useful you will notice that we provide sample cURL commands on the “API Help” tab of the console.gnip.com dashboard, as well as many examples in our support.gnip.com documentation. Gnip is an API company and cURL is a great tool for exercising our many API-based products.  

Here are some of the things you can do with cURL and Gnip’s data services:
Using a GET command, retrieve your current ruleset from your real-time PowerTrack stream or Data Collector feed (and write it to a file while you are it).
Using a POST command, add rules to your system. In fact you could use cURL to port rules from one stream to another by writing one stream’s rules to a file, then POSTing that file to another.
Discover the amount of data associated with a rule from the past 30-days using the Search API and its count method.
Collect data from the Search API by submitting a rule with either a GET or POST command.
Stream real-time data from your PowerTrack stream.  

Using cURL to stream data can be a very useful troubleshooting tool. Using cURL allows us to assess ‘raw’ streaming performance, essentially seeing the amount of data volumes and latency a network/hardware environment can support with client-side software removed from the picture. For example, if a client system is having difficulty keeping up with a data stream, using cURL often provides insights on where the bottlenecks reside. 

##Setting up Curl on Windows 7

cURL is usually built-in to MacOS and Linux environments, but there is no native support for cURL on Windows.  Since Windows users could often benefit from using cURL, below we provide a ‘recipe’ for getting cURL set up on a Windows 7 machine.

For this recipe the target machine was running 64-bit Window 7 Professional, Service Pack 1.  Several of the components used in this recipe have 32-bit/64-bit variants, so the appropriate ones need to be used for your architecture.

###Steps to install cURL on Windows 7:  

###1) Install the curl.exe.
cURL executables can be downloaded from http://curl.haxx.se/download.html.
For 64-bit Windows download the “Win64 - Generic” version that supports SSL (latest version is curl-7.33.0-win64-ssl-sspi.zip).  This zip file deploys a single curl.exe. For 32-bit Windows, download an appropriate executable.
Place curl.exe in any folder included in your PATH environmental variable list.  For generating this recipe, this was placed in the c:\Windows\System32 folder.

###2) Test basic cURL functionality.
Run curl from a Command prompt, C:\Windows\System32>curl.  
If you get a message instructing you to use the --help or --manual options then curl is working and you are ready for the next step.
If you get an error message about MSVCR100.dll, install the MS Visual C++ 2010* Redistributable Package (x64), available at http://www.microsoft.com/en-us/download/details.aspx?id=14632 and retry.
 
*Note that many on-line recipes for getting curl on Windows make reference to the Microsoft Visual C++ 2008 Redistributable Package.  While that should still work, the updated 2010 version was used here.

###3) Enable support for SSL.
Download the latest bundle of Certficate Authority Public Keys from http://curl.haxx.se/ca/cacert.pem.
Rename this file from cacert.pem to curl-ca-bundle.crt.
Place curl-ca-bundle.crt in any folder included in your PATH environmental variable list.  Again, for generating this recipe, this was placed in the c:\Windows\System32 folder.
Test that cURL is working with SSL.  
You can attempt to cURL any HTTPS website, such as https://google.com or your PowerTrack or Data Collector endpoints.  
If you do not receive any errors, you should be all set, and you can go to the final step.
If SSL is still not correctly enabled you will see error messages such as these:
curl: (60) SSL certificate problem: unable to get local issuer certificate
curl: (60) SSL certificate problem: self signed certificate in certificate chain
In these cases, try the following:
Install a Windows version of OpenSSL.
Go to http://slproweb.com/products/Win32OpenSSL.html and download the Win64 OpenSSL v1.0.0L Light package (or the 32-bit version if you are on a 32-bit OS) and install it.
Note that if you can not get cURL SSL enabled, you have the option to run cURL in an insecure mode with the -k (or --insecure) option:

###4) Verify with Rules API GET command. 

At this point you should be able to use any of the sample cURL commands to exercise Gnip various APIs.  For example, requesting the list of rules from your Rules API endpoint:

```
C:\Users\jmoffitt>curl -v -umy_username@mycompany.com "https://api.gnip.com/accou
nts/my_account/publishers/twitter/streams/track/prod/rules.json"
Enter host password for user 'my_username@mycompany.com':
* Adding handle: conn: 0x543ee0
* Adding handle: send: 0
* Adding handle: recv: 0
* Curl_addHandleToPipeline: length: 1
* - Conn 0 (0x543ee0) send_pipe: 1, recv_pipe: 0
* About to connect() to api.gnip.com port 443 (#0)
*   Trying 216.46.190.147...
* Connected to api.gnip.com (216.46.190.147) port 443 (#0)
* successfully set certificate verify locations:
*   CAfile: C:\Windows\system32\curl-ca-bundle.crt
  CApath: none
* SSLv3, TLS handshake, Client hello (1):
* SSLv3, TLS handshake, Server hello (2):
* SSLv3, TLS handshake, CERT (11):
* SSLv3, TLS handshake, Server finished (14):
* SSLv3, TLS handshake, Client key exchange (16):
* SSLv3, TLS change cipher, Client hello (1):
* SSLv3, TLS handshake, Finished (20):
* SSLv3, TLS change cipher, Client hello (1):
* SSLv3, TLS handshake, Finished (20):
* SSL connection using RC4-SHA
* Server certificate:
*    	subject: O=*.gnip.com; OU=Domain Control Validated; CN=*.gnip.com
*    	start date: 2011-12-19 23:35:35 GMT
*    	expire date: 2015-01-22 17:12:21 GMT
*    	subjectAltName: api.gnip.com matched
*    	issuer: C=US; ST=Arizona; L=Scottsdale; O=GoDaddy.com, Inc.; OU=http://
certificates.godaddy.com/repository; CN=Go Daddy Secure Certification Authority;
 serialNumber=07969287
*    	SSL certificate verify ok.
* Server auth using Basic with user 'my_username@mycompany.com’
> GET /accounts/my_account/publishers/twitter/streams/track/prod/rules.json HTTP/1.1
> Authorization: Basic am1vZmZpdHRAZ25pcGNlbnGJmUIOuY29tOmE2YXZhbnQ=
> User-Agent: curl/7.33.0
> Host: api.gnip.com
> Accept: */*
>
< HTTP/1.1 200 OK
< Content-Type: application/json
< Content-Length: 196
<
{"rules":[{"value":"point_radius(-74.00676 40.729047 10mi)","tag":"new_york"},{"value":
"point_radius:[-93.2667 44.9833 10mi]","tag":"mpls"}]}* 
```

Helpful links
cURL website
http://curl.haxx.se/
http://curl.haxx.se/download.html
cURL on Windows
http://stackoverflow.com/questions/9507353/steps-to-setup-curl-in-windows
http://guides.instructure.com/s/2204/m/4214/l/83393-how-do-i-install-and-use-curl-on-a-windows-machine
http://superuser.com/questions/134685/run-curl-commands-from-windows-console
http://www.confusedbycode.com/curl/
http://stackoverflow.com/questions/949959/why-do-64-bit-dlls-go-to-system32-and-32-bit-dlls-to-syswow64-on-64-bit-windows
Unix Commands on Windows 7
See this article


