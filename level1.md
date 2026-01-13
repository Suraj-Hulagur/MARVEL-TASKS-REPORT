
# **TASK 1: Working of a Version Control**
---

### Introduction
In this task, I learned the fundamentals of **Git version control** and practiced essential commands for managing code repositories.  
I understood how version control helps track changes, collaborate with teams, and maintain project history efficiently.  

---

### What I Did
- Installed Git and initialized a local repository using `git init`.  
- Created multiple commits and practiced branching with multiple branches.  
- Practiced git merge to combine branches and understood fast forward vs three way merge.  
- Used git rebase to rewrite commit history and maintain a linear project timeline.  
- Experimented with git reset --hard to move HEAD to previous commits and undo changes.  
- Applied git revert to safely undo commits without rewriting history.  
- Used git cherry-pick to apply specific commits from one branch to another.  
- Worked with git log to view commit history and understand branch relationships.  
- Created multiple files readme.txt, hello.txt, names.txt to practice version tracking. 

---

### Screenshots
![Git Branch Operations](https://i.postimg.cc/wTLZSKzh/Screenshot-2025-10-25-172946.png)  
![Git Merge and Rebase](https://i.postimg.cc/J4Vd3TXV/Screenshot-2025-10-25-172553.png)  
![Git Cherry-pick and Reset](https://i.postimg.cc/sDrwGMFB/Screenshot-2025-10-25-170349.png)  

---

### Conclusion
This task gave me hands on experience with Git version control workflows and helped me understand branching strategies.  
I learned the difference between merge vs rebase, reset vs revert, and when to use cherry-pick for selective commits.  

---


# **TASK 3: Create an Application on EC2 Instance**
---

### Introduction
In this task, I learned to deploy a web application on AWS EC2 instance.  
I understood why AWS is preferred over platforms like Vercel and learnt that it provides better control over storage, server management, and scalability.  

---

### What I Did
- Selected Mumbai region (ap-south-1) for lowest latency from India.  
- Launched a t3.micro instance(more efficient than t2.micro) with Amazon Linux 2023 AMI.  
- Configured security groups to allow HTTP (port 80) and SSH (port 22) access.  
- Connected to EC2 locally via SSH using WSL with `.pem` key file.  
- Installed and configured Apache web server using `yum` package manager.  
- Deployed a simple HTML application and accessed it via public IP.  
- Set up billing alerts to avoid unexpected charges and terminated instance after testing.  

---

### Screenshots
![EC2 Instance Dashboard](https://i.postimg.cc/yNwTVt1w/Screenshot-2025-10-04-140623.png)  
![SSH Connection Success](https://i.postimg.cc/CKW4FXh6/Screenshot-2025-10-04-141302.png)  
![Deployed Application](https://i.postimg.cc/q79GkSJ5/Screenshot-2025-10-04-142259.png) 
![Instance Dashboard](https://i.postimg.cc/3wPZKV8z/Screenshot-2025-10-04-142310.png)  


---

### Conclusion
This task gave me hands on experience with cloud infrastructure deployment and Linux server administration.  
I learned the importance of security configuration and cost management when working with AWS services.  

---

# **TASK 5: Kali Linux – Basic Penetration Testing**

---

### Introduction
In this task, I explored Kali Linux, a Linux distribution specifically designed for penetration testing and cybersecurity analysis.  
The objective of this task was to understand how ethical hackers and security professionals analyze systems to identify vulnerabilities and misconfigurations.  
This task provided foundational exposure to penetration testing concepts such as reconnaissance, network scanning, and service enumeration.

---

### What I Did
- Installed Kali Linux as a virtual machine using VirtualBox.  
- Explored the Kali Linux desktop environment and terminal interface.  
- Studied the basic phases of penetration testing including reconnaissance and scanning.  
- Used Nmap (Network Mapper) to scan a target system from Kali Linux.  
- Identified open ports, running services, and service versions on the target.  
- Understood how exposed services can lead to potential security risks.  
- Learned the importance of performing penetration testing only with proper authorization.

---

### Tools Used
- **Kali Linux** – Penetration testing operating system  
- **Nmap** – Network scanning and security auditing tool  
- **VirtualBox** – Virtualization software

---

---

### Screenshots
![Kali Linux Nmap Scanning](https://i.postimg.cc/43DsbZVY/Screenshot_2026_01_12_160319.png)  
![Nmap Scan Results](https://i.postimg.cc/8khP4FyG/Screenshot_2026_01_12_160404.png)

---


### Conclusion
This task provided me with a basic understanding of penetration testing workflows and network security fundamentals.  
I learned how tools like Nmap are used to analyze systems and identify possible security weaknesses.  

---


---
# **TASK 6: Socket.IO – Real Time Two Way Chat Application**

---

### Introduction
In this task, I developed a real time chat application using Node.js and Socket.IO to understand how bidirectional communication works between a client and a server.  
Unlike traditional HTTP based communication, Socket.IO enables persistent connections using WebSockets, allowing messages to be exchanged instantly.  
This task focused on implementing and testing real time communication with multiple clients connected simultaneously.

---

### What I Did
- Created a Node.js project and initialized it using `npm`.  
- Installed required dependencies: Express and Socket.IO.  
- Implemented a backend server to handle Socket.IO connections and events.  
- Configured the server to:
  - Handle client connections
  - Assign usernames to users
- Tested real time two way communication by opening the application in multiple browser sessions.  

---

### Technologies Used
- **Node.js** – Server side JavaScript runtime  
- **Express.js** – Web application framework  
- **Socket.IO** – Real time, bidirectional communication  
- **HTML & CSS** – Frontend interface  
- **Git & GitHub** – Version control and repository hosting

---

### How Socket.IO Works in This Application
Each browser tab represents a Socket.IO client.  
When a user opens the application, the client emits a join event with the username.  
Messages are sent using Socket.IO events instead of HTTP requests.  
The server listens for message events and broadcasts them to all connected clients.  
This enables real time two way communication without page refreshes.

---

### Screenshots
![Screenshot-2026-01-12-170605](https://i.postimg.cc/TP56t9Mp/Screenshot-2026-01-12-191811.png)  
![Screenshot-2026-01-12-170623](https://i.postimg.cc/xCKS211F/Screenshot-2026-01-12-191801.png)

---

### Key Learnings
- Socket.IO enables persistent, real time communication using WebSockets.  
- Real time applications follow an event driven architecture rather than request–response.  
- Multiple clients can communicate simultaneously through a single server.  
- Version control using Git is essential for managing and sharing projects.  
- Simple UI design is sufficient to demonstrate backend communication concepts effectively.

---

### Conclusion
This task provided hands on experience with building a real time web application using Socket.IO. I gained practical understanding of WebSockets, client server event handling, and real time message broadcasting.  

---


# **TASK 7: OSI Model**
---

### Introduction
In this task, I studied the OSI (Open Systems Interconnection) model and understood how computer systems communicate over networks.  
The OSI model is a 7 layer theoretical framework that standardizes network communication, while the TCP/IP model is the practical 5 layer implementation used on the internet.  

---

### What I Learned
- OSI Model 7 Layers: Application, Presentation, Session, Transport, Network, Data Link, and Physical layers.  
- TCP/IP Model: Practical implementation merging Application, Presentation, and Session into one Application layer.  
- Network Protocols: HTTP, FTP, SMTP, DNS, TCP, UDP, IP, and how they work across different layers.  
- IP Addressing: Difference between IPv4 (32 bit) and IPv6 (128 bit), subnet masks, and DHCP.  
- Transport Layer: TCP vs UDP, 3 way handshake, congestion control, and connection management.  
- Network Devices: Routers, switches, hubs, bridges, and their roles in network communication.  
- Network Topologies: Bus, Ring, Star, Tree, and Mesh configurations.  
- Security: Firewalls, NAT (Network Address Translation), and packet filtering.  

---

### OSI Model in Cloud Computing
- **Application Layer**: Cloud services like AWS S3, Google Cloud Storage provide APIs for data access.  
- **Transport Layer**: TCP ensures reliable data transfer between cloud servers and clients.  
- **Network Layer**: Routers direct traffic across global cloud infrastructure using IP addressing.  
- **Data Link & Physical Layers**: Cloud providers use fiber optic cables and data centers for physical connectivity.  

Understanding the OSI model helps in troubleshooting network issues, designing scalable cloud architectures, and securing data transmission.  

---

### Notes Link
I made detailed handwritten notes while understanding the OSI model and networking concepts. Sorry for the bad handwriting, I had made these for myself.
[View My Notes (PDF)](https://drive.google.com/file/d/1ErvrBiVOIGzyXEX6HdX8nabaG2fXb-uA/view?usp=drive_link)  


---

### Conclusion
This task helped me understand how data flows through network layers from application to physical transmission.  
I learned the difference between OSI (theoretical) and TCP/IP (practical) models and how protocols like TCP, UDP, HTTP, and DNS enable internet communication.  

# TASK 8: IaaS, PaaS, and SaaS

Cloud computing enables users to access computing resources including servers, storage, databases, and software through the internet rather than owning and maintaining physical hardware.

Based on the degree of control users possess and the responsibility assumed by cloud providers, cloud services are primarily categorized into three models: Infrastructure as a Service (IaaS), Platform as a Service (PaaS), and Software as a Service (SaaS).

These models provide flexibility, scalability, and cost effectiveness for individuals and organizations alike.

---

## 1. Infrastructure as a Service (IaaS)

IaaS delivers fundamental computing infrastructure such as virtual machines, storage, and networking through the internet.

Under this model, users maintain complete control over operating systems and applications, while cloud providers handle the physical hardware components.

### Key Features
- Users retain extensive control over infrastructure
- Infrastructure scales dynamically according to demand
- Eliminates the need for purchasing or maintaining physical servers

### Examples
- Amazon EC2
- Microsoft Azure Virtual Machines
- Google Compute Engine

### Use Case
System administrators and developers primarily utilize IaaS when they require complete flexibility to configure servers, host websites, or deploy customized applications.

---

## 2. Platform as a Service (PaaS)

PaaS offers a pre configured platform where developers can build, test, and deploy applications without managing servers or operating systems.

Cloud providers handle infrastructure management, runtime environments, and system updates.

### Key Features
- Emphasis on application development rather than infrastructure
- Integrated tools and services streamline deployment
- Accelerates development cycles and minimizes maintenance overhead

### Examples
- Google App Engine
- AWS Elastic Beanstalk
- Microsoft Azure App Service

### Use Case
Developers who prefer concentrating exclusively on coding and application development without managing underlying infrastructure find PaaS particularly beneficial.

---

## 3. Software as a Service (SaaS)

SaaS provides fully functional software applications through the internet. Users access these applications via web browsers without local installation or maintenance requirements.

Service providers manage all updates, security measures, and data handling.

### Key Features
- Requires no local installation
- Accessible from any location with internet connectivity
- Automatic updates and backup systems

### Examples
- Gmail
- Google Drive
- Zoom
- Canva

### Use Case
End users who need immediate access to software for communication, collaboration, or productivity tasks rely on SaaS solutions.

---

## Comparison Table: IaaS vs PaaS vs SaaS

| Feature | IaaS | PaaS | SaaS |
|------|------|------|------|
| User Control | High | Medium | Low |
| Target Users | System Administrators | Developers | End Users |
| Infrastructure Management | User | Cloud Provider | Cloud Provider |
| Application Management | User | User | Cloud Provider |
| Examples | AWS EC2, Azure VM | App Engine, Beanstalk | Gmail, Zoom |
| Common Use Case | Hosting servers | App development | Using software |

---

## Conclusion

This task provided me insights into how cloud computing services are structured according to control levels and responsibility distribution.

- **IaaS** provides maximum flexibility and control over infrastructure
- **PaaS** streamlines the application development process
- **SaaS** allows direct software utilization without backend management concerns

Each model addresses specific requirements, and collectively they enhance the efficiency and accessibility of cloud computing across various user categories.

# **TASK 9: Encryption Techniques – Secure Messaging Application**

---

### Introduction
In this task, I studied and implemented encryption and decryption techniques to understand how secure communication works in real world applications.  
The focus was on learning classical ciphers, modern encryption concepts, and building a simple secure messaging flow in Python.  
I also explored the difference between encryption and hashing, the role of symmetric and asymmetric keys, and how prime numbers are used in RSA encryption.

---

### What I Did
- Studied basic concepts of ciphers, keys, and secure communication.  
- Learned classical ciphers including Caesar cipher, Vigenère cipher, and Substitution cipher to understand core encryption logic.  
- Understood why SHA256 is a hashing algorithm (one way, irreversible) and not an encryption method.  
- Learned the difference between symmetric encryption (same key) and asymmetric encryption (public and private keys).  
- Studied how RSA uses very large prime numbers to generate secure public–private key pairs.  
- Implemented a secure messaging simulation in Python using the PyCryptodome library and AES symmetric encryption.  
- Encrypted messages before transmission and decrypted them on the receiver side using the same shared secret key.  

---

### Encryption Techniques 
- **Classical Ciphers**  
  - Caesar Cipher: Shifts characters by a fixed number; useful for understanding basic substitution logic.  
  - Vigenère Cipher: Uses a repeating keyword to encrypt text, making it stronger than a simple Caesar shift.  
  - Substitution Cipher: Maps each character to another character using a fixed substitution table.  
  These are mainly educational and not secure for modern cryptographic use.

- **Encryption vs Hashing (SHA256)**  
  - Encryption is reversible and is used to protect data during storage or communication when decryption is required.  
  - SHA256 is an irreversible hashing algorithm, used for password storage and data integrity checks rather than for encryption.

- Symmetric and Asymmetric Encryption  
  - *Symmetric Encryption*:  
    - Uses the same secret key for encryption and decryption.  
    - Fast and suitable for encrypting message content (e.g., AES).  
  - *Asymmetric Encryption*:  
    - Uses a public key for encryption and a private key for decryption.  
    - Commonly used for secure key exchange and digital signatures (e.g., RSA).

- Role of Prime Numbers in RSA
  - RSA security relies on the difficulty of factoring the product of two very large prime numbers.  
  - These primes are used to generate the **public and private keys**, forming the basis of RSA’s strength.

---

### Secure Messaging Implementation
In this task, I simulated a basic secure chat flow using Python:  
- The sender encrypts each message using AES symmetric encryption with a shared secret key.  
- The encrypted ciphertext is transmitted instead of plaintext.  
- The receiver uses the same secret key to decrypt the ciphertext back into the original message.  
This demonstrates how real secure messaging systems protect data in transit by ensuring only authorized parties with the correct key can read the messages.

---

### Screenshots
![Secure Chat – Encryption Flow](https://i.postimg.cc/fyTzdRn9/Screenshot-2026-01-13-084052.png)

---

### GitHub Repository
The complete source code for this task is available on GitHub:  
 [Click](https://github.com/Suraj-Hulagur/secure_chat)  

---
### Conclusion
This task helped me gain practical understanding of encryption techniques and secure communication workflows.  
By implementing encryption and decryption using Python and PyCryptodome, I learned how real world secure messaging systems work internally.  
I understood the importance of symmetric encryption for secure data transmission, how hashing differs from encryption, and why RSA relies on prime numbers for key generation.  


---

# **TASK 10: IP Addressing and Web Scraping – Job Listings Scraper**

---

### Introduction
In this task, I explored web scraping using Python to understand how data can be programmatically extracted from websites.  
The task also helped reinforce my understanding of IP addressing and TCP/IP communication, as web scraping relies on HTTP requests sent over the internet to a server's IP address.  
I implemented a simple job listings scraper that extracts job related information from a static webpage.

---

### Web Scraping and IP Addressing
Web scraping starts with an HTTP request sent from the client to the server.  
This request is routed to the server's IP address using TCP/IP protocols.  
The server responds with HTML content, which is then parsed locally.  
Extracted data is processed and displayed to the user.  
This demonstrates how application layer protocols rely on underlying network layers for communication.

---

### What I Did
- Studied the basics of web scraping and how HTTP requests work.  
- Used Python to send HTTP GET requests to a job listings webpage.  
- Parsed HTML content using BeautifulSoup.  
- Extracted job information
- Displayed the extracted data in the terminal.


---


### Website Used for Scraping
To safely demonstrate web scraping concepts, I used a static job listings page:  
[click](https://realpython.github.io/fake-jobs/)  

This allowed me to focus on HTML parsing and data extraction without dealing with JavaScript rendered content or scraping limitations.

---

### Screenshots
![Job Scraper Output](https://i.postimg.cc/j2Dsvqv8/Screenshot-2026-01-13-091426.png)

---

### GitHub Repository
The complete source code for this task is available on GitHub:  
[click](https://github.com/Suraj-Hulagur/job_scrapper)  

The repository contains:
- Python script for job scraping
- Required library usage
- Clean and minimal project structure

---

### Conclusion
This task helped me understand how web scraping works internally and how internet communication relies on IP addressing and standard protocols.  
By implementing a job listings scraper in Python, I gained practical experience in sending HTTP requests, parsing HTML content, and extracting meaningful data from webpages.

---


