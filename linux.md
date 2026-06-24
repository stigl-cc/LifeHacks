# Highlighted less and cat
Highlighted cat(alias): `alias ccat='highlight -O ansi --force'`
Highlited less(in .sh file): `highlight -O ansi --force $1 | less`

# Not working TTY on an nvidia gpu
Simply add `nvidia_drm.modeset=1 nvidia_drm.fbdev=0` to your kernel arguments, in case of grub /etc/default/grub

# Small tty fonts on hidpi
Set the kernel argument of: `fbcon=font:TER16x32`

# Your resolution not being respected in tty
You can just configure it in your bootloader(like grub), and linux kernel seems to inherit it unless set otherwise
So if you use grub, it can look like `GRUB_GFXMODE=1920x1200x32`

# Fix doing focus next in i3wm on multiple monitors
Add: `focus_wrapping workspace`, and it will always limit it to the 1 monitor

# Controlling i3wm from bash
Simply prefix the i3 command with i3-msg, like so: `i3-msg workspace 3` and execute
 
# Controlling any playing media locally
`playerctl next`
`playerctl prev`
works on vlc, firefox, mpv, etc.

# Getting CPU temps
Inside: `/sys/class/thermal/*`
Is in milli celsius(Celsius * 1000)

# Getting CPU frequencies
Simply execute: `watch -n.3 "grep \"^[c]pu MHz\" /proc/cpuinfo"` and watch

# Execute as sudo but remain environment/theme
If you want to for example retain you user profile, often including the theme for your programs, simply use `sudo -E`

# No mouse on linux, controlling your mouse via keyboard
Simply execute: `setxkbmap -option keypad:pointerkeys` and then press Shift + Numlock, and control the mouse with your numkeys. Everything except scrolling works fine.

# Change audio to mono
```
pacmd \
load-module module-remap-sink \
sink_name=mono \
master=alsa_output.pci-0000_00_1f.3.analog-stereo \
channels=2 \
channel_map=mono,mono
```

# Change brightness of an external monitor:
For +20 brightness:
```
brightness=$(ddcutil getvcp 10 -t | awk '{print $4}' )
new_brightness=$(($brightness + 20))
limited_new_brightness=$(($new_brightness>100 ? 100 : $new_brightness))
echo "$brightness => $limited_new_brightness"
ddcutil setvcp 10 $limited_new_brightness
```
For -20 brightness:
```
brightness=$(ddcutil getvcp 10 -t | awk '{print $4}' )
new_brightness=$(($brightness - 20))
limited_new_brightness=$(($new_brightness<0 ? 0 : $new_brightness))
echo "$brightness => $limited_new_brightness"
ddcutil setvcp 10 $limited_new_brightness
```

# Run user-services on startup(even if user doesn't log in)
You need to enable linger
Check if enabled: 
`loginctl list-users`
Enable:
`loginctl enable-linger stigl`

# Run podman containers as a service(rootless)
1) Create directory for them
`mkdir .config/systemd/user/ -p`
2) Auto-generate service file
`podman generate systemd <container-name> > ~/.config/systemd/user/container-website.service`
3) Check out the file if its valid
`cat .config/systemd/user/container-website.service`
4) Success, now you can enable/start the service(the systemctl --user status and stuff works too)

# localhost isnt working for cloudflared(or your service in general)
Your server(tcp/udp//-ip  listener) might not support IPv6, try out `127.0.0.1`

# List all shutdowns/reboots/boots and stuff
`last -x`

# Disable sleep/hibernation
-> `/etc/systemd/sleep.conf`
```
[Sleep]

AllowSuspend=no
AllowHibernation=no
```

# Fix laggy webgl in firefox
`gfx.x11-egl.force-disabled = true`

# Stop firefox from creating desktop
`nano .config/user-dirs.dirs` 
And set your home(or any other directory) as the desktop directory.
