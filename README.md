# Linux-Controller-Fixes
A collection of fixes picked up over time to allow game controllers (primarily HOTAS systems) to work as expected under Linux.
Applies to both Wine and native applications. Primarily intended to aid beginners in troubleshooting common control issues.

Errors and fixes collated over time across Fedora, OpenSUSE, Void and EndeavourOS (Arch).

Updated as issues are encountered, identified and resolved, purely as time permits.

## Contents

   * [Controllers](#controllers)
      * [HOTAS](#hotas)
         * [Wine](#hotas-issues-in-wine-applications)
         * [System](#hotas-issues-in-native-applications)
      * [Other](#other-controllers)
         * [System](#issues-in-native-applications)
   * [Headtracking](#headtracking)
      * [Hardware Options](#hardware)
         * [Clips](#clips)
         * [Hats](#hats)
         * [Sensors](#cameras)
      * [Software Options](#software)
         * [Opentrack](#opentrack)
         * [Linuxtrack](#linuxtrack)
      * [Issues](#headtracking-issues)

*Note: While I have various experiences in tweaking controllers under Linux (thanks to my interest in space/flight simulations), I'm by no means an expert.
I'm not responsible for any damage, data loss, system damage or instability that could arise from implementing any fixes described here, however unlikely that may be. Never blindly follow guides on the internet, especially without a backup in place.*

## Controllers

From my rather limited experience with controllers (defined for the sake of this section as joypads/gamepads), controllers seem to be well supported across the board. I'm sure there are some exceptions, but my testing has revealed no problems so far. I make use of both a Steelseries Nimbus and Stratus XL over Bluetooth.

### HOTAS

This section covers common issues with HOTAS systems on Linux, in both native and Wine applications.

#### HOTAS issues in Wine applications

##### Axes detected by system applications but not under Wine

This issue was the first controller problem I encountered and spent months on narrowing down. While the issue itself isn't particularly complex, there are issues with similar symptoms, meaning one usually just has to try all the fixes until the problem is resolved. That said, if all native applications detect the controller correctly, it's likely this is the fix needed.

* First, identify which axis isn't being detected. In this example, I'll be using the Thrustmaster TWCS Throttle, but this method should work (in principal, at least) across any controller Wine detects.

*[(Source)](https://www.reddit.com/r/wine_gaming/comments/hxfevf/hotas_style_joysticks_in_wine_with_not_all_axes/)*

^ Regedit fix (WIP)

##### Game not launching/input not detected

In cases where the controller (tested with a native utility such as [jstest-gtk](https://github.com/Grumbel/jstest-gtk/tree/master) and game (via [Lutris](https://lutris.net/)) are working fine but inputs are unrecognised or incorrect, it's possible that Lutris has been meddling where it ought not. I've only had this occur for one title, but this fix not only allowed the game to get past a launch screen but also be fully playable. It's rather simple:

* In Lutris, select the game experiencing controller problems. Open the contextual menu and select '*Configure*'.
* In the configuration menu, switch to the '*Runner options*' tab. Scroll to the bottom to find the '*Autoconfigure joypads*' option.
* This is sometimes set by default, presumably configured by the installer for the relevant game. Disable the option.
* Save the configuration, return to the library screen and open the Wine menu. Select '*Wine Control Panel*'. Alternatively, you can execute `WINEPREFIX=$PATHTOGAME wine joy.cpl`.
* In the 'Game Controllers' window, ensure that the relevant controller is active. Once confirmed, close the Wine control panel.
* Launch the game and test.

This, as may be obvious, prevents Lutris from making any changes to how the Wine runner in use recognises your controllers. I always disable this option unless it's specifically needed, as it hampers troubleshooting other issues, especially with multiple (or even worse, multi-device) controllers connected.

#### HOTAS issues in native applications

No titles to test with but info still valuable
`linuxconsoletools` and `jscal` go here

### Other controllers

This section covers common issues with other controller types (wheels, button panels etc.) on Linux, in both native and Wine applications.

#### Issues in native applications

I haven't experienced any issues with native applications yet, though I don't own any titles that exclusively make use of a controller - I'll test any I come across. I have, however, had problems with controller detection, so I'll list those instead.

##### Controller detected as a mouse/inputs moving the cursor

*Note to Wayland users: this only applies to Xorg. I'm sure there is a Wayland alternative, but I have no experience with systems using Wayland so cannot confirm.*

This occurs when X (the Unix window system) detects a controller as a mouse input. I've only seen this with my steering wheel (SRW-S1), and I believe it's motion-control features are to blame for this incorrect identification. Regardless, it's a problem that can be solved by doing the following:

* Consult `Xorg.0.log`. Find it at `/var/log/Xorg.0.log/`, or `~/.local/share/xorg/` if you're running rootless Xorg.
* Search (use `Ctrl-F`) for the name of your controller. It's most easy to find it by looking for the name of the manufacturer.
* When you find the controller in question, find it's `evdev` identifier - this takes the form of `eventX`, where X is a number.
  * While here, also try to find what X believes your device to be - if it's controlling the mouse, this is likely to be `pointer`.
  * It is possible that you may also need the devices `jsX` identifier also, but I have not seen a case that requires it as of yet.
* Now you know the path of your device (under Linux, devices have a path just as a file or folder does - see `/dev/`), you can direct X to ignore any 'mouse-like' inputs. Browse to `/usr/share/X11/xorg.conf.d/`, and make a `.conf` file following the naming scheme `XX-devicetype.conf` - I'll be using `50-joystick.conf` as I already had a custom joystick rule present.
* In this newly created file, place the following:
```
Section "InputClass"
        Identifier "libinput pointer catchall"
        MatchDevicePath "/dev/input/event27"
        Driver "evdev"
        Option "Ignore" "on"
EndSection
```
Be sure to replace `/dev/input/event27` with the device number you found earlier. Change `pointer` to a different device type if needed.

This tells X a few things. First, it should be looking for any `pointer` devices connected, specifically to `event27`. If one is found, it is to be ignored by the window system entirely - **not** moving the cursor when inputs are provided.
* Restart to have the rule take effect.
* The connected device should no longer affect the cursor. In my case, it isn't detected as a controller at all, but I'm fairly certain it's an issue with the SRW-S1 itself.

*[(Source 1)](https://bbs.archlinux.org/viewtopic.php?id=185410) [(Source 2)](https://www.reddit.com/r/linux_gaming/comments/lj781z/presumed_xorg_controllermouse_issue/)*

## Headtracking

It's increasingly common these days for sim pilots, be their craft made for air or space, to use a headtracking solution for increased immersion and situational awareness. While niche, headtracking is supported quite well on Linux, if one knows where to look.

### Hardware

There are plenty of options when it comes to the physical headtracking hardware, each with different features and audiences.

#### Clips

Delan, trackhat, TrackIR, homemade
Mention mounting, adhesive/velcro and microsuction

#### Hats

Trackhat, source more if able

#### Cameras

TrackIR sensor, PS3Eye, DIY modding

### Software

#### Opentrack

Certainly the most popular option, Opentrack is...

#### Linuxtrack

A lesser known but certainly still valid choice, Linuxtrack is...

### Issues

V4L2 camera config, Linuxtrack firmware manual install, Opentrack wine limitations
