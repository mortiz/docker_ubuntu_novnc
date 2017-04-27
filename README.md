# docker_ubuntu_novnc

## What is docker_ubuntu_novnc anyway?

It is a Ubuntu zesty base image with x11vnc and novnc installed. It is intended to act as the base image for a full desktop environment like Mate, KDE, xfce, i3, etc. Please see my other repositories for examples.

## Usage

Dockerfile
```
FROM rigormortiz/ubuntu_novnc:zesty

...
```

## Noteworthy

### Supervisor

This image is based on my ubuntu supervisor image and, as such, runs on supervisor. Desktops based on this image will need to ship a supervisor config file similar to this one: (mate)

```
[program:dbus]
priority=14
directory=/
command=dbus-launch
user=%(ENV_DESKTOP_USERNAME)s
autostart=true
autorestart=true

[program:mate]
priority=15
directory=/home/%(ENV_DESKTOP_USERNAME)s
command=/usr/bin/mate-session
user=%(ENV_DESKTOP_USERNAME)s
autostart=true
autorestart=true
environment=DISPLAY=":1",HOME="/home/%(ENV_DESKTOP_USERNAME)s"
```

### Exposed ports

- x11vnc is exposed on port `5900`
- noVNC's http interface is exposed on port `6080`

### Misc.
- Default user is `ubuntu`. This can be changed via the `DESKTOP_USERNAME` variable in the Dockerfile
- Default VNC password is currently the same as `DESKTOP_USERNAME`. See TODO in next section. It can be easily changed at runtime with:

```
x11vnc -storepasswd somePassword /home/${DESKTOP_USERNAME}/.vnc/passwd
```

### TODO

- support for ssl
- dynamically generated VNC password
