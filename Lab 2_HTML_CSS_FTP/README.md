# Lab 02 – Basic HTML, CSS, and FTP Server Setup

This lab introduces fundamental web development (HTML and CSS) and server interaction skills in a cybersecurity context. It also includes setting up an FTP server on Ubuntu for uploading and serving web content.

---

## 🎯 Objectives

- Create and test HTML5 pages using Notepad++
- Style pages with embedded CSS
- Serve content using FTP to an Ubuntu web server
- Configure vsFTPd for secure file transfer
- Connect from Windows using FileZilla FTP client

---

## 🖥️ Environment Overview

| System           | Role                   | IP Address     | Notes                        |
|------------------|------------------------|----------------|------------------------------|
| Windows 10       | Client + Notepad++     | 10.0.0.30      | Used for HTML authoring      |
| Ubuntu Server    | Web & FTP Server       | 10.0.0.200     | Hosts vsFTPd and HTML files  |

---

## 🧪 Part 01: Create HTML Pages

### ✅ Basic HTML5 Page

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>My Basic HTML5 Page</title>
</head>
<body>
  <h2>Hello World!</h2>
  <p>My FOL username is: <b>oabolurin</b></p>
</body>
</html>

File saved as mypage.html
Opened in browser via double-click
📸 Screenshot for Slide 01: Notepad++, browser output, URL bar, and hostname via CMD

### ✅ Modified HTML Page
Changed <h2> to <h1>
Made username bold
Added second <p> and <hr />
Added <div>This is my first division text</div>

📸 Screenshot for Slide 02: mynewpage.html in Notepad++, browser view, and URL


### ✅ CSS Styling
<style type="text/css">
  h1 { font: 40px Arial; color:#0000FF; }
  div { font: 30px Helvertica; color:red; }
</style>

📸 Screenshot for Slide 03: mycsspage.html with corrected CSS visible in Notepad++ and rendered in browser


🛠️ Part 02: Set Up vsFTPd on Ubuntu
✅ Installation & Configuration
sudo apt install vsftpd
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.BACK
sudo nano /etc/vsftpd.conf


Changes made:
anonymous_enable=NO
local_enable=YES
write_enable=YES
allow_writeable_chroot=YES
chroot_local_user=YES
file_open_mode=0777
local_umask=022
userlist_enable=YES
userlist_file=/etc/vsftpd.userlist
userlist_deny=NO

✅ Create FTP User and Directories
sudo adduser oabolurin-ftp
sudo mkdir /var/www/html/oabolurin-ftp
sudo chmod a-w /var/www/html/oabolurin-ftp
sudo mkdir /var/www/html/oabolurin-ftp/files
sudo chown oabolurin-ftp:oabolurin-ftp /var/www/html/oabolurin-ftp/files

📄 Created test file:
echo "testing FTP access" | sudo tee /var/www/html/oabolurin-ftp/files/ftp_test.txt

📸 Screenshot for Slide 04: FileZilla connected as anonymous, CMD showing net config workstation


🔐 Part 03: Test FTP Access (Named User)
✅ Add User to Allowed List
echo "oabolurin-ftp" | sudo tee -a /etc/vsftpd.userlist

📸 Screenshot for Slide 05: FileZilla connected as oabolurin-ftp, CMD hostname


✅ Configure FTP Root for User
In /etc/vsftpd.conf, add:
user_sub_token=$USER
local_root=/var/www/html/$USER/files

Restart the service:
sudo systemctl restart vsftpd

🧠 Reflections
This lab introduces how basic web files are created, styled, and served. This is a foundational knowledge for understanding how attackers may upload or manipulate files on misconfigured servers. Configuring vsFTPd with controlled user access reinforces the importance of secure file transfer.
