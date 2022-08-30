# Trying Wayland on Pop!_OS
Using AMD CPU + Nvidia GPU

## Requirements
* Nvidia driver version 510+
* Pop!_OS 22.04
* Other ??

### Tasks
- [X] Make sure up to date Nvidia driver are installed.
- [X] Change settings in GDM3
- [X] Add start parameter to enable Nvidia DRM.
- [X] Make sure udev rules don't disable Wayland
- [ ] Make Wayland default WM


### Check Nvidia driver version 
There are multiple ways to check the current version.
ie. Nvidia X server or nvidia-smi

Here we are going to check using `nvidia-smi`

Here is an example of the output from `nvidia-smi` from my system, where we can see that we are running driver version 515.
So should be good.

```bash
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 515.48.07    Driver Version: 515.48.07    CUDA Version: 11.7     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA GeForce ...  Off  | 00000000:0A:00.0  On |                  N/A |
| 31%   40C    P0    34W / 245W |   1395MiB /  8192MiB |      1%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
```


### Change settings in GDM3
We need to comment out a line in here which disables Wayland

Simply run this command, and comment out the line `WaylandEnable=false`
```bash
sudo vi /etc/gdm3/custom.conf
```


### Enable Nvidia DRM
We need to add a parameter to the boot.

Simply run this command, and add this `nvidia-drm.modeset=1` parameter at the end of the other parameters of `options`
```bash
sudo vi /boot/efi/loader/entries/Pop_OS-current.conf
```

### Changes to udev rules
For nwo we simple comment 1 lines in our `61-gdm.rules`

Simply run this command, and go to the bottom and comment out the line which contains this string `gdm-runtime-config set daemon WaylandEnable false`

```bash
sudo vi /usr/lib/udev/rules.d/61-gdm.rules
```

Source: [Reddit](https://www.reddit.com/r/pop_os/comments/iyh890/guide_enabling_wayland_on_nvidia/)

---