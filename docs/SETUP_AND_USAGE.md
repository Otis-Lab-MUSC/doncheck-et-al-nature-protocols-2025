# REACHER System - Installation and Usage Guide

This guide provides comprehensive instructions for setting up and using the REACHER (Rodent Experiment Application Controls and Handling Ecosystem for Research) system. The system consists of multiple coordinated repositories that work together to provide a complete drug self-administration behavioral control platform for head-fixed mice.

---

## Table of Contents

1. [System Overview](#system-overview)
2. [Prerequisites](#prerequisites)
3. [Git Installation and Configuration](#git-installation-and-configuration)
4. [Repository Cloning](#repository-cloning)
5. [Python Environment Setup](#python-environment-setup)
6. [Arduino IDE Setup](#arduino-ide-setup)
7. [Hardware Setup](#hardware-setup)
8. [Running a Session](#running-a-session)
9. [Troubleshooting](#troubleshooting)

---

## System Overview

The REACHER system is composed of four coordinated repositories:

### Repository Architecture

1. **reacher** (Core Python Package)
   - Core server for session management, event logging, and real-time control
   - Python-based framework
   - Provides the backend functionality and interface components
   - GitHub: `https://github.com/Otis-Lab-MUSC/reacher`

2. **labrynth** (Frontend Application)
   - Pre-built modifiable frontend application
   - Browser-based interface for running experiments
   - Manages multiple simultaneous sessions
   - GitHub: `https://github.com/Otis-Lab-MUSC/labrynth`

3. **reacher-firmware** (Arduino Firmware)
   - Low-level Arduino sketches for hardware control
   - Controls solenoids, pumps, sensors, levers, and cues
   - Multiple paradigms: Fixed Ratio (FR), Progressive Ratio (PR), Variable Interval (VI), Omission
   - GitHub: `https://github.com/Otis-Lab-MUSC/reacher-firmware`

---

## Prerequisites

### System Requirements

| Component | Minimum | Recommended | High-Performance |
|-----------|---------|-------------|------------------|
| **CPU** | Quad-core (e.g., Intel i3) | 6-8 core (e.g., Intel i5/i7, AMD Ryzen 5) | 12-core+ (e.g., AMD Ryzen 9, Intel i9) |
| **RAM** | 8 GB | 16 GB | 32 GB+ |
| **Storage** | 256 GB SSD | 512 GB SSD | 1 TB NVMe SSD+ |
| **OS** | Windows 10 (64-bit), Linux, macOS | Ubuntu/Debian Linux, Windows 11, macOS | Linux with optimized kernels |
| **GPU** | Integrated graphics | Mid-range (NVIDIA GTX 1660) | High-end (NVIDIA RTX 3080+) |

### Required Software

- **Git**: Version control system
- **Python**: 3.8 or higher (3.9-3.11 recommended)
- **Arduino IDE**: For uploading firmware to microcontrollers
- **Web Browser**: For Labrynth interface

### Hardware Requirements

- Arduino-compatible microcontroller (e.g., Arduino UNO)
- USB-A to USB-B cable
- Experimental rig components (levers, pumps, cues, etc.)
- Optional: Raspberry Pi for distributed setups

---

## Git Installation and Configuration

### Windows

#### Option 1: Git for Windows (Recommended)

1. Download Git installer:
   - Visit: https://git-scm.com/download/win
   - Download the 64-bit installer

2. Run the installer:
   ```powershell
   # If downloaded to Downloads folder, run:
   cd ~\Downloads
   .\Git-<version>-64-bit.exe
   ```

3. Installation options (recommended settings):
   - Editor: Use Visual Studio Code or your preferred editor
   - PATH: "Git from the command line and also from 3rd-party software"
   - HTTPS: "Use the OpenSSL library"
   - Line endings: "Checkout Windows-style, commit Unix-style"
   - Terminal: "Use Windows' default console window" or "Use MinTTY"
   - Extras: Enable "Git Credential Manager"

4. Verify installation:
   ```powershell
   git --version
   ```

#### Option 2: Via Chocolatey

```powershell
# Install Chocolatey package manager first (if not installed)
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072
iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Install Git
choco install git -y

# Refresh environment variables
refreshenv

# Verify
git --version
```

### macOS

#### Option 1: Via Homebrew (Recommended)

```bash
# Install Homebrew if not already installed
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Git
brew install git

# Verify
git --version
```

#### Option 2: Via Xcode Command Line Tools

```bash
# Install Xcode Command Line Tools (includes Git)
xcode-select --install

# Verify
git --version
```

### Linux (Ubuntu/Debian)

```bash
# Update package index
sudo apt update

# Install Git
sudo apt install git -y

# Verify installation
git --version
```

### Git Configuration

After installing Git, configure your identity (required for commits):

```bash
# Set your name and email
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Set default branch name to 'main' (optional but recommended)
git config --global init.defaultBranch main

# Enable credential caching to avoid repeated password prompts
# On Windows (using Git Credential Manager):
git config --global credential.helper manager

# On macOS (using Keychain):
git config --global credential.helper osxkeychain

# On Linux (cache for 1 hour):
git config --global credential.helper 'cache --timeout=3600'

# Verify configuration
git config --list
```

---

## Repository Cloning

### Choose Your Installation Directory

First, create a directory for the project:

**Windows (PowerShell):**
```powershell
# Create project directory
New-Item -Path "$HOME\Projects\REACHER" -ItemType Directory -Force
cd "$HOME\Projects\REACHER"
```

**macOS/Linux (Bash):**
```bash
# Create project directory
mkdir -p ~/Projects/REACHER
cd ~/Projects/REACHER
```

### Clone All Repositories

You have two options for cloning: HTTPS (simpler) or SSH (more secure, requires SSH key setup).

#### Option 1: HTTPS (Simpler, No SSH Key Required)

**Windows (PowerShell):**
```powershell
# Clone all repositories
git clone https://github.com/Otis-Lab-MUSC/doncheck-et-al-nature-protocols-2025.git
git clone https://github.com/Otis-Lab-MUSC/reacher.git
git clone https://github.com/Otis-Lab-MUSC/labrynth.git
git clone https://github.com/Otis-Lab-MUSC/reacher-firmware.git

# Verify all repositories are present
ls
```

**macOS/Linux (Bash):**
```bash
# Clone all repositories
git clone https://github.com/Otis-Lab-MUSC/doncheck-et-al-nature-protocols-2025.git
git clone https://github.com/Otis-Lab-MUSC/reacher.git
git clone https://github.com/Otis-Lab-MUSC/labrynth.git
git clone https://github.com/Otis-Lab-MUSC/reacher-firmware.git

# Verify all repositories are present
ls -la
```

#### Option 2: SSH (Recommended for Contributors)

First, set up SSH keys (if not already done):

**Generate SSH Key:**
```bash
# Generate a new SSH key
ssh-keygen -t ed25519 -C "your.email@example.com"

# When prompted, press Enter to accept default location
# Enter a passphrase (optional but recommended)

# Start ssh-agent
# On Windows (PowerShell):
Start-Service ssh-agent

# On macOS/Linux:
eval "$(ssh-agent -s)"

# Add your SSH key
# On Windows (PowerShell):
ssh-add $HOME\.ssh\id_ed25519

# On macOS/Linux:
ssh-add ~/.ssh/id_ed25519

# Copy public key to clipboard
# On Windows (PowerShell):
Get-Content $HOME\.ssh\id_ed25519.pub | Set-Clipboard

# On macOS:
pbcopy < ~/.ssh/id_ed25519.pub

# On Linux:
cat ~/.ssh/id_ed25519.pub | xclip -selection clipboard
# Or manually copy the output of: cat ~/.ssh/id_ed25519.pub
```

Then add the key to GitHub:
1. Go to GitHub.com → Settings → SSH and GPG keys
2. Click "New SSH key"
3. Paste your public key and save

**Clone with SSH:**
```bash
# Clone all repositories using SSH
git clone git@github.com:Otis-Lab-MUSC/doncheck-et-al-nature-protocols-2025.git
git clone git@github.com:Otis-Lab-MUSC/reacher.git
git clone git@github.com:Otis-Lab-MUSC/labrynth.git
git clone git@github.com:Otis-Lab-MUSC/reacher-firmware.git
```

---

## Python Environment Setup

### Install Python

#### Windows

1. **Download Python:**
   - Visit: https://www.python.org/downloads/
   - Download Python 3.11 (or latest 3.x version)

2. **Run installer:**
   - ☑️ Check "Add Python to PATH"
   - Click "Install Now"

3. **Verify installation:**
   ```powershell
   python --version
   pip --version
   ```

#### macOS

```bash
# Install Python via Homebrew
brew install python@3.11

# Verify installation
python3 --version
pip3 --version

# Create symlinks (optional, for convenience)
ln -s $(which python3) /usr/local/bin/python
ln -s $(which pip3) /usr/local/bin/pip
```

#### Linux (Ubuntu/Debian)

```bash
# Update package index
sudo apt update

# Install Python and pip
sudo apt install python3.11 python3.11-venv python3-pip -y

# Verify installation
python3 --version
pip3 --version
```

### Setup Virtual Environments

Create separate virtual environments for each Python repository:

#### For REACHER (Core Package)

**Windows (PowerShell):**
```powershell
# Navigate to reacher directory
cd "$HOME\Projects\REACHER\reacher"

# Create virtual environment
python -m venv venv

# Activate virtual environment
.\venv\Scripts\Activate.ps1

# Upgrade pip
python -m pip install --upgrade pip

# Install dependencies
pip install -r requirements.txt

# Install the reacher package in development mode
pip install -e .

# Deactivate when done
deactivate
```

**macOS/Linux (Bash):**
```bash
# Navigate to reacher directory
cd ~/Projects/REACHER/reacher

# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate

# Upgrade pip
python -m pip install --upgrade pip

# Install dependencies
pip install -r requirements.txt

# Install the reacher package in development mode
pip install -e .

# Deactivate when done
deactivate
```

#### For Labrynth (Frontend Application)

**Windows (PowerShell):**
```powershell
# Navigate to labrynth directory
cd "$HOME\Projects\REACHER\labrynth"

# Create virtual environment
python -m venv venv

# Activate virtual environment
.\venv\Scripts\Activate.ps1

# Upgrade pip
python -m pip install --upgrade pip

# Install dependencies
pip install -r requirements.txt

# IMPORTANT: Install the reacher package
# Option 1: Install from the local reacher directory
pip install -e ..\reacher

# Option 2: Install from wheel file (if available)
# Download the wheel from: https://github.com/Otis-Lab-MUSC/reacher/releases/download/v1.1.1/reacher-1.1.1-py3-none-any.whl
# pip install reacher-1.1.1-py3-none-any.whl

# Deactivate when done
deactivate
```

**macOS/Linux (Bash):**
```bash
# Navigate to labrynth directory
cd ~/Projects/REACHER/labrynth

# Create virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate

# Upgrade pip
python -m pip install --upgrade pip

# Install dependencies
pip install -r requirements.txt

# IMPORTANT: Install the reacher package
# Option 1: Install from the local reacher directory
pip install -e ../reacher

# Option 2: Install from wheel file (if available)
# Download using curl or wget:
# curl -L -o reacher-1.1.1-py3-none-any.whl https://github.com/Otis-Lab-MUSC/reacher/releases/download/v1.1.1/reacher-1.1.1-py3-none-any.whl
# pip install reacher-1.1.1-py3-none-any.whl

# Deactivate when done
deactivate
```

### Verifying Python Setup

Test that everything is installed correctly:

**Windows (PowerShell):**
```powershell
# Activate labrynth environment
cd "$HOME\Projects\REACHER\labrynth"
.\venv\Scripts\Activate.ps1

# Test imports
python -c "import reacher; print('REACHER version:', reacher.__version__ if hasattr(reacher, '__version__') else 'imported successfully')"
python -c "import panel; print('Panel version:', panel.__version__)"
python -c "import serial; print('PySerial imported successfully')"

deactivate
```

**macOS/Linux (Bash):**
```bash
# Activate labrynth environment
cd ~/Projects/REACHER/labrynth
source venv/bin/activate

# Test imports
python -c "import reacher; print('REACHER version:', reacher.__version__ if hasattr(reacher, '__version__') else 'imported successfully')"
python -c "import panel; print('Panel version:', panel.__version__)"
python -c "import serial; print('PySerial imported successfully')"

deactivate
```

---

## Arduino IDE Setup

### Install Arduino IDE

#### Windows

1. **Download Arduino IDE 2.x:**
   - Visit: https://www.arduino.cc/en/software
   - Download "Windows Win 10 and newer, 64 bits" installer

2. **Run installer and follow prompts**

3. **Verify installation:**
   - Launch Arduino IDE
   - Check Help → About Arduino to verify version

#### macOS

```bash
# Option 1: Via Homebrew Cask
brew install --cask arduino-ide

# Option 2: Download DMG from https://www.arduino.cc/en/software
# Then drag Arduino IDE to Applications folder
```

#### Linux (Ubuntu/Debian)

```bash
# Download AppImage
cd ~/Downloads
wget https://downloads.arduino.cc/arduino-ide/arduino-ide_latest_Linux_64bit.AppImage

# Make executable
chmod +x arduino-ide_*.AppImage

# Run Arduino IDE
./arduino-ide_*.AppImage

# Optional: Create desktop entry
mkdir -p ~/.local/share/applications
cat > ~/.local/share/applications/arduino-ide.desktop << EOF
[Desktop Entry]
Name=Arduino IDE
Exec=$HOME/Downloads/arduino-ide_latest_Linux_64bit.AppImage
Icon=arduino-ide
Type=Application
Categories=Development;
EOF
```

### Install Required Arduino Libraries

The firmware requires the following Arduino libraries:

1. **Launch Arduino IDE**

2. **Install ArduinoJson library:**
   - Go to: Tools → Manage Libraries (or Ctrl+Shift+I)
   - Search for "ArduinoJson"
   - Install "ArduinoJson by Benoit Blanchon" (version 6.x or later)

3. **Verify built-in libraries:**
   - `Arduino.h` - Built-in
   - `SoftwareSerial.h` - Built-in

### Configure Arduino for Your Board

1. **Connect your Arduino via USB**

2. **Select your board:**
   - Tools → Board → Arduino AVR Boards → Arduino Uno (or your specific board)

3. **Select the COM port:**
   - Tools → Port → Select your Arduino's port
   - Windows: COM3, COM4, etc.
   - macOS: /dev/cu.usbmodem* or /dev/cu.usbserial*
   - Linux: /dev/ttyACM0, /dev/ttyUSB0, etc.

### Upload Firmware to Arduino

1. **Choose a paradigm from the firmware repository:**
   - `operant_FR` - Fixed Ratio (most commonly used, stable)
   - `omission-beta` - Omission contingency (beta)
   - `operant_PR-beta` - Progressive Ratio (beta)
   - `operant_VI-beta` - Variable Interval (beta)

2. **Open the sketch:**

**Windows (PowerShell):**
```powershell
# Navigate to firmware directory
cd "$HOME\Projects\REACHER\reacher-firmware"

# For Fixed Ratio paradigm (recommended to start):
# Open: operant_FR\operant_FR.ino in Arduino IDE
```

**macOS/Linux (Bash):**
```bash
# Navigate to firmware directory
cd ~/Projects/REACHER/reacher-firmware

# For Fixed Ratio paradigm (recommended to start):
# Open: operant_FR/operant_FR.ino in Arduino IDE
```

3. **In Arduino IDE:**
   - File → Open → Navigate to the `.ino` file
   - Verify/Compile: Click the checkmark button (or Sketch → Verify/Compile)
   - Upload: Click the arrow button (or Sketch → Upload)
   - Wait for "Done uploading" message

4. **Verify upload:**
   - Open Serial Monitor: Tools → Serial Monitor
   - Set baud rate to 115200
   - You should see initialization messages from the Arduino

### Linux-Specific: USB Permissions

On Linux, you may need to add your user to the `dialout` group:

```bash
# Add user to dialout group
sudo usermod -a -G dialout $USER

# Log out and log back in, or run:
newgrp dialout

# Verify group membership
groups
```

---

## Hardware Setup

### Pin Configuration (Standard for All Firmware)

The REACHER firmware uses the following pin configuration:

| Pin | Function | Type | Notes |
|-----|----------|------|-------|
| 2 | Frame timestamp trigger | Input | For imaging synchronization |
| 3 | Cue speaker | PWM Output | Must be PWM-capable |
| 4 | Pump | Digital Output | Controls infusion pump |
| 5 | Lick circuit | Digital Input | Detects licking behavior |
| 6 | Laser | Digital Output | For optogenetic stimulation |
| 9 | Imaging trigger | Digital Output | Triggers imaging system |
| 10 | Right-hand lever | Digital Input | Active/inactive lever |
| 13 | Left-hand lever | Digital Input | Active/inactive lever |

### Wiring Guidelines

1. **Connect peripherals to Arduino according to pin configuration**
2. **Use appropriate power supplies for high-current devices (pumps, lasers)**
3. **Implement proper isolation for inputs (optocouplers recommended)**
4. **Use pull-down resistors on input pins for stable readings**
5. **Connect Arduino to computer via USB**

### Testing Hardware

Use Arduino IDE Serial Monitor to test hardware connectivity:

1. Upload firmware and open Serial Monitor (115200 baud)
2. Send command: `LINK` - Should respond with confirmation
3. Test individual components with commands like:
   - `ARM_LEVER_RH` - Arm right-hand lever
   - `ARM_PUMP` - Arm pump
   - `ARM_CS` - Arm cue speaker

---

## Running a Session

### Quick Start: Pre-built Application (Easiest)

If you prefer not to run from source code, download the pre-built application:

**Windows:**
```powershell
# Download from browser or using PowerShell:
Invoke-WebRequest -Uri "https://github.com/Otis-Lab-MUSC/labrynth/releases/latest/download/labrynth_x64.exe" -OutFile "labrynth.exe"

# Run the application
.\labrynth.exe
```

**macOS:**
```bash
# Download DMG
curl -L -o labrynth.dmg "https://github.com/Otis-Lab-MUSC/labrynth/releases/latest/download/labrynth_x64.dmg"

# Open DMG and drag to Applications
open labrynth.dmg
```

**Linux (Ubuntu/Debian):**
```bash
# Download DEB package
wget https://github.com/Otis-Lab-MUSC/labrynth/releases/latest/download/labrynth_amd64.deb

# Install
sudo dpkg -i labrynth_amd64.deb

# Fix dependencies if needed
sudo apt --fix-broken install

# Run
labrynth
```

### Running from Source Code

#### Method 1: Using the Launcher (Recommended)

**Windows (PowerShell):**
```powershell
# Navigate to labrynth UI directory
cd "$HOME\Projects\REACHER\labrynth\ui\src"

# Activate virtual environment
..\..\venv\Scripts\Activate.ps1

# Run the application
python main.py

# The launcher window will open, and your browser will launch with the dashboard
# Keep the launcher window open while using the application
```

**macOS/Linux (Bash):**
```bash
# Navigate to labrynth UI directory
cd ~/Projects/REACHER/labrynth/ui/src

# Activate virtual environment
source ../../venv/bin/activate

# Run the application
python main.py

# The launcher window will open, and your browser will launch with the dashboard
# Keep the launcher window open while using the application
```

#### Method 2: Direct Panel Serve

**Windows (PowerShell):**
```powershell
cd "$HOME\Projects\REACHER\labrynth"
.\venv\Scripts\Activate.ps1

# Run Panel server directly (advanced)
panel serve ui\src\main.py --show --port 7007
```

**macOS/Linux (Bash):**
```bash
cd ~/Projects/REACHER/labrynth
source venv/bin/activate

# Run Panel server directly (advanced)
panel serve ui/src/main.py --show --port 7007
```

### Using the Labrynth Interface

Once the application is running, follow these steps:

#### Step 1: Create a New Session

1. In the "Create a session" area, enter a unique name (e.g., "Experiment_001")
2. Click **"New wired session"** for a local setup
3. A new tab will open for your session

#### Step 2: Connect to Microcontroller

1. Navigate to the **Home Tab**
2. Click **"Search Microcontrollers"** to scan for available COM ports
3. Select the COM port for your Arduino from the dropdown
   - Windows: COM3, COM4, etc.
   - macOS: /dev/cu.usbmodem*
   - Linux: /dev/ttyACM0, /dev/ttyUSB0
4. Click **"Connect"** to establish serial connection
5. Verify connection status shows "Connected"

#### Step 3: Configure Experiment (Program Tab)

1. Go to **Program Tab**
2. **Select Hardware:** Check components you'll use:
   - Levers (left/right)
   - Pump
   - Cue speaker
   - Laser
   - Lick circuit
3. **Set Limits:**
   - Maximum session time (e.g., 3600 seconds = 1 hour)
   - Maximum infusion count (if applicable)
   - Other paradigm-specific limits
4. **File Configuration:**
   - Enter data filename (e.g., "mouse_01_session_1")
   - Choose save location on your computer
   - Files are saved to `~/REACHER/` by default if not specified

#### Step 4: Adjust Hardware (Hardware Tab)

1. Navigate to **Hardware Tab**
2. **Arm/Disarm Devices:**
   - Toggle switches to enable/disable components
   - Set which lever is "active" (delivers reward)
3. **Set Parameters:**
   - **Cue:** Frequency (Hz), duration (ms)
   - **Pump:** Duration (ms), active state
   - **Laser:** Duration (ms), frequency (Hz), mode (continuous/burst)
   - **Levers:** Debounce time

#### Step 5: Configure Timing (Schedule Tab)

1. Go to **Schedule Tab**
2. Set paradigm-specific parameters:
   - **Fixed Ratio:** Number of presses per reward
   - **Timeout:** Duration after reward delivery (ms)
   - **Trace Interval:** Delay between cue and reward (ms)
   - **Progressive Ratio:** Increment value (for PR paradigm)
   - **Variable Interval:** Random interval range (for VI paradigm)

#### Step 6: Run Experiment (Monitor Tab)

1. Switch to **Monitor Tab**
2. Review all settings one final time
3. Click **"Start"** to begin the session
4. **During the session:**
   - Real-time graphs display lever presses and infusions
   - Event log shows all behavioral events with timestamps
   - Statistics panel shows cumulative counts
5. **Control options:**
   - **Pause:** Temporarily pause data collection (can resume)
   - **Stop:** End the session (cannot resume)

#### Step 7: Export Data

1. After stopping the session, click **"Export data"**
2. Data is saved as CSV files:
   - Behavioral events (lever presses, infusions, etc.)
   - Frame timestamps (if imaging sync is used)
   - Session metadata
3. Default location: `~/REACHER/` or your specified directory

### Data Output Format

The system generates CSV files with the following format:

**Behavioral Data (example):**
```csv
Device,Event,Start_Time,End_Time
RH_LEVER,ACTIVE_PRESS,1523,1623
PUMP,INFUSION,1650,3650
RH_LEVER,TIMEOUT_PRESS,4200,4300
```

**Frame Data (example - if using imaging):**
```csv
Frame_Number,Timestamp
1,1000
2,1033
3,1066
```

### Multiple Sessions

Labrynth supports running multiple sessions simultaneously:

1. Create additional sessions by entering new names and clicking "New wired session"
2. Each session appears as a separate tab
3. Each session can connect to a different Arduino on a different COM port
4. Configure and run each session independently
5. Tabs are color-coded and labeled for easy identification

---

## Troubleshooting

### Git Issues

**Problem: "git: command not found"**
- **Solution:** Git is not installed or not in PATH
  - Reinstall Git and ensure "Add to PATH" is checked
  - Restart your terminal/PowerShell after installation

**Problem: "Permission denied (publickey)" when cloning via SSH**
- **Solution:** SSH key not configured
  - Use HTTPS cloning method instead
  - Or set up SSH key following the SSH setup instructions above

**Problem: "fatal: not a git repository"**
- **Solution:** You're not in a Git repository directory
  - Navigate to the correct directory with `cd`

### Python/Virtual Environment Issues

**Problem: "python: command not found"**
- **Windows:** Use `python` (not `python3`)
- **macOS/Linux:** Use `python3` explicitly, or create alias
- Verify Python is installed: Check PATH environment variable

**Problem: "cannot activate virtual environment"**
- **Windows PowerShell:** You may need to enable script execution:
  ```powershell
  Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
  ```
- **Windows:** Use `.\venv\Scripts\Activate.ps1` (not `activate`)
- **macOS/Linux:** Use `source venv/bin/activate` (not `.\venv\`)

**Problem: "No module named 'reacher'"**
- **Solution:** The reacher package is not installed in your labrynth environment
  ```bash
  # Activate labrynth venv first, then:
  pip install -e ../reacher
  ```

**Problem: Package installation fails with "error: externally-managed-environment"**
- **Solution (Linux):** This is a Debian/Ubuntu safeguard. Use virtual environments:
  ```bash
  # Always create and activate venv before installing packages
  python3 -m venv venv
  source venv/bin/activate
  pip install -r requirements.txt
  ```

### Arduino/Firmware Issues

**Problem: Arduino IDE cannot find COM port**
- **Windows:** Install Arduino drivers from arduino.cc
- **macOS:** Grant permission in System Preferences → Security & Privacy
- **Linux:** Add user to `dialout` group (see Linux-Specific USB Permissions)
- Try unplugging and replugging Arduino
- Check Device Manager (Windows) or `ls /dev/tty*` (macOS/Linux)

**Problem: Upload fails with "avrdude: stk500_recv(): programmer is not responding"**
- **Solutions:**
  - Wrong board selected: Verify Tools → Board matches your Arduino
  - Wrong COM port: Verify Tools → Port shows your Arduino
  - USB cable issue: Try a different cable (must support data, not just power)
  - Reset Arduino: Press reset button before uploading

**Problem: "SoftwareSerial.h: No such file or directory"**
- **Solution:** Built-in library not found (rare)
  - Reinstall Arduino IDE
  - Verify board package is installed: Tools → Board → Boards Manager

**Problem: "ArduinoJson.h: No such file or directory"**
- **Solution:** Library not installed
  - Tools → Manage Libraries → Search "ArduinoJson" → Install

### Labrynth Application Issues

**Problem: "Address already in use" or port 7007 error**
- **Solution:** Another instance is running or port is occupied
  ```powershell
  # Windows: Find and kill process on port 7007
  netstat -ano | findstr :7007
  taskkill /PID <PID> /F
  ```
  ```bash
  # macOS/Linux: Find and kill process
  lsof -ti:7007 | xargs kill -9
  ```

**Problem: Browser doesn't open automatically**
- **Solution:** Manually navigate to `http://localhost:7007`

**Problem: "Cannot connect to microcontroller"**
- **Solutions:**
  1. Verify Arduino is connected and COM port is correct
  2. Check that firmware is uploaded and running (open Serial Monitor at 115200 baud to verify)
  3. Ensure no other program is using the serial port (close Serial Monitor, other apps)
  4. Try clicking "Search Microcontrollers" again
  5. Restart the Arduino (unplug/replug USB)

**Problem: No data appearing in graphs or event log**
- **Solutions:**
  1. Verify hardware is properly connected to Arduino pins
  2. Check that devices are "armed" in Hardware Tab
  3. Confirm Serial Monitor shows events when you interact with hardware
  4. Verify the session is "Started" in Monitor Tab
  5. Check that lever is set as "active" if expecting rewards

**Problem: Session crashes or stops responding**
- **Solutions:**
  1. Check Python console/terminal for error messages
  2. Verify your Python version is 3.8 or higher
  3. Ensure all dependencies are installed: `pip list`
  4. Try creating a fresh virtual environment
  5. Check system resources (CPU, RAM) - close other applications

### Data Export Issues

**Problem: "Cannot find data file" or export fails**
- **Solutions:**
  1. Verify you stopped the session before exporting
  2. Check file permissions in save directory
  3. Ensure destination directory exists and is writable
  4. Check default location: `~/REACHER/` or `C:\Users\<username>\REACHER\`

**Problem: Data file is empty or incomplete**
- **Solutions:**
  1. Verify session was actually started (not just armed)
  2. Check that events occurred during the session
  3. Ensure session was stopped properly (not forced closed)

### Platform-Specific Issues

#### Windows Specific

**Problem: "WindowsPath object not recognized"**
- Use raw strings or forward slashes in paths:
  ```python
  # Good:
  r"C:\Users\username\folder"
  "C:/Users/username/folder"
  
  # Bad:
  "C:\Users\username\folder"  # Backslashes interpreted as escape characters
  ```

**Problem: Long path names cause issues**
- Enable long paths in Windows:
  ```powershell
  # Run as Administrator
  New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
  ```

#### macOS Specific

**Problem: "Operation not permitted" errors**
- Grant terminal/IDE full disk access:
  - System Preferences → Security & Privacy → Privacy → Full Disk Access
  - Add Terminal or your IDE

#### Linux Specific

**Problem: USB device permissions denied**
- Add user to dialout group (requires logout):
  ```bash
  sudo usermod -a -G dialout $USER
  ```

**Problem: ModuleNotFoundError after pip install**
- Ensure you're using the venv's Python:
  ```bash
  which python  # Should point to venv/bin/python
  ```

### Getting Additional Help

If you encounter issues not covered here:

1. **Check Documentation:**
   - Individual repository READMEs
   - Nature Protocols paper: Doncheck et al. (2025)
   - GitHub Issues: https://github.com/Otis-Lab-MUSC/

2. **Community Support:**
   - Open an issue on the relevant GitHub repository
   - Include:
     - Operating system and version
     - Python version
     - Complete error message
     - Steps to reproduce

3. **Contact:**
   - Author: Joshua Boquiren (thejoshbq@proton.me)
   - Lab: Otis Lab (http://www.otis-lab.org)

---

## Quick Reference Commands

### Git Commands
```bash
# Check status
git status

# Pull latest changes
git pull origin main

# Check current branch
git branch

# View commit history
git log --oneline -10
```

### Python Virtual Environment
```powershell
# Windows PowerShell
.\venv\Scripts\Activate.ps1  # Activate
deactivate                    # Deactivate
pip list                      # List installed packages
```

```bash
# macOS/Linux
source venv/bin/activate      # Activate
deactivate                    # Deactivate
pip list                      # List installed packages
```

### Running Labrynth
```bash
# From labrynth/ui/src directory, with venv activated
python main.py
```

### Arduino Serial Commands (for testing)
```
LINK              # Establish connection
ARM_LEVER_RH      # Arm right lever
ARM_PUMP          # Arm pump
ARM_CS            # Arm cue speaker
START-PROGRAM     # Start experiment
```

---

## Appendix: System Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                    REACHER System Architecture              │
└─────────────────────────────────────────────────────────────┘

    ┌──────────────────┐
    │   User Browser   │ ←──→ http://localhost:7007
    └────────┬─────────┘
             │
             ↓
    ┌──────────────────┐
    │  Labrynth (UI)   │ ←──→ Panel dashboard, session management
    └────────┬─────────┘
             │
             ↓
    ┌──────────────────┐
    │ REACHER (Python) │ ←──→ Core logic, serial communication
    └────────┬─────────┘
             │
             ↓ Serial (USB)
    ┌──────────────────┐
    │ Arduino + Firmware│ ←──→ Hardware control, event logging
    └────────┬─────────┘
             │
             ↓
    ┌──────────────────┐
    │  Physical Rig    │ ←──→ Levers, pumps, cues, sensors
    └──────────────────┘
```

---

## Citation

If using these resources, please cite:

> Doncheck, E.M. et al. Drug self-administration in head-fixed mice. *Nat. Protoc.* (2026). https://doi.org/PLACEHOLDER

---

*Last updated: January 2026*
