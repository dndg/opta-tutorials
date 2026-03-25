# Getting started with Finder OPTA and PLC IDE

Guide for installing Arduino PLC IDE and activating Finder OPTA license.

## Overview

**Arduino PLC IDE** allows you to program **Finder OPTA** using the five
standard **IEC 61131-3** languages: Ladder Diagram (LD), Functional Block
Diagram (FBD), Structured Text (ST), Sequential Function Chart (SFC), and
Instruction List (IL).

This guide shows how to connect Finder OPTA to Arduino PLC IDE, activate the
device license, and configure it for proper use with the development
environment.

## What you will need

Before you begin, make sure you have:

- [PLC Finder OPTA](https://opta.findernet.com/en/arduino) (x1)
- USB-C cable (x1)
- The **Arduino PLC IDE** installer. You can download it [at this
  link](https://opta.findernet.com/en/software#arduino).

## Instructions

To get the Arduino PLC IDE software, visit the official site and download the
installer named **Arduino PLC IDE Installer**. The software is compatible with
Windows 10 or later on x64 architectures.

![Website](assets/en/00-website.png)

The installer includes the IDE and all necessary drivers, libraries, and cores
to use Finder OPTA.

**_Note: If you've previously installed Arduino PLC IDE, ensure old versions
are removed. Also, delete the folder `C:\Users\<username>\AppData\Local\T`._**

### Installing Arduino PLC IDE

Before starting, you must accept the license agreement. Check the confirmation
box and click _Install_ to proceed.

![License Agreement](assets/01-license-agreement.png)

First, the **PLC IDE Tools** will be installed, a set of essential tools for
the IDE to function correctly. Click _Next_ to continue.

![PLC IDE Tools](assets/02-plc-ide-tools.png)

Next, you’ll be asked to choose an installation directory for the **PLC IDE
Tools**. Choose a destination folder or use the default path, then click
_Next_:

![Destination Folder](assets/03-dest-folder.png)

Click _Install_ to start installing **PLC IDE Tools** in the selected
destination:

![Installation](assets/04-installation.png)

Once installation completes, press the _Finish_ button to continue.

![Completion](assets/05-completed.png)

Now the Arduino PLC IDE setup will launch. Again, select a destination folder
and press _Next_.

![PLC IDE Install](assets/06-plc-ide-folder.png)

Proceed with installation by clicking the _Install_ button.

![Installation](assets/07-installation.png)

After the installation completes, press _Finish_ to close the window.

![Installation Success](assets/08-installation-success.png)

A final window will appear; close it to exit the installer.

![Setup Success](assets/09-setup-success.png)

**_Note: If the software does not work correctly after installation, restarting
your computer can help complete the integration of drivers, libraries, and
dependencies. If issues persist, try reinstalling with antivirus disabled and
running the setup with administrative privileges._**

### Setting up Finder OPTA memory

Arduino PLC IDE version 1.1 and above requires your Finder OPTA to have its
memory set up in a specific way. The easiest way to do this is with the **OPTA
Arduino Factory Reset** app, which handles everything automatically.

[Download OPTA Arduino Factory Reset](https://opta.findernet.com/software#arduino), follow the steps in the app,
then come back here to continue.

Make sure you're using OPTA Arduino Factory Reset v2.0.0 or above
alongside Arduino PLC IDE 1.1 or above. When in doubt, always download the
latest versions from Finder's website.

![Factory reset](assets/10-factory-reset.png)

### Creating the project and installing the runtime

This section explains how to create a new project with Finder OPTA and install
the runtime on the device using Arduino PLC IDE. This step is essential as the
runtime acts as a communication bridge between Finder OPTA and the development
environment.

First, **connect Finder OPTA to your computer using the USB-C cable** and open
Arduino PLC IDE. The welcome screen will appear:

![New Project](assets/en/01-new-project.png)

To begin, create a new project by clicking the _New project_ button or
_File > New project_. Before proceeding, make sure to select the latest system
version available for the project: choose _Opta 1.x_, where _x_ is the highest
number listed.

![Project Name](assets/en/02-project-name.png)

The new project screen will look like the following:

![Project Name](assets/en/03-empty-project.png)

Click the _Resources_ tab in the bottom left, or click **View > Tool
Windows > Resources**. A tree structure will open with an _Opta_ entry;
click it to access Finder OPTA configuration screen.

![OPTA Menu](assets/en/04-opta-menu.png)

Now let’s download the runtime to the device. Scroll down the configuration
page until Finder OPTA COM port selector appears.

![COM Port](assets/en/05-com-port.png)

Click _Download_ to install the software on Finder OPTA.

![Runtime Download](assets/en/06-runtime-download.png)

**_Note: Always download the runtime whenever you update Arduino PLC IDE to a
new version. This ensures proper communication between Finder OPTA and Arduino
PLC IDE._**

#### Error resolution

During the download, you may see an error message like this:

![Runtime Download Error](assets/en/07-runtime-download-error.png)

If this appears, follow these steps:

1. Press the reset button twice using a pen tip or sharp object.
2. Ensure the LED above the reset button blinks.
3. Click the _Download_ button again to retry the runtime installation.

If problems persist, run Arduino PLC IDE as an administrator. This can resolve
permission issues preventing communication with Finder OPTA.

### Connecting to the device

After installing the runtime, you need to configure communication with Finder
OPTA. Open the menu _On-line > Set up communication_.

![Connection Setup](assets/en/08-connection-setup.png)

From the window that appears, access the Modbus protocol properties.

![Connection Setup](assets/en/09-modbus.png)

Verify that `Port` is the COM port of Finder OPTA.

![Connection Setup](assets/en/10-modbus-settings.png)

Click _OK_ to apply changes and exit communication settings. Now connect to
the device by clicking _On-line > Connect_.

![Connect](assets/en/11-connect.png)

Once connected, the configuration screen will show the device details, license
status, and connection status in the bottom panel.

![Connection Success](assets/en/13-connection-success.png)

### Activating Finder OPTA license

Finder OPTA comes with a pre-granted license that must be activated. On the
device configuration screen, click the _Activate PLC runtime_ button while
Finder OPTA is connected to the PC.

A confirmation message will notify that Arduino PLC IDE will disconnect. Press
_OK_ to proceed.

![License Activation](assets/en/14-license-activation.png)

Another message will ask you to restart the device after activation. Press
_OK_ to continue.

![Restart Target](assets/en/15-restart-target.png)

Restart Finder OPTA by pressing the reset button once, then repeat the
connection procedure.

![License Activated](assets/en/16-license-activated.png)

The device activation is complete. The **Status** of Finder OPTA is **OK** and
it is ready to use with Arduino PLC IDE.

### Downloading a program

To run the first program on Finder OPTA, we’ll use the default counter code
included in the project. This simple counter increments its value by 1 every
execution cycle. The goal is to verify the device runs code correctly and
updates values in real-time inside Arduino PLC IDE.

Open the _Project_ section next to _Resources_, or click **View > Tool
Windows > Project**. Then select the _main_ item from the tree to display the
counter code.

![Code](assets/en/17-code.png)

To download the program to the device, compile the code using the top-left
button.

![Code Compilation](assets/en/18-code-compilation.png)

Now start downloading the program by clicking _On-line > Download Code_.

![Code Download](assets/en/19-code-download.png)

A confirmation message will appear, click _Yes_ to proceed.

![Confirm Reset](assets/en/20-confirm-reset.png)

The code has now been successfully downloaded to the device, which is now
executing it.

### Verifying program execution

To verify the program is running properly on Finder OPTA, monitor the counter
value directly inside Arduino PLC IDE. This is done using a **Watch**, a tool
to access and monitor variable values.

You must be connected to Finder OPTA. If your PC disconnected after
downloading, reconnect as before. Then go to _View > Tool Window > Watch_.

![Add Watch](assets/en/21-add-watch.png)

A panel will open on the right with an _Insert new item_ button. Click it.

![Add New Item](assets/en/22-add-new-item.png)

We’re now selecting variables to monitor. Click the first _Browse_ button.

![Browse Variables](assets/en/23-browse-variables.png)

From the program variable list, select `cnt`, which represents the counter
value.

![Variables List](assets/en/24-variables-list.png)

Press _OK_ to apply and close the windows. If the variable is being monitored
correctly, you’ll see a screen like this.

![Real Time Reading](assets/en/25-real-time-reading.png)

If the `cnt` counter — inside the highlighted red cell — is constantly
increasing, it means Finder OPTA is executing the code and updating data in
real time inside Arduino PLC IDE.

## Conclusion

You have successfully completed the initial setup of **Finder OPTA** with
**Arduino PLC IDE**. Now that **Finder OPTA** is properly configured and fully
operational, you can start exploring the potential of **IEC 61131-3**
languages.

<!-- To learn more, we recommend following the next [tutorial](...). -->
