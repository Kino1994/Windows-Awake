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

## Sound Configuration for Important Notifications

To ensure you don't miss any critical notifications, like calls, you can set up Windows to play sounds on specific audio devices. Here’s how to configure your sound settings:

1. **Set Up Notifications**:
   - In your notification application (such as Microsoft Teams or similar), go to the **Notifications and Activity** section.
   - Make sure options like **Play sounds for urgent and priority contact notifications** and **Play sounds for incoming calls** are enabled.

2. **Install the Audio Module**:
   - Before switching audio devices, you'll need to install the required PowerShell module. Open PowerShell as an administrator and run:
     ```powershell
     Install-Module -Name AudioDeviceCmdlets -Scope CurrentUser
     ```
   - Confirm the installation when prompted.

3. **List Available Audio Devices**:
   - To see a list of available audio devices and choose the correct one, run the following command:
     ```powershell
     Get-AudioDevice -List
     ```
   - Note down the name or ID of the device you want to set as the default.

4. **Switch Audio Output Device in Windows**:
   - Select your preferred audio device for notifications, ensuring that sounds play on the device of your choice ensuring notification sounds play on a specific device, such as speakers, to make sure notifications are loud and clear.
   - Use PowerShell to set the audio output device by its index.
   - Run the following command, replacing `<Device ID>` with the index number of your desired audio device (obtained from step 3):
     ```powershell
     Set-AudioDevice -Index <Device ID>
     ```
   - This will allow notifications to play on the specific sound device you selected, ensuring you don’t miss any important alerts even while resting.


5. **Example of `idle` Function with Audio Device Switching**:
   - You can modify the `idle` function to switch to a specific audio device during the idle process and then restore the default device afterward. This setup ensures notifications are loud and clear while idle and then resets to the default audio device when finished. Here’s an example:
     ```bash
     function idle() {
         powershell.exe Set-AudioDevice -Index 2 # Set device for idle, like speakers
         powershell.exe -Command "\$shell = New-Object -ComObject wscript.shell; while (\$true) { \$shell.SendKeys(\"{PRTSC}\"); Start-Sleep -Seconds 60 }"
         powershell.exe Set-AudioDevice -Index 1 # Restore the default device
     }
     ```
   - The index values (2 and 1 in this example) depend on your personal audio setup. Be sure to list your devices first (see step 3) to find the correct indices for your configuration.

## Important Note

This method is a workaround for specific situations where conventional methods are not feasible. Please use responsibly and in accordance with your organization's IT policies.
