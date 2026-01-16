# abstouch-wl

Use your touchpad as an absolute input device

Works for both X11 and Wayland.

## Features

- Map your touchpad input to a tablet-like device
- Customizable actions for touch, left-click and right-click events
- Two finger scrolling
- Custom scroll speed
- [TODO] ~~Scroll by holding down a button~~

## Running

Download the binary from the releases or compile from source.

Run:
```
sudo ./abstouch-wl
```

This will ask you a few questions and generate a new command that looks like the following:

```
./abstouch-wl -i /dev/input/event5 --on-touch ignore --on-left 272 --on-right 272 -s 1
```

Running this command will take control over your touchpad device and forward all your movements to a virtual tablet.

## Building

```
cargo build --release
```

## How does it work

Modern compositors like Hyprland, Sway, Gnome, KDE etc. use the Wayland protocol. Unlike traditional X11 window managers, Wayland compositors has a wastly different way of handling user input, making them incompatible with the previous abstouch-nux project.

Unlike X11, where we can just tell the display server to warp the mouse cursor to some position with XWarpCursor, we need to create a virtual device that we own and stop the original input device from talking to application by grabbing it.

This program uses the evdev library to grab a specified device (stop it from sending events to other applications) and then creates two devices, one that will act as a drawing tablet for the device and the other that acts as a mouse for the scrolling action. Then it translates the relevant touchpad events into the configured absolute events.

There should not be any calibration required unlike abstouch-nux since the actual movement of the cursor should be handled by the compositor.

Since I am not using any Wayland specific api, this program should work on X11 window managers as well. (Tested on i3wm)

## Similar Works

AbsoluteTouchEx (by apsun) for Windows: [https://github.com/apsun/AbsoluteTouchEx](https://github.com/apsun/AbsoluteTouchEx)

Abstouch-nux (originally by acedron) for X11: [https://github.com/rszyma/abstouch-nux](https://github.com/rszyma/abstouch-nux)