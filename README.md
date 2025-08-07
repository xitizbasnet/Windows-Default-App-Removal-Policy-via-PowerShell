## 📦 Windows Default App Removal Policy via PowerShell

## 🧭 Overview

This PowerShell script is designed for **IT administrators** to automate the removal of **built-in Microsoft Store (UWP) applications** using a **Group Policy-backed registry configuration**. It targets common pre-installed apps such as **Bing News**, **Sticky Notes**, **Clipchamp**, **Xbox apps**, and even **Microsoft Copilot**, among others.

By modifying the following registry location:

```
HKLM:\SOFTWARE\Policies\Microsoft\Windows\Appx\RemoveDefaultMicrosoftStorePackages
```

...the script sets a policy that will instruct Windows to **remove specific default apps** during system provisioning or when a new user profile is created.

---

## 🧱 Use Case

This script is ideal for:

* Enterprises standardizing and decluttering Windows installations.
* System administrators preparing a golden image.
* Automated deployments via MDT, Intune, or Group Policy.
* Privacy-focused organizations seeking to limit bloatware.

---

## ⚠️ Important Considerations

| Topic             | Description                                                                                                                                |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| 🛑 Admin Rights   | The script **must be run as Administrator**.                                                                                               |
| 🧪 Existing Users | It primarily affects **new user profiles** or during **system provisioning**. Does **not uninstall apps for current users** automatically. |
| 🏢 Environment    | Designed for use in **enterprise environments** using Windows Pro, Education, or Enterprise editions.                                      |
| 🧼 Clean Install  | Best used on a fresh OS installation or prior to user login in automated deployments.                                                      |
| 🧰 App Scope      | Targets only **UWP (Microsoft Store)** apps, not traditional Win32 applications.                                                           |

---

## 🔍 Registry Policy Details

The script configures the following:

* **Registry Path**:

  ```
  HKLM:\SOFTWARE\Policies\Microsoft\Windows\Appx\RemoveDefaultMicrosoftStorePackages
  ```

* **Enabled Flag**:

  ```reg
  "Enabled"=dword:00000001
  ```

* **Per-App Configuration**:
  For each targeted app, a subkey is created with:

  ```reg
  "RemovePackage"=dword:00000001
  ```

---

## 📋 List of App Package Family Names (PFNs) Targeted for Removal

| App Name                       | PFN                                                                      |
| ------------------------------ | ------------------------------------------------------------------------ |
| Clipchamp                      | `Clipchamp.Clipchamp_yxz26nhyzhsrt`                                      |
| Bing News                      | `Microsoft.BingNews_8wekyb3d8bbwe`                                       |
| Bing Weather                   | `Microsoft.BingWeather_8wekyb3d8bbwe`                                    |
| Xbox Game Bar & Services       | Multiple                                                                 |
| Media Player                   | `Microsoft.MediaPlayer_8wekyb3d8bbwe`                                    |
| Microsoft Office Hub           | `Microsoft.MicrosoftOfficeHub_8wekyb3d8bbwe`                             |
| Solitaire Collection           | `Microsoft.MicrosoftSolitaireCollection_8wekyb3d8bbwe`                   |
| Sticky Notes                   | `Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe`                           |
| Outlook (Preview)              | `Microsoft.OutlookForWindows_8wekyb3d8bbwe`                              |
| Paint                          | `Microsoft.Paint_8wekyb3d8bbwe`                                          |
| Snip & Sketch                  | `Microsoft.ScreenSketch_8wekyb3d8bbwe`                                   |
| To Do                          | `Microsoft.Todos_8wekyb3d8bbwe`                                          |
| Photos                         | `Microsoft.Windows.Photos_8wekyb3d8bbwe`                                 |
| Calculator                     | `Microsoft.WindowsCalculator_8wekyb3d8bbwe`                              |
| Camera                         | `Microsoft.WindowsCamera_8wekyb3d8bbwe`                                  |
| Feedback Hub                   | `Microsoft.WindowsFeedbackHub_8wekyb3d8bbwe`                             |
| Notepad                        | `Microsoft.WindowsNotepad_8wekyb3d8bbwe`                                 |
| Sound Recorder                 | `Microsoft.WindowsSoundRecorder_8wekyb3d8bbwe`                           |
| Xbox Identity, Overlay, Speech | Multiple                                                                 |
| Groove Music / Movies & TV     | `Microsoft.ZuneMusic_8wekyb3d8bbwe`, `Microsoft.ZuneVideo_8wekyb3d8bbwe` |
| Microsoft Teams (Consumer)     | `MicrosoftTeams_8wekyb3d8bbwe`                                           |
| LinkedIn Info                  | `7EE7776C.LinkedInforWindows_w1wdnht996qgy`                              |
| Microsoft Copilot              | `Microsoft.Copilot_8wekyb3d8bbwe`                                        |

