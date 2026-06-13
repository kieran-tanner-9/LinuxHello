# Contributing to Linux Hello

We welcome contributions from the community! Whether you want to fix bugs, add new features, improve the stylesheet theme, or write tests, your help is appreciated.

## How to Contribute

### 1. Reporting Bugs & Feature Requests
*   Before opening an issue, search existing open and closed issues to check if it has already been reported.
*   Clearly describe the bug, environment (OS, desktop environment, session type like X11 or Wayland), camera device details, and steps to reproduce.
*   Provide standard logs by running `linux-hello` in verbose mode or monitoring system logs (`journalctl -u linux-hello-switch.service` for switching issues).

### 2. Proposing Changes (Pull Requests)
1.  **Fork** the repository and create a new feature branch (`git checkout -b feature/amazing-new-feature`).
2.  Follow standard Python PEP 8 formatting guidelines.
3.  Ensure your code compiles and has no syntax warnings:
    ```bash
    python3 -m py_compile linux-hello
    ```
4.  **Local Testing**:
    *   Test the CLI subcommands using `sudo ./linux-hello [cmd]`.
    *   Test the PyQt6 GUI dashboard using `python3 ./linux-hello gui`.
5.  Commit your changes with clear, descriptive commit messages.
6.  Push your branch to your fork and submit a Pull Request to the `main` branch.

## Project Structure

*   `linux-hello`: The core Python script containing all CLI functions, udev trigger handlers, installation scripts, and the PyQt6 GUI dashboard layout.
*   `logo.png`: The official repository and desktop launcher brand logo.

## Code of Conduct

Please be respectful and constructive in all communication, issue discussions, and pull requests.
