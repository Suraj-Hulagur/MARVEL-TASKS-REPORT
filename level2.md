# **TASK 2: CI/CD (Continuous Integration & Continuous Delivery) - Jenkins**

---

### Introduction

For this task I set up Jenkins and built a pipeline to understand how CI/CD actually works in practice. Jenkins automates the process of pulling code, installing dependencies, running tests, and deploying an application every time changes are pushed to a repository. I ran Jenkins as a Docker container, connected it to a GitHub repository, and wrote a Jenkinsfile defining the full pipeline with separate stages for checkout, build, test, and deploy.

---

### What I Did

* Learned the difference between Continuous Integration, which automatically builds and tests code on every change, and Continuous Delivery/Deployment, which automates releasing that tested code.
* Ran Jenkins using Docker instead of a native install, mapping it to port 8081 on my machine and unlocking it with the initial admin password generated inside the container.
* Installed the suggested Jenkins plugins and created an admin account to access the dashboard.
* Created a new Pipeline job in Jenkins and configured it to pull its pipeline definition directly from a GitHub repository using Pipeline script from SCM.
* Created a small js repository on GitHub containing a package.json, a server.js file, and a Jenkinsfile.
* Wrote the Jenkinsfile with four stages: Checkout, Install Dependencies, Running Tests, and Starting App.
* Installed Node.js and npm inside the running Jenkins container, since the base Jenkins image does not include them by default.
* Ran the pipeline using Build Now and reviewed the Console Output to confirm each stage executed correctly.

---

### Pipeline Stages

| Stage | What it does |
|---|---|
| Checkout | Pulls the latest code from the GitHub repository |
| Install Dependencies | Runs `npm install` to set up required packages |
| Running Tests | Runs `npm test` to verify the code before deployment |
| Starting App | Runs `node server.js` to start the application |


---

### Results

Running the pipeline pulled the repository, installed dependencies with npm install, ran the test script which printed "basic test passed", and started the server which printed "Server is listening on port 3000". All four stages completed successfully and the build finished with status SUCCESS.

---

### Screenshots

**Build status showing pipeline completed successfully**

