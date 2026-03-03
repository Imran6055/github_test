# Audio Architecture in WSLg

WSLg provides Linux audio support using PulseAudio integrated with the RDP audio channel.

---

# PulseAudio Overview

PulseAudio is a Linux sound server that manages audio streams between applications and output devices.

It provides:

- A server daemon
- Client libraries (libpulse)
- Audio routing modules
- Mixing and resampling

---

# PulseAudio Inside WSLg

WSLg runs a PulseAudio server inside the system distribution.

Flow:

Linux App → PulseAudio → RDP audio virtual channel → Windows audio subsystem

---

## Key Characteristics

- PulseAudio starts automatically with WSLg.
- It uses sockets under /mnt/wslg.
- It should not be manually started.
- Applications must use the PulseAudio backend.

---

## Environment Variable

Applications typically rely on:

export PULSE_SERVER=unix:$XDG_RUNTIME_DIR/pulse/native

---

## Why ALSA Devices Are Not Exposed

WSL2 does not expose physical ALSA devices directly.

Instead:

- Applications route audio to PulseAudio.
- PulseAudio forwards audio to Windows via RDP.

---

# RDP Audio Integration

WSLg forwards audio using Remote Desktop Protocol virtual channels.

This allows:

- Native Windows playback
- Synchronization with Windows audio devices
- Seamless audio experience

PulseAudio acts as the bridge between Linux applications and Windows audio.