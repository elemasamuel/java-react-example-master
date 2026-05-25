# Module 5 - Cloud & Infrastructure as a Service (IaaS)

## What I built and did

### 1. Created a DigitalOcean Droplet

Provisioned a Linux Ubuntu virtual machine (Droplet) on DigitalOcean to use as a remote server.

<img width="1649" height="626" alt="image" src="https://github.com/user-attachments/assets/dd77dbbf-c73c-4e3b-938b-77b5846790cc" />


### 2. Configured a firewall

Set up a firewall on the Droplet with inbound and outbound rules. By default DigitalOcean servers are not protected, so I explicitly opened the ports needed - SSH on port 22, and a custom TCP rule on port 7071 to expose the app.

<img width="1634" height="560" alt="image" src="https://github.com/user-attachments/assets/180049cb-33d7-4df3-a778-c48312a3108c" />


### 3. Set up SSH access

Retrieved my local public SSH key and added it to DigitalOcean so I could connect to the server securely:

```bash
cat .ssh/id_rsa.pub
ssh root@<droplet-ip-address>
```

### 4. Installed Java on the server

```bash
apt update
apt install openjdk-8-jre-headless
```

### 5. Built the JAR file locally

```bash
gradle build
```

### 6. Uploaded the artifact to the Droplet

Copied the built JAR from my local machine to the remote server using SCP:

```bash
scp build/libs/java-react-example-master.jar root@68.183.35.218:/root
```

### 7. Ran the application on the remote server

```bash
# Run normally
java -jar java-react-example-master.jar

# Run in detached (background) mode
java -jar java-react-example-master.jar &
```

<img width="1899" height="860" alt="image" src="https://github.com/user-attachments/assets/19455a25-91c7-4889-86c1-abff34f97421" />


Then accessed it in the browser at `68.183.35.218:7071`.

<img width="1889" height="965" alt="image" src="https://github.com/user-attachments/assets/04d8b7b7-4155-41cb-aa07-eca382e2ab16" />


### 8. Checked the running process and active connections

```bash
# Check if app is still running in background
ps aux | grep java

# List servers with active connections
netstat -lpnt
```

<img width="1461" height="85" alt="image" src="https://github.com/user-attachments/assets/caf0fbe4-ae85-4b1e-8b30-aebfd1ab7092" />


### 9. Created a non-root user for security

```bash
# Create new user
adduser samuel

# Add to sudo group
usermod -aG sudo samuel

# Switch to the new user
su - samuel
```

Then set up SSH access for the new user by creating a `.ssh/authorized_keys` file and pasting in the public key, so I could SSH in directly as that user:

```bash
ssh samuel@<droplet-ip-address>
```

---

## Key concepts covered

- Cloud Computing - delivery of computing services over the internet
- IaaS (Infrastructure as a Service) - renting servers on demand instead of buying hardware
- Firewall configuration - inbound and outbound rules, opening specific ports
- SSH key-based authentication
- SCP - secure file transfer to a remote server
- Running and managing a Java process on a remote Linux server
- Linux user management and the principle of least privilege (avoid working as root)
- Packaging frontend code with webpack, managing dependencies with npm/yarn, bundling into a WAR file

---

## Tools used

| Tool | Purpose |
|------|---------|
| DigitalOcean | IaaS cloud provider |
| Ubuntu (Linux) | Server OS on the Droplet |
| SSH | Secure remote access |
| SCP | File transfer to remote server |
| Java / JRE | Runtime for the deployed application |
| Gradle | Build tool to package the JAR |
| Firewall (DO) | Network security - port access control |