---

## 🚀 Execution Steps

### ✅ Prerequisites

* Run the script as **Administrator**
* Ensure PowerShell is version 5.1 or later
* Run on supported editions (Pro, Enterprise, or Education)

### 📂 Recommended Execution Methods

* Directly via PowerShell console
* Integrated into MDT, SCCM, Intune, or Autopilot pre-provisioning
* Included in your base system image

---

## 🧾 Final PowerShell Script

```powershell
# -------------------------------
# Windows App Removal Policy Script
# -------------------------------
# Run this script as Administrator

$basePath = "HKLM:\SOFTWARE\Policies\Microsoft\Windows\Appx\RemoveDefaultMicrosoftStorePackages"
$packages = @(
    "Clipchamp.Clipchamp_yxz26nhyzhsrt",
    "Microsoft.BingNews_8wekyb3d8bbwe",
    "Microsoft.BingWeather_8wekyb3d8bbwe",
    "Microsoft.GamingApp_8wekyb3d8bbwe",
    "Microsoft.MediaPlayer_8wekyb3d8bbwe",
    "Microsoft.MicrosoftOfficeHub_8wekyb3d8bbwe",
    "Microsoft.MicrosoftSolitaireCollection_8wekyb3d8bbwe",
    "Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe",
    "Microsoft.OutlookForWindows_8wekyb3d8bbwe",
    "Microsoft.Paint_8wekyb3d8bbwe",
    "Microsoft.ScreenSketch_8wekyb3d8bbwe",
    "Microsoft.Todos_8wekyb3d8bbwe",
    "Microsoft.Windows.Photos_8wekyb3d8bbwe",
    "Microsoft.WindowsCalculator_8wekyb3d8bbwe",
    "Microsoft.WindowsCamera_8wekyb3d8bbwe",
    "Microsoft.WindowsFeedbackHub_8wekyb3d8bbwe",
    "Microsoft.WindowsNotepad_8wekyb3d8bbwe",
    "Microsoft.WindowsSoundRecorder_8wekyb3d8bbwe",
    "Microsoft.Xbox.TCUI_8wekyb3d8bbwe",
    "Microsoft.XboxGamingOverlay_8wekyb3d8bbwe",
    "Microsoft.XboxIdentityProvider_8wekyb3d8bbwe",
    "Microsoft.XboxSpeechToTextOverlay_8wekyb3d8bbwe",
    "Microsoft.ZuneMusic_8wekyb3d8bbwe",
    "Microsoft.ZuneVideo_8wekyb3d8bbwe",
    "MicrosoftTeams_8wekyb3d8bbwe",
    "7EE7776C.LinkedInforWindows_w1wdnht996qgy",  # LinkedIn
    "Microsoft.Copilot_8wekyb3d8bbwe"             # Copilot
)

# Create the base registry key and enable the policy
if (-not (Test-Path $basePath)) {
    New-Item -Path $basePath -Force | Out-Null
}
Set-ItemProperty -Path $basePath -Name "Enabled" -Type DWord -Value 1

# Create subkeys for each package and set RemovePackage=1
foreach ($pkg in $packages) {
    $pkgPath = Join-Path $basePath $pkg
    if (-not (Test-Path $pkgPath)) {
        New-Item -Path $pkgPath -Force | Out-Null
    }
    Set-ItemProperty -Path $pkgPath -Name "RemovePackage" -Type DWord -Value 1
}

Write-Host "Policy and package subkeys created successfully."
```

---

## 📎 Optional Enhancements

* Add `Get-AppxPackage` and `Remove-AppxPackage` commands to **remove apps for current user**.
* Wrap script in `.ps1` and sign it for execution via GPO or Intune.
* Pair with a **Windows provisioning package (.ppkg)** for complete automation.

---

## 🤝 Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

---

## 🧪 Disclaimer

This script is provided **as-is**, with no warranties or guarantees. Always test in a controlled environment before deploying to production.

---

