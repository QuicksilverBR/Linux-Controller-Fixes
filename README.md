# Linux-Controller-Fixes
A collection of fixes picked up over time to allow game controllers (primarily HOTAS systems) to work as expected under Linux.
Applies to both Wine and native applications.

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

## Controllers

State of controller support on Linux

### HOTAS

This section covers common issues with HOTAS systems on Linux, in both native and Wine applications.

#### HOTAS issues in Wine applications

Regedit fix
Lutris autoconfig

#### HOTAS issues in native applications

No titles to test with but info still valuable
`linuxconsoletools` and `jscal` go here

### Other controllers

This section covers common issues with other controller types (gamepad, wheel etc.) on Linux, in both native and Wine applications.

#### Issues in native applications

xconf/mouse goes here

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
