# Windows-Awake

## Overview

This repository provides a straightforward solution for Windows users in environments where system sleep settings are restricted. The scripts herein are designed to simulate key presses, preventing the computer from entering sleep mode. This is particularly useful in corporate settings where such modifications are locked down.

## Why This Approach?

- **Ideal for Office Workers, Government Officials and Home Workers with Restricted PC Functionality**: Keeps PCs awake without manual intervention.
- **Enhances Productivity**: Frees up the mouse and keyboard for use with other devices.
- **Non-Intrusive**: Uses simulated PRINT SCREEN key presses, the least disruptive to ongoing work.
- **Microsoft Teams Friendly**: Ensures applications like Teams or Slack appear "always active", requiring only these applications be opened beforehand and remain open.
- **CPU Efficiency**: The script is not CPU intensive as it runs at considerable intervals, set below the system's sleep threshold.

## PowerShell Script

The PowerShell script simulates the pressing of the PRINT SCREEN key at regular intervals (1 minute as configured in the script) to keep the computer awake. The `SendKeys("{PRTSC}")` command simulates a PRINT SCREEN key press.

```powershell
$shell = New-Object -ComObject wscript.shell
while ($true) { $shell.SendKeys("{PRTSC}"); Start-Sleep -Seconds 60 }
```

## WSL (Windows Subsystem for Linux) Function

For WSL users, the following function can be added to .bashrc. (Note: .bash_aliases is not suitable due to syntax constraints. Characters like "=" are not allowed. See [Bash Reference Manual](https://www.gnu.org/software/bash/manual/html_node/Aliases.html).

```bash
function idle() {
   powershell.exe -Command "\$shell = New-Object -ComObject wscript.shell; while (\$true) { \$shell.SendKeys(\"{PRTSC}\"); Start-Sleep -Seconds 60 }"
}
```

Then activate this function by running ```idle``` in your WSL shell.

## Customizing the Key Press

Want to use a different key? Choose any key or refer to the comprehensive list of keys and their corresponding codes at Microsoft's SendKeys Documentation. [Microsoft's SendKeys Documentation](https://learn.microsoft.com/en-us/dotnet/api/system.windows.forms.sendkeys?view=windowsdesktop-8.0).

## Important Note

This method is a workaround for specific situations where conventional methods are not feasible. Please use responsibly and in accordance with your organization's IT policies.
