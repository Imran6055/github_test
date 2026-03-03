WSLg (Windows Subsystem for Linux GUI) is the subsystem that enables Linux GUI apps (Wayland + X11) to run seamlessly inside Windows. It extends WSL 2’s VM‑based architecture.


![alt text](<Designer (3)-1.png>)


## Key note:
WSLg uses a **Wayland compositor (Weston)** running inside the WSL2 Linux VM, and then sends the rendered GUI frames to Windows using RDP — not X11 forwarding or local rendering.


# Components involved:
WSLg internally manages these processes via a daemon called **WSLGd**, which orchestrates everything needed to run GUI apps, including:

- Weston (Wayland compositor)
- XWayland (X11 → Wayland compatibility)
- PulseAudio (audio)
- D‑Bus daemon
- RDP client `(mstsc.exe or msrdc)` that displays the GUI output on Windows.

![alt text](<Screenshot 2026-01-19 113731.png>)

## How a Linux GUI App Becomes a Windows App

1. You launch a Linux GUI app inside WSL
   example: **thunar &**

   ![alt text](image-33.png)

2. WSLGd initializes or manages:

- Environment variables
- Shared memory mounts via virtiofs
- Weston compositor
- XWayland
- PulseAudio
- D‑Bus


# Wayland (Modern Linux Display Protocol):

Wayland is a display protocol and architecture designed to replace the X Window System. It defines how applications (clients) communicate with a **Wayland compositor**, which acts as the display server.

* A communication protocol between a display server (compositor) and its clients.
* Comes with both a protocol specification and a **C library** implementation.
* Meant to be simpler, more secure, and more efficient than X11.

### Architecture & Components:

Wayland uses a client–server model where:

* Clients: applications (e.g., gedit, Firefox)
* Server: the compositor (e.g., Weston, Mutter, KWin) - WSL uses Weston compositor
* Communication takes place via Unix domain sockets.

### How rendering works

* Kernel receives an input event through evdev (event device).
* The compositor examines its scenegraph to determine which window should receive the event.
* Client receives event and updates UI via EGL rendering.
* Client notifies compositor about the **damage region** (updated area).
* Compositor re‑composites the screen and submits directly to KMS (Kernel Mode Setting).

### Why Wayland over X11

Unlike X11:

* No extra round‑trips through an X server.
* Compositor has full control over window transformations and rendering.
* Eliminates redundant paths `no separate X renderer → compositor → hardware sequence`.

# X11 — The Traditional Linux/Unix Window System

X11 (or X Window System) is the older, widely adopted display system dating back to 1984.

* A network‑transparent windowing system
* Uses the X protocol for communication between clients and the X server.

### Architecture and Components

X11 is built on a client–server model, but inverted compared to typical terminology:

* **Xserver** runs on the user machine (connected to display) and it is responisible for manage Manages screen, keyboard, mouse, draws windows, handles input.
* **Xclient** runs application anywhere on the network  and its reponsible for requests drawing operations from the server


### Network Transperancy 

X11 was designed for remote GUIs: a client on Machine A can display on Machine B easily.

**Flow of Rendering**
1. Client sends drawing commands to X server.
2. X server renders graphics and handles input events.

### X11 Limitations

* Complex, outdated architecture
* Security weaknesses (any X client can log keys of others)
* Performance overheads (extra layers and context switching)

Wayland removes these with a compositor‑centric design.

# D‑BUS Inter‑Process Communication (IPC) on Linux

D‑Bus (Desktop Bus) is a message bus system for applications and services to communicate.

It’s used widely across: `GNOME & KDE Plasma, Systemd,Desktop services` (notifications, portals, session management)

* D-Bus Offers a shared communication channel for processes.
* Simplifies IPC by replacing complex socket/pipe setups with a central bus.

### Buses

There are two main buses:

* System Bus → Used by system daemons (NetworkManager, power management)
* Session Bus → Used by user-level applications

