# WSLg Internal Architecture

WSLg runs inside a specialized system distribution within WSL2. It provides the infrastructure required for Linux GUI applications.

---

## WSLGd (WSLg Daemon)

WSLg internally manages its components through a daemon called **WSLGd**.

WSLGd is responsible for:

- Initializing environment variables
- Starting Weston
- Starting XWayland
- Starting PulseAudio
- Managing D-Bus session services
- Configuring shared memory mounts via virtiofs
- Managing RDP backend connectivity

This ensures GUI support is automatically available when WSL starts.

---

## Weston (Wayland Compositor)

Weston runs inside the WSL2 VM and acts as the display server.

Responsibilities:

- Receives buffers from Wayland clients
- Composites windows
- Handles input routing
- Sends rendered output to the RDP backend

Weston does not render directly to physical hardware. Instead, it uses an RDP backend to transmit frames to Windows.

---

## XWayland

XWayland provides compatibility for legacy X11 applications.

Flow:

X11 App → XWayland → Wayland protocol → Weston → RDP backend → Windows

This allows older Linux GUI applications to function without modification.

---

## Shared Memory and Mounts

WSLg uses virtiofs for efficient file and memory sharing between Windows and the WSL2 VM.

The directory:

/mnt/wslg

Contains runtime sockets and shared resources, including:

- Wayland socket
- PulseAudio socket
- XDG runtime directories

---

## GPU Acceleration

WSLg supports GPU acceleration via the WSL2 GPU paravirtualization stack.

Linux applications can access GPU acceleration, and rendered frames are still transmitted through the RDP backend.