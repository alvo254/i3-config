# i3wm Setup Guide

> Lightweight desktop stack (~500MB RAM vs ~2GB for GNOME)

---

## 1. GNOME Settings in i3

**Problem:** i3 is not a full DE — some GNOME panels fail without GNOME services running.

```bash
XDG_CURRENT_DESKTOP=GNOME gnome-control-center
```

> Tricks the app into thinking GNOME is running. Only needed for specific GNOME panels.

---

## 2. Bluetooth

**Tool:** Blueman

```bash
systemctl status bluetooth
sudo apt install blueman
```

**i3 config:**

```
exec --no-startup-id blueman-applet
```

> Adds a tray icon for pairing headphones, keyboards, etc.

---

## 3. WiFi / Network Manager

**Tool:** nm-applet

```bash
sudo apt install network-manager-gnome
```

**i3 config:**

```
exec --no-startup-id nm-applet
```

> Also handles VPN connections and wired networks.

---

## 4. Audio / Volume

**Tools:** pavucontrol, PipeWire, WirePlumber

```bash
sudo apt install pavucontrol pipewire-audio wireplumber
```

> pavucontrol handles Bluetooth audio routing and per-app volume control.

---

## 5. Volume Key Bindings

**Tool:** pamixer

```bash
sudo apt install pamixer
```

**i3 config:**

```
bindsym XF86AudioRaiseVolume exec --no-startup-id pamixer -i 5
bindsym XF86AudioLowerVolume exec --no-startup-id pamixer -d 5
bindsym XF86AudioMute        exec --no-startup-id pamixer -t
```

---

## 6. Terminal

**Tool:** Terminator

```bash
sudo apt install terminator
```

**i3 config:**

```
bindsym $mod+Return exec terminator
```

> Terminator supports split panes — useful for tmux-style layouts without tmux.

---

## 7. App Launcher

**Tool:** Rofi

```bash
sudo apt install rofi
```

**i3 config:**

```
bindsym $mod+d exec rofi -show drun
```

> Also works as a window switcher: `rofi -show window` and SSH launcher.

---

## 8. Notifications

**Tool:** Dunst

```bash
sudo apt install dunst
```

**i3 config:**

```
exec --no-startup-id dunst
```

> Edit `~/.config/dunst/dunstrc` to style notifications and set urgency levels.

---

## 9. Clipboard History

**Tool:** CopyQ

```bash
sudo apt install copyq
```

**i3 config:**

```
exec --no-startup-id copyq
```

> Stores unlimited clipboard history. Useful for tokens, commands, and rotating secrets.

---

## 10. Screenshots

**Tool:** Flameshot

```bash
sudo apt install flameshot
```

**i3 config:**

```
bindsym Print exec flameshot gui
```

> `flameshot gui` opens interactive annotation mode. `flameshot full` for instant capture.

---

## Full Install (One-liner)

```bash
sudo apt install \
  blueman \
  network-manager-gnome \
  pavucontrol pamixer \
  pipewire-audio wireplumber \
  rofi \
  dunst \
  copyq \
  flameshot \
  terminator
```

---

## Full i3 Config Block

Add to `~/.config/i3/config`:

```
# Startup
exec --no-startup-id nm-applet
exec --no-startup-id blueman-applet
exec --no-startup-id dunst
exec --no-startup-id copyq

# Volume keys
bindsym XF86AudioRaiseVolume exec --no-startup-id pamixer -i 5
bindsym XF86AudioLowerVolume exec --no-startup-id pamixer -d 5
bindsym XF86AudioMute        exec --no-startup-id pamixer -t

# Launcher & terminal
bindsym $mod+d      exec rofi -show drun
bindsym $mod+Return exec terminator

# Screenshot
bindsym Print exec flameshot gui
```

---

## Feature Summary

|Feature|Tool|
|---|---|
|Bluetooth|Blueman|
|WiFi|nm-applet|
|Audio mixer|pavucontrol|
|Volume keys|pamixer|
|App launcher|Rofi|
|Notifications|Dunst|
|Clipboard|CopyQ|
|Screenshots|Flameshot|
|Terminal|Terminator|

---

## DevOps / Platform Engineer Addons

### Kubernetes Context in Status Bar

```bash
# polybar module
[module/k8s]
type = custom/script
exec = kubectl config current-context
interval = 5
```

### AWS Profile Indicator

```bash
[module/aws]
type = custom/script
exec = echo "AWS: ${AWS_PROFILE:-default}"
interval = 5
```

### VPN Status (WireGuard)

```bash
exec = wg show | grep -q interface && echo "VPN: ON" || echo "VPN: OFF"
```

### Multi-Monitor Workspace Assignment

```
# ~/.config/i3/config
workspace 1 output HDMI-1
workspace 2 output HDMI-1
workspace 3 output DP-1
workspace 4 output DP-1
```

### Auto-attach tmux on Terminal Open

```bash
# ~/.bashrc or ~/.zshrc
if [ -z "$TMUX" ]; then
  tmux attach || tmux new-session
fi
```
