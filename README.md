# AyuGram Desktop: Native Linux Installation Guide (Flatpak Sandbox Bypass)

This guide provides a complete, from-scratch native installation path for running **AyuGram Desktop** on modern Linux distributions (such as Linux Mint or Ubuntu) when standard Flatpak installations fail. 

If you use custom desktop themes or X11, Flatpak's sandbox often blocks the active display connection (resulting in the dreaded `qt.qpa.xcb: could not connect to display` error or silent application crashes). Since AyuGram does not directly distribute standalone generic Linux binaries, this method leverages the official packaging assets to build a 100% native environment directly on your host machine.

---

## The Problem
AyuGram developers distribute the Linux client strictly via Flatpak container environments. However, custom user configurations (like macOS-style panel themes or custom X11 layouts) frequently block container environments from accessing active monitor display streams. This guide extracts the core binary tool completely out of the sandbox layer, keeping things beautifully compatible with system theme engines, local premium integrations, and anti-delete trackers.

---

## The Step-by-Step Native Solution

### Step 1: Download the Target Flatpak Package 
Before opening the terminal, ensure you have the required component saved into your primary user directory:
1. Go to the official AyuGram distribution channels.
2. Download the standard compilation Flatpak release target bundle (e.g., `ayugram-desktop-6.7.8.flatpak`) directly into your `~/Downloads` folder.
*(Note: You do not need to download the standalone Telegram base manually; the script below pulls down the core runtime environment automatically).*

### Step 2: Run the Automated Extraction Sequence
Open your terminal inside your installation folder (`cd ~/Downloads`) and run this unified script block to lay down the dependencies, clone files, extract branding materials, and clear old app drawer records:

```bash
# 1. Install the Flatpak bundle target system-wide to register build components
sudo flatpak install ayugram-desktop-*.flatpak -y

# 2. Pull down the official Linux standalone architecture structure from Telegram servers
wget -O tsetup.tar.xz [https://telegram.org/dl/desktop/linux](https://telegram.org/dl/desktop/linux)

# 3. Unpack the clean background support architecture directory locally
tar -xf tsetup.tar.xz -C ~/.local/share/

# 4. Extract the raw binary payload directly out of the Flatpak file system structure
sudo cp -f /var/lib/flatpak/app/com.ayugram.desktop/x86_64/master/active/files/bin/ayugram-desktop ~/.local/share/Telegram/Telegram
sudo chown $USER:$USER ~/.local/share/Telegram/Telegram
chmod +x ~/.local/share/Telegram/Telegram

# 5. Extract the high-res authentic application icon asset directly from the internal files
mkdir -p ~/.local/share/icons
cp -f /var/lib/flatpak/app/com.ayugram.desktop/x86_64/master/active/export/share/icons/hicolor/512x512/apps/com.ayugram.desktop.png ~/.local/share/icons/ayugram-logo.png 2>/dev/null || cp -f /var/lib/flatpak/app/com.ayugram.desktop/x86_64/master/active/files/share/icons/hicolor/512x512/apps/com.ayugram.desktop.png ~/.local/share/icons/ayugram-logo.png

# 6. Safely remove the Flatpak container so it can't create duplicate ghost entries
sudo flatpak uninstall com.ayugram.desktop -y
```

---

## Create the Clean System App Launcher

To force your Linux desktop environment to map window assignments to your shortcuts securely (preventing running apps from changing into fallback gear icons on your dock panels), construct your launcher file with a custom window identifier tracking token (`StartupWMClass`):

```bash
cat << 'EOF' > ~/.local/share/applications/AyuGram.desktop
[Desktop Entry]
Version=1.0
Name=AyuGram Desktop
Comment=AyuGram Native Private Desktop Client
Exec=/home/stark/.local/share/Telegram/Telegram -- %u
Icon=/home/stark/.local/share/icons/ayugram-logo.png
Terminal=false
Type=Application
Categories=Network;InstantMessaging;
MimeType=x-scheme-handler/tg;
Keywords=tg;chat;im;messaging;messenger;sms;ayugram;
StartupWMClass=Telegram
EOF
```

---

## Rebuilding the Desktop System Layout

Execute this final structural block to authorize layout execution and clear out lingering visual window cache layers:

```bash
# Authorize the desktop shortcut profile to execute natively
chmod +x ~/.local/share/applications/AyuGram.desktop

# Force the system configuration layer database to synchronize 
update-desktop-database ~/.local/share/applications/

# Restart your desktop layout workspace manager panel to lock inside the new icons
killall cinnamon --restart 2>/dev/null || killall gnome-shell -r 2>/dev/null
```

---

## Bonus: Applying Custom Anime/Chibi Icons
If you want to swap the default purple icon for one of the custom icons (like the Chibi anime characters) packed inside the app's source code, simply run one of these blocks in your terminal after finishing the main installation:

**Chibi 2 (Anime Girl Face):**
```bash
wget -qO ~/.local/share/icons/ayugram-custom.png [https://raw.githubusercontent.com/AyuGram/AyuGramDesktop/dev/Telegram/Resources/art/ayu/chibi2/app.png](https://raw.githubusercontent.com/AyuGram/AyuGramDesktop/dev/Telegram/Resources/art/ayu/chibi2/app.png)
sed -i 's|^Icon=.*|Icon=/home/stark/.local/share/icons/ayugram-custom.png|' ~/.local/share/applications/AyuGram.desktop
update-desktop-database ~/.local/share/applications/
killall cinnamon --restart 2>/dev/null || killall gnome-shell -r 2>/dev/null
```

**Chibi 1 (Girl Hanging on Logo):**
```bash
wget -qO ~/.local/share/icons/ayugram-custom.png [https://raw.githubusercontent.com/AyuGram/AyuGramDesktop/dev/Telegram/Resources/art/ayu/chibi/app.png](https://raw.githubusercontent.com/AyuGram/AyuGramDesktop/dev/Telegram/Resources/art/ayu/chibi/app.png)
sed -i 's|^Icon=.*|Icon=/home/stark/.local/share/icons/ayugram-custom.png|' ~/.local/share/applications/AyuGram.desktop
update-desktop-database ~/.local/share/applications/
killall cinnamon --restart 2>/dev/null || killall gnome-shell -r 2>/dev/null
```

---

## 🎉 Done!
Your application menu will display a single, perfectly integrated **AyuGram Desktop** icon. It will track smoothly from your app drawer straight into your running application dock with perfect window decoration layouts and optimal, un-sandboxed speed!
