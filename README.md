# 🚀 AyuGram Desktop: Native Linux Installation Guide (Flatpak Sandbox Bypass)

This guide provides a clean, native installation path for running **AyuGram Desktop** on modern Linux distributions (such as Linux Mint or Ubuntu) when standard Flatpak installations fail. 

If you use custom desktop themes or X11, Flatpak's sandbox often blocks the display connection (resulting in the dreaded `qt.qpa.xcb: could not connect to display` error or silent crashes). Since AyuGram does not provide generic Linux binaries on their release page, this method extracts the compiled binary out of the Flatpak isolation layers and runs it natively on your host system.

---

## 🛑 The Problem
AyuGram developers distribute the Linux client strictly via Flatpak. However, custom user configurations (like macOS-style layouts) frequently block the sandbox from accessing the active monitor display. This guide skips the sandbox entirely while keeping the app fully functional with Ghost Mode and custom themes intact.

---

## 🛠️ The Step-by-Step Native Solution

### Step 1: Install the Raw Package Core
First, download the target `.flatpak` bundle from the community releases and install it system-wide. This forces your machine to extract the compiled binary assets to a known location:

```bash
# Navigate to your download folder (assuming the flatpak file is here)
cd ~/Downloads

# Install the flatpak target package system-wide
sudo flatpak install ayugram-desktop-*.flatpak -y
```

### Step 2: Extract the Binary to a Native Local Shell
Instead of running it through Flatpak's isolated runtime container, we will create a secure path directly inside your home directory and pull the raw executable out:

```bash
# Create a dedicated native application storage folder
mkdir -p ~/.local/share/Telegram

# Force-copy the raw compiled binary directly out of the Flatpak system partition
sudo cp -f /var/lib/flatpak/app/com.ayugram.desktop/x86_64/master/active/files/bin/ayugram-desktop ~/.local/share/Telegram/Telegram

# Grant the new binary file native host execution security privileges
chmod +x ~/.local/share/Telegram/Telegram
```

### Step 3: Completely Scrub Stale Icon Records
Installing via Flatpak creates broken application shortcuts that will clutter your system menu and refuse to open. Run this block to cleanly purge all the ghost icons:

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

## 🎉 Done!
Open your application menu! The broken duplicate icons will be completely gone, leaving exactly one perfectly integrated **AyuGram Desktop** icon. It will open instantly with native window decorations, full theme support, Ghost Mode, and anti-delete trackers fully active!
