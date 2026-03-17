# Overriding Locked Windows Services (The TrustedInstaller Method)

Windows treats you like a child and locks you out of your own system resources. If you want true optimization, you need to strip out bloatware like Windows Defender. Normally, Microsoft blocks you from touching these registry keys. 

This guide shows you how to gain **TrustedInstaller** privileges to forcefully edit locked services. 

**WARNING:** You are bypassing OS protections. If you break your OS, that's on you. 

---

### Step 1: Add "Run as TrustedInstaller" to Context Menu
First, you need a registry script (`RunAsTI.reg`) that adds a context menu option to run programs with TrustedInstaller rights. The system will warn you. Ignore it and run it.

![Run warning](/doc/1.png)
![Registry Editor warning](/doc/2.png)

### Step 2: Boot into Safe Mode
You cannot do this in standard Windows because Tamper Protection is running. Boot your PC into **Safe Mode**.

Navigate to your Windows Tools. Right-click on **Registry Editor** (or **Services**). You will now see the new option: `Run as trustedinstaller`. Click it. 

![Run Regedit as TI](/doc/3.png)
![Run Services as TI](/doc/4.png)

### Step 3: Find the Locked Service
With Regedit running as TrustedInstaller, Microsoft's locks are bypassed. Navigate to the services directory:
`Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services`

![Registry Services Path](/doc/5.png)

Scroll down until you find the service you want to modify. In this example, we are targeting `WinDefend` (Windows Defender).

![WinDefend Key](/doc/6.png)

### Step 4: Force the Modification
Look for the `Start` DWORD value on the right side. This controls how the service boots. Double-click it.

To forcefully disable the service, change the Value Data to **`4`** (Disabled). *(Note: The screenshots show other numbers being tested, but `4` is the standard disable code).*

**This is completely reversible.** If you ever want to enable the service again, just repeat these steps and change the `Start` value back to **`2`** (Automatic).

![Editing DWORD](/doc/7.png)
![Editing DWORD](/doc/8.png)

### Step 5: Reboot
Once you have modified the locked keys, you are done. Restart your computer normally to exit Safe Mode.

![Restarting Windows](/doc/9.png)

### Step 6: Verify Optimization
Open Task Manager. Search for the service you just killed (e.g., "defend"). If you disabled `WinDefend` correctly, the main service will be dead.

**Taking it further:** If you see leftover bloatware services still running in Task Manager—like **Antimalware Core Service (`MDCoreSvc`)** or **AMD Crash Defender Service**—you can kill them too. Just find their specific folders in the registry using the exact same TrustedInstaller method and change their `Start` values to `4`.

![Task Manager Verification](/doc/10.png)

---
*End of tutorial. Keep your system lean.*
