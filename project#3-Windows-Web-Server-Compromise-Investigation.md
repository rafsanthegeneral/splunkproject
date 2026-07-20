# 🛡️ Windows web server compromize detection project using Splunk
This project focuses on how to detect whether your web server has been compromised, and—once a compromise occurs—how an attacker could leverage that foothold to take over an entire Windows system. The goal is to help understand the behaviors and patterns that attackers typically exhibit when targeting a web server, and how those initial intrusions can escalate into full system takeover. By analyzing these attack vectors, defensive strategies can be improved to identify suspicious activity early and limit the potential damage.

## Lab Materials

- **Splunk Enterprise Machine** – A machine running Splunk Enterprise.  
  [Installation Guide](https://github.com/rafsanthegeneral/splunkproject/blob/master/project%231-Setup-Splunk-Enterprise%26SplunkUniversalForwader.md)

- **Windows Machine + Splunk Forwarder** – A Windows host with the Splunk Universal Forwarder installed for log shipping.  
  [Forwarder Setup (same guide)](https://github.com/rafsanthegeneral/splunkproject/blob/master/project%231-Setup-Splunk-Enterprise%26SplunkUniversalForwader.md)

- **Web Server (Laragon)** – A lightweight local web server. This lab uses Laragon (WAMP).  
  [Download Laragon 8.6.1](https://github.com/leokhoa/laragon/releases/download/8.6.1/laragon-wamp.exe)

For setup the webserver you just download and install the laragon on windows computer. You can follow [HERE](https://www.youtube.com/watch?v=Y_oRbC_cCBU).

#### Configure Splunk Universal Forwarder to Monitor Apache Logs

After installing the web server, you need to configure the Splunk Universal Forwarder to monitor the Apache log files.

Navigate to the following directory:

```text
C:\Program Files\SplunkUniversalForwarder\etc\system\local
```

Locate the `inputs.conf` file, open it with a text editor, and add the following configuration:

```ini
[monitor://C:\laragon\bin\apache\httpd-2.4.66-260223-Win64-VS18\logs\access_log]
index=serveraccesslog
disabled=false
source=apache:access

[monitor://C:\laragon\bin\apache\httpd-2.4.66-260223-Win64-VS18\logs\error_log]
index=servererrorlog
disabled=false
source=apache:error
```

![Configure Splunk Forwarder](/images/project-3/image1.png)

> **Note**
>
> - The default Laragon installation path is `C:\laragon`. If you installed Laragon on a different drive or directory, update the paths in the `inputs.conf` file accordingly.
> - The folder name `httpd-2.4.66-260223-Win64-VS18` is specific to the Apache version installed on my system. Your folder name may be different depending on the version of Apache included with your Laragon installation.
> - Before saving the configuration, navigate to:
>
>   ```text
>   C:\laragon\bin\apache\
>   ```
>
>   and verify the exact Apache folder name. Replace `httpd-2.4.66-260223-Win64-VS18` in the configuration with the folder name on your system if it differs.

![Apache Installation Directory](/images/project-3/image2.png)

## Lab Setup 

After set all the lab meterials, it's time to setup the lab . I am gonna host a vulnarable web app on that web server. I am planning make a SQLI vulanarable web application then using this vulnerability, i upload the shell on the server and  compromize the windows computer as an attaker. 

### Set Up a Vulnerable Web Application

After successfully installing Laragon, open the **Laragon Control Center** and navigate to the web root directory.

![Laragon Control Center](/images/project-3/image3.png)
Once the web root directory is open, locate the `index.php` file and open it using Notepad or any text editor.
Replace the existing contents of the file with the following PHP code, then save the file.

```
<?php
$host = "127.0.0.1";
$user = "root";      // change to root if needed
$pass = "";
$db   = "library";
$conn = new mysqli($host,$user,$pass,$db);
if($conn->connect_error){
    die("Connection Failed");
}

/*
----------------------------------------
ADD BOOK USING GET
Example:
index.php?add=1&title=Book&author=Someone&category=Novel
----------------------------------------
*/
$message="";
if(isset($_GET['add'])){

    $title = trim($_GET['title'] ?? "");
    $author = trim($_GET['author'] ?? "");
    $category = trim($_GET['category'] ?? "");
    if($title!="" && $author!=""){

        $stmt = $conn->prepare("INSERT INTO books(title,author,category) VALUES(?,?,?)");
        $stmt->bind_param("sss",$title,$author,$category);
        $stmt->execute();

        $message="Book Added Successfully!";
    }
}
$search = $_GET['search'] ?? "";
if ($search != "") {
    $sql = "SELECT * FROM books WHERE title LIKE '%$search%'";
    $result = $conn->query($sql);
} else {
    $result = $conn->query("SELECT * FROM books ORDER BY id DESC");
}

?>
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Library</title>
<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Arial;
}
body{
background:#eef3f9;
padding:30px;
}
.container{
max-width:1000px;
margin:auto;
}
.header{
background:linear-gradient(135deg,#4f46e5,#2563eb);
padding:35px;
border-radius:15px;
color:white;
text-align:center;
margin-bottom:25px;
box-shadow:0 15px 30px rgba(0,0,0,.15);
}
.search-box{
display:flex;
gap:10px;
margin-bottom:25px;
flex-wrap:wrap;
}
.search-box input{
flex:1;
padding:15px;
border:none;
border-radius:10px;
font-size:16px;

}
.search-box button{
padding:15px 25px;
border:none;
background:#2563eb;
color:white;
border-radius:10px;
cursor:pointer;
font-size:16px;
}
.search-box button:hover{
background:#1d4ed8;
}
.message{
background:#16a34a;
padding:15px;
color:white;
border-radius:10px;
margin-bottom:20px;
}
.grid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(280px,1fr));
gap:20px;
}
.card{
background:white;
padding:20px;
border-radius:15px;
box-shadow:0 10px 20px rgba(0,0,0,.08);
transition:.3s;
}
.card:hover{
transform:translateY(-5px);
}
.title{
font-size:22px;
font-weight:bold;
margin-bottom:10px;
color:#1e3a8a;
}
.author{
margin-bottom:8px;
}
.category{
display:inline-block;
background:#dbeafe;
padding:6px 12px;
border-radius:20px;
color:#1d4ed8;
font-size:14px;
}
.footer{
margin-top:10px;
font-size:12px;
color:#666;
}
.example{
margin-top:35px;
background:white;
padding:20px;
border-radius:12px;
line-height:1.8;
}
code{
background:#f2f2f2;
padding:5px 8px;
border-radius:6px;
}
@media(max-width:600px){
.search-box{
flex-direction:column;
}
.search-box button{
width:100%;
}
}
</style>
</head>
<body>
<div class="container">
<div class="header">
<h1>📚 Library Search</h1>
<p>Search books instantly</p>
</div>
<?php
if($message!=""){
echo "<div class='message'>$message</div>";
}
?>
<form class="search-box">
<input
type="text"
name="search"
placeholder="Search Book..."
value="<?php echo htmlspecialchars($search); ?>"
>
<button>Search</button>
</form>
<div class="grid">
<?php
while($row=$result->fetch_assoc()){
?>
<div class="card">
<div class="title">
<?php echo htmlspecialchars($row['title']); ?>
</div>
<div class="author">
<b>Author:</b>
<?php echo htmlspecialchars($row['author']); ?>
</div>
<div class="category">
<?php echo htmlspecialchars($row['category']); ?>
</div>
<div class="footer">
Added:
<?php echo $row['created_at']; ?>
</div>
</div>
<?php
}
?>
</div>
<div class="example">
<h3>Add Book Using GET</h3>
<br>
Example:
<br><br>
<code>
index.php?add=1&title=Atomic Habits&author=James Clear&category=Self Help
</code>
</div>
</div>
</body>
</html>
```
![alt text](/images/project-3/image4.png)

After saving the `index.php` file, you also need to configure the database.
Open the **Laragon Control Center** again and click the **Database** button.

![Create Database](/images/project-3/image5.png)

A database management window will open. Create a new database named `library`, then click **OK**.

![Database Created](/images/project-3/image6.png)![Open Query Tab](/images/project-3/image7.png)

Once the database has been created, select the newly created `library` database from the left panel.
Next, open the **Query** tab, paste the following SQL script into the query editor, and execute it.

```sql
CREATE TABLE books (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    author VARCHAR(255) NOT NULL,
    category VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO books(title,author,category)
VALUES
('Harry Potter','J.K. Rowling','Fantasy'),
('The Hobbit','J.R.R. Tolkien','Fantasy'),
('Clean Code','Robert C. Martin','Programming');

```
![alt text](/images/project-3/image8.png)

Then browser your local ip on your browser. You will find your server ip top of the laragon control center window. 

![alt text](/images/project-3/image9.png)

After browser the server ip (in my case that is `192.168.110.91`) you will see the vulnarbale web application is running. 

![alt text](/images/project-3/image10.png)

## 🚀 Adversary Attack Simulation 

After performing reconnaissance and multiple rounds of scanning, we assume that the attacker has successfully identified a **SQL Injection (SQLi)** vulnerability in the target web application in the `?search=` end point.

During testing, the attacker discovers that the application's **search functionality** is vulnerable. Entering an additional single quotation mark (`'`) into the search field causes the application to return a SQL error with more sensative information such as webroot path(c:/laragon/www/), indicating that user input is not properly sanitized before being used in a database query.

![alt text](/images/project-3/image11.png)

After identifying the SQL Injection vulnerability, the attacker uses **[sqlmap](https://github.com/sqlmapproject/sqlmap)** to automate the exploitation process.

`sqlmap` is an open-source penetration testing tool that detects and exploits SQL Injection vulnerabilities. Depending on the database type, the database user's privileges, and the server configuration, `sqlmap` may be able to perform advanced actions such as:

- Enumerating databases, tables, and columns
- Dumping sensitive data
- Reading and writing files on the server (when permitted)
- Executing operating system commands (only when supported by the target database and configuration)

> **Note**
> File uploads or operating system command execution are **not** possible on every vulnerable application. These capabilities depend on database-specific features, user privileges, and server misconfigurations.
> In this demonstration, **[Metasploit Framework](https://docs.metasploit.com/docs/using-metasploit/getting-started/nightly-installers.html)** is used to automate the post-exploitation process. Once the SQL Injection vulnerability has been successfully exploited, Metasploit is used to obtain a Meterpreter session and demonstrate a complete system compromise within the controlled lab environment. It's can intrigate with sqlmap. 

To use `sqlmap`, the attacker must first install the latest version of **Python 3**, as `sqlmap` is written in Python.

The command used by the attaker :
```python
 python3 sqlmap.py -u 'http://192.168.110.91/index.php?search=asd' --technique=U --os-pwn
```

![alt text](/images/project-3/video1.gif)

At this stage, the attacker has successfully gained full control of the target system. The Security Operations Center (SOC) analyst now begins the incident response process by investigating the attack, identifying indicators of compromise (IOCs), analyzing logs and alerts, determining the attacker's actions, and taking the necessary steps to contain, eradicate, and remediate the threat.

## 📊 Attack Detection and Cyber Kill Chain Analysis with Splunk Enterprise
### 🔗 Cyber Kill Chain Mapping

| Kill Chain Phase | Activity | 
|------------------|----------|
| **1. Reconnaissance** 🔍 | Browse the target website and inspect available pages and parameters. |
| **2. Weaponization** 🛠️ | Prepare the payloads |
| **3. Delivery** 📤 | Submit the malicious payload|
| **4. Exploitation** 💥 | Exploit The payload |
| **5. Installation** 📦 | Install any backdoor target Server|
| **6. Command & Control (C2)** 📡 | Backdoor Server|
| **7. Actions on Objectives** 🎯 | Retrieve sensitive records, enumerate the database, or bypass authentication. |

---

### 🔍 Reconnaissance

The **Reconnaissance** phase is the first stage of the Cyber Kill Chain. During this phase, the attacker gathers information about the target application to identify potential attack vectors and hidden resources.

For web applications, attackers commonly perform **active reconnaissance** using directory and file enumeration tools, such as:

- `dirsearch`
- `feroxbuster`
- `ffuf`

These tools help discover:

- Hidden directories and files
- Administrative panels
- Backup files
- API endpoints
- Login pages
- Other exposed resources that may be vulnerable

In this demonstration, we will use one of these tools to enumerate the target web application and generate Apache access logs, which will later be analyzed in **Splunk Enterprise**.