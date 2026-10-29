<div align="center">

# 🇯🇵 Japanese Input on CachyOS

**Type Hiragana, Katakana, and Kanji on CachyOS with Fcitx5 and Mozc.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](LICENSE)
![Platform](https://img.shields.io/badge/Platform-CachyOS-00b3a4?style=flat-square&logo=archlinux&logoColor=white)
![Desktop](https://img.shields.io/badge/Desktop-KDE%20Plasma-1d99f3?style=flat-square&logo=kde&logoColor=white)
![IME](https://img.shields.io/badge/IME-Fcitx5%20%2B%20Mozc-e60012?style=flat-square)
![Session](https://img.shields.io/badge/Session-Wayland%20%7C%20X11-6f42c1?style=flat-square)

[Quick Start](#-quick-start) •
[Installation](#-installation) •
[Usage](#-usage) •
[Troubleshooting](#-troubleshooting) •
[Uninstall](#-uninstall)

</div>

---

## 📖 Overview

This guide sets up Japanese text input on **CachyOS** using **Fcitx5** as the input method framework and **Mozc** as the Japanese conversion engine. It works with KDE Plasma and other desktop environments.

**What this guide covers**

- ✅ KDE Plasma on **Wayland** (recommended path)
- ✅ KDE Plasma on **X11**
- ✅ GTK, Qt, X11, and Xwayland applications
- ✅ Hiragana, Katakana, and Kanji conversion
- ✅ Common problems and diagnostics

> [!NOTE]
> You do **not** need to change your system language or keyboard layout. Fcitx5 and Mozc convert text typed on a Latin keyboard into kana and Kanji.

---

## ⚡ Quick Start

For KDE Plasma on Wayland, the whole setup takes four steps:

```bash
# 1. Install the packages
sudo pacman -Syu fcitx5 fcitx5-mozc fcitx5-configtool fcitx5-gtk fcitx5-qt noto-fonts-cjk
```

2. Open **System Settings → Input & Output → Keyboard → Virtual Keyboard** and select **Fcitx 5**.
3. Log out and log back in, then run `fcitx5-configtool` and add **Mozc** under the **Input Method** tab.
4. Press `Ctrl+Space` in any text field and type `arigatou` → `ありがとう`.

Using X11 or running into trouble? Continue with the full guide below.

---

## 🧭 Choose Your Setup

Check which session type you are running:

```bash
echo "$XDG_SESSION_TYPE"
```

| Session | Recommended setup |
|---|---|
| **KDE Plasma Wayland** | Let KWin start Fcitx5 from the *Virtual Keyboard* settings. Do not set global GTK/Qt input-method variables unless an application needs them. |
| **KDE Plasma X11** | Start Fcitx5 with desktop autostart and set the `GTK_IM_MODULE`, `QT_IM_MODULE`, and `XMODIFIERS` variables. |
| **Other Wayland desktops** | Install the same packages, then follow the input-method instructions for your compositor or desktop environment. |

---

## 📦 Installation

### 1. Install Fcitx5 and Mozc

Update the package database and install everything in one command:

```bash
sudo pacman -Syu fcitx5 fcitx5-mozc fcitx5-configtool fcitx5-gtk fcitx5-qt noto-fonts-cjk
```

| Package | Purpose |
|---|---|
| `fcitx5` | Input method framework |
| `fcitx5-mozc` | Japanese IME based on Mozc |
| `fcitx5-configtool` | Graphical Fcitx5 configuration tool |
| `fcitx5-gtk` | Integration for GTK applications |
| `fcitx5-qt` | Integration for Qt applications |
| `noto-fonts-cjk` | Fonts for Japanese, Chinese, and Korean text |

### 2. Configure KDE Plasma on Wayland

KWin should launch Fcitx5 in a Plasma Wayland session.

1. Open **System Settings**.
2. Go to **Input & Output → Keyboard → Virtual Keyboard**.
3. Select **Fcitx 5**.
4. Apply the change.
5. Log out and log back in.

> [!WARNING]
> Do not also launch `fcitx5` from a shell startup file. Starting it twice can produce this warning:
>
> ```text
> Fcitx should be launched by KWin under KDE Wayland
> ```

If Fcitx5 disappears after login, return to **Virtual Keyboard** and confirm that **Fcitx 5** is still selected. Also check that Plasma's on-screen keyboard support has not been disabled.

### 3. Add Mozc to Fcitx5

Open the configuration tool from the application launcher, or run:

```bash
fcitx5-configtool
```

Then:

1. Select the **Input Method** tab.
2. Disable **Only Show Current Language** if Mozc is not listed.
3. Search for `Mozc`.
4. Add **Mozc** to the active input methods.
5. Keep an English keyboard entry as the first item and Mozc as the second.
6. Click **Apply**.

The default shortcut for switching input methods is usually `Ctrl+Space`. You can change it under **Global Options → Trigger Input Method**.

> [!TIP]
> Plasma's keyboard-layout switcher and Fcitx5's input-method switcher are separate systems. Use the Fcitx5 shortcut to switch between the Latin keyboard and Mozc.

### 4. Test Japanese input

1. Open Kate, Firefox, or any application with a text field.
2. Press `Ctrl+Space` to activate Mozc.
3. Type `arigatou`. It should appear as `ありがとう`.
4. Press `Space` to display conversion candidates.
5. Select `有り難う`, then press `Enter`.

You can also check Fcitx5 from a terminal:

```bash
fcitx5-remote
```

| Return value | Meaning |
|:---:|---|
| `0` | Fcitx5 is not running |
| `1` | Fcitx5 is running, but the input method is inactive |
| `2` | The input method is active |

---

## ⌨️ Usage

Mozc accepts rōmaji typed on a Latin keyboard.

| Goal | How | Example |
|---|---|---|
| **Hiragana** | Type the word, press `Enter` without converting | `konnichiwa` → `こんにちは` |
| **Kanji** | Type the reading, press `Space`, choose a candidate, press `Enter` | `nihon` → `にほん` → `日本` |
| **Katakana** | Type the reading, press `F7` before confirming | `konpyuutaa` → `コンピューター` |

- Press `Space` again to show more candidates. Use the arrow keys or number keys to choose one.
- On a laptop, you may need to press `Fn+F7` for Katakana.

### Conversion keys

| Key | Action while composing text |
|:---:|---|
| `Space` | Convert kana to Kanji or show more candidates |
| `Enter` | Confirm the current text or candidate |
| `Esc` | Cancel the current conversion |
| `F6` | Convert to Hiragana |
| `F7` | Convert to full-width Katakana |
| `F8` | Convert to half-width Katakana |
| `F9` | Convert to full-width Latin characters |
| `F10` | Convert to half-width Latin characters |

---

## 🖥️ X11 and Xwayland Configuration

> [!NOTE]
> Skip this section if Japanese input already works in all your applications.

### KDE Plasma on X11

Create a per-user environment file:

```bash
mkdir -p ~/.config/environment.d
cat > ~/.config/environment.d/90-fcitx5.conf <<'EOF'
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
EOF
```

Log out and log back in. Fcitx5 normally provides an XDG autostart entry, so it starts with the desktop session.

> [!IMPORTANT]
> The value is `fcitx`, **not** `fcitx5`.

### Individual applications on Wayland

Native Wayland applications generally work best through Wayland's text-input protocol. Setting `GTK_IM_MODULE` or `QT_IM_MODULE` globally can bypass that path, so only use an override when a specific application does not work.

```bash
# GTK application
GTK_IM_MODULE=fcitx application-name

# Qt application
QT_IM_MODULE=fcitx application-name

# Legacy X11 / Xwayland application
XMODIFIERS=@im=fcitx application-name
```

If an override fixes the application, add it to that application's launcher instead of applying it to the entire Wayland session.

---

## 🔧 Troubleshooting

<details>
<summary><b>Run the diagnostic tool first</b></summary>

<br>

```bash
fcitx5-diagnose
```

The report is long, but its warnings usually point to missing addons, environment problems, duplicate processes, or applications that cannot find the input-method module.

> [!CAUTION]
> Do not apply every suggestion blindly. In a native Wayland session, warnings about unset `GTK_IM_MODULE` or `QT_IM_MODULE` may be harmless.

</details>

<details>
<summary><b>Fcitx5 is not running</b></summary>

<br>

Check the process:

```bash
pgrep -a fcitx5
```

- **KDE Plasma Wayland:** reselect **Fcitx 5** under **System Settings → Input & Output → Keyboard → Virtual Keyboard**, then log out and back in.
- **X11:** start it once for testing:

  ```bash
  fcitx5 -d
  ```

</details>

<details>
<summary><b>Mozc does not appear in the configuration tool</b></summary>

<br>

Confirm that the package is installed:

```bash
pacman -Q fcitx5-mozc
```

Then reopen `fcitx5-configtool`, disable **Only Show Current Language**, and search for Mozc again.

</details>

<details>
<summary><b>The shortcut does nothing</b></summary>

<br>

Open `fcitx5-configtool` and check **Global Options → Trigger Input Method**. Another Plasma shortcut may already use the same key combination.

Check the current state directly:

```bash
fcitx5-remote
fcitx5-remote -t   # toggles the active input method
```

</details>

<details>
<summary><b>Japanese works in some applications only</b></summary>

<br>

1. Confirm whether the failing application is native Wayland or Xwayland.
2. Make sure `fcitx5-gtk` and `fcitx5-qt` are installed.
3. Test the application with one of the per-application variables described [above](#individual-applications-on-wayland).
4. Run `fcitx5-diagnose` while the application is open.

For Chromium-based applications on Wayland, text-input protocol support can vary by version. If the candidate window is misplaced or input does not work, first confirm that `fcitx5-gtk` is installed. Testing the application under Xwayland can also help isolate the issue.

</details>

<details>
<summary><b>Japanese characters appear as boxes</b></summary>

<br>

Install or reinstall a CJK font, then rebuild the font cache:

```bash
sudo pacman -S noto-fonts-cjk
fc-cache -fv
```

</details>

<details>
<summary><b>Duplicate Fcitx5 processes on Plasma Wayland</b></summary>

<br>

Check for user-created autostart entries and shell startup commands:

```bash
pgrep -a fcitx5
grep -R "fcitx5" ~/.config/autostart ~/.profile ~/.bash_profile ~/.zprofile 2>/dev/null
```

Remove custom `fcitx5 &` commands when KWin already launches Fcitx5 through the Virtual Keyboard setting.

</details>

---

## 🗑️ Uninstall

Remove the packages installed by this guide:

```bash
sudo pacman -Rns fcitx5-mozc fcitx5-configtool fcitx5-gtk fcitx5-qt
```

> [!NOTE]
> Pacman may keep `fcitx5` if another installed package still depends on it. Review the package list before confirming removal.

If you created the X11 environment file from this guide, remove it as well:

```bash
rm ~/.config/environment.d/90-fcitx5.conf
```

Log out and back in after removing the configuration.

---

## 💡 Why Fcitx5 and Mozc?

- **Fcitx5** integrates well with Qt-based desktops such as KDE Plasma.
- **Mozc** is an actively maintained Japanese conversion engine derived from Google Japanese Input.
- **Anthy** is still available, but it has seen little upstream development for many years and is no longer the best default for a new setup.

---

## 📚 References

- [ArchWiki: Fcitx5](https://wiki.archlinux.org/title/Fcitx5)
- [ArchWiki: Japanese localization](https://wiki.archlinux.org/title/Localization/Japanese)
- [Fcitx5 documentation](https://fcitx-im.org/wiki/Fcitx_5)
- [Using Fcitx5 on Wayland](https://fcitx-im.org/wiki/Using_Fcitx_5_on_Wayland)
- [Mozc source repository](https://github.com/google/mozc)

---

## 🤝 Contributing

Corrections and desktop-specific notes are welcome. Open an issue or submit a pull request and include:

- the desktop environment and version
- whether the session uses Wayland or X11
- the affected application
- relevant output from `fcitx5-diagnose`

---

## 📄 License

This guide is released under the [MIT License](LICENSE).

<div align="center">

<sub>If this guide helped you, consider giving the repo a ⭐</sub>

</div>
