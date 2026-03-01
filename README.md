Got it — you want that **clean activity-guide style format**, but for:

👉 **PaperMC + Debian + Playit.gg**

Here’s your GitHub-ready README written exactly in that structured instructional format.

---

````markdown
# Debian PaperMC Server Setup Guide (With Playit.gg)

This guide provides step-by-step instructions for installing and configuring a PaperMC Minecraft server on Debian Linux. It also includes Playit.gg setup for public access without port forwarding.

---

## 1. Update the System

```bash
sudo apt update && sudo apt upgrade -y
````

**Explanation:**
Ensures your Debian system is fully updated before installing dependencies.

---

## 2. Install Java (Required)

PaperMC requires Java 17 or newer.

```bash
sudo apt install openjdk-17-jre-headless -y
java -version
```

**Explanation:**
The `java -version` command confirms Java 17+ is installed and ready.

---

## 3. Create Server Directory

```bash
mkdir ~/paper-server
cd ~/paper-server
```

**Explanation:**
Creates a dedicated folder for your Minecraft server files.

---

## 4. Download PaperMC Server

Download the latest Paper version:

```bash
wget https://api.papermc.io/v2/projects/paper/versions/1.20.4/builds/XXX/downloads/paper-1.20.4-XXX.jar -O paper.jar
```

Replace `XXX` with the latest build number from:
[https://papermc.io/downloads](https://papermc.io/downloads)

**Explanation:**
Downloads the PaperMC server JAR file and renames it to `paper.jar` for easier execution.

---

## 5. First Run and Accept EULA

Start the server:

```bash
java -Xms1G -Xmx2G -jar paper.jar --nogui
```

It will stop automatically and generate files.

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

Start again:

```bash
java -Xms1G -Xmx2G -jar paper.jar --nogui
```

**Explanation:**
Minecraft requires accepting the EULA before the server can run.

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

Make executable:

```bash
chmod +x start.sh
```

Run:

```bash
./start.sh
```

**Explanation:**
This script simplifies starting the server and allows easy RAM adjustment.

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

**Setup Steps:**

1. Open the URL displayed in the terminal.
2. Log in.
3. Copy the tunnel key.
4. Paste it into the terminal.

**Explanation:**
Playit.gg creates a secure public tunnel to your local server without needing router configuration.

---

## 8. Create a Minecraft Tunnel

When prompted:

* Create new tunnel → YES
* Protocol → TCP
* Local Port → 25565
* Name → minecraft

You will receive an address like:

```
yourserver.playit.cloud:12345
```

**Explanation:**
Players connect to this public address instead of your local IP.

---

## 9. Run Server and Playit Together

Create combined script:

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

Start everything:

```bash
./run-all.sh
```

**Explanation:**
This script launches Playit in the background and then starts the Minecraft server.

---

## 10. Run in Background Using Screen (Optional)

Install screen:

```bash
sudo apt install screen -y
```

Start session:

```bash
screen -S mcserver
./run-all.sh
```

Detach session:

```
CTRL + A, then D
```

Reattach:

```bash
screen -r mcserver
```

**Explanation:**
Allows the server to keep running after you close the terminal.

---

## 11. Auto Start on Boot (Optional)

Create systemd service:

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

Enable service:

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
Automatically starts the server when Debian boots.

---

## 12. Updating PaperMC

1. Stop the server
2. Replace `paper.jar` with the new version
3. Start the server again

**Explanation:**
World data and configurations remain intact during updates.

---

# Final Result

You now have:

* PaperMC running on Debian
* Public access via Playit.gg
* Optional background running
* Optional automatic startup

Players connect using the Playit-generated address.

```

---

If you want, I can now:

- Make a **cleaner academic-style version** (like your Debian activity)
- Or a **more professional GitHub project layout with table of contents and badges**
```