[![Screenshot-2026-08-09-023457.png](https://i.postimg.cc/rpsRZ6Db/Screenshot-2026-08-09-023457.png)](https://postimg.cc/jWVjCFQQ)



**Console output showing all four stages running in sequence**
[![Screenshot-2026-08-09-023700.png](https://i.postimg.cc/yYhRkyqw/Screenshot-2026-08-09-023700.png)](https://postimg.cc/XpJqPFY8)



[![Screenshot-2026-08-09-023712.png](https://i.postimg.cc/DfXLKKxG/Screenshot-2026-08-09-023712.png)](https://postimg.cc/ThTprBM2)

---

### GitHub Repository

The complete source code and Jenkinsfile for this task are available on GitHub:
[Click](https://github.com/Suraj-Hulagur/jenkins-ci-demo)

---
# **TASK 5: Wireshark – Packet Capture and Network Traffic Analysis**

---

### Introduction

For this task I used Wireshark inside my Kali VM to capture live network traffic and understand how data actually moves across a network at the packet level. I captured traffic on the eth0 interface while browsing normally, then went through the capture looking specifically for unencrypted HTTP traffic, retransmissions, and the TCP three way handshake, and used the Statistics menu to see the overall traffic pattern over time.

---

### What I Did

* Started a live capture on the eth0 interface and generated real traffic by browsing a few sites, including a plain HTTP site to guarantee some unencrypted traffic would show up, since most browsers now auto upgrade everything to HTTPS by default.
* Applied the `http` display filter to isolate unencrypted requests and responses, and opened one of the packets to look at the actual HTTP headers being sent in plain text.
* Applied the `tcp.analysis.retransmission` filter to check for any packets that had to be resent, which is a sign of packet loss or an unstable connection.
* Applied the `tcp.flags.syn==1` filter to find TCP handshake packets, and opened one to look at the SYN and ACK flags directly.
* Opened Statistics and Analyze the I/O Graph to see overall packet activity over time, with TCP errors highlighted separately from normal traffic.

---

### HTTP Traffic

Filtering on `http` showed clear GET requests and 200 OK responses to `info.cern.ch`, along with a request for its favicon. Opening one of these packets showed the full HTTP request in plain readable text, including the Host header, the User Agent string, and the Accept headers, none of it encrypted in any way. This is a direct, visible example of why HTTP is considered insecure compared to HTTPS, where the same kind of data would instead show up as encrypted content.

### Retransmissions

The `tcp.analysis.retransmission` filter picked up several retransmitted packets during the capture, some marked as regular retransmissions and a few marked as fast retransmissions. A fast retransmission happens when the sender gets duplicate acknowledgments and resends data immediately instead of waiting for a timeout, which points to a brief moment of packet loss or congestion rather than a fully broken connection. The number of retransmissions found was small compared to the total packets captured, which suggests the connection was mostly stable with only occasional loss.

### Three Way Handshake

Filtering on `tcp.flags.syn==1` showed a stream of SYN and SYN, ACK packets, one for every new TCP connection being opened during the capture. Opening one of the SYN, ACK packets and expanding the Flags field showed the raw flag value `0x012`, confirming both the SYN and ACK bits were set together, which is exactly the second step of the three way handshake, the server acknowledging the client's connection request while proposing its own.

### Statistics and I/O Graph

The I/O Graph gave a clear view of overall traffic across the capture, with a few sharp spikes reaching close to 2000 packets per second at certain points. The graph also plotted TCP errors as a separate red bar on the same timeline, showing where retransmissions occurred relative to the overall traffic volume.

---

### Results

The capture confirmed several things in practice rather than just in theory. Plain HTTP genuinely sends everything, including headers, in readable text, which is visible the moment you open one of the packets. A small number of retransmissions and fast retransmissions showed up, which is a normal part of real network traffic rather than a sign of a broken connection. The three way handshake is directly visible as SYN followed by SYN, ACK followed by ACK for every connection opened, with the raw flag values confirming this in the packet details. The I/O Graph made it possible to see overall traffic volume over time alongside where TCP errors occurred.

---

### Screenshots

**Expanded HTTP packet showing the request sent in plain, unencrypted text**
[![Screenshot-2026-08-09-042734.png](https://i.postimg.cc/KcnJFLYJ/Screenshot-2026-08-09-042734.png)](https://postimg.cc/0ryp0zDw)

**Retransmitted packets found using the tcp.analysis.retransmission filter**
[![Screenshot-2026-08-09-042802.png](https://i.postimg.cc/hGk1RPRR/Screenshot-2026-08-09-042802.png)](https://postimg.cc/rd9rSTpf)

**Expanded SYN, ACK packet showing the three way handshake flags**
[![Screenshot-2026-08-09-043002.png](https://i.postimg.cc/rsJ1410K/Screenshot-2026-08-09-043002.png)](https://postimg.cc/m1P1fFSs)

**Statistics I/O Graph showing traffic over time with TCP errors marked**
[![Screenshot-2026-08-09-043046.png](https://i.postimg.cc/kXJFmpzq/Screenshot-2026-08-09-043046.png)](https://postimg.cc/Btz15MJV)
---

# **TASK 6: Docker – Containers, Images, and Dockerfiles**

---

### Introduction

For this task I learned the basics of Docker by containerizing a chat application, a Node.js and Socket.IO based real time chat app. The goal was to understand what a Docker image and container actually are, and how a Dockerfile defines the steps to build one. I wrote a Dockerfile for the app, built an image from it, ran it as a container, and confirmed the app worked correctly inside the container by chatting between two browser tabs.

---

### What I Did

* Learned the difference between a Docker image and a Docker container, where an image is a packaged, reusable blueprint and a container is a running instance of that image.
* Understood that a Dockerfile is a text file containing step by step instructions Docker follows to build an image, such as which base environment to start from, what to copy in, and what command to run when the container starts.
* Wrote a Dockerfile for my Node.js chat application, starting from an official Node base image instead of building the environment from scratch.
* Learned why dependencies are installed in a separate step before copying the rest of the code, which allows Docker to reuse that step from cache when only application code changes and not the dependency list.
* Built a Docker image from the Dockerfile using `docker build`.
* Ran the image as a container using `docker run`, mapping the container's internal port to a port on my machine so the app could be reached from a browser.
* Verified the containerized app worked exactly like running it normally, by opening two browser tabs and exchanging real time messages through Socket.IO from inside the container.

---

### Commands Used

| Command | What it does |
|---|---|
| `docker build -t chatapp .` | Builds an image named chatapp from the Dockerfile in the current folder |
| `docker run -d -p 3000:3000 --name chatapp-test chatapp` | Runs the image as a container, mapping port 3000 on the host to port 3000 in the container |
| `docker ps` | Lists running containers to confirm the app is up |
| `docker stop chatapp-test` | Stops the running container |
| `docker rm chatapp-test` | Removes the stopped container |

---

### Results

The image built successfully in seven steps, pulling the Node 18 base image, installing dependencies, and copying in the application code, ending with `Successfully tagged chatapp:latest`. Running the container and opening `localhost:3000` in the browser loaded the chat application exactly as expected. Opening a second tab and exchanging messages confirmed that Socket.IO's real time, two way communication worked correctly from inside the container, with each tab showing the other user's messages instantly.

---

### Screenshots

**Dockerfile and successful build and run in the terminal**
[![Screenshot-2026-08-09-030523.png](https://i.postimg.cc/L8tBzFcL/Screenshot-2026-08-09-030523.png)](https://postimg.cc/JshkLv4z)


**Chat working across two browser tabs through the containerized app**
[![Screenshot-2026-08-09-030859.png](https://i.postimg.cc/wBFXGcxC/Screenshot-2026-08-09-030859.png)](https://postimg.cc/JyDHyXkx)


[![Screenshot-2026-08-09-030904.png](https://i.postimg.cc/63md9WvC/Screenshot-2026-08-09-030904.png)](https://postimg.cc/VrXSgc4N)

---

### GitHub Repository

The complete source code and Dockerfile for this task are available on GitHub:
[Click](https://github.com/Suraj-Hulagur/socketio_chat)

---
# **TASK 7: Dockerize – Docker Networking, Volumes, and Storage**

---

### Introduction

For this task I containerized the backend of my Level 0 chat application, ran a MongoDB database as a separate container, connected both using a custom bridge network, and used a volume to make sure the database's data was not lost when the container was removed. Dockerizing an application basically means packaging it along with its dependencies and runtime environment into a container image, so it runs the same way no matter what machine it's on.

---

### Bridge Network

A bridge network is a Docker network driver that lets multiple containers on the same host talk to each other. Containers placed on a custom bridge network can find and reach each other using their container name instead of a fixed IP address, and are isolated from the outside world unless a port is explicitly published.

### Volumes

A Docker volume is a persistent storage location managed by Docker that exists outside a container's own filesystem. Data written to a volume stays intact even if the container using it is stopped, removed, or recreated, which is why volumes are the right place to store anything that needs to outlive a single container, like a database.

### Storage

A container's own writable layer is temporary and tied to that specific container. Anything written directly inside the container without a volume disappears the moment the container is deleted. That's fine for short lived data, but not for anything that actually needs to stick around, which is why I made sure the database wrote into a volume instead of its own internal storage.

---

### What I Did

I started by creating a custom bridge network so my containers would be able to find each other by name. Then I ran MongoDB on that network, attaching a volume so its data wouldn't disappear if the container ever got removed. After that I ran my chat app container from Task 6 on the same network as MongoDB, so both containers were now sitting side by side.

To actually confirm the networking worked, I opened a shell inside the running chat app container and tried reaching MongoDB by its container name. The base Node image didn't have `ping` installed, so I used Node itself instead, first resolving the hostname to an IP address, then opening a real connection to MongoDB's port from inside the app container. Both worked, which confirmed the two containers could talk to each other purely through Docker's internal networking.

I didn't want to just take the volume's word for it either, so I tested it properly. I inserted a test document into MongoDB, confirmed it was there, then deleted the MongoDB container completely and spun up a brand new one attached to the same volume. The document was still there in the new container, which is the actual proof that the data lived in the volume and not in the container itself.

---

### Results

Both containers ran successfully side by side on the same bridge network. The chat app container was able to resolve MongoDB by name and open a live connection to it, which confirmed the networking side worked exactly as intended, without needing to hardcode any IP addresses anywhere. Deleting and recreating the MongoDB container while keeping the same volume attached, and seeing the same data come back, confirmed that the volume genuinely persists data independent of any single container's lifecycle rather than just being a flag I added without checking it actually worked.

---

### Screenshots

**Dockerfile and both containers running together, connected via docker exec with DNS resolution and connection proof to MongoDB**
[![Screenshot-2026-08-09-035619.png](https://i.postimg.cc/7PcZNG4W/Screenshot-2026-08-09-035619.png)](https://postimg.cc/JtN8rhGN)


**Volume persistence proof: docker volume ls, inserting a test document, deleting the MongoDB container, recreating it, and finding the same document still there**
[![Screenshot-2026-08-09-035836.png](https://i.postimg.cc/NfHfCsfv/Screenshot-2026-08-09-035836.png)](https://postimg.cc/zVqrV1gx)

---

### GitHub Repository

The complete source code and Dockerfile for this task are available on GitHub:
[Click](https://github.com/Suraj-Hulagur/socketio_chat)

---


# **TASK 9: Hashing – Secure Password Storage**

---

### Introduction

For this task I looked into how passwords should actually be stored in a database, and wrote a small Python program to store and verify them securely. I understood why plain text and even plain hashing are not enough, and what salting and slow hashing algorithms actually fix. I implemented once with hashlib and once with argon2.

---

### What I Did

* Learned why storing passwords as plain text is unsafe, and why encryption is not the right choice either since it can be reversed if the key is ever exposed.
* Understood how a hash function works, where the same input always gives the same output, but the output cannot be turned back into the input.
* Read about rainbow tables, which are lookup tables built ahead of time by hashing large lists of common passwords, so a leaked database of hashes can be matched against them instead of being cracked.
* Understood that salting means adding a random value to a password before hashing it, so the same password does not produce the same hash for every user, which makes a rainbow table built without that salt useless.
* Learned that salting alone does not slow down someone trying to guess a password directly against one specific hash, because the hash function itself still computes just as fast.
* Read about why algorithms like argon2 exist, since they are built to be slow and to use more memory on purpose, which makes guessing attempts against them far more expensive than against a plain hash.
* Wrote and ran a hashlib version with a manually generated salt, and an argon2 version using the argon2 cffi library, to compare what each one actually stores.

---

### Hashing, Salting and Slow Hashing

**Hashing** is one way only. We put something in, we get a fixed length scrambled value out, and there is no operation that turns it back.

**Rainbow tables** are precomputed hash lookups built from common passwords or word lists, made once and reused against any hash that shows up in a leak.

**Salting** adds a random value unique to each user before hashing, so two people with the same password end up with different stored hashes, which breaks a rainbow table approach.

**Slow hashing** through argon2 or bcrypt adds cost and memory usage to each individual hash computation, which is what makes guessing attempts against one specific hash slow, something salting by itself does not do.

---

### Implementation

I wrote two scripts using plain functions, no classes, so the logic stays easy to follow.

`hashing_demo.py` enerates a random salt using os.urandom, joins it with the password, hashes the result with SHA256, and stores the salt and hash as two separate values that I have to manage myself.

`argon.py` uses PasswordHasher from argon2 cffi, which creates the salt on its own and does the slow hashing internally, and returns one string with everything bundled inside it.

Looking at the two outputs side by side made the difference clear. The hashlib version gives a salt and a hash that I generated and stored by hand. The argon2 version gives one string that already has the salt and the cost settings built in, so there is nothing extra to manage.

---

### Screenshots

**hashlib**
[![Screenshot-2026-08-08-173648.png](https://i.postimg.cc/jdDZyFK7/Screenshot-2026-08-08-173648.png)](https://postimg.cc/1fZGQM8m)

**argon2**
[![Screenshot-2026-08-08-174419.png](https://i.postimg.cc/CMkGGB6t/Screenshot-2026-08-08-174419.png)](https://postimg.cc/hh4Qgjnb)

---

### GitHub Repository

The complete source code for this task is available on GitHub:
[Click](https://github.com/Suraj-Hulagur/Hashing)



# **TASK 10: NMap – Network Scanning and Host Analysis**

---

### Introduction

For this task I used Nmap inside a Kali Linux virtual machine to scan my own Windows machine and understand what a network scan actually reveals about a target. I bridged the Kali VM onto WiFi so it had its own address separate from Windows, then used Nmap to check if the host was reachable, scan its ports, identify the service running on any open port. I saved the final scan to a file as the task required.

---

### What I Did

* Set the Kali VM network adapter to Bridged mode so it received its own IP address on the same network as my Windows machine, instead of hiding behind it.
* Found both IP addresses using `ip a` on Kali and `ipconfig` on Windows, and confirmed both were on the same subnet.
* Tried a basic `ping` from Kali to the Windows IP and got zero replies, which showed that Windows was silently blocking ICMP by default.
* Used `nmap -Pn` to skip the broken ping check and scan the host directly anyway.
* Ran a service and version detection scan using `-sV` to see what was actually running behind any open port.
* Ran OS detection using `-O` to see what operating system Nmap could infer from the responses.
* Saved the complete scan output to a file using `-oN nmap_report.txt` and confirmed its contents with `cat`.

---

### Commands Used

| Command | What it does |
|---|---|
| `ip a` | Shows Kali's own IP address on the network |
| `ipconfig` | Shows Windows' IP address from the Windows side |
| `ping <ip>` | Checks if the target responds at a basic network level |
| `sudo nmap -Pn <ip>` | Skips host discovery and scans the target directly, needed since ping was blocked |
| `sudo nmap -Pn -sV -O <ip>` | Adds service version detection and OS guessing to the scan |
| `sudo nmap -Pn -sV -O <ip> -oN nmap_report.txt` | Runs the same scan and writes the full output to a text file |

---

Using `-Pn` let the scan continue by treating the host as up without relying on ping.

Out of the top 1000 commonly scanned ports, 999 came back filtered, meaning Windows Firewall was silently dropping the connection attempts instead of actively rejecting them. Only one port responded:

```
8080/tcp open http Apache httpd
```

Service detection identified this as an Apache HTTP server running on port 8080.

OS detection identified the target as Microsoft Windows 11 at 91 percent confidence.

The full scan was saved to `nmap_report.txt` for reference.

---

### Analysis

Almost every port on the target was filtered rather than open, showing the Windows Firewall actively dropping most probes instead of responding to them. The one open port, 8080, revealed an Apache server running on the machine, confirming Nmap's ability to identify not just open ports but the actual services behind them.

OS detection correctly identified the target as Windows 11, showing how Nmap can fingerprint an operating system purely from network response patterns without any direct access to the machine.

---

### Conclusion

This task gave me a practical understanding of how a network scan actually works, from confirming a host is reachable, to scanning its ports, to identifying services and the operating system behind them. Running it against my own two machines made the difference between an open, closed, and filtered port concrete, and demonstrated Nmap's use for network discovery and security auditing.

---

### Screenshots
**Full scan with service and OS detection**
[![Screenshot-2026-08-08-210308.png](https://i.postimg.cc/g2kFVx3z/Screenshot-2026-08-08-210308.png)](https://postimg.cc/xc74YTsh)


**Saved scan output confirmed with cat**
[![Screenshot-2026-08-08-210448.png](https://i.postimg.cc/L68NvL0R/Screenshot-2026-08-08-210448.png)](https://postimg.cc/gr1V2XWB)
---








