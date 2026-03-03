# RAIL Integration in WSLg

WSLg uses RAIL (Remote Application Integrated Locally) to present Linux applications as native Windows windows.

RAIL is a feature of Remote Desktop Protocol that allows individual remote applications — not entire desktops — to appear as local applications.

---

## How RAIL Works in WSLg

1. Weston renders the Linux application.
2. The RDP backend transmits window content.
3. Windows receives the application as a RemoteApp.
4. The window integrates with the Windows desktop environment.

---

## User Experience Benefits

- Linux apps appear in the Windows taskbar.
- Applications can be pinned.
- Windows snapping works.
- Alt+Tab integration functions normally.
- Each Linux app runs as an independent window.

---

## Why RAIL Is Important

Without RAIL, WSLg would need to display a full remote desktop.

With RAIL:

- Each Linux app behaves like a native Windows application.
- There is no visible remote session.
- Integration feels seamless.

RAIL is a key component that makes WSLg practical for daily development workflows.