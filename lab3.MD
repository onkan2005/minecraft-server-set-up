````markdown
# Minecraft Server Setup on Debian Linux with Playit.gg

---

## 1. Update System
Update package lists and upgrade installed packages.
```bash
sudo apt update && sudo apt upgrade -y
````

---

## 2. Install Dependencies

Install Java 17, wget, and screen. Verify Java installation.

```bash
sudo apt install -y openjdk-17-jdk wget screen
java -version
```

---

## 3. Create Server Directory

Create a directory for the Minecraft server and navigate into it.

```bash
mkdir -p ~/minecraft-server
cd ~/minecraft-server
```

---

## 4. Download Minecraft Server

Download the latest Minecraft server `.jar` file. Replace `<LATEST_SERVER_JAR>` with the current URL from Minecraft’s website.

```bash
wget https://launcher.mojang.com/v1/objects/<LATEST_SERVER_JAR>.jar -O server.jar
```

---

## 5. Accept EULA

Create `eula.txt` to accept the Minecraft End User License Agreement.

```bash
echo "eula=true" > eula.txt
```

---

## 6. Start Server in Screen

Start the server in a screen session to run in the background. Adjust RAM allocation with `-Xmx` (max) and `-Xms` (min).

```bash
screen -S minecraft
java -Xmx2G -Xms1G -jar server.jar nogui
```

Detach screen session: `Ctrl+A` then `D`
Reattach screen session: `screen -r minecraft`

---

## 7. Install Playit.gg Client

Download and run Playit.gg client to create a TCP tunnel for external access.

```bash
wget https://playit.gg/download/playit-linux
chmod +x playit-linux
./playit-linux
```

Follow on-screen instructions to log in and create a tunnel for port `25565`. Copy the public address.

---

## 8. Connect to Server

Open Minecraft Java Edition → Multiplayer → Add Server → Enter Playit.gg public address → Join

---

## 9. Autostart Server with Systemd

Create a systemd service file to run the server on boot.

```bash
sudo nano /etc/systemd/system/minecraft.service
```

Paste:

```ini
[Unit]
Description=Minecraft Server
After=network.target

[Service]
User=<username>
WorkingDirectory=/home/<username>/minecraft-server
ExecStart=/usr/bin/java -Xmx2G -Xms1G -jar server.jar nogui
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Enable and start service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable minecraft
sudo systemctl start minecraft
sudo systemctl status minecraft
```

---

## 10. Firewall (if using UFW)

Allow Minecraft port.

```bash
sudo ufw allow 25565/tcp
sudo ufw status
```

Keep Playit.gg client running to maintain the tunnel.

```
```