**D-BusCore Concepts:**

Messages: Method calls, Method returns, Signals (events broadcast to listeners)

Objects: Identified via object paths, implement interfaces

Interfaces: Define methods, signals, properties

**Names**
Two naming schemes:

* Unique names (runtime‑generated, like :1.42)
* Well‑known names (org.freedesktop.NetworkManager)

### D-Bus Architecture

* Implements a client‑server model where the server is the message bus daemon.
* Clients communicate via structured messages routed through the bus.

## What PulseAudio Is (The Real Audio System Behind WSLg)


PulseAudio is a sound server for Linux systems that provides a unified way for applications to output audio. It sits between audio‑producing applications and the underlying audio hardware or drivers.

Pluse Audio defines:
* A server process that manages audio streams
* A client API used by applications (e.g., Firefox, PortAudio, SOX)
* Modules for routing, mixing, resampling, and network audio

PulseAudio is widely used in Linux desktops and is the default audio system for many distributions.

## PulseAudio Architecture

PulseAudio is based on a client–server IPC model:

**PulseAudio server (daemon)**: Mixes audio streams, manages sinks/sources, handles routing.

**PulseAudio clients**: Applications sending audio (Firefox, VLC, PortAudio‑based apps)

**Modules**: Provide audio backend support (ALSA, network, RDP sinks)

**Protocol**: PulseAudio’s native TCP / Unix socket protocol

### Key details:

* Uses method calls + event signals similar to D‑Bus design patterns
* Provides virtual “sinks” (outputs) and “sources” (inputs)
* Can route audio over TCP, Unix sockets, or specialized modules

## How PulseAudio Works Specifically Inside WSLg

WSLg implements Linux audio by running a PulseAudio server inside the WSLg system distro, which communicates with Windows audio through the RDP audio channel.

**Architecture:**
WSLg launches a PulseAudio server as part of its GUI stack

**Inside WSLg**:

* Linux apps send audio to PulseAudio using the standard `libpulse` client libraries.
* PulseAudio loads special modules that output audio to WSLg’s RDP sink.
* Audio is forwarded to Windows using Remote Desktop Protocol virtual channels.
* Windows plays the sound using its native audio subsystem.

This explains why:

* Linux GUI apps (e.g., Firefox) have working audio in WSLg
* Headless CLI audio apps fail unless configured to use PulseAudio properly

### The PulseAudio system inside WSLg:

* Runs automatically when WSLg starts
* Uses $XDG_RUNTIME_DIR mounted under `/mnt/wslg`
* Is **not** meant to be manually run (errors occur if you try)

Since pulseaudio starts automatically under WSLg
If you try to manually run pulseaudio, you often see errors such as:

* This program is not intended to be run as root
* XDG_RUNTIME_DIR … is not owned by us

These occur because WSLg already runs PulseAudio internally.

## Key Behaviors and Implementation Notes

1. PulseAudio as the WSLg audio server:

Multiple sources confirm that WSLg uses PulseAudio for audio forwarding by default.

2. No virtual ALSA devices, ALSA doesn’t present virtual devices inside WSL2 instead:

Applications must route audio via PulseAudio
PortAudio, SOX, SDL, etc. must use the PulseAudio backend

3. Environment variables: 

PulseAudio clients in WSL typically rely on the environment variable:

`export PULSE_SERVER=unix:$XDG_RUNTIME_DIR/pulse/native`


## Pulse Audio Summary

![alt text](image-34.png)

PulseAudio is the **bridge** between Linux audio and Windows audio inside WSLg.

# Putting Them Together in WSLg Context

* Wayland is used for rendering modern Linux apps (native to Weston inside WSLg).
* X11 apps run via XWayland, translating X → Wayland.
* D‑Bus enables desktop services (clipboard sync, portals, notifications) within the Linux user session.
* Pulse audio is responsible for audio output inside WSLg




