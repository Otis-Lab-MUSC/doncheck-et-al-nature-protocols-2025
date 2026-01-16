# REACHER System - Development Guide

This guide provides comprehensive instructions for developers who want to modify the REACHER system, including how to coordinate changes across multiple repositories, testing procedures, and release workflows.

---

## Table of Contents

1. [Development Environment Setup](#development-environment-setup)
2. [Repository Structure](#repository-structure)
3. [Development Workflow](#development-workflow)
4. [Modifying the Core REACHER Package](#modifying-the-core-reacher-package)
5. [Modifying Labrynth Frontend](#modifying-labrynth-frontend)
6. [Modifying Arduino Firmware](#modifying-arduino-firmware)
7. [Coordinating Changes Across Repositories](#coordinating-changes-across-repositories)
8. [Testing](#testing)
9. [Building and Packaging](#building-and-packaging)
10. [Version Management](#version-management)
11. [Release Process](#release-process)
12. [Contribution Guidelines](#contribution-guidelines)

---

## Development Environment Setup

### Prerequisites

Ensure you have completed the setup steps from `SETUP_AND_USAGE.md`:
- Git installed and configured
- All repositories cloned
- Python virtual environments created
- Arduino IDE installed

### Additional Development Tools

Install additional tools for development:

#### Code Editor/IDE

**Recommended:**
- **Visual Studio Code** (cross-platform)
- **PyCharm** (Python development)
- **Arduino IDE 2.x** (firmware development)

**VS Code Extensions:**
```bash
# Install VS Code extensions via command line (if using VS Code)
code --install-extension ms-python.python
code --install-extension ms-python.vscode-pylance
code --install-extension ms-toolsai.jupyter
code --install-extension platformio.platformio-ide
code --install-extension vsciot-vscode.vscode-arduino
```

#### Development Dependencies

**Windows (PowerShell):**
```powershell
# Navigate to each repository and activate venv, then:

# For reacher
cd "$HOME\Projects\REACHER\reacher"
.\venv\Scripts\Activate.ps1
pip install pytest pytest-cov black flake8 mypy
pip install -e ".[dev]"  # If setup.py has dev extras
deactivate

# For labrynth
cd "$HOME\Projects\REACHER\labrynth"
.\venv\Scripts\Activate.ps1
pip install pytest black flake8
deactivate
```

**macOS/Linux (Bash):**
```bash
# For reacher
cd ~/Projects/REACHER/reacher
source venv/bin/activate
pip install pytest pytest-cov black flake8 mypy
pip install -e ".[dev]"  # If setup.py has dev extras
deactivate

# For labrynth
cd ~/Projects/REACHER/labrynth
source venv/bin/activate
pip install pytest black flake8
deactivate
```

---

## Repository Structure

### REACHER (Core Python Package)

```
reacher/
├── src/
│   └── reacher/
│       ├── __init__.py           # Package initialization
│       ├── kernel/               # Core functionality
│       │   ├── __init__.py
│       │   └── reacher.py        # Main REACHER class, serial handling
│       ├── interface/            # Local/wired dashboard components
│       │   ├── __init__.py
│       │   ├── dashboard.py      # Main dashboard coordinator
│       │   ├── interface.py      # Interface wrapper
│       │   ├── home_tab.py       # Serial connection management
│       │   ├── program_tab.py    # Experiment configuration
│       │   ├── hardware_tab.py   # Hardware control panel
│       │   ├── schedule_tab.py   # Timing configuration
│       │   └── monitor_tab.py    # Real-time monitoring
│       ├── remote/               # Network/wireless dashboard (beta)
│       │   └── [similar structure to interface/]
│       └── assets/               # Icons, images
├── tests/                        # Unit tests
├── debian/                       # Debian package configuration
├── setup.py                      # Package configuration
├── requirements.txt              # Python dependencies
└── README.md                     # Documentation
```

**Key Files to Modify:**
- `kernel/reacher.py` - Core serial communication, event handling
- `interface/*_tab.py` - Individual dashboard tabs
- `interface/dashboard.py` - Dashboard coordination
- `setup.py` - Version, dependencies, package metadata

### Labrynth (Frontend Application)

```
labrynth/
├── ui/
│   ├── src/
│   │   ├── main.py               # Application entry point
│   │   └── assets/               # UI assets (icons, banners)
│   ├── setup.spec                # PyInstaller configuration
│   └── setup.iss                 # Inno Setup configuration (Windows)
├── docs/                         # Documentation
├── requirements.txt              # Python dependencies (includes reacher)
└── README.md                     # Documentation
```

**Key Files to Modify:**
- `ui/src/main.py` - Main application logic, launcher window
- `ui/setup.spec` - Build configuration for PyInstaller
- `requirements.txt` - Dependencies (ensure reacher version matches)

### REACHER Firmware (Arduino)

```
reacher-firmware/
├── operant_FR/                   # Fixed Ratio paradigm (stable)
│   ├── operant_FR.ino           # Main Arduino sketch
│   ├── Cue.cpp / Cue.h          # Cue speaker control
│   ├── Device.cpp / Device.h    # Base device class
│   ├── Laser.cpp / Laser.h      # Laser control
│   ├── Lever.cpp / Lever.h      # Lever input handling
│   ├── LickCircuit.cpp / .h     # Lick detection
│   └── Pump.cpp / Pump.h        # Pump control
├── omission-beta/                # Omission paradigm (beta)
├── operant_PR-beta/              # Progressive Ratio (beta)
├── operant_VI-beta/              # Variable Interval (beta)
├── docs/                         # Doxygen documentation
├── Doxyfile                      # Doxygen configuration
└── README.md                     # Documentation
```

**Key Files to Modify:**
- `<paradigm>/<paradigm>.ino` - Main sketch logic
- `<paradigm>/*.cpp` and `*.h` - Device class implementations
- Pin configurations are standardized across all paradigms

---

## Development Workflow

### Branch Strategy

Follow a feature-branch workflow:

```bash
# Create a new feature branch
git checkout -b feature/your-feature-name

# Make changes and commit
git add .
git commit -m "Add feature: description"

# Push to remote
git push origin feature/your-feature-name

# Create pull request on GitHub
```

### Commit Message Conventions

Use clear, descriptive commit messages:

```
# Format: <type>: <subject>

# Types:
feat: New feature
fix: Bug fix
docs: Documentation changes
style: Code style changes (formatting, no logic change)
refactor: Code refactoring
test: Adding or updating tests
chore: Maintenance tasks (dependencies, build, etc.)

# Examples:
feat: Add variable interval paradigm support
fix: Resolve serial port connection timeout issue
docs: Update installation instructions for Windows
refactor: Simplify dashboard tab initialization
test: Add unit tests for lever debouncing logic
```

### Code Style

#### Python

Follow PEP 8 style guidelines:

```bash
# Format code with black
black src/

# Check style with flake8
flake8 src/ --max-line-length=100

# Type checking with mypy
mypy src/
```

**Configuration files:**

`.flake8`:
```ini
[flake8]
max-line-length = 100
exclude = .git,__pycache__,venv
ignore = E203, W503
```

`pyproject.toml` (for black):
```toml
[tool.black]
line-length = 100
target-version = ['py38', 'py39', 'py310', 'py311']
```

#### Arduino/C++

Follow Arduino style guidelines:
- Indent with 2 spaces
- Class names in PascalCase
- Function names in camelCase
- Constants in UPPER_CASE
- Comment complex logic
- Use Doxygen-style documentation comments

---

## Modifying the Core REACHER Package

The REACHER package is the heart of the system, handling serial communication, event logging, and dashboard components.

### Understanding the Architecture

**Data Flow:**
1. `reacher.py` establishes serial connection to Arduino
2. Two threads handle serial communication:
   - **Reader thread:** Reads data from Arduino, queues it
   - **Processor thread:** Processes queued data, updates UI
3. Dashboard tabs interact with REACHER instance via callbacks
4. Events are logged to CSV files in real-time

### Common Modification Scenarios

#### Scenario 1: Adding a New Hardware Device

**Step 1: Modify Arduino firmware first** (see firmware section)

**Step 2: Update REACHER Python code**

1. **Locate the hardware tab** (`src/reacher/interface/hardware_tab.py`):

```python
# Add new device controls
self.new_device_arm = pn.widgets.Toggle(name="Arm New Device", value=False)
self.new_device_param = pn.widgets.IntInput(name="Parameter", value=100)

# Add callbacks
self.new_device_arm.param.watch(self._on_new_device_arm, 'value')
```

2. **Add serial commands** in callback:

```python
def _on_new_device_arm(self, event):
    if event.new:
        self.reacher.send_command("ARM_NEW_DEVICE")
    else:
        self.reacher.send_command("DISARM_NEW_DEVICE")
```

3. **Update UI layout:**

```python
# In hardware_tab.py layout() method
new_device_section = pn.Column(
    pn.pane.Markdown("### New Device"),
    self.new_device_arm,
    self.new_device_param
)
```

#### Scenario 2: Modifying Event Logging

**File:** `src/reacher/kernel/reacher.py`

```python
def _process_serial_data(self, data: str) -> None:
    """Process data from Arduino."""
    # Existing parsing logic
    parts = data.split(',')
    
    # Add handling for new event type
    if parts[0] == "NEW_DEVICE":
        device_name = parts[0]
        event_type = parts[1]
        start_time = int(parts[2])
        end_time = int(parts[3])
        
        # Log to CSV
        self._log_event(device_name, event_type, start_time, end_time)
        
        # Update UI if callback exists
        if self.on_new_device_event:
            self.on_new_device_event(event_type, start_time, end_time)
```

#### Scenario 3: Adding a New Dashboard Tab

1. **Create new tab file** (`src/reacher/interface/new_tab.py`):

```python
import panel as pn

class NewTab:
    """New functionality tab."""
    
    def __init__(self, reacher):
        self.reacher = reacher
        self._create_widgets()
    
    def _create_widgets(self):
        self.button = pn.widgets.Button(name="Do Something")
        self.button.on_click(self._on_button_click)
    
    def _on_button_click(self, event):
        # Implement functionality
        self.reacher.send_command("NEW_COMMAND")
    
    def layout(self):
        return pn.Column(
            pn.pane.Markdown("## New Tab"),
            self.button
        )
```

2. **Register tab in dashboard** (`src/reacher/interface/dashboard.py`):

```python
from .new_tab import NewTab

class Dashboard:
    def __init__(self, reacher):
        # Existing tabs
        self.home_tab = HomeTab(reacher)
        self.new_tab = NewTab(reacher)  # Add new tab
        
    def layout(self):
        return pn.Tabs(
            ("Home", self.home_tab.layout()),
            ("New", self.new_tab.layout()),  # Add to tabs
            # ... other tabs
        )
```

### Testing REACHER Changes

**Windows (PowerShell):**
```powershell
cd "$HOME\Projects\REACHER\reacher"
.\venv\Scripts\Activate.ps1

# Run unit tests
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=src/reacher --cov-report=html

# View coverage report
start htmlcov/index.html

deactivate
```

**macOS/Linux (Bash):**
```bash
cd ~/Projects/REACHER/reacher
source venv/bin/activate

# Run unit tests
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=src/reacher --cov-report=html

# View coverage report
open htmlcov/index.html  # macOS
xdg-open htmlcov/index.html  # Linux

deactivate
```

### Manual Integration Testing

1. **Install modified REACHER in Labrynth:**

```bash
# Navigate to labrynth
cd labrynth
source venv/bin/activate  # or .\venv\Scripts\Activate.ps1 on Windows

# Install modified reacher in editable mode
pip install -e ../reacher

# Run labrynth
python ui/src/main.py
```

2. **Test with hardware:**
   - Connect Arduino with test firmware
   - Create session in Labrynth
   - Test new functionality end-to-end

---

## Modifying Labrynth Frontend

Labrynth is the application wrapper around REACHER, providing the launcher and packaging configuration.

### Common Modifications

#### Scenario 1: Changing Application Branding

1. **Replace icon files** in `ui/src/assets/`:
   - `labrynth-icon.png` (256x256 recommended)
   - `labrynth-icon.ico` (Windows)
   - `labrynth-icon.icns` (macOS)
   - `labrynth-banner-wider.png` (600px wide recommended)

2. **Update window title** in `ui/src/main.py`:

```python
class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Your Custom Name")  # Change title
```

3. **Update footer** in `ui/src/main.py`:

```python
footer = pn.pane.HTML(
    """
    <div style="text-align: center; padding: 10px;">
        <p>Copyright © 2025 Your Lab</p>
        <p>Your custom text</p>
    </div>
    """
)
```

#### Scenario 2: Changing Default Port

```python
# In ui/src/main.py
def serve_interface():
    template = pn.template.BootstrapTemplate(...)
    pn.serve(template, port=8080, show=True)  # Change from 7007

# Also update in reopen_session():
def reopen_session(self):
    if self.panel_process.is_alive():
        url = "http://localhost:8080"  # Change from 7007
        webbrowser.open(url)
```

#### Scenario 3: Adding Startup Configuration

```python
# In ui/src/main.py, before serve_interface():

import json
from pathlib import Path

CONFIG_PATH = Path.home() / ".labrynth" / "config.json"

def load_config():
    """Load user configuration."""
    if CONFIG_PATH.exists():
        with open(CONFIG_PATH) as f:
            return json.load(f)
    return {"default_save_dir": str(Path.home() / "REACHER")}

def save_config(config):
    """Save user configuration."""
    CONFIG_PATH.parent.mkdir(exist_ok=True)
    with open(CONFIG_PATH, 'w') as f:
        json.dump(config, f, indent=2)

# Use in main application
config = load_config()
```

### Testing Labrynth Changes

**Test locally without building:**

```bash
# Activate venv
cd labrynth
source venv/bin/activate  # or .\venv\Scripts\Activate.ps1

# Run directly
python ui/src/main.py

# Test all features manually
```

---

## Modifying Arduino Firmware

The firmware controls hardware and communicates with REACHER via serial.

### Understanding Firmware Architecture

**Key Components:**

1. **Device Classes** (Cue, Laser, Lever, LickCircuit, Pump):
   - Encapsulate pin control and state management
   - Provide consistent interface

2. **Main Loop** (`PROGRAM()` function):
   - Monitors hardware state
   - Processes lever presses
   - Triggers rewards based on paradigm logic
   - Logs events to serial

3. **Serial Commands**:
   - Two-way communication with REACHER
   - String-based protocol (115200 baud)
   - JSON for setup data

### Common Modification Scenarios

#### Scenario 1: Adding a New Paradigm

1. **Copy existing paradigm as template:**

**Windows (PowerShell):**
```powershell
cd "$HOME\Projects\REACHER\reacher-firmware"
Copy-Item -Recurse operant_FR operant_NEW
Rename-Item operant_NEW\operant_FR.ino operant_NEW.ino
```

**macOS/Linux (Bash):**
```bash
cd ~/Projects/REACHER/reacher-firmware
cp -r operant_FR operant_NEW
mv operant_NEW/operant_FR.ino operant_NEW/operant_NEW.ino
```

2. **Modify main sketch** (`operant_NEW/operant_NEW.ino`):

```cpp
// Update paradigm-specific logic in PROGRAM() function

void PROGRAM() {
    if (linkedToGUI && !stopProgram) {
        // Your paradigm logic here
        
        // Example: Different reward schedule
        if (activeRightLever && rightLeverObject.wasPressed()) {
            rightLeverPressCount++;
            
            // Custom condition
            if (rightLeverPressCount >= currentRatio && !timeoutStatus) {
                // Trigger reward
                triggerReward();
                
                // Reset or increment as needed
                rightLeverPressCount = 0;
            }
            
            // Log event
            String eventType = timeoutStatus ? "TIMEOUT_PRESS" : "ACTIVE_PRESS";
            logLeverPress("RH_LEVER", eventType);
        }
        
        // Monitor other devices...
    }
}

void triggerReward() {
    // Custom reward sequence
    if (cueActive) cueObject.startBurst();
    if (pumpActive) pumpObject.turnOn();
    timeoutStatus = true;
    timeoutStartTime = millis();
}
```

3. **Update serial commands** (if needed):

```cpp
void monitorSerialCommands() {
    // Add new commands
    if (command == "SET_NEW_PARAM") {
        int value = getValue();
        // Process parameter
    }
    // ... existing commands
}
```

4. **Update JSON setup** (optional):

```cpp
void sendSetupJSON() {
    // Add new fields to JSON
    doc["paradigm"] = "NEW";
    doc["new_parameter"] = defaultNewParam;
    // ... existing fields
    
    serializeJson(doc, Serial);
    Serial.println();
}
```

#### Scenario 2: Modifying Pin Configuration

**WARNING:** Pin changes must be coordinated across all paradigms and documented for users.

```cpp
// In device declarations at top of sketch

// Old:
// #define RIGHT_LEVER_PIN 10

// New:
#define RIGHT_LEVER_PIN 11  // Changed from 10

Lever rightLeverObject(RIGHT_LEVER_PIN, 100);  // Update all references
```

**Important:** Update documentation in:
- README.md
- Pin configuration tables in setup guides
- Hardware schematics if applicable

#### Scenario 3: Adding a New Device Type

1. **Create device class files** (e.g., `Solenoid.h` and `Solenoid.cpp`):

**Solenoid.h:**
```cpp
#ifndef SOLENOID_H
#define SOLENOID_H

#include "Device.h"

class Solenoid : public Device {
private:
    bool state;
    unsigned long onTime;
    unsigned long duration;

public:
    Solenoid(int pin, unsigned long duration);
    void turnOn();
    void turnOff();
    void update();  // Call in loop to auto-turn-off after duration
    bool isOn();
};

#endif
```

**Solenoid.cpp:**
```cpp
#include "Solenoid.h"
#include <Arduino.h>

Solenoid::Solenoid(int pin, unsigned long duration) 
    : Device(pin), state(false), onTime(0), duration(duration) {
    pinMode(pin, OUTPUT);
    digitalWrite(pin, LOW);
}

void Solenoid::turnOn() {
    digitalWrite(pin, HIGH);
    state = true;
    onTime = millis();
}

void Solenoid::turnOff() {
    digitalWrite(pin, LOW);
    state = false;
}

void Solenoid::update() {
    if (state && (millis() - onTime >= duration)) {
        turnOff();
    }
}

bool Solenoid::isOn() {
    return state;
}
```

2. **Integrate into sketch:**

```cpp
#include "Solenoid.h"

// Declare
#define SOLENOID_PIN 7
Solenoid solenoidObject(SOLENOID_PIN, 1000);  // 1 second duration

// In setup()
// Already initialized

// In PROGRAM()
void PROGRAM() {
    // Update device state
    solenoidObject.update();
    
    // Trigger based on condition
    if (someCondition) {
        solenoidObject.turnOn();
        logEvent("SOLENOID", "ACTIVATED");
    }
}

// Add serial commands
void monitorSerialCommands() {
    if (command == "ARM_SOLENOID") {
        solenoidArmed = true;
    }
    if (command == "TRIGGER_SOLENOID") {
        if (solenoidArmed) solenoidObject.turnOn();
    }
}
```

### Testing Firmware Changes

1. **Compile and upload:**
   - Open `.ino` file in Arduino IDE
   - Verify (checkmark button) - check for compilation errors
   - Upload (arrow button) to Arduino
   - Monitor Serial Monitor (115200 baud) for output

2. **Serial command testing:**

```
# In Serial Monitor, send commands:
LINK
ARM_SOLENOID
TRIGGER_SOLENOID

# Observe device behavior and serial output
```

3. **Integration testing with REACHER:**
   - Update Python code to send new commands
   - Test end-to-end functionality
   - Verify data logging

### Firmware Documentation

Update Doxygen comments:

```cpp
/**
 * @brief Triggers the solenoid valve.
 * 
 * Activates the solenoid for the configured duration. Automatically
 * turns off after duration expires.
 * 
 * @note Requires solenoid to be armed before triggering.
 */
void triggerSolenoid() {
    if (solenoidArmed) {
        solenoidObject.turnOn();
    }
}
```

Generate documentation:

```bash
cd reacher-firmware
doxygen Doxyfile
# Output in docs/html/index.html
```

---

## Coordinating Changes Across Repositories

When changes span multiple repositories, careful coordination is essential.

### Dependency Management

**Labrynth depends on REACHER:**
```
labrynth → reacher (specific version)
```

**Version compatibility matrix:**
| Labrynth | REACHER | Firmware |
|----------|---------|----------|
| v1.1.1   | v1.1.1  | v1.0.1-alpha |
| v1.2.0   | v1.2.0  | v1.1.0   |

### Change Scenarios

#### Scenario 1: Adding Feature That Requires Changes in Multiple Repos

**Example:** Adding a new hardware device

**Step 1: Plan the changes**
1. Firmware: Add device class and serial commands
2. REACHER: Add hardware tab controls
3. Labrynth: No changes needed (uses REACHER)

**Step 2: Create feature branches in all repos**

```bash
# In each repository:
git checkout -b feature/add-solenoid-support
```

**Step 3: Implement in order (bottom-up)**

1. **Firmware first:**
   ```bash
   cd reacher-firmware
   git checkout -b feature/add-solenoid-support
   # Add Solenoid class, update sketch
   git add .
   git commit -m "feat: Add solenoid device support"
   git push origin feature/add-solenoid-support
   ```

2. **REACHER second:**
   ```bash
   cd reacher
   git checkout -b feature/add-solenoid-support
   # Add controls in hardware_tab.py
   # Add command handling
   git add .
   git commit -m "feat: Add solenoid controls to hardware tab"
   git push origin feature/add-solenoid-support
   ```

3. **Labrynth third (if needed):**
   ```bash
   cd labrynth
   # Update requirements.txt if REACHER version changes
   # Usually no changes needed
   ```

**Step 4: Test integration**

```bash
# Install modified REACHER in Labrynth's venv
cd labrynth
source venv/bin/activate
pip install -e ../reacher

# Upload modified firmware to Arduino

# Run and test
python ui/src/main.py
```

**Step 5: Create pull requests**

Create PRs in this order:
1. Firmware PR (merged first)
2. REACHER PR (reference firmware PR)
3. Labrynth PR (reference REACHER PR)

Link PRs in descriptions:
```markdown
## Related PRs
- Firmware: Otis-Lab-MUSC/reacher-firmware#123
- REACHER: Otis-Lab-MUSC/reacher#456

## Testing
- [x] Tested with firmware PR #123
- [x] Tested end-to-end functionality
- [x] Updated documentation
```

#### Scenario 2: Breaking Change in REACHER

**Example:** Changing serial command protocol

**Step 1: Version planning**
- REACHER: Major version bump (v1.x.x → v2.0.0)
- Firmware: Must update to match
- Labrynth: Update dependency version

**Step 2: Implement changes**

1. **Update REACHER:**
   ```python
   # Old: String commands "ARM_PUMP"
   # New: JSON commands {"command": "ARM", "device": "PUMP"}
   
   # Update reacher.py
   def send_command(self, command: dict):
       json_str = json.dumps(command)
       self.serial.write(json_str.encode())
   ```

2. **Update Firmware:**
   ```cpp
   // In monitorSerialCommands()
   void monitorSerialCommands() {
       if (Serial.available()) {
           String input = Serial.readStringUntil('\n');
           
           // Parse JSON
           StaticJsonDocument<256> doc;
           deserializeJson(doc, input);
           
           String command = doc["command"];
           String device = doc["device"];
           
           if (command == "ARM" && device == "PUMP") {
               pumpActive = true;
           }
       }
   }
   ```

3. **Update version numbers:**
   - `reacher/setup.py`: version="2.0.0"
   - Firmware: Update version comments
   - Update compatibility documentation

**Step 3: Migration guide**

Create MIGRATION.md:
```markdown
# Migration Guide: v1.x.x → v2.0.0

## Breaking Changes

### Serial Command Protocol

**Old (v1.x):**
```python
reacher.send_command("ARM_PUMP")
```

**New (v2.0):**
```python
reacher.send_command({"command": "ARM", "device": "PUMP"})
```

## Firmware Compatibility

- v2.0.0 requires firmware v2.0.0 or later
- v1.x.x firmware is not compatible

## Upgrade Steps

1. Update REACHER package
2. Upload new firmware to Arduino
3. Update Labrynth (if using)
```

#### Scenario 3: Bug Fix That Affects Multiple Repos

**Step 1: Identify affected repositories**

Example bug: Incorrect timestamp calculation

- Firmware: Timestamps off by start time
- REACHER: Parsing timestamps incorrectly
- Labrynth: Not affected

**Step 2: Fix in both repos**

```bash
# In firmware
cd reacher-firmware
git checkout -b fix/timestamp-calculation

# Fix in .ino file
# ...

git commit -m "fix: Correct timestamp offset calculation"
git push origin fix/timestamp-calculation

# In REACHER
cd reacher
git checkout -b fix/timestamp-parsing

# Fix in reacher.py
# ...

git commit -m "fix: Parse timestamps with correct offset"
git push origin fix/timestamp-parsing
```

**Step 3: Coordinate release**

- Both fixes should be released together
- Use patch version bump (v1.1.1 → v1.1.2)
- Note in changelog that both must be updated

### Testing Multi-Repository Changes

**Integration test checklist:**
- [ ] Firmware compiles and uploads successfully
- [ ] REACHER can establish serial connection
- [ ] Labrynth launches and creates sessions
- [ ] End-to-end feature functionality works
- [ ] Data is logged correctly
- [ ] No regressions in existing features

**Test script example:**

```bash
#!/bin/bash
# test_integration.sh

set -e

echo "=== Integration Test Suite ==="

echo "1. Testing REACHER package..."
cd reacher
source venv/bin/activate
pytest tests/ -v
deactivate

echo "2. Installing REACHER in Labrynth..."
cd ../labrynth
source venv/bin/activate
pip install -e ../reacher

echo "3. Testing Labrynth imports..."
python -c "from reacher.interface.interface import Interface; print('OK')"

echo "4. Manual test: Launch Labrynth and verify hardware connection"
read -p "Press enter after verifying hardware test..."

echo "=== All tests passed ==="
```

---

## Testing

### Unit Testing

#### REACHER Tests

Location: `reacher/tests/`

**Example test:**

```python
# tests/kernel/test_reacher.py
import pytest
from reacher.kernel.reacher import REACHER

def test_reacher_initialization():
    """Test REACHER initializes with correct defaults."""
    reacher = REACHER()
    assert reacher.serial_port is None
    assert reacher.connected is False

def test_send_command():
    """Test command sending."""
    reacher = REACHER()
    # Mock serial connection
    reacher.serial = MockSerial()
    
    reacher.send_command("TEST")
    assert reacher.serial.last_command == "TEST"

class MockSerial:
    def __init__(self):
        self.last_command = None
    
    def write(self, data):
        self.last_command = data.decode()
```

**Run tests:**
```bash
cd reacher
source venv/bin/activate
pytest tests/ -v --cov=src/reacher
```

#### Firmware Tests

Firmware testing is primarily manual:

1. **Compilation test:** Verify sketch compiles without errors
2. **Upload test:** Upload to Arduino and verify in Serial Monitor
3. **Command test:** Send serial commands and verify responses
4. **Hardware test:** Trigger devices and verify behavior

**Automated firmware testing** (advanced):
- Use Arduino CLI with test frameworks
- PlatformIO with unit testing support
- Hardware-in-the-loop testing with scripted serial communication

### Integration Testing

Create test scenarios that span repositories:

**Test Scenario Example:**

```markdown
## Test: Session Run with Data Export

### Prerequisites:
- Arduino with operant_FR firmware uploaded
- REACHER v1.1.1 installed
- Labrynth running

### Steps:
1. Launch Labrynth
2. Create new session "test_session_01"
3. Connect to Arduino on COM3
4. Configure: FR=1, timeout=20s
5. Arm right lever and pump
6. Start session
7. Manually press right lever 5 times
8. Verify: 5 lever presses logged, 5 infusions delivered
9. Stop session
10. Export data
11. Verify CSV contains 10 rows (5 presses + 5 infusions)

### Expected Results:
- [ ] All devices respond correctly
- [ ] Data appears in real-time graph
- [ ] CSV file created with correct data
- [ ] No errors in console
```

### Regression Testing

Maintain a checklist of critical functionality:

```markdown
## Regression Test Checklist

### Serial Communication
- [ ] Connect to Arduino
- [ ] Send commands
- [ ] Receive data
- [ ] Handle disconnect gracefully

### Hardware Control
- [ ] Arm/disarm each device
- [ ] Trigger rewards
- [ ] Adjust parameters
- [ ] Timeout behavior

### Data Management
- [ ] Session start/stop
- [ ] Real-time logging
- [ ] CSV export
- [ ] Multiple sessions

### UI Functionality
- [ ] Create sessions
- [ ] Switch between tabs
- [ ] Update graphs
- [ ] Error messages
```

---

## Building and Packaging

### Building REACHER Package

#### Create Python Wheel

```bash
cd reacher
source venv/bin/activate

# Update version in setup.py first
# version="1.1.2"

# Build package
python setup.py sdist bdist_wheel

# Output in dist/
# reacher-1.1.2-py3-none-any.whl
# reacher-1.1.2.tar.gz
```

#### Create Debian Package

```bash
cd reacher

# Update version in debian/changelog

# Build
python3 -m stdeb.command bdist_deb

# Output in deb_dist/
# python3-reacher_1.1.2-1_all.deb
```

### Building Labrynth Application

Labrynth uses PyInstaller to create standalone executables.

#### Prerequisites

```bash
cd labrynth
source venv/bin/activate
pip install pyinstaller
```

#### Build for Windows

**On Windows:**
```powershell
cd labrynth\ui
.\..\..\venv\Scripts\Activate.ps1

# Build with PyInstaller
pyinstaller setup.spec --clean

# Output in dist/main/
# To create installer, use Inno Setup with setup.iss

# Run Inno Setup Compiler
iscc setup.iss

# Output: labrynth_setup.exe
```

#### Build for macOS

**On macOS:**
```bash
cd labrynth/ui
source ../../venv/bin/activate

# Build with PyInstaller
pyinstaller setup.spec --clean

# Output: dist/main.app

# Create DMG
hdiutil create -volname "Labrynth" -srcfolder dist/main.app -ov -format UDZO labrynth.dmg
```

#### Build for Linux

**On Linux:**
```bash
cd labrynth/ui
source ../../venv/bin/activate

# Build with PyInstaller
pyinstaller setup.spec --clean

# Create DEB package
mkdir -p labrynth-deb/usr/local/bin
mkdir -p labrynth-deb/usr/share/applications
mkdir -p labrynth-deb/DEBIAN

# Copy executable
cp dist/main labrynth-deb/usr/local/bin/labrynth

# Create control file
cat > labrynth-deb/DEBIAN/control << EOF
Package: labrynth
Version: 1.1.1
Architecture: amd64
Maintainer: Your Name <email@example.com>
Description: REACHER Labrynth Application
 Frontend application for REACHER behavioral control system.
EOF

# Create desktop entry
cat > labrynth-deb/usr/share/applications/labrynth.desktop << EOF
[Desktop Entry]
Name=Labrynth
Exec=/usr/local/bin/labrynth
Type=Application
Categories=Science;
EOF

# Build DEB
dpkg-deb --build labrynth-deb
mv labrynth-deb.deb labrynth_1.1.1_amd64.deb
```

### Automated Building with GitHub Actions

Example workflow (`.github/workflows/build-installers.yml`):

```yaml
name: Build Installers

on:
  push:
    tags:
      - 'v*'

jobs:
  build-windows:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: '3.11'
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install pyinstaller
      - name: Build executable
        run: pyinstaller ui/setup.spec --clean
      - name: Upload artifact
        uses: actions/upload-artifact@v2
        with:
          name: labrynth-windows
          path: ui/dist/main.exe

  build-macos:
    runs-on: macos-latest
    steps:
      # Similar to Windows

  build-linux:
    runs-on: ubuntu-latest
    steps:
      # Similar to Windows
```

---

## Version Management

### Semantic Versioning

Follow [Semantic Versioning](https://semver.org/): `MAJOR.MINOR.PATCH`

- **MAJOR:** Breaking changes (e.g., v1.0.0 → v2.0.0)
- **MINOR:** New features, backward compatible (e.g., v1.1.0 → v1.2.0)
- **PATCH:** Bug fixes, backward compatible (e.g., v1.1.1 → v1.1.2)

### Version File Locations

**REACHER:**
- `setup.py`: `version="1.1.1"`
- `src/reacher/__init__.py`: `__version__ = "1.1.1"`

**Labrynth:**
- `requirements.txt`: `reacher==1.1.1` or `reacher>=1.1.1`
- Version displayed in UI (if applicable)

**Firmware:**
- Comment header in each `.ino` file:
  ```cpp
  /**
   * @file operant_FR.ino
   * @version 1.0.1-alpha
   * @date 2025-01-15
   */
  ```

### Updating Versions

When preparing a release:

1. **Decide version number** based on changes
2. **Update all version files**
3. **Update CHANGELOG.md**
4. **Create git tag**

```bash
# Update version in files, then:
git add .
git commit -m "chore: Bump version to 1.2.0"
git tag -a v1.2.0 -m "Release v1.2.0"
git push origin main
git push origin v1.2.0
```

### Changelog Management

Maintain CHANGELOG.md in each repository:

```markdown
# Changelog

All notable changes to this project will be documented in this file.

## [1.2.0] - 2025-02-15

### Added
- Solenoid device support
- Network session mode (beta)

### Changed
- Improved serial connection stability

### Fixed
- Timestamp offset calculation bug

## [1.1.1] - 2025-01-20

### Fixed
- Hardware tab UI layout issue
- Data export path handling on Windows
```

---

## Release Process

### Pre-Release Checklist

- [ ] All tests passing
- [ ] Version numbers updated in all files
- [ ] CHANGELOG.md updated
- [ ] Documentation updated
- [ ] Integration testing completed
- [ ] BREAKING.md created (if breaking changes)
- [ ] MIGRATION.md created (if needed)

### Release Steps

#### 1. Firmware Release

```bash
cd reacher-firmware
git checkout main
git pull origin main

# Create release branch
git checkout -b release/v1.1.0

# Update version in file headers
# Update README.md

git add .
git commit -m "chore: Prepare v1.1.0 release"
git push origin release/v1.1.0

# Create PR to main
# After merge:
git checkout main
git pull
git tag -a v1.1.0 -m "Release v1.1.0"
git push origin v1.1.0
```

**GitHub Release:**
1. Go to repository → Releases → New Release
2. Choose tag: v1.1.0
3. Title: "REACHER Firmware v1.1.0"
4. Description: Copy from CHANGELOG.md
5. Upload assets:
   - operant_FR.zip
   - operant_PR-beta.zip
   - etc.
6. Mark pre-release if beta/alpha
7. Publish

#### 2. REACHER Package Release

```bash
cd reacher
git checkout main
git pull origin main

# Build package
python setup.py sdist bdist_wheel

# Test installation
pip install dist/reacher-1.1.1-py3-none-any.whl

# Create release
git tag -a v1.1.1 -m "Release v1.1.1"
git push origin v1.1.1
```

**GitHub Release:**
- Upload wheel file: `reacher-1.1.1-py3-none-any.whl`
- Upload tarball: `reacher-1.1.1.tar.gz`

**PyPI Upload** (optional):
```bash
pip install twine
twine upload dist/*
```

#### 3. Labrynth Application Release

```bash
cd labrynth
git checkout main
git pull origin main

# Update requirements.txt to use new REACHER version
# reacher==1.1.1

# Build for each platform (or use CI/CD)
# See Building and Packaging section

# Create release
git tag -a v1.1.1 -m "Release v1.1.1"
git push origin v1.1.1
```

**GitHub Release:**
- Upload: `labrynth_x64.exe` (Windows)
- Upload: `labrynth_x64.dmg` (macOS)
- Upload: `labrynth_amd64.deb` (Linux)

### Post-Release

1. **Announce release:**
   - Update main README with latest version
   - Notify users/collaborators

2. **Monitor for issues:**
   - Watch GitHub Issues
   - Respond to bug reports promptly

3. **Update documentation:**
   - Ensure setup guides reference new version
   - Update compatibility matrix

---

## Contribution Guidelines

### For External Contributors

1. **Fork the repository** on GitHub

2. **Clone your fork:**
   ```bash
   git clone https://github.com/YOUR-USERNAME/reacher.git
   cd reacher
   git remote add upstream https://github.com/Otis-Lab-MUSC/reacher.git
   ```

3. **Create feature branch:**
   ```bash
   git checkout -b feature/your-feature
   ```

4. **Make changes and commit:**
   ```bash
   git add .
   git commit -m "feat: Add feature description"
   ```

5. **Push to your fork:**
   ```bash
   git push origin feature/your-feature
   ```

6. **Create Pull Request:**
   - Go to original repository on GitHub
   - Click "New Pull Request"
   - Select your fork and branch
   - Fill out PR template

### Pull Request Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix (non-breaking change fixing an issue)
- [ ] New feature (non-breaking change adding functionality)
- [ ] Breaking change (fix or feature causing existing functionality to break)
- [ ] Documentation update

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex code
- [ ] Documentation updated
- [ ] No new warnings generated
- [ ] Tests added for new functionality

## Related Issues
Closes #123
```

### Code Review Process

1. **Automated checks run:**
   - Linting
   - Unit tests
   - Build verification

2. **Maintainer reviews:**
   - Code quality
   - Functionality
   - Documentation
   - Tests

3. **Revisions:**
   - Address feedback
   - Push updates to same branch

4. **Approval and merge:**
   - Maintainer merges PR
   - Branch deleted

---

## Best Practices Summary

### General

- ✅ Always work in feature branches
- ✅ Write clear commit messages
- ✅ Keep changes focused and atomic
- ✅ Update documentation with code
- ✅ Test thoroughly before pushing

### Python

- ✅ Follow PEP 8 style guide
- ✅ Use type hints
- ✅ Write docstrings
- ✅ Add unit tests for new functions
- ✅ Use virtual environments

### Arduino

- ✅ Follow consistent naming conventions
- ✅ Comment complex logic
- ✅ Keep functions small and focused
- ✅ Test on actual hardware
- ✅ Use Doxygen documentation

### Multi-Repository

- ✅ Coordinate breaking changes
- ✅ Maintain version compatibility
- ✅ Test integration end-to-end
- ✅ Document dependencies
- ✅ Create clear migration guides

---

## Additional Resources

### Documentation

- [REACHER Documentation](https://github.com/Otis-Lab-MUSC/reacher)
- [Labrynth Documentation](https://github.com/Otis-Lab-MUSC/labrynth)
- [Firmware Documentation](https://github.com/Otis-Lab-MUSC/reacher-firmware/docs)

### External Resources

- [Git Documentation](https://git-scm.com/doc)
- [Python Package Guide](https://packaging.python.org/)
- [Arduino Reference](https://www.arduino.cc/reference/en/)
- [Panel Documentation](https://panel.holoviz.org/)
- [PySerial Documentation](https://pyserial.readthedocs.io/)

### Community

- **GitHub Issues:** Report bugs and request features
- **Discussions:** Ask questions and share ideas
- **Email:** thejoshbq@proton.me (maintainer)
- **Lab Website:** http://www.otis-lab.org

---

## Citation

If using these resources, please cite:

> Doncheck, E.M. et al. Drug self-administration in head-fixed mice. *Nat. Protoc.* (2026). https://doi.org/PLACEHOLDER

---

*Last updated: January 2026*
