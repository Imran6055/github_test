# WSLg Architecture Overview

WSLg (Windows Subsystem for Linux GUI) enables Linux graphical applications (Wayland and X11) to run seamlessly on Windows through WSL2.

Unlike traditional X11 forwarding, WSLg renders Linux applications inside the WSL2 virtual machine using a Wayland compositor (Weston). The rendered output is then transmitted to Windows using Remote Desktop Protocol (RDP).

This architecture allows Linux GUI applications to appear as native Windows applications while preserving hardware acceleration and audio integration.

---

## 📚 Documentation Structure

For detailed information, refer to the following documents:

1. [WSLg Internal Architecture](./wslg-architecture.md)
2. [Wayland and X11 in WSLg](./wayland-vs-x11.md)
3. [D-Bus in WSLg](./dbus-in-wslg.md)
4. [Audio Architecture in WSLg](./wslg-audio.md)
5. [RAIL Integration in WSLg](./wslg-rail-integration.md)

---

## Key Design Principle

WSLg uses a Wayland compositor (Weston) running inside the WSL2 Linux VM. Rendered GUI frames are transmitted to Windows using RDP — not traditional X11 forwarding and not direct hardware rendering on Windows.

---

## High-Level Flow

1. A Linux GUI application is launched inside WSL.
2. The application renders using Wayland (or X11 via XWayland).
3. Weston composites the window.
4. The composed output is sent through an RDP backend.
5. Windows displays the application using RemoteApp (RAIL) integration.
6. Audio is forwarded through the RDP audio channel.

---

## Core Components

- WSL2 virtual machine
- WSLg system distribution
- Weston (Wayland compositor)
- XWayland (X11 compatibility)
- PulseAudio (audio server)
- D-Bus (IPC)
- RDP backend with RAIL support