# <p align="center"><img src="logo.png" alt="Linux Hello Logo" width="180"><br>Linux Hello</p>

`Linux Hello` is a polished, feature-rich Python wrapper and PyQt6 graphical management panel designed to bring a premium, Windows Hello™-style facial authentication experience to Linux systems (specifically tested on TUXEDO OS / Ubuntu 24.04 Noble).

It runs seamlessly on modern desktops, handles dynamic webcam priority/hotplugging, guides the user with a real-time visual enrollment assistant, and secures the face signature database from unauthorized profile additions using a built-in Euclidean matching verification algorithm.

---

## Key Features

*   **Sleek PyQt6 Graphical Interface**: Styled with a Catppuccin Mocha theme (dark slate, lavender/blue highlights) applied globally across all dialogs and popup windows.
*   **Live Preview Enrollment Assistant**: Shows a real-time `320x240` video frame canvas inside the enrollment dialog to help you frame your face correctly.
*   **5-Second Intelligent Countdown**: Counts down from 5 seconds before capturing a scan, automatically releasing the camera node hardware lock before Howdy launches (preventing webcam capture conflicts on Linux).
*   **Face Profile Security Matcher**: Integrates a custom verification check when enrolling or improving a profile. It compares the newly scanned face vector against existing ones in `/usr/share/dlib/<user>.dat`. If the Euclidean distance is $> 0.55$, it alerts the user, rejects the addition, and auto-purges the signature.
*   **Unified Profile Grouping**: Groups multiple facial signatures under a single user profile list item (e.g. `Profile: kieran (2 signatures)`) to reduce UI clutter.
*   **Dynamic Camera Switcher**: Configures camera priorities (e.g. favoring your external IR webcam over internal laptop cams) and updates active Howdy settings dynamically via `udev` hotplug rules and `systemd` services.
*   **Safe Wayland Integration**: Runs as a standard non-root user for Wayland compatibility, requesting administrative rights via PolicyKit (`pkexec`) only when executing system tasks.

---

## Installation & Setup

1.  **Run the Installer**:
    Clone the repository and run the zero-frustration system installer:
    ```bash
    sudo ./linux-hello install
    ```
    This script will:
    *   Add the required `ppa:ubuntuhandbook1/howdy` repository.
    *   Install Howdy, OpenCV, v4l-utils, and PyQt6.
    *   Deploy the executable to `/usr/local/bin/linux-hello`.
    *   Create a desktop launcher entry `/usr/share/applications/linux-hello.desktop` so you can launch it from your application launcher dashboard.

2.  **Configure Camera Priority**:
    Open the GUI, select your preferred video devices (IR cameras will be marked with `[IR]`), arrange their order using the Up/Down buttons, and click **Save Priority**.

3.  **Toggle Switching**:
    Click **Toggle Switching** to enable udev rules that automatically point Howdy to the highest-priority connected camera whenever USB webcams are plugged or unplugged.

4.  **Enroll Your Face**:
    Click **Enroll Face** (or select a profile and click **Improve...**), align your face in the camera frame during the 5-second countdown, and wait for verification to succeed.

5.  **Enable PAM**:
    Click **Toggle PAM** (or run `sudo linux-hello enable-pam`) to activate face authentication for login, locks, and `sudo` commands.

---

## CLI Command Usage

For administrators who prefer the command line:

```bash
Usage: sudo linux-hello [command]

Available Commands:
  status             Show configuration, packages, camera list, and active state.
  install            Add repository and install Howdy & dependencies.
  setup-camera       Configure which camera path Howdy should use.
  test-camera        Launch an interactive ASCII terminal test stream for a camera.
  enroll             Scan your face and add a new face signature.
  enroll-and-verify  Securely enroll face signature with mismatch checking. (requires root)
  enable-pam         Safely configure PAM to enable facial login and sudo authentication.
  disable-pam        Disable facial recognition and restore default PAM authentication.
  set-priority       Save camera list in preferred order. (requires root)
  udev-trigger       Update Howdy config to highest priority connected camera.
  enable-switching   Install and enable dynamic switching udev & systemd service.
  disable-switching  Remove dynamic switching rules and service.
  gui                Open the graphical manager control panel.
  help               Display this help message.
```

---

## Credits & References

`Linux Hello` acts as a polished wrapper around several prominent open-source projects:

*   **Howdy** (https://github.com/boltgolt/howdy): The primary Windows Hello™-style PAM authentication system for Linux.
*   **Dlib** (http://dlib.net/): The machine learning toolkit used by Howdy to run facial landmark detection and 128D neural network feature extraction.
*   **OpenCV** (https://opencv.org/): Powering the live video capture, window previewing, and ASCII rendering pipelines.
*   **PyQt6** (https://www.riverbankcomputing.com/software/pyqt/): Providing the cross-platform Qt6 application framework bindings.

---

## License

This project is licensed under the MIT License - see the `LICENSE` file for details.

---

## Trademarks & Disclaimer

Windows Hello™ is a registered trademark of Microsoft Corporation in the United States and/or other countries. `Linux Hello` is an independent open-source project and is not affiliated with, sponsored by, or endorsed by Microsoft Corporation.
