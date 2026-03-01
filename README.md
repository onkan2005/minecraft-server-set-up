````markdown
# Debian PaperMC Server Setup Guide (With Playit.gg)

This guide provides step-by-step instructions for installing and configuring a PaperMC Minecraft server on Debian Linux. It also includes Playit.gg setup for public access without port forwarding.

---

## 1. Update the System

```bash
sudo apt update && sudo apt upgrade -y
````

**Explanation:**
Updates all installed packages to the latest versions to ensure system stability and security.

---

## 2. Install Java (Required)

PaperMC requires Java 17 or newer.

```bash
sudo apt install openjdk-17-jre-headless -y
java -version
```

**Explanation:**
Installs Java runtime and verifies that Java 17+ is properly installed.

---

## 3. Create Server Directory

```bash
mkdir ~/paper-server
cd ~/paper-server
```

**Explanation:**
Creates a dedicated directory to store server files and configurations.

---

## 4. Download PaperMC Server

Download the latest Paper build:

```bash
wget https://api.papermc.io/v2/projects/paper/versions/1.20.4/builds/XXX/downloads/paper-1.20.4-XXX.jar -O paper.jar
```

Replace `XXX` with the latest build number from [https://papermc.io/downloads](https://papermc.io/downloads)

**Explanation:**
Downloads the Paper server file and renames it to `paper.jar` for easier execution.

---

## 5. First Run and Accept EULA

Start the server:

```bash
java -Xms1G -Xmx2G -jar paper.jar --nogui
```

The server will stop automatically after generating configuration files.

Edit the EULA file:

```bash
nano eula.txt
```

Change:

```
eula=false
```

To:

```
eula=true
```

Start the server again:

```bash
java -Xms1G -Xmx2G -jar paper.jar --nogui
```

**Explanation:**
Accepting the EULA is required before the Minecraft server can run.

---

## 6. Create a Start Script

```bash
nano start.sh
```

Add:

```bash
#!/bin/bash
java -Xms2G -Xmx4G -jar paper.jar --nogui
```

Make it executable:

```bash
chmod +x start.sh
```

Run the server:

```bash
./start.sh
```

**Explanation:**
This script simplifies server startup and allows easy RAM configuration.

---

## 7. Install Playit.gg (No Port Forwarding Required)

Download Playit client:

```bash
wget https://playit.gg/downloads/cli/playit-linux
chmod +x playit-linux
```

Run Playit:

```bash
./playit-linux
```

Setup steps:

1. Open the URL shown in the terminal.
2. Log in to your account.
3. Copy the tunnel key.
4. Paste it back into the terminal.

**Explanation:**
Playit.gg creates a secure public tunnel to your local Minecraft server without router configuration.

---

## 8. Create a Minecraft Tunnel

When prompted:

* Create new tunnel → YES
* Protocol → TCP
* Local Port → 25565
* Name → minecraft

You will receive an address similar to:

```
yourserver.playit.cloud:12345
```

**Explanation:**
Players will connect to this public address instead of your local IP address.

---

## 9. Run Server and Playit Together

Create a combined script:

```bash
nano run-all.sh
```

Add:

```bash
#!/bin/bash

./playit-linux &
sleep 10
./start.sh
```

Make executable:

```bash
chmod +x run-all.sh
```

Start both services:

```bash
./run-all.sh
```

**Explanation:**
Starts Playit in the background and then launches the Minecraft server.

---

## 10. Run in Background Using Screen (Optional)

Install screen:

```bash
sudo apt install screen -y
```

Start a session:

```bash
screen -S mcserver
./run-all.sh
```

Detach from session:

```
CTRL + A, then D
```

Reattach later:

```bash
screen -r mcserver
```

**Explanation:**
Allows the server to continue running even after closing the terminal.

---

## 11. Auto Start on Boot (Optional)

Create a systemd service:

```bash
sudo nano /etc/systemd/system/minecraft.service
```

Add:

```ini
[Unit]
Description=Paper Minecraft Server
After=network.target

[Service]
User=YOUR_USERNAME
WorkingDirectory=/home/YOUR_USERNAME/paper-server
ExecStart=/home/YOUR_USERNAME/paper-server/run-all.sh
Restart=always

[Install]
WantedBy=multi-user.target
```

Replace `YOUR_USERNAME` with your Debian username.

Enable and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable minecraft
sudo systemctl start minecraft
```

Check status:

```bash
sudo systemctl status minecraft
```

**Explanation:**
Ensures the Minecraft server automatically starts when the system boots.

---

## 12. Updating PaperMC

1. Stop the server
2. Replace `paper.jar` with the new version
3. Start the server again

**Explanation:**
World data and configurations remain intact during updates.

```
```
