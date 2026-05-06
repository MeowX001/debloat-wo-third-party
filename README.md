# 🧹 Debloat Windows Without Third-Party Tools
> A no-nonsense guide for power users who want a leaner Windows install without relying on bloatware removers like BloatyNosy or similar tools.

---

## ⚠️ Disclaimer
Some steps here (especially disabling Defender) are **not recommended for average users**. Make a system restore point before proceeding.

```
Settings > System > About > System Protection > Create
```

---

## Step 1 — Remove Unnecessary Apps (PowerShell)

List all installed Appx packages:

```powershell
Get-AppxPackage | Select Name
```

Feed the output to an AI and ask it to generate a removal script for unnecessary apps. Or use the ready-made script below.

<details>
<summary>📄 View debloat script</summary>

```powershell
# =========================
# SAFE WINDOWS 11 DEBLOAT
# =========================

$apps = @(
    # Xbox (safe to remove if you don't game via Xbox)
    "Microsoft.XboxGamingOverlay",
    "Microsoft.XboxGameCallableUI",
    "Microsoft.GamingServices",

    # Consumer / preinstalled junk
    "Microsoft.BingWeather",
    "Microsoft.WindowsAlarms",
    "Microsoft.WindowsCamera",
    "Microsoft.Windows.DevHome",
    "Microsoft.MixedReality.Portal",
    "Microsoft.YourPhone",
    "MicrosoftWindows.CrossDevice",

    # Optional Microsoft apps
    "Microsoft.MicrosoftStickyNotes",
    "Microsoft.Paint",
    "Microsoft.Windows.Photos",
    "Microsoft.ScreenSketch",

    # Outlook (new bloat version)
    "Microsoft.OutlookForWindows",

    # Widgets / Web Experience (removes widgets fully)
    "MicrosoftWindows.Client.WebExperience",

    # Third-party junk
    "BlipStudioInc.BlipApp",

    # Windows 365 (not useful unless you use cloud PC)
    "MicrosoftCorporationII.Windows365"
)

foreach ($app in $apps) {
    Write-Host "Removing $app..."
    Get-AppxPackage -Name $app -AllUsers | Remove-AppxPackage -ErrorAction SilentlyContinue
}

# Remove provisioned packages (preinstalled for new users)
foreach ($app in $apps) {
    Get-AppxProvisionedPackage -Online | Where-Object {$_.DisplayName -eq $app} | Remove-AppxProvisionedPackage -Online -ErrorAction SilentlyContinue
}

Write-Host "Debloat complete."
```

</details>

Run PowerShell as **Administrator**, paste the script, and execute.

---

## Step 2 — Disable Useless Windows Services (`services.msc`)

Open Run (`Win + R`) → type `services.msc`

The following services are safe to **Disable** for most users. If you think you'll ever need one, set it to **Manual** instead.

| Service Name | Display Name | Why Disable |
|---|---|---|
| `DiagTrack` | Connected User Experiences and Telemetry | Sends usage data to Microsoft |
| `dmwappushservice` | WAP Push Message Routing Service | Also telemetry-related |
| `SysMain` | SysMain (Superfetch) | Annoying on SSDs, preloads apps into RAM |
| `WSearch` | Windows Search | Disk/RAM hog if you don't use search |
| `Fax` | Fax | Unless you own a fax machine in 2025 |
| `XblAuthManager` | Xbox Live Auth Manager | Useless without Xbox |
| `XblGameSave` | Xbox Live Game Save | Useless without Xbox |
| `XboxNetApiSvc` | Xbox Live Networking Service | Useless without Xbox |
| `MapsBroker` | Downloaded Maps Manager | For the Maps app nobody uses |
| `RetailDemo` | Retail Demo Service | Only used in store display mode |
| `SharedAccess` | Internet Connection Sharing | Only if you share internet via this PC |
| `WerSvc` | Windows Error Reporting | Sends crash reports to Microsoft |
| `lfsvc` | Geolocation Service | Disables location tracking |
| `TabletInputService` | Touch Keyboard and Handwriting | Useless on non-touchscreen PCs |
| `WMPNetworkSvc` | Windows Media Player Network Sharing | Shares WMP library over network |
| `wisvc` | Windows Insider Service | Useless if not in Insider Program |
| `icssvc` | Windows Mobile Hotspot Service | If you never use your PC as a hotspot |
| `PhoneSvc` | Phone Service | For the Phone Link / Your Phone app |
| `PcaSvc` | Program Compatibility Assistant | Nags you about old apps |
| `RemoteRegistry` | Remote Registry | Security risk — disable unless needed |

---

## Step 3 — Minimize Startup Apps

Open Task Manager (`Ctrl + Shift + Esc`) → **Startup apps** tab

Disable everything you don't need launching at boot. Less = faster startup.

---

## Step 4 — Disable Defender *(Not recommended for most users)*

> ⚠️ **Only do this if you know what you're doing.** Use Malwarebytes or similar for occasional manual scans instead.

See: [How to disable Windows Defender / anti-malware](https://unleashed-platinum-b9b.notion.site/disable-anti-malware-18f3a2dc59ec80919364d4b03809e421)

---

## Step 5 — Disable Automatic Updates

```
Win + R → gpedit.msc
```

Navigate to:
```
Computer Configuration
  └── Administrative Templates
        └── Windows Components
              └── Windows Update
                    └── Manage end user experience
                          └── Configure Automatic Updates → Set to Disabled
```

---

## Contributing

Found an outdated service or a new bloatware package? PRs are welcome. Please note which Windows version you tested on.

---

## License

MIT — do whatever you want with this.
