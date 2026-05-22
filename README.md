# 🚀 AyuGram Desktop: Native Linux Installation Guide (Flatpak Sandbox Bypass)

This guide provides a complete, from-scratch native installation path for running **AyuGram Desktop** on modern Linux distributions (such as Linux Mint or Ubuntu) when standard Flatpak installations fail. 

If you use custom desktop themes or X11, Flatpak's sandbox often blocks the display connection (resulting in the dreaded `qt.qpa.xcb: could not connect to display` error or silent crashes). Since AyuGram does not directly distribute generic Linux binaries, this method uses the official Telegram base as a structural shell and injects the AyuGram compiled binary inside it, running it 100% natively on your host system.

---

## 🛑 The Problem
AyuGram developers distribute the Linux client strictly via Flatpak bundles. However, custom user configurations (like macOS-style layouts) frequently block the sandbox from accessing the active monitor display. This guide skips the sandbox entirely while keeping the app fully functional with Ghost Mode, anti-delete trackers, and custom themes intact.

---

## 🛠️ The Step-by-Step Native Solution

### Step 1: Download Both Required Components
Before touching the terminal, we need the AyuGram payload and the official Telegram structural base.
1. **Download AyuGram:** Go to the official AyuGram GitHub Releases page and download the Linux Flatpak bundle (e.g., `ayugram-desktop-6.7.8.flatpak`) into your **Downloads** folder.
2. **Download Telegram Base:** Go to the official Telegram website (`desktop.telegram.org`) and download the generic "Telegram for Linux x64" archive (`tsetup.tar.xz`) into your **Downloads** folder.

### Step 2: Install the AyuGram Payload (Extraction Phase)
Open your terminal. We will install the downloaded Flatpak bundle system-wide. This forces your machine to unpack the compiled AyuGram binary to a known location on your drive so we can extract it.

```bash
# Navigate to your download folder
cd ~/Downloads

# Install the flatpak target package system-wide (press Y when prompted)
sudo flatpak install ayugram-desktop-*.flatpak -y
```

### Step 3: Setup the Native Telegram Scaffolding
Next, we will extract the official Telegram base into your hidden user applications directory. This provides the correct environment for AyuGram to hook into.

```bash
# Extract the official Telegram structure directly into your local applications folder
tar -xf ~/Downloads/tsetup*.tar.xz -C ~/.local/share/
```
*(This automatically creates a folder at `~/.local/share/Telegram` with the base files).*

### Step 4: The Engine Swap (The Magic Trick)
Now we pull the raw compiled AyuGram executable straight out of the Flatpak prison and overwrite the stock Telegram executable.

```bash
# Force-copy the AyuGram binary over the stock Telegram executable
sudo cp -f /var/lib/flatpak/app/com.ayugram.desktop/x86_64/master/active/files/bin/ayugram-desktop ~/.local/share/Telegram/Telegram

# Grant the new binary file native host execution security privileges
chmod +x ~/.local/share/Telegram/Telegram
```

### Step 5: Completely Scrub Stale Icon Records
Installing via Flatpak creates broken application shortcuts that will clutter your system menu and refuse to open. Run this block to cleanly purge all ghost icons:

```bash
# Erase user-space ghost menu shortcuts
rm -f ~/.local/share/applications/telegram*.desktop
rm -f ~/.local/share/applications/ayugram*.desktop

# Erase system-wide template duplicates
sudo rm -f /usr/share/applications/telegram*.desktop
sudo rm -f /usr/share/applications/ayugram*.desktop
```

---

## 🖥️ Create the Unified System App Launcher

To integrate the application perfectly into your system's app drawer, generate a clean, custom desktop entry file manually:

```bash
nano ~/.local/share/applications/AyuGram.desktop
```

Copy and paste this exact configuration block into the text editor:

```ini
[Desktop Entry]
Version=1.0
Name=AyuGram Desktop
Comment=AyuGram Native Private Desktop Client
Exec=/home/stark/.local/share/Telegram/Telegram -- %u
Icon=telegram
Terminal=false
Type=Application
Categories=Network;InstantMessaging;
MimeType=x-scheme-handler/tg;
Keywords=tg;chat;im;messaging;messenger;sms;ayugram;
```
*(Press `Ctrl + O` then `Enter` to save, and `Ctrl + X` to exit).*

---

## 🔄 Finalizing and Refreshing the System

Execute this final string of commands to apply the execution permissions, clear out stale system icon memory, and rebuild the application database:

```bash
# Authorize the desktop shortcut profile to execute
chmod +x ~/.local/share/applications/AyuGram.desktop

# Force the system menu database to synchronize the new launcher
update-desktop-database ~/.local/share/applications/

# Restart your desktop window layout manager panel to complete the cache flush
# (Your screen/taskbar will blink for one second)
killall cinnamon --restart 2>/dev/null || killall gnome-shell -r 2>/dev/null
```

---

### Step 6: Cleanup the Flatpak Container (Optional)
Since we successfully extracted the native binary, you no longer need the heavy Flatpak container taking up gigabytes of space on your drive. You can safely uninstall it:

```bash
sudo flatpak uninstall com.ayugram.desktop -y
```

---


## 🎉 Done!
Open your application menu! The broken duplicate icons will be completely gone, leaving exactly one perfectly integrated **AyuGram Desktop** icon. It will open instantly with native window decorations, full theme support, Ghost Mode, and anti-delete trackers fully active!
