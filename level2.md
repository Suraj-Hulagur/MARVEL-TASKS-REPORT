# TASK 1: AWS Lambda – Serverless Chat App

---

### Introduction

For this task I deployed a real-time chat app using AWS Lambda and API Gateway's WebSocket support instead of a traditional server. Messages are handled through a single Lambda function, with connections tracked in DynamoDB so the app can broadcast to everyone connected.

---

### Architecture

- **DynamoDB table** (`ChatConnections`) – stores active connection IDs
- **Lambda function** (`chat-handler`, Node.js) – handles all events through a single function using the WebSocket route key
- **API Gateway WebSocket API** – routes: `$connect`, `$disconnect`, `$default`, all pointed at the same Lambda

When a client connects, its connection ID gets stored in DynamoDB. When a message comes in, the Lambda scans the table and pushes that message out to every stored connection. When a client disconnects, its ID gets removed.

---

### Setup

Created the Lambda function, gave its execution role DynamoDB access plus a custom policy for `execute-api:ManageConnections` (needed to push messages back to clients), then built the WebSocket API in API Gateway with the three routes wired to the Lambda, and deployed it to a `production` stage.

---

### Testing

Connected two separate WebSocket clients to the deployed endpoint using `wscat`. Sent a message from one and confirmed it was broadcast and received on the other, verifying the connect, broadcast, and fan-out logic all worked correctly end to end.

---

### Results

Serverless WebSocket chat app deployed and verified working, with two independent clients exchanging a real-time message through Lambda and API Gateway with no server to manage.

---

### Screenshots

