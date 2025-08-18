# SATO BLE Vision Installation Guide

This guide walks you through setting up the SATO BLE Vision environment on a Windows x64 system, including Python installation, essential package installations, Mosquitto MQTT broker setup, configuration changes, and preparing your Python program for execution.

## Table of Contents
1. [Install Python 3.7+](#1-install-python-37-windows-x64)
2. [Install SATO BLE Vision Package](#2-install-sato-ble-vision-package)
3. [Install and Configure Mosquitto MQTT Broker](#3-install-and-configure-mosquitto-mqtt-broker)
4. [Configure SATO BLE Vision](#4-configure-sato-ble-vision)
5. [Launch and Run](#5-launch-and-run)
6. [Troubleshooting](#6-troubleshooting)

---

## 1. Install Python 3.7+ (Windows x64)

### Step 1.1: Download Python 3.7.7 or Higher
- **Visit the official Python release archive (minimum version):**
  - https://www.python.org/downloads/release/python-377/
- **Or get the newest version here:**
  - https://www.python.org/downloads/release/python-3130/
- **Scroll to Files section.**
- **Download:**
  - Windows x86-64 executable installer (For 64-bit systems).

### Step 1.2: Install Python
1. **Launch the installer.**
2. **At the bottom of the installer window, check the box:**
   ```
   ☑ Add Python to PATH
   ```
3. **Click on "Customize installation":**
   - Proceed through the defaults.
   - On the final screen, choose:
     - ☑ Install for all users (Optional but recommended)
     - ☑ Add Python to environment variables (if not already selected)
4. **Click Install.**

### Step 1.3: Verify Installation
- **Open Command Prompt**
- **Type:**
  ```bash
  python --version
  pip --version
  ```
- **Expected output:**
  ```
  Python 3.7.7 (or higher)
  pip 20.x.x (or higher)
  ```

---

## 2. Install SATO BLE Vision Package

### Step 2.1: Upgrade pip
```bash
python -m pip install --upgrade pip
```

### Step 2.2: Install SATO BLE Vision

### Install from PyPI (Recommended)
```bash
pip install sato-ble-vision
```


### Step 2.3: Verify Installation
```bash
sato-ble-vision --help
```
or
```bash
python -m sato_ble_vision.main --help
```

---

## 3. Install and Configure Mosquitto MQTT Broker

### Step 3.1: Download Mosquitto
- **Visit:**
  - https://mosquitto.org/download/
- **Under Windows, download the Mosquitto installer (.exe) for your system (x64).**

### Step 3.2: Install Mosquitto
- **Run the installer.**
- **Important: During installation, choose to install service, documentation, and default config.**
- **After installation, add the Mosquitto install path to system PATH:**
  1. Open Start Menu → search for "Environment Variables"
  2. Click on "Environment Variables"
  3. Under System Variables, find and edit "Path"
  4. Add the path to your Mosquitto install, e.g.:
     ```
     C:\Program Files\mosquitto
     ```

### Step 3.3: Update mosquitto.conf File
1. **Navigate to:**
   ```
   C:\Program Files\mosquitto
   ```
2. **Locate the mosquitto.conf file.**

#### Step 3.3.1: Modify File Permissions (Admin Access)
- **Right-click mosquitto.conf → Properties → Security tab.**
- **Click Edit to change permissions.**
- **Select Users, then check Allow for Modify and Write.**
- **Click Apply and OK.**

#### Step 3.3.2: Edit Configuration
- **Open mosquitto.conf in Notepad (right-click → Edit).**
- **Add the following lines at the top or end:**
  ```conf
  listener 1883 0.0.0.0
  allow_anonymous true
  ```

---

## 4. Configure SATO BLE Vision

### Step 4.1: Initial Configuration
The first time you run SATO BLE Vision, it will create a default configuration file in your home directory:
```
~/.sato_ble_vision/configuration.json
```

### Step 4.2: Edit Configuration File
Open the configuration file and update it with your API credentials:

```json
{
    "last_used": "DEFAULT",
    "DEFAULT": {
        "AuthURL": "https://api.wiliot.com/v1/auth/token/api",
        "ResolveUrl": "https://api.wiliot.com/v1/owner/YOUR_OWNER_ID/resolve",
        "AuthKey": "YOUR_AUTH_KEY_HERE",
        "TOPIC": "eiotpv1/printer/#"
    }
}
```

### Step 4.3: Required Configuration Fields
- **AuthURL**: Authentication endpoint URL
- **ResolveUrl**: Tag resolution API endpoint
- **AuthKey**: Your API authentication key
- **TOPIC**: MQTT topic to subscribe to

---

## 5. Launch and Run

### Step 5.1: Launch SATO BLE Vision
You can launch the application in several ways:

#### Option A: Using the Command Line
```bash
sato-ble-vision
```

#### Option B: Using Python Module
```bash
python -m sato_ble_vision.main
```

#### Option C: Create a Batch File (Optional)
Create a `run_sato_ble_vision.bat` file:
```batch
@echo off
cd /d "C:\path\to\your\project\directory"
sato-ble-vision
pause
```

### Step 5.2: Using the GUI
1. **Start Broker**: Click to start the Mosquitto MQTT broker
2. **Connect Client**: Connect to the MQTT broker
3. **Start Logging**: Begin recording tag data to CSV files
4. **View Data**: Switch between different data views:
   - Tag Data: Individual tag events
   - Serial Log: Grouped by serial number
   - General Log: System messages
   - Topic Log: Raw MQTT messages

---

## 6. Troubleshooting

### Common Issues and Solutions

#### Issue 1: "Mosquitto not found"
**Solution:**
- Ensure Mosquitto is installed and in your system PATH
- Verify the installation path is added to environment variables
- Restart your command prompt after PATH changes

#### Issue 1.1: "Mosquitto already running on port 1883"
**Solution:**
- Check if Mosquitto is already running:
  ```bash
  netstat -ano | findstr :1883
  ```
- If you see port 1883 in use, find the process ID (PID) and kill it:
  ```bash
  taskkill /PID <PID> /F
  ```
- Or kill all Mosquitto processes:
  ```bash
  taskkill /IM mosquitto.exe /F
  ```
- Then restart Mosquitto from the SATO BLE Vision application

#### Issue 2: "Configuration errors"
**Solution:**
- Check your API credentials in the configuration file
- Verify the configuration file path: `~/.sato_ble_vision/configuration.json`
- Ensure JSON syntax is valid

#### Issue 3: "Permission errors"
**Solution:**
- Ensure the application has write access to your home directory
- Run as administrator if necessary
- Check file permissions on the configuration directory

#### Issue 4: "Python version issues"
**Solution:**
- Ensure you're using Python 3.7 or higher
- Verify the correct Python version is in your PATH
- Use `python --version` to confirm

#### Issue 5: "Package installation fails"
**Solution:**
- Upgrade pip: `python -m pip install --upgrade pip`
- Try installing with verbose output: `pip install -v sato-ble-vision`
- Check your internet connection and firewall settings

### Log Files
Log files are stored in `~/.sato_ble_vision/logs/` and include:
- Tag event data
- Raw MQTT messages
- Error logs

### Getting Help
- **GitHub Issues**: https://github.com/dylanbarnett99/sato-ble/issues
- **Documentation**: https://github.com/dylanbarnett99/sato-ble
- **PyPI Page**: https://pypi.org/project/sato-ble-vision/

---

## Additional Resources

- **SATO BLE Vision GitHub Repository**: https://github.com/dylanbarnett99/sato-ble
- **PyPI Package Page**: https://pypi.org/project/sato-ble-vision/
- **Mosquitto Documentation**: https://mosquitto.org/documentation/
- **Python Documentation**: https://docs.python.org/

---

*This installation guide is based on the original IoT3dge installation instructions, adapted for the SATO BLE Vision package.*
