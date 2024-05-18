# How I Retrieved the flags

## Flag 1:pclub{path_trav3rsa1_1s_fun}

The web app didn't have any forms/input fields so I was hinted to look through the websites html content, especially on the gallery page since it fetched images. There I saw that the images were fetched using
`http://74.225.254.195:2324/getFile?file=/home/kaptaan/PClub-DVWA/static/images/gallery/6.jpeg`
so naturally, I tried `http://74.225.254.195:2324/getFile?file=/home/kaptaan/PClub-DVWA/routes.txt`
and discovered the first flag.


http://74.225.254.195:2324/getFile?file=/home/kaptaan/PClub-DVWA/static/images/gallery/6.jpeg

## Flag 2: 60:45:bd:af:3f:e5
MAC Address of eth0 Interface- 60:45:bd:af:3f:e5

I retreived it using the ip details route using command injection

## All the Flags?

At this point, I was trying to look around and I managed to view the README.md file, the contents are attached at the end of this file. So basically, I am done here, but rather than using the README,
I attempted to solve these on my own.


## Flag 3: pclub{01d_1s_g01d_sql1}

Got this using SQL Injection after seeing getBlogDetail on the routes.txt and then I realised getBlogDetail was used by the blogs route.
SQL map got the tables and stuff and then I retrieved password of `kaptaan` using reverse MD5 tools for common passwords and login using secretary_login. Then I found flag.txt

I used crackstation.com for getting the reverse MD5s.

## Flag 4: pclub{data_1s_3v3ryth1ng}

Under `amansg22` in secretary_login, the file is `ARIITK.jpeg`. Then I use stegano tools and get `qr_code.png` and scan it to get a base64 encoded string `Y3B5aG97cW5nbl8xZl8zaTNlbGd1MWF0fQ==`

Decoding this gives `cpyho{qngn_1f_3i3elgu1at}`
Clearly, ROT13 gives `pclub{data_1s_3v3ryth1ng}`

## Flag 5: pclub{d0_y0u_kn0w_7h47_h0w_much_1_5truggl3d_w1th_0p3n55l}

Using `strings whathash` and `radare2`, I found the SHA1 key. I used a decoder to decode it to get `garimailoveyou`. Using `radare2` I figured out that the username is supposed to be anything but `Haardikk`


::::::::::::::
README.md
::::::::::::::
# PClub DVWA
Damn Vulnerable Web Application (DVWA) for Programming Club, IITK
## Requirements
Languages Used: Python3, JavaScript, CSS, HTML

Modules/Packages used (for python3):
* os
* json
* pathlib
* hashlib
* mysql.connector
* flask
* colorama
* sys
* requests

Install the dependencies:
```bash
pip install -r requirements.txt
```
## Setup
In your System's MySQL Server, do the following:
* Make a new user on *localhost*
* Grant user all the permissions for database **pclub_secy_task** on *localhost*

This can be achieved by running the following commands on MySQL:
```mysql
CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL ON pclub_secy_task.* TO 'user'@'localhost';
```
Then create a file in the Directory of PClub-DVWA called ***db_user.json*** and it should contain the following json data:
```json
{"user":user,"password":password}
```
Then run the ***create_data.py*** to Create the required Databases, Tables, Columns and Values in your System's MySQL Server.

## Flags
* /route.txt
* /static/files/amansg22/ariitk.jpeg
* /static/files/kaptaan/flag.txt
* MAC Address of eth0 Interface
## Vulnerabilities
* Path Traversal in : /getFile
* SQL Injection in : /getBlogDetail
* Command Injection in : /ipDetails
## SQL Database
The Database is *pclub_secy_task*. It has 3 tables:
* USERS
* BLOGS
* HINTS

In USERS Table, there are 3 columns:
* user: CC ID of the Programming Club Secies from Tenure 2022-23
* userhash: SHA3 512 Hash of Users (to avoid SQL Injection on Login Page)
* password: MD5 Hash of Passwords randomly choosen from *rockyou.txt*

In BLOGS Table, there are 4 columns:
* id
* title
* content
* link

In HINTS Table, there is only 1 column:
* hint
## Procedure
* After opening Gallery, we see that sources of the Images is something like */getFile?file=/home/kaptaan/IIT_Kanpur/Clubs/PClub/Secretary-Recruitment/2023-24/PClub-DVWA/static/images/gallery/{image_index}.jpeg*. This lets us know that it is vulnerable to Path Traversal Vulnerability. So we send a **GET** request */getFile?file=/home/kaptaan/IIT_Kanpur/Clubs/PClub/Secretary-Recruitment/2023-24/PClub-DVWA/routes.txt* and we get the First flag and a hing.
* After opening Blogs, we see that the Title, Content and Link of the Blogs are fetched through */getBlogDetails* endpoint. After running ***sqlmap*** on it, we found that **blog** parameter is vulnerable to ***SQL INJECTION*** vulnerability. After that we dump the Database, we found from the HINTS table that the passwords are weak and uses MD5 Hashing Algorithm, after running rockyou, we find every password.
* On logging in with *kaptaan* user, we find that there is a flag.
* On logging in with *amansg22* user, we find that there is logo of ***Aerial Robotics***. After using ***steghide*** on it, we found a QR Code, after scanning the QR Code we get a **base64 encoded string** and decoding it and doing **ROT13** on it, we get the flag.
* On logging in with *ritvikg22* user, we find the link to netcat server and pwn challenge.
* After getting *routes.txt*, we see an endpoint **/ipDetails**. When we open it, we see a Input Tag asking for an IP Address. After entering a valid IP and hitting search, we get the details of the IP Address. When we type a wrong IP we don't see that data. This is vulnerable to command injection and after seeing the Hint we got from routes.txt, we run the following *; ifc

