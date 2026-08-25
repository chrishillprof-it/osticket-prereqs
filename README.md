<p align="center">
  <img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

# osTicket: Prerequisites and Installation

This repository documents, **step-by-step**, the prerequisites and installation of **osTicket** (an open-source help desk ticketing system) on a **Windows 10 Azure Virtual Machine** using **IIS + PHP + MySQL**.

This is **Part 1** of a three-part osTicket project series:

- **osTicket: Prerequisites and Installation** (this repo)
- osTicket: Post-Installation Configuration
- osTicket: Ticket Lifecycle Examples

---

## Video Demonstration

Coming soon (screenshots and video tutorial will be added later).

---

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines / Compute)
- Windows 10
- Remote Desktop (RDP)
- Internet Information Services (IIS) with CGI
- PHP 7.3.8 (NTS)
- MySQL 5.5.62
- HeidiSQL

---

## Required Downloads

The following files are required to complete this lab. In this project, installation assets are hosted externally for convenience.


- [Download osTicket Installation Files (ZIP)](https://drive.google.com/uc?export=download&id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD)
- [Microsoft Visual C++ Redistributable (x86)](https://drive.google.com/file/d/1s1OsGF3-ioO0_9LYizPRiVuIkb3lFJgH/view?usp=share_link)
- [MySQL Server 5.5.62 (Windows)](https://drive.google.com/file/d/1_OWh9p7VQLcrB0q_V7qT8yHl0xo5gv7z/view?usp=share_link)
- [HeidiSQL Database Client](https://docs.google.com/document/d/1WovrX2DaS9xkfaSr4LXyB4YnnWpXIgPCMMbbfgHmGVw/edit)

> Note: Large installation files may be hosted externally (e.g., Google Drive) to keep the GitHub repository lightweight.

---

## Security Note (Read This)

**Do not store passwords in plain text in real-world documentation.**  
This lab includes credentials for ease of use and repeatability, but in production environments you should store credentials in a password manager (e.g., KeePass, LastPass, NordPass) and follow your organization’s credential handling policies.

---

# Installation Steps (Do Not Skip / In Order)

Follow **each step below in order**. This is written to be reproducible by someone following along for the first time.

---

## 1) Create an Azure Virtual Machine (Windows 10, 4 vCPUs)

Create a Windows 10 VM in Microsoft Azure with the following settings:

- VM Name: `osticket-vm`
- Size: **4 vCPUs**
- Username: `labuser`
- Password: `osTicketPassword1!`

<img width="1183" height="811" alt="image" src="https://github.com/user-attachments/assets/b7f6a71d-7fe6-42ae-a64e-5ea455b33808" />

<img width="1177" height="805" alt="image" src="https://github.com/user-attachments/assets/0c757ff6-1368-417e-a814-956587fd3b92" />


---

## 2) Log into the VM with Remote Desktop

Connect to the VM using **Remote Desktop (RDP)** and log in with:

- Username: `labuser`
- Password: `osTicketPassword1!`

After you create the VM, it should start automatically. Navigate to the resource and copy the public IP address as shown in the following screenshots:

<img width="1693" height="821" alt="image" src="https://github.com/user-attachments/assets/f5226400-fa97-445a-90fc-ab52c5fe60c7" />

Next, open Remote Desktop and paste the public IP address to open the login window.

<img width="537" height="306" alt="image" src="https://github.com/user-attachments/assets/1dbce845-6883-4bb1-a417-c72ff82829db" />

Log in with the credentials used when creating the VM:

- Username: `labuser`
- Password: `osTicketPassword1!`



---

## 3) Download and unzip the osTicket installation bundle

Inside the VM (`osticket-vm`):

1. Download [`osTicket-Installation-Files.zip`](https://drive.google.com/uc?export=download&id=1b3RBkXTLNGXbibeMuAynkfzdBC1NnqaD) 
2. Unzip it onto your **Desktop**
3. Confirm the extracted folder is named:

    osTicket-Installation-Files

You will use the contents of this folder to install osTicket and the required dependencies.

---

## 4) Install / Enable IIS in Windows (WITH CGI)

Enable IIS and CGI support:

- Install/Enable **Internet Information Services (IIS)** in Windows Features
- Ensure **CGI** is enabled at:

    World Wide Web Services  
    -> Application Development Features  
    -> [X] CGI
  <img width="773" height="736" alt="image" src="https://github.com/user-attachments/assets/66387067-57c8-4841-b420-d1e21d64c00d" />


---

## 5) Install PHP Manager for IIS

From the `osTicket-Installation-Files` folder, install:

- `PHPManagerForIIS_V1.5.0.msi`

---

## 6) Install IIS Rewrite Module

From the `osTicket-Installation-Files` folder, install:

- `rewrite_amd64_en-US.msi`

---

## 7) Create the PHP directory

Create the following directory on the VM:

- `C:\PHP`

---

## 8) Install PHP 7.3.8 (NTS)

From the `osTicket-Installation-Files` folder:

1. Unzip PHP 7.3.8:

   - `php-7.3.8-nts-Win32-VC15-x86.zip`

2. Extract the contents into:

   - `C:\PHP`

---

## 9) Install Visual C++ Redistributable (x86)

From the `osTicket-Installation-Files` folder, install:

- `VC_redist.x86.exe`

---

## 10) Install MySQL 5.5.62

From the `osTicket-Installation-Files` folder, install:

- `mysql-5.5.62-win32.msi`

During setup, use these selections:

- Typical Setup  
- Launch Configuration Wizard (after install)  
- Standard Configuration  
- Username: `root`  
- Password: `root`

---

## 11) Open IIS as Administrator

Open **Internet Information Services (IIS) Manager** with administrative privileges:

- Right-click IIS Manager  
- Run as Administrator

---

## 12) Register PHP in IIS

Inside IIS:

1. Open **PHP Manager**
  <img width="1416" height="730" alt="image" src="https://github.com/user-attachments/assets/bcfa8b54-49c0-41a9-8264-072bd3c528e6" />

2. Register the PHP executable by pointing it to:

    C:\PHP\php-cgi.exe
<img width="1176" height="653" alt="image" src="https://github.com/user-attachments/assets/bec03a5d-c6cb-41a1-a95a-af8eb88b764b" />


---

## 13) Reload IIS (Stop / Start)

Reload IIS:

- Open IIS
- **Stop** the server
- **Start** the server

(You are intentionally cycling IIS to ensure the PHP registration and modules load correctly.)

---

## 14) Install osTicket v1.15.8

From the `osTicket-Installation-Files` folder:

1. Unzip:

   - `osTicket-v1.15.8.zip`

2. Copy the `upload` folder into:

    c:\inetpub\wwwroot
<img width="1023" height="614" alt="image" src="https://github.com/user-attachments/assets/239a8d79-f6d8-4526-a67a-3926b67707ac" />


3. Inside `c:\inetpub\wwwroot`, rename:

- `upload` → `osTicket`
<img width="1017" height="616" alt="image" src="https://github.com/user-attachments/assets/3661a42d-827d-4a6e-8980-b8f9a4bdf757" />


---

## 15) Reload IIS (Stop / Start) again

Reload IIS again:

- Open IIS
- **Stop** the server
- **Start** the server

---

## 16) Browse to the osTicket site in IIS

In IIS:

1. Go to:

- Sites → Default Web Site → osTicket

2. On the right side, click:

- **Browse \*:80**

This opens osTicket in your browser.

<img width="748" height="941" alt="image" src="https://github.com/user-attachments/assets/b9a7005b-d1b1-430d-a5b5-bbf9b373a664" />


---

## 17) Enable required PHP extensions for osTicket

At this point, you should see that some extensions are **not enabled**.

Go back to IIS:

1. Sites → Default Web Site → osTicket  
2. Double-click **PHP Manager**  
3. Click **Enable or disable an extension**  
4. Enable these extensions:

- `php_imap.dll`
- `php_intl.dll`
- `php_opcache.dll`

<img width="1166" height="730" alt="image" src="https://github.com/user-attachments/assets/040000bb-993f-484f-86e3-87eb77659440" />


Now refresh the osTicket page in your browser and observe that the requirement checks change.

<img width="739" height="839" alt="image" src="https://github.com/user-attachments/assets/4b25741a-1c3e-436c-8543-799246fb7943" />

---

## 18) Rename ost-config.php

Rename the osTicket configuration file:

- From:

    C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php

- To:

    C:\inetpub\wwwroot\osTicket\include\ost-config.php

---

## 19) Assign permissions to ost-config.php

Set permissions on:

- `C:\inetpub\wwwroot\osTicket\include\ost-config.php`

Steps:

1. Disable inheritance → **Remove All**
2. Add new permissions:
   - **Everyone** → **All** (Full Control)

This is required so the web installer can write configuration values.

---

## 20) Continue osTicket setup in the browser

Return to the browser setup page and click **Continue**.

Enter:

- Helpdesk Name: `Helpdesk`
- Default email: (this receives email from customers)

---

## 21) Install and use HeidiSQL to create the database

From the `osTicket-Installation-Files` folder:

1. Install **HeidiSQL**
2. Open HeidiSQL
3. Create a new session using:
   - Username: `root`
   - Password: `root`
4. Connect to the session
5. Create a database named:

    osTicket

---

## 22) Continue osTicket setup in the browser (database settings)

Return to the osTicket installer in the browser and enter:

- MySQL Database: `osTicket`
- MySQL Username: `root`
- MySQL Password: `root`

Click:

- **Install Now!**

---

## 23) Confirm installation success

If the installation completes successfully, validate that both portals load:

- **Admin / Staff Control Panel (SCP) Login:**
  http://localhost/osTicket/scp/login.php
  
  <img width="1883" height="924" alt="image" src="https://github.com/user-attachments/assets/65424436-17fa-4fb1-a28f-434d170c4f21" />


- **End User Portal:**  
  http://localhost/osTicket/
  
  <img width="1052" height="683" alt="image" src="https://github.com/user-attachments/assets/25ed4ad7-a1db-480c-bbfc-9d5810509f5e" />


At this stage, osTicket is fully installed and operational within the Windows 10 Azure virtual machine.

---

With the application running, required dependencies installed, and core services functioning, this concludes the **Prerequisites and Installation** phase.

➡️ Continue to [osTicket: Post-Installation Configuration](https://github.com/kylekincaid/post-install-config) to secure the deployment and prepare it for real-world use.
