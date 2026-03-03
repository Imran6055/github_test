# D-Bus in WSLg

D-Bus (Desktop Bus) is an inter-process communication (IPC) system used widely across Linux desktop environments.

In WSLg, D-Bus enables communication between GUI applications and system services.

---

## Purpose

D-Bus allows:

- Notifications
- Clipboard integration
- Session management
- Desktop portals
- Application coordination

---

## Bus Types

There are two primary buses:

- System Bus → Used by system-level services
- Session Bus → Used by user applications

WSLg ensures a functional session bus for GUI applications.

---

## Architecture

D-Bus uses a client–server model:

- A bus daemon routes messages
- Applications send structured messages
- Signals and method calls enable communication

This allows Linux desktop services to operate normally within WSLg.