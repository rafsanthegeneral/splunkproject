## 🛡️ About Splunk as a SIEM Solution

Splunk is an industry-leading **SIEM (Security Information and Event Management)** platform designed to provide comprehensive, real-time security visibility across enterprise infrastructures. By indexing massive streams of machine data, Splunk empowers Security Operations Centers (SOCs) to monitor, detect, analyze, and respond to threats at scale.

---

### 🚀 Key Capabilities

* **Data Ingestion & Indexing:** Consumes multi-structured data from virtually any source (firewalls, endpoints, cloud infrastructure, OS logs) without requiring predefined schemas.
* **Advanced Threat Detection:** Utilizes the **Splunk Search Processing Language (SPL)** to build complex queries, correlate disparate logs, and uncover sophisticated attack patterns.
* **Real-Time Alerting & Dashboards:** Transforms raw logs into actionable visualization panels and triggers automated alerts when anomalous behavior or known Indicators of Compromise (IoCs) are detected.
* **Incident Response & SOAR:** Integrates with Splunk SOAR (Security Orchestration, Automation, and Response) to automate playbooks and minimize Mean Time to Respond (MTTR).

---

### 🏗️ Core Architecture Components

To build an efficient pipeline, a typical Splunk SIEM deployment relies on a tiered architecture:

| Component | Role | Function |
| :--- | :--- | :--- |
| **Forwarders (UF/HF)** | Data Collection | Lightweight agents deployed on endpoints/servers to collect and send logs. |
| **Indexers** | Storage & Processing | Receives, parses, indexes, and stores the incoming log data. |
| **Search Heads** | User Interface | The frontend tier where analysts write SPL queries, build dashboards, and manage alerts. |

---

> 💡 **Why Splunk?** > In a modern security landscape, visibility is everything. Splunk bridges the gap between massive, fragmented log data and actionable threat intelligence, acting as the centralized "brain" of defensive cybersecurity operations.


# LAB SETUP GUIDE 

### 🏗️ Environment Setup & Prerequisites

Before beginning the configuration, ensure your environment matches or meets the minimum requirements outlined below:

#### 🖥️ Host Machine (Splunk Enterprise / Receiver)
* **Operating System:** Xubuntu 24.04.4 LTS x86_64
* **Role:** Splunk Enterprise Server Deployment

#### 💻 Target Virtual Machine (Splunk Universal Forwarder)
* **Operating System:** Windows 7 (x64) or higher
* **Role:** Endpoint Log Collection

> 💡 **Resource Optimization:** If you are running a low-spec lab environment, you can utilize lightweight Windows ISOs to minimize RAM and CPU overhead. Recommended lightweight versions include:
> * [Windows X-Lite](https://windowsxlite.com/)
> * [Windows 10 Lite Edition via Archive.org](https://archive.org/details/windows-10-lite-edition-19h2-x64)


# 🖥️ Host Machine Setup: Splunk Enterprise (Receiver)

To deploy Splunk Enterprise as your centralized log receiver, follow the registration and installation steps below.

### 📋 Step 1: Create a Splunk Account
1. Head over to the official [Splunk Registration Page](https://www.splunk.com/en_us/form/sign-up.html?module=nav&redirecturl=https://www.splunk.com/).
2. Fill out the required details to create your free account.
3. Verify your registration and log in to the portal.

### 📥 Step 2: Download the Splunk Enterprise Installer
Once logged in, navigate to the [Splunk Enterprise Download Center](https://www.splunk.com/en_us/download/splunk-enterprise.html) to choose the package optimized for your operating system.

![Spunk Download Webpage](images/image.png)

> 💡 **Xubuntu/Linux Users:** > Because i am using `Xubuntu 24.04` environment, select the **Linux** tab and download the `.tar` Pakage.

```bash
tar -xfv {filename}.tar
```
Once the download is complete, extract the contents of the archive and navigate to the Splunk binary directory to begin configuration:

1. Extract the `.tgz` or `.tar` archive to your desired installation directory.
2. Open your terminal and change your directory to `splunk/bin`:

```bash
cd /path/to/extracted/splunk/bin
./splunk start --accept-license
```
Upon running the startup command, the installer will prompt you to configure your administrator credentials:

1. **Create Administrator Username:** Enter a secure username (e.g., `admin`).
2. **Create Administrator Password:** Enter and confirm a strong password.

Once the initialization process completes, your Splunk instance will be live and accessible via your web browser at:

👉 **[http://localhost:8000](http://localhost:8000)**

![alt text](images/image2.png)

After All done you Get the Splunk Dashboard ready to go.

![alt text](images/image3.png)

Then Login with credentials you created during the initial setup phase. 

![alt text](images/image4.png)

Succesfully Installed !!! Splunk Enterprise .

# 🖥️ Forwader Machine Setup: Splunk Forwader (Log Sender or Agent)

For the forwarder node, a VirtualBox Windows VM will serve as the target endpoint. We will install the native Splunk Universal Forwarder Windows agent to collect and ship logs to our Xubuntu receiver.

#### 📥 Step 1: Download the Windows Agent
1. Navigate to the official [Splunk Universal Forwarder Download Page](https://www.splunk.com/en_us/download.html). *(Note: If your session has expired, you will be prompted to log back in).*
2. Select the **Windows** tab.
3. Choose the appropriate installer package based on your VM architecture:
   * **Windows 64-bit (x64)** (Recommended)
   * **Windows 32-bit (x86)**

### ⚙️ Step 2: Running the Universal Forwarder Installer

Locate your downloaded `.msi` file inside your Windows VM and execute it to begin the installation wizard.

#### 1️⃣ Define Forwarder Credentials
During the initial phase of the setup wizard, you will be prompted to create administrative credentials. 
* **Username:** Create a local admin account for managing the forwarder agent.
* **Password:** Enter and confirm a strong password.

![Splunk Forwarder User Account Configuration](images/image6.png)

#### 2️⃣ Configure the Deployment & Receiving Servers
Next, the installer will request network information to establish a connection with your Splunk Enterprise host machine.

* **Deployment Server:** You can leave this blank if you are managing the configuration locally.
* **Receiving Indexer (Splunk Enterprise):** Input the static **IP Address** of your Xubuntu host machine along with the default Splunk indexing port (`9997`).

![Splunk Enterprise Server IP Configuration](images/image7.png)

#### 3️⃣ Complete the Installation
Review your settings, click **Install**, and wait for the wizard to successfully copy all files and initialize the background services on the Windows machine.




