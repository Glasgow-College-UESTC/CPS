
## Problem
This error occurs when a test connection fails during the hardware setup process. You might also see this error message after using the sdrinfo command.

```
Error Code -12
```

## Possible Solution
This error occurs due to a USB connectivity issue. Possible solutions include:

If you are using Windows and are using Zadig software to install the driver, try selecting either the RTL283UHIDIR or RTL2832U interfaces (if they are available) instead of "Bulk-In, Interface (Interface 0)". When you select the WinUSB option on the right pane, you might get a warning that a System File is going to be changed while installing the driver. By installing the SDR drivers using Zadig software, you overwrite the default drivers for the device. Depending on how your operating system is set up, you might receive this warning before the start of the installation.

If you do not find `RTL283UHIDIR` or `RTL2832U` interfaces in the list, navigate to 'Options' and make sure that you select 'List all Devices' and clear 'Ignore Hubs or Composite Parents' check box.

If you are using a USB 3.0 port, try plugging the RTL-SDR radio into a USB 2.0 port or try testing the connection on another computer if possible.

Unplug the RTL-SDR radio and verify that it disappears from the list. Replug and verify that it again appears in the list.

Enter these commands at the MATLAB command prompt:

```
rehash toolbox
rehash toolboxcache
```

### Note

If the hardware setup process does not start the Zadig software, you can start it manually by using this command at the command prompt:

```
C:\ProgramData\MATLAB\SupportPackages\<version>\3P.instrset\zadig.instrset\...
... zadig\zadig-<version>.exe
```