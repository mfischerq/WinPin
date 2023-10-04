# WinPin
Allows to 'pin' an application window to a predefined position/size on screen.   Whenever you restart an application you configured a pinned window for, it's window will be positioned to the pinned position/size. This is especially helpful for applications that do not restore their last position/size when restarted.

Motivation
==========
Some people prefer to have the application windows on their desktops organized in a fixed manner.
- Firefox always on the right of the screen
- Thunderbird also on the right, but a bit smaller
- Terminal always on the bottom left
- and so on...

Unfortunately most of the programs do not restore their last position when restarted.

There are several ways to resolve this issue:
1. Start the application you want to be placed to a fixed position/size size with the --geometry arg.
   This works but you have to either start these programs in the console or modify existing/
   create new launchers for them.
2. Configure the window manager to place the windows to a fixed position/size.
   Yes, I think the WM would be a perfect place to implement this functionality.
   Unfortunately I do not know of a popular WM that offers this feature...
3. Use a program like devilspie. That does the job, but it does no longer have a GUI, so it's a bit
   of work to configure.
4. Do it yourself. This is exactly what WinPin is doing.

What does WinPin do?
====================
WinPin allows you to 'pin' an application window to a predefined position/size on screen.
This means that whenever you open a program you have pinned before, the application window opens
at this predefined position/size.

Note that you still can move/resize such a window after WinPin has positioned it. WinPin only takes 
control when a window is created.

How to use WinPin?
==================
Typically WinPin has to be started automatically (on session startup) to be useful.
It starts into the systray and supervises all windows that are created.

To configure windows to be pinned, right click on the WinPin systray icon and select "Show UI" 
(or left click on the systray icon).
The Config UI shows the currently opened windows of all desktops on the left. The configured pinned 
windows are shown on the right.

To pin an application window:
-----------------------------
- start the application
- position the application window to the position/size you want it to be pinned to
- Select the application in the left list of active windows and press the 'Pin' button
- A new pinned window will then be added to the list of pinned windows on the right
- Note that the pinned geometry is displayed as part of the entry in the list of pinned windows

From now on: when you start the pinned application, it will be displayed at the location/size 
you pinned it to.

Fine-tuning the geometry of a pinned application window:
--------------------------------------------------------
Some windows have an unusual geometry/window-hierarchy that can not be easily introspected.
It might happen that such windows are not restored at the pinned location/size but a bit off.
(Thunderbird is an example of such an application). 
In this case:
- select the pinned window of your application in the right list
- you will be presented with the detailed, editable geometry of this window
- edit the geometry to compensate for offsets you do not want
- press the 'Test' button to let the window be moved/resized to the changed geometry
- when you are satisfied with the result, press the 'Save' button

How does WinPin work internaly?
===============================
WinPin uses the 'gb.Desktop' component to retrieve and manipulate desktop windows.
A 'DesktopWatcher' notifies WinPin whenever windows do change (new windows, closed windows, ...)

Whenever a new window is reported, WinPin matches this window against the stored 'pinned windows'.
This matching is done by equality of WindowClass (WM_CLASS). So: when a new window appears that has
a WindowClass that one of the 'pinned windows' have, the geometry stored with this 'pinned window' is
applied to this window.

Note that the configured 'pinned windows' are automaticaly persisted to the 
'Desktop.ConfigDir &/ Application.Name' folder (this usually maps to ~/.config/WinPin).