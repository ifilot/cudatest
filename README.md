# Installation

Follow the steps below for a first-time installation of the CUDA Toolkit.

1. Login as root after disabling X-server with CTRL+ALT+F2
2. Unload module with

```
systemctl isolate multi-user.target
modprobe -r nvidia-drm
```
3. Install latest CUDA Toolkit (now 9.2), make sure you also install the accompanying drivers!
