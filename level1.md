
# **TASK 1: Working of a Version Control**
---

### Introduction
In this task, I learned the fundamentals of **Git version control** and practiced essential commands for managing code repositories.  
I understood how version control helps track changes, collaborate with teams, and maintain project history efficiently.  

---

### What I Did
- Installed **Git** and initialized a local repository using `git init`.  
- Created multiple commits and practiced **branching** with multiple branches.  
- Practiced **git merge** to combine branches and understood fast-forward vs three-way merge.  
- Used **git rebase** to rewrite commit history and maintain a linear project timeline.  
- Experimented with **git reset --hard** to move HEAD to previous commits and undo changes.  
- Applied **git revert** to safely undo commits without rewriting history.  
- Used **git cherry-pick** to apply specific commits from one branch to another.  
- Worked with **git log** to view commit history and understand branch relationships.  
- Created multiple files (`readme.txt`, `hello.txt`, `names.txt`, etc.) to practice version tracking. 

---

### Screenshots
![Git Branch Operations](https://i.postimg.cc/wTLZSKzh/Screenshot-2025-10-25-172946.png)  
![Git Merge and Rebase](https://i.postimg.cc/J4Vd3TXV/Screenshot-2025-10-25-172553.png)  
![Git Cherry-pick and Reset](https://i.postimg.cc/sDrwGMFB/Screenshot-2025-10-25-170349.png)  

---

### Conclusion
This task gave me hands-on experience with **Git version control workflows** and helped me understand branching strategies.  
I learned the difference between **merge vs rebase**, **reset vs revert**, and when to use **cherry-pick** for selective commits.  

---


# **TASK 3: Create an Application on EC2 Instance**
---

### Introduction
In this task, I learned to deploy a web application on **AWS EC2 instance**.  
I understood why AWS is preferred over platforms like Vercel and learnt that it provides better control over storage, server management, and scalability.  

---

### What I Did
- Selected **Mumbai region (ap-south-1)** for lowest latency from India.  
- Launched a **t3.micro instance** (more efficient than t2.micro) with Amazon Linux 2023 AMI.  
- Configured **security groups** to allow HTTP (port 80) and SSH (port 22) access.  
- Connected to EC2 locally via **SSH using WSL** with `.pem` key file.  
- Installed and configured **Apache web server** using `yum` package manager.  
- Deployed a simple HTML application and accessed it via public IP.  
- Set up **billing alerts** to avoid unexpected charges and terminated instance after testing.  

---

### Screenshots
![EC2 Instance Dashboard](https://i.postimg.cc/yNwTVt1w/Screenshot-2025-10-04-140623.png)  
![SSH Connection Success](https://i.postimg.cc/CKW4FXh6/Screenshot-2025-10-04-141302.png)  
![Deployed Application](https://i.postimg.cc/q79GkSJ5/Screenshot-2025-10-04-142259.png) 
![Instance Dashboard](https://i.postimg.cc/3wPZKV8z/Screenshot-2025-10-04-142310.png)  


---

### Conclusion
This task gave me hands-on experience with **cloud infrastructure deployment** and Linux server administration.  
I learned the importance of **security configuration** and **cost management** when working with AWS services.  


---


# **TASK 7: OSI Model**
---

### Introduction
In this task, I studied the **OSI (Open Systems Interconnection) model** and understood how computer systems communicate over networks.  
The OSI model is a 7-layer theoretical framework that standardizes network communication, while the **TCP/IP model** is the practical 5-layer implementation used on the internet.  

---

### What I Learned
- **OSI Model 7 Layers**: Application, Presentation, Session, Transport, Network, Data Link, and Physical layers.  
- **TCP/IP Model**: Practical implementation merging Application, Presentation, and Session into one Application layer.  
- **Network Protocols**: HTTP, FTP, SMTP, DNS, TCP, UDP, IP, and how they work across different layers.  
- **IP Addressing**: Difference between IPv4 (32-bit) and IPv6 (128-bit), subnet masks, and DHCP.  
- **Transport Layer**: TCP vs UDP, 3-way handshake, congestion control, and connection management.  
- **Network Devices**: Routers, switches, hubs, bridges, and their roles in network communication.  
- **Network Topologies**: Bus, Ring, Star, Tree, and Mesh configurations.  
- **Security**: Firewalls, NAT (Network Address Translation), and packet filtering.  

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
This task helped me understand how **data flows through network layers** from application to physical transmission.  
I learned the difference between **OSI (theoretical) and TCP/IP (practical)** models and how protocols like **TCP, UDP, HTTP, and DNS** enable internet communication.  

