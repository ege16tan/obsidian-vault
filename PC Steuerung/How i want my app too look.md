# How I Want the App to Look

These notes describe the setup experience only. The rest of the app can be designed later.

## 1. Windows PC Setup

### Installer

- Install the PC agent with the Windows installer.
- On the final installer page, show that the Web UI uses port **8000**.
- Provide a button to **Open setup / Web UI** when installation finishes.
- The installer should work without administrator rights wherever possible. If Windows requires elevation, ask only once and only for the required step.

### First setup in the Web UI

Use a short, step-by-step wizard with a **Next** button:

1. **PC name** — ask what this computer should be called in the Android app.
2. **Wake-on-LAN** — disabled by default. When enabled, animate in a MAC-address field and offer a way to find the computer's network adapters automatically. Show the selected MAC address and the PC's IP address in the Web UI.
3. **Sign-in** — create a username and password. An existing token-based option may also be offered, but mark it as not recommended.
4. **Finish** — save the setup and show the Android connection QR code.

The QR code should make Android setup quick. It may contain the PC name, IP address, port, and selected Wake-on-LAN MAC address. It must never contain the username, password, or token; those are entered on the phone.

## 2. Android App Setup

On first launch, show a setup wizard with **Next** and two ways to connect.

### QR-code setup

1. Choose **Set up with QR code** and scan the code shown by the PC Web UI.
2. Fill in the PC name, IP address, port, and Wake-on-LAN MAC address from the code.
3. Ask for the username and password, or the token if token sign-in was selected.
4. Save the profile and open the app.

### Manual setup

1. Configure Wake-on-LAN first. It should be optional and can stay disabled.
2. Enter the PC's IP address and port.
3. Try the connection and show whether the PC can be reached.
4. Ask for the username and password, or token, then save the profile and open the app.

## Scope

For now, implement and refine these setup screens and flows. The main app design is outside this note's scope.