**Lambda function code**
[![Whats-App-Image-2026-10-03-at-11-48-15.jpg](https://i.postimg.cc/HkTXc7kp/Whats-App-Image-2026-10-03-at-11-48-15.jpg)](https://postimg.cc/QHnHvC7P)

**IAM role permissions for the Lambda function**
[![Whats-App-Image-2026-10-03-at-11-35-37.jpg](https://i.postimg.cc/s2NhN69x/Whats-App-Image-2026-10-03-at-11-35-37.jpg)](https://postimg.cc/KKtjK5Wh)

**API Gateway routes wired to the Lambda integration**
[![Whats-App-Image-2026-10-03-at-11-37-57.jpg](https://i.postimg.cc/gj96Yx7P/Whats-App-Image-2026-10-03-at-11-37-57.jpg)](https://postimg.cc/LYTsD8Ry)

**Two WebSocket clients connected, message broadcast and received**
[![Whats-App-Image-2026-10-03-at-11-44-53.jpg](https://i.postimg.cc/kXmtzt8G/Whats-App-Image-2026-10-03-at-11-44-53.jpg)](https://postimg.cc/rzfmtsf2)
[![Whats-App-Image-2026-10-03-at-11-44-55.jpg](https://i.postimg.cc/rFkt3tWf/Whats-App-Image-2026-10-03-at-11-44-55.jpg)](https://postimg.cc/pysTnrsj)

---


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
# **TASK 3: SSH – Scripted Key Discovery and Transfer Between Servers**

---

### Introduction

For this task I wrote a shell script that logs into a server over SSH, searches it for private and public key files, and transfers whatever it finds to a second server. To do this safely without touching any real machine, I set up two Docker containers to act as two independent servers, connected them on their own network, and used those as the source and destination for the script.

---

### Setup

I created a Docker network so the two containers could reach each other, then started two plain Ubuntu containers on it, naming them serverA and serverB. SSH doesn't come installed on a base Ubuntu image, so I installed and started the SSH server on both containers separately, then set a root password on each one so the script would have something to authenticate with.

I ran into one snag here. Ubuntu's default SSH configuration blocks root login over a password by default, so my first attempt at running the script failed with a permission denied error even though the password was correct. I fixed this by editing sshd_config on both containers to explicitly allow root login and password authentication, then restarted SSH on each before trying again.

To have something realistic for the script to find, I generated a test RSA key pair inside serverA using ssh-keygen, which created a private and a public key file in a keys folder.

---

### The Script

The script takes a source host and a destination host, SSHes into the source and runs a find command across the entire filesystem looking for anything matching common key file patterns, private keys, PEM files, and public keys. If it finds anything, it loops through each result, pulls the file down locally using scp, then pushes it back up to the destination server, cleaning up the temporary local copy each time. I used sshpass so the script could supply the password automatically instead of needing someone to type it in during each connection.

---

### Results

Running the script against the two containers, it connected to serverA and found five key files, three SSH host keys that get generated automatically whenever SSH is installed, plus the private and public key I had generated for testing. All five were transferred one by one to serverB. Checking the destination folder afterward showed all five files present with matching sizes, confirming the whole flow worked end to end, logging in, searching, and uploading, without needing any manual steps once the script was run.

---

### Screenshots

**Docker network and containers created, SSH installed on both servers**

[![Screenshot-2026-08-09-232249.png](https://i.postimg.cc/T27YBcwB/Screenshot-2026-08-09-232249.png)](https://postimg.cc/dDCYyrG9)

**IP addresses of both containers on the custom network**

[![Screenshot-2026-08-09-233232.png](https://i.postimg.cc/KzcxHKyY/Screenshot-2026-08-09-233232.png)](https://postimg.cc/GBfWBmt0)



**Script running successfully: connecting, finding five key files, and transferring all of them, followed by confirmation on the destination server**

[![Screenshot-2026-08-09-234020.png](https://i.postimg.cc/1tSrhQWc/Screenshot-2026-08-09-234020.png)](https://postimg.cc/Lqy1LrFn)



---
# **TASK 4: Terraform**

---

### Introduction

For this task I used Terraform to set up AWS stuff using code instead of clicking around in the console every time. You write what you want in a file and it builds it for you, and can change or delete it the same way. I started small with one EC2 instance to get the flow down, then built out a proper network around it.

---

### What I Did

* Installed Terraform and AWS CLI, and set up a dedicated IAM user for Terraform to use instead of root.
* Wrote a `main.tf` for a single EC2 instance and ran through `init`, `validate`, `plan`, `apply`, and `destroy` to get the full workflow down.
* Built out a real network around it next, a VPC, a subnet inside it, an internet gateway, a route table, and a security group opening SSH, HTTP and HTTPS, with the instance running Apache through a user_data script.
* The instance took a while to attach when I first set it up through a separate network interface resource, switched to attaching the subnet and security group directly on the instance instead, which brought the create time down to about 12 seconds.
* Changed the instance's name tag and reapplied to see Terraform update it in place.
* Used `terraform output` and `terraform state list` to check what was actually running, then destroyed everything and confirmed the state was empty.

---

### Terraform Commands

| Command | What it does |
|---|---|
| `terraform init` | Sets up the folder and downloads the AWS provider |
| `terraform validate` | Checks if the config file is valid |
| `terraform plan` | Shows what's about to happen before it happens |
| `terraform apply` | Actually creates/changes the stuff on AWS |
| `terraform output` | Prints out values like instance id after apply |
| `terraform state list` | Shows what resources Terraform is tracking right now |
| `terraform destroy` | Deletes everything Terraform made |

---

### The Network Setup

Built a VPC for the private network space, a subnet inside it for the instance to live in, an internet gateway so it could reach the internet, a route table pointing traffic to that gateway, and a security group for the right ports. All of it came up in under 15 seconds.

---

### Results

Got the full Terraform flow working end to end, single instance first, then a complete network around it, modified a live resource and watched Terraform update it correctly, and destroyed everything cleanly with `state list` coming back empty at the end.

---

### Conclusion

Writing the Terraform file itself was the easy part, the more useful bit was getting comfortable with `plan` before every `apply`, checking `state list` and `output` to see what's actually live, and using `destroy` to clean up properly instead of deleting things by hand in the console.

---

### Screenshots

**terraform init and validate**
[![Whats-App-Image-2026-10-03-at-08-55-29.jpg](https://i.postimg.cc/W1zhrmgm/Whats-App-Image-2026-10-03-at-08-55-29.jpg)](https://postimg.cc/WdRNck9t)

**terraform plan showing the instance to be created**
[![Whats-App-Image-2026-10-03-at-08-56-49.jpg](https://i.postimg.cc/L5mssjmz/Whats-App-Image-2026-10-03-at-08-56-49.jpg)](https://postimg.cc/JDd86DDn)

**Network setup in the config file**
[![Whats-App-Image-2026-10-03-at-09-14-51.jpg](https://i.postimg.cc/rsy8Tc9n/Whats-App-Image-2026-10-03-at-09-14-51.jpg)](https://postimg.cc/5YGc5hTL)

**terraform apply creating the instance**
[![Whats-App-Image-2026-10-03-at-09-00-30.jpg](https://i.postimg.cc/d1wFRvpf/Whats-App-Image-2026-10-03-at-09-00-30.jpg)](https://postimg.cc/LqCG2dHk)

**terraform destroy and empty state list after**
[![previdsfaew.webp](https://i.postimg.cc/ryhT4Bj2/previdsfaew.webp)](https://postimg.cc/s1Zbd0bT)

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

# **TASK 8: Web Scraping and Automation – Flight Ticket Price Analysis**

---

### Introduction

For this task I built a script that automatically searches for flights on Kayak, scrapes the prices shown on the page, saves them into a CSV file, and emails that file out. I used Selenium to control a real Chrome browser and pull data off a page that loads everything dynamically through JavaScript, rather than a static page you could just download and parse directly. I searched for flights from Bengaluru to Los Angeles for this run.

---

### What I Did

* Set up Selenium with ChromeDriver, using webdriver manager so the correct driver version gets downloaded automatically instead of managing it by hand.
* Pointed the script at a direct Kayak search results URL for a Bengaluru to Los Angeles flight instead of trying to fill in the search form through Selenium, since Kayak's form is heavy JavaScript and more fragile to automate reliably.
* Wrote the results into a CSV file with the route and the scraped price text.
* Set up a Gmail App Password, sending mail through smtplib, and used that to send the CSV as an email attachment once scraping finished.

---

### Selenium and Why It Was Needed

Selenium is a browser automation tool that controls a real browser rather than just downloading a page's raw HTML. This matters for a site like Kayak, where the flight results are not present in the initial page source at all and only appear after JavaScript runs and fetches the data separately. A simple request to the page would return an almost empty shell, so Selenium's ability to actually load the page like a real browser and wait for content to render was necessary to get anything useful out of it.

---

### Bot Detection Challenge
 
The most significant obstacle in this task was not the scraping logic itself but getting past Kayak's bot detection. Running Chrome in headless mode, the default and most efficient way to run Selenium, triggered Kayak's detection immediately and returned a page explaining that it believed the request was coming from a bot rather than a person. Running a full, visible instance of Chrome instead, along with hiding a few of the more obvious signs that the browser was being automated, was enough to get past this and load the real search results consistently.

### Results

The script successfully loaded real Kayak search results for Bengaluru to Los Angeles and captured genuine prices ranging from around 554 dollars up to over 1000 dollars, along with details like Economy Basic fare labels that were picked up alongside the prices. Unique price entries were saved to a CSV file, and the script then successfully sent that CSV as an email attachment using Gmail's SMTP server.

---

### Screenshots

**Kayak search results loading successfully after switching to a visible browser and hiding automation signals**
[![Screenshot-2026-08-10-072659.png](https://i.postimg.cc/wMr6P43h/Screenshot-2026-08-10-072659.png)](https://postimg.cc/LgzKgTtX)

**Terminal output showing scraped prices, the CSV being saved, and the email being sent successfully**
[![Screenshot-2026-08-10-073550.png](https://i.postimg.cc/vZrsLZ7v/Screenshot-2026-08-10-073550.png)](https://postimg.cc/21SMCmKq)

**Final CSV file opened in Excel, showing the saved route and price data**

[![Screenshot-2026-08-10-073649.png](https://i.postimg.cc/P5Wjz5zH/Screenshot-2026-08-10-073649.png)](https://postimg.cc/87z9TD0X)

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










