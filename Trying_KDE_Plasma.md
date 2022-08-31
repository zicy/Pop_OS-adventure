# Trying KDE Plasma on Pop!_OS
Using X11 for now, and maybe Wayland latter ?

## Requirements
* hm

### Tasks
- [X] Install KDE Plasma
- [ ] Try it out, any customization needed ?
- [ ] Chrome don't use KWallet
- [X] Configure 'dock' on each screen



### Install KDE Plasma
Used guide from System76

```bash
sudo apt install kde-standard
```

Choose to continue with GDM3 for now.

After that simple restart and because its not the default to use KDE, remember to choose it during login.

Source: [System76](https://support.system76.com/articles/desktop-environment/#kde-plasma)


### Dock/taskbar customize
Simply use KDE Plasma 'editmode'

On each of the extra screen do the following.
* Right click and choose 'Add panel'
* Choose 'Default Pane'
* Right click new dock and choose 'Edit mode'
* Hover over each widget in the dock and choose 'Remove' exempt from the widget which shows open apps
* Hover over the icon widget and choose configure (Configuration to this is per dock, not globally)
    * Under 'Appearance' change maximum rows from 2 => 1
    * Under 'Behavior' check the checkbox called 'From current screen'
    * Press OK
* Press 'Add spacer' X 2 (Only needed to center apps on dock)
    * Drag the spacers so that its on each side of the apps 
* Press ESC X 2 to exit 'Edit mode'
* Right click on the pined apps to remove them (Pined apps are per bar, not globally)

---