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
   