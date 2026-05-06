# Disable Windows Defender (Including Anti-Malware Service Executable)

> ⚠️ **For power users only.** Without Defender, your PC has no real-time protection. Use Malwarebytes or similar for occasional manual scans. Do NOT do this on a shared or work PC.

---

## How to Use

These reg files must be applied in **Safe Mode** because Windows protects Defender from being modified while running normally.

### Steps

1. **Boot into Safe Mode**
   ```
   Settings → System → Recovery → Advanced startup → Restart now
   → Troubleshoot → Advanced options → Startup Settings → Restart → Press 4
   ```

2. **Open the folder containing the `.reg` file**

3. **Drag and drop** the reg file onto `regedit.exe`
   - You can find it at `C:\Windows\regedit.exe`
   - Or open Run (`Win + R`) → type `regedit` → drag the file into the window

4. **Accept the UAC prompt** and confirm the merge

5. **Restart normally**

---

## Files

| File | What it does |
|---|---|
| `Disable_Defender.reg` | Disables Defender and Anti-Malware Service Executable |
| `Enable_Defender.reg` | Restores everything back to Windows defaults |

---

## Disable_Defender.reg

```reg
Windows Registry Editor Version 5.00

; Registry File By Adamx
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\wdboot]
"Start"=dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\wdfilter]
"Start"=dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WinDefend]
"Start"=dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SecurityHealthService]
"Start"=dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\wdnisdrv]
"Start"=dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\mssecflt]
"Start"=dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WdNisSvc]
"Start"=dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Sense]
"Start"=dword:00000004

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\wscsvc]
"Start"=dword:00000004

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender]
"DisableAntiSpyware"=dword:00000001
"DisableRoutinelyTakingAction"=dword:00000001
"ServiceKeepAlive"=dword:00000000

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender\Real-Time Protection]
"DisableBehaviorMonitoring"=dword:00000001
"DisableIOAVProtection"=dword:00000001
"DisableOnAccessProtection"=dword:00000001
"DisableRealtimeMonitoring"=dword:00000001
```

---

## Enable_Defender.reg

```reg
Windows Registry Editor Version 5.00

; Registry File By Adamx
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\wdboot]
"Start"=dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\wdfilter]
"Start"=dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WinDefend]
"Start"=dword:00000002

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SecurityHealthService]
"Start"=dword:00000002

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\wdnisdrv]
"Start"=dword:00000003

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\mssecflt]
"Start"=dword:00000000

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WdNisSvc]
"Start"=dword:00000003

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Sense]
"Start"=dword:00000003

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\wscsvc]
"Start"=dword:00000002

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender]
"DisableAntiSpyware"=-
"DisableRoutinelyTakingAction"=-
"ServiceKeepAlive"=-

[HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows Defender\Real-Time Protection]
"DisableBehaviorMonitoring"=-
"DisableIOAVProtection"=-
"DisableOnAccessProtection"=-
"DisableRealtimeMonitoring"=-
```

---

## Notes

- `dword:00000004` = Disabled
- `dword:00000002` = Automatic
- `dword:00000003` = Manual
- `dword:00000000` = Boot-start
- A value of `-` in the enable file means the key is **deleted**, restoring Windows default behavior

