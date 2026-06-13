# Security Policy

## Supported Versions

Only the latest release of `Linux Hello` is supported with security updates. Please ensure you are running the latest version from the master/main branch.

| Version | Supported          |
| ------- | ------------------ |
| >= 1.0  | :white_check_mark: |
| < 1.0   | :x:                |

## Security Model and Boundaries

`Linux Hello` acts as an administrative front-end and secure enrollment wrapper for [Howdy](https://github.com/boltgolt/howdy). Below is a breakdown of the security design, the nature of the face profiles, and the layers of protection active on the system.

### 1. Biometric Profiles & Face Vector Security (One-Way Hashing)
When you enroll your face, the system captures frames from your video camera in volatile system memory (RAM).
*   **Zero Image Retention**: Once the frames are processed, the raw images are **immediately discarded**. Neither `Linux Hello` nor `Howdy` saves photos, videos, or raw graphical bitmaps of your face onto the persistent disk storage.
*   **128D Feature Vectors**: The dlib ResNet-50 deep learning model analyzes facial landmarks and extracts a **128-dimensional floating-point array** (e.g. `[-0.1169, 0.2077, 0.0418, ...]`) representing the spatial geometry of your face.
*   **One-Way Biometric Hash**: This vector functions as a one-way mathematical abstraction of your face. It is mathematically impossible to reverse-engineer or reconstruct a visual image or photo of your face from this array of 128 floats.

### 2. Levels of Protection

#### Layer A: Data-at-Rest Protection
Enrolled face signatures are stored inside `/usr/share/dlib/<user>.dat` (or fallback directories like `/var/lib/howdy/models/`).
*   **File Permissions**: These database files are owned by `root:root` with standard read permissions (`-rw-r--r--`). While any system process can read the float vectors, **only the root user** or a process elevated via PolicyKit/sudo can write, edit, or delete them.
*   **PAM Security**: Authentication requests go through the Linux PAM (Pluggable Authentication Modules) stack, which executes face-matching locally under system-level PAM parameters.

#### Layer B: Profile Injection Prevention (The Security Matcher)
A common vulnerability in facial recognition wrappers is that an administrator or someone with temporary root access could inject *their own* face signature into *your* user profile. 
*   `Linux Hello` implements a **Euclidean Distance Matcher** during enrollment.
*   When a new signature is added to an existing user profile, the wrapper compares the new vector to all existing vectors using Euclidean distance.
*   If the minimum distance is $> 0.55$, the scanned face is identified as a different person. The signature is immediately purged from the database, the operation aborts with a security status code (`10`), and the GUI prompts a critical security warning. This prevents unauthorized users from hijacking your profile even if they obtain root credentials.

#### Layer C: Physical Spoofing Protection (RGB vs. IR Cameras)
The security level of your authentication setup depends directly on your camera hardware:
*   **RGB (Standard Webcams)**: Standard color cameras only capture light in the visible spectrum. They are highly vulnerable to simple 2D print or screen-based spoofing (e.g., holding up a high-resolution color photo or a phone screen displaying your face).
*   **IR (Infrared Webcams - Recommended)**: Active infrared webcams illuminate the scene with IR light and capture frames using an IR sensor. Standard printed photos or smartphone screens do not reflect IR light in the same way a human face does (they usually appear entirely blank, black, or flat to the IR sensor). Using an IR camera prevents 2D photo bypass attacks.

### 3. Local Access Limitations
If an attacker has physical access to your hardware, knows the root password, and can boot into a recovery shell, they can bypass any software security model by disabling the PAM modules or deleting the signature databases. `Linux Hello` is designed to secure interactive desktop screens, sudo command prompts, and remote/GUI login sessions.

## Reporting a Vulnerability

**Do not open public GitHub issues for security vulnerabilities.**

If you discover a security vulnerability within `Linux Hello`, please report it by sending an email with details, reproduction steps, and potential patches to the maintainer's contact email (if listed in the repository settings) or via a private vulnerability report on GitHub if available.

### Review Timeline

*   We will acknowledge receipt of your report within 48 hours.
*   We will provide a detailed evaluation and fix timeline within 7 days.
*   Once a fix is merged, we will coordinate public disclosure.
