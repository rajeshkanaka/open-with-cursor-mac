<div align="center">

# 🚀 Open with Cursor - macOS Finder Integration

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![macOS](https://img.shields.io/badge/macOS-000000?style=flat&logo=apple&logoColor=white)](https://www.apple.com/macos)
[![Cursor](https://img.shields.io/badge/Cursor-IDE-blue)](https://cursor.sh)

<img src="cursorlogo.png" width="120" alt="Cursor Logo">

*Seamlessly integrate Cursor IDE into your macOS Finder for an enhanced development workflow*

[Installation](#-installation) • [Features](#-features) • [Usage](#-usage) • [Troubleshooting](#-troubleshooting)

</div>

---

## ✨ Features

- 🖱️ Right-click integration in Finder context menu
- 📁 Open files and folders directly in Cursor
- 🪄 Background area support for quick access
- ⚡ Lightning-fast workflow enhancement
- 🔒 Secure local implementation

## 📋 Prerequisites

- macOS 10.15 or later
- [Cursor IDE](https://cursor.sh) installed in your Applications folder
- Administrative access for setup

## 🚀 Installation

### Step 1: Create Automator Quick Action

1. Launch **Automator** from your Applications
2. Create a **New Document** → Select **Quick Action**
3. Configure workflow settings:
   ```
   Workflow receives current: files or folders
   Application: Finder
   ```
4. Add **Run Shell Script** action with settings:
   - Shell: `/bin/bash`
   - Pass input: `as arguments`

5. Insert the following script:

```bash
#!/bin/bash

cursorExePath="$HOME/Applications/Cursor.app/Contents/MacOS/Cursor"

if [ -f "$cursorExePath" ]; then
  open -a "$cursorExePath" "$@"
else
  osascript -e 'display notification "Cursor executable not found" with title "Error"'
fi
```

### Step 2: Configure AppleScript Integration

<details>
<summary>Click to expand AppleScript configuration</summary>

1. Open **Script Editor** from Applications
2. Create new script with:

```applescript
set cursorExePath to (POSIX path of (path to home folder)) & "Applications/Cursor.app/Contents/MacOS/Cursor"

if (do shell script "test -f " & quoted form of cursorExePath) is "0" then
  do shell script quoted form of cursorExePath
else
  display notification "Cursor executable not found" with title "Error"
end if
```

3. Save as application named `Open with Cursor` in Applications
</details>

### Step 3: Service Configuration

<details>
<summary>Click to expand service setup</summary>

1. Create `com.cursor.openwithcursor.plist`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.cursor.openwithcursor</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/bin/open</string>
        <string>-a</string>
        <string>Open with Cursor</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
</dict>
</plist>
```

2. Save to `~/Library/LaunchAgents/`
</details>

### Step 4: Activate the Service

```bash
launchctl load ~/Library/LaunchAgents/com.cursor.openwithcursor.plist
```

## 🎯 Usage

1. **Files/Folders**: Right-click → Services → Open with Cursor
2. **Finder Background**: Right-click empty area → Open with Cursor

## 🔧 Troubleshooting

<details>
<summary>Common Issues & Solutions</summary>

### Service Not Appearing
- Verify Cursor.app is in Applications folder
- Restart Finder: `killall Finder`
- Reload launch agent:
  ```bash
  launchctl unload ~/Library/LaunchAgents/com.cursor.openwithcursor.plist
  launchctl load ~/Library/LaunchAgents/com.cursor.openwithcursor.plist
  ```

### Permission Issues
- Check System Preferences → Security & Privacy → Privacy → Automation
- Ensure Automator and Script Editor have necessary permissions
</details>

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<div align="center">

Made with ❤️ by the rajeshkanaka 

**[⬆ back to top](#-open-with-cursor---macos-finder-integration)**

</div> 
