# Create `.sh` File to Run Minecraft Server + playit.gg

---

## 1. Go to Your Minecraft Folder

```bash
cd ~/minecraft
```

---

## 2. Create Script File

```bash
nano start.sh
```

---

## 3. Paste This Inside

```bash
#!/bin/bash

# Start Minecraft Server in background
screen -dmS mcserver java -Xmx2G -Xms2G -jar server.jar nogui

# Wait a few seconds before starting playit
sleep 5

# Start playit
playit
```

Replace `server.jar` if your jar has different name.

Save:

```
CTRL + X
Y
ENTER
```

---

## 4. Make Script Executable

```bash
chmod +x start.sh
```

---

## 5. Run Script

```bash
./start.sh
```

---

# How It Works

* Minecraft runs inside `screen`
* playit runs in current terminal
* Server keeps running even if you detach screen

---

## To Reopen Minecraft Console

```bash
screen -r mcserver
```
