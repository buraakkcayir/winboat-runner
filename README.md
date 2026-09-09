# WinBoat Runner

An intelligent, zero-friction wrapper script for running seamless Windows applications (such as Microsoft Office) inside a **Docker Windows container** on Linux via **FreeRDP 3**.

---

## ✨ Features

- **Dynamic Lazy Startup:** Starts the Docker container automatically only when an application is launched.
- **RDP PDU Handshake Probe:** Uses a lightweight Python socket probe that checks for genuine RDP Connection Request PDU negotiation, preventing premature FreeRDP connection crashes during cold boot or Windows updates.
- **Path Translation:** Seamlessly translates Linux local paths (`$HOME/...`) to Windows network share paths (`\\host\Data\...`) for direct document opening from file managers (e.g., Dolphin).
- **Automated Idle Shutdown:** Background daemon uses file locking (`flock`) to monitor active `xfreerdp` sessions and gracefully stops the Docker container after an idle timeout (default: 90 seconds) to conserve system memory and CPU.
- **PulseAudio / PipeWire Integration:** Native microphone and audio passthrough.

---

## 📦 Requirements

- **Docker:** Running a Windows container (e.g. `dockurr/windows`).
- **FreeRDP 3:**
```bash
sudo pacman -S freerdp
```
- **Python 3:** (for socket probe)

---

## ⚙️ Configuration

Copy the example configuration file to your user config directory:
```bash
mkdir -p ~/.config/winboat
cp winboat.conf.example ~/.config/winboat/winboat.conf
```

Edit `~/.config/winboat/winboat.conf` to set your credentials and container details:
```bash
CONTAINER_NAME="WinBoat"
IDLE_TIMEOUT=90
RDP_USER="your_windows_user"
RDP_PASS="your_windows_password"
HOST_SHARE=\host\Data
```

---

## 🚀 Usage

Install the script into your PATH (e.g., `~/.local/bin/winboat-runner`):
```bash
cp winboat-runner ~/.local/bin/
```

### Launching Applications:
```bash
# Open Microsoft Word
winboat-runner word

# Open a document directly
winboat-runner word ~/Documents/report.docx

# Open Excel or PowerPoint
winboat-runner excel ~/Documents/budget.xlsx
winboat-runner powerpoint

# Open full Windows desktop
winboat-runner desktop
```

---

## 🔒 Security

- The runner passes a configured password to FreeRDP through standard input
  instead of exposing it in the client process arguments.
- FreeRDP uses trust-on-first-use certificate verification. A changed
  certificate is rejected after the first connection; remove the locally
  persisted certificate record only when the container was intentionally
  reinstalled.
- Keep `~/.config/winboat/winboat.conf` readable only by your user:
  ```bash
  chmod 600 ~/.config/winboat/winboat.conf
  ```

---

## 📜 License
MIT License
