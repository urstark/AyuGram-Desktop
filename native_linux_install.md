# AyuGram Native Installation Guide (Linux)

This guide documents the steps to perform a clean, native, non-sandboxed installation of AyuGram Desktop on Linux using the prebuilt Arch Linux binary provided by the CachyOS repository. This method avoids Flatpak sandboxing issues and provides native performance while ensuring you can still update normally.

## Step 1: Prepare the Directory
AyuGram needs to be placed securely inside your local share directory where it has read/write permissions for its own auto-updater and portable data.
```bash
mkdir -p ~/.local/share/AyuGram
```

## Step 2: Download the Native Binary
Download the prebuilt native package from the CachyOS mirror. This is an officially recommended alternative to Flatpak in the AyuGram repository.
```bash
wget -qO /tmp/ayugram.pkg.tar.zst https://mirror.cachyos.org/repo/x86_64/cachyos/ayugram-desktop-7.0.9-1-x86_64.pkg.tar.zst
```

## Step 3: Extract the Package
Extract the `.pkg.tar.zst` file to a temporary directory.
```bash
mkdir -p /tmp/ayugram-pkg
tar -I zstd -xf /tmp/ayugram.pkg.tar.zst -C /tmp/ayugram-pkg
```

## Step 4: Install the Binary
Move the native binary from the extracted temporary folder into your `~/.local/share/AyuGram/` folder, and make sure it is executable.
```bash
cp /tmp/ayugram-pkg/usr/bin/AyuGram ~/.local/share/AyuGram/AyuGram
chmod +x ~/.local/share/AyuGram/AyuGram
```

## Step 5: Create a Desktop Shortcut
To launch AyuGram easily from your application menu, create a `.desktop` shortcut file.
```bash
cat << 'EOF' > ~/.local/share/applications/AyuGram.desktop
[Desktop Entry]
Version=1.0
Name=AyuGram
Comment=AyuGram Native Private Desktop Client
Exec=/home/stark/.local/share/AyuGram/AyuGram -- %u
Icon=telegram
Terminal=false
Type=Application
Categories=Network;InstantMessaging;
MimeType=x-scheme-handler/tg;
Keywords=tg;chat;im;messaging;messenger;sms;ayugram;
StartupWMClass=Telegram
EOF
```

## Step 6: Update Desktop Database
Refresh your desktop environment so the new shortcut appears in your application launcher.
```bash
update-desktop-database ~/.local/share/applications/
```

## Step 7: Logging In & Updating
You can now open **AyuGram** from your applications menu. 
- You will need to log into your accounts manually on a fresh installation.
- Since it is installed in your local user directory, it has full write access to its own folder. When future updates are available, you can usually trigger them from inside the app's settings just like on Windows.
