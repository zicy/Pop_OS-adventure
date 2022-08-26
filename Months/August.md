# August

### Tasks
- [X] Update from Pop!_OS 21.04 => 22.04.
- [X] Make sure all installed software is up to date.
- [X] Make Razer Keyboard/ mouse lights static instead of breath
- [ ] Disable middlemouse click doing 'random' paste (From X server ?)
- [X] Getting Steam up and running.
- [X] Update Nvidia drivers
- [X] Getting non linux game PlateUp to run in Steam via Proton
- [X] Get Xbox controller to with Steam
- [ ] Get ie Sea of Thieves (Xbox live game) to run using Steam and Proton

---

### Update Pop!_OS
Updated through Settings => OS Upgrade & Recovery 

### Disable middlemouse click past
* Install 'gnome-tweaks' from Pop!_Shop
* Open 'GNOME Tweaks'
* Under 'Keyboard & Mouse' untick 'Middle Click Paste'
* * For me this only disabled it in GNOME, hmmm.

Source: [askubuntu.com](https://askubuntu.com/a/1144039)
Found this answer which seems to work everywhere.

```bash
echo '"echo -n | xsel -n -i; pkill xbindkeys; xdotool click 2; xbindkeys"' >> ~/.xbindkeysrc
xbindkeys -p
echo "" >> ~/.profile 
echo "xbindkeys -p" >> ~/.profile
```

Need to test it works after reboot.

### Razer 'Support'
Have allready installed [OpenRazer](https://openrazer.github.io/)

OpenRazer mentions 4 GUI on their website.
* [Polychromatic](https://polychromatic.app/)
* [RazerGenie](https://github.com/z3ntu/RazerGenie)
* [razerCommander](https://gitlab.com/gabmus/razercommander)
* [Snake](http://bithatch.co.uk/snake.html)

I are trying out 'Polychromatic' as it seems to be the biggest and also the first in the list :)

After installing i simply opened 'Polychromatic' and set my colors to my usual color with static, and the keyboard and mouse changed color as specified.

~~Just need to test that it's saved between restarts.~~

#### Keymapping
Polychromatic does not by it self support remapping button for ie F13.
But they do recommend the tool [key-mapper](https://github.com/sezanzeb/input-remapper)

You just simply download the .deb from the release page.
When downloaded open it and choose install.

In the tool do the following
* Press the field in the middle called 'new entry'.
* Then press 'Change Key'.
* Press the button you want to remap.
* In the text field enter what to map it to, in my case `KEY_F19`
* Press 'Apply' on the left hand side.

Need to test if still works without problem after OS restart.
It did not work.
Have modified the '~/.config/input-remapper/config.json'
Have added this to the autoload setting, need to test again after restart.
```json
    "autoload": {
        "Razer Razer DeathAdder V2": "F19"
    },
```

### Steam
* Install 'Steam' from Pop!_Shop, i choose deb instead of flatpack.

After Steam have been installed, just sign in as usually.
If the game does not natively support Linux and just displays "Available for ![windows official](https://cdn.emojidex.com/emoji/px32/windows_official.png?1618818637 "windows official")" the just do the following simple steps.

* Right click game
* Choose "Properties"
* Choose "Compatibility"
* Tick the "Force the use of a specific Steam Play compatibility tool"
* If needed choose a Proton version in the list ([ProtonDB](https://www.protondb.com/))
* Try and launch the game

The game launches, but the controller mappings was not quite right.

To enable 'Steam Play' for all games instead of doing it for each of them individually you can also do this.
* Press 'Steam' (Top left)
* Press 'Settings'
* Press 'Steam Play'
* Tick the box 'Enable Steam Play for all other titles'
* * Here its also possible to set the default Proton version.

#### gamemoderun
Installed 'gamemoderun' using Pop!_Shop [Github](https://github.com/FeralInteractive/gamemode).
Its suposedely makes games run faster/better ?
Need to test this.

To use it add one of these as 'launch options' for each game you want to use it with
```
gamemoderun mangohud %command%
```

or

```
gamemoderun %command%
```

### Xbox Controller
Simply opened Bluetooth and set the controller in paring mode.
The selected it in the menu and it was then connected.

After a bit of googling i found [xpadneo](https://github.com/atar-axis/xpadneo).
To install it i simply did the following.
```bash
mkdir -p $HOME/xpadneo/
cd $HOME/xpadneo/
git clone https://github.com/atar-axis/xpadneo.git
cd xpadneo
sudo ./install.sh
reboot
```

After reboot tested controller in the game PlateUp and it now worked as expected, without any additional changed needed.


### Nvidia Drivers
In Pop!_OS 22.04 there are now the option to choose driver version in the Pop!_Shop

After trouble doing it through Pop!_Shop i found a Reddit thread which i used to do it manually.

Source: [Reddit](https://www.reddit.com/r/pop_os/comments/t0pqvp/nvidia_upgrade_help_drivers_now_downgradable/)
```bash
sudo apt purge --autoremove '*nvidia*' '*nvidia*:i386'
sudo apt install nvidia-driver-515
```

Then after a reboot the Nvidia drivers was updated, confirmed using 'Nvidia X server'

---

### Strang things seen during this??
* After updating nvidia drivers to 515 and then trying to set main monitor to 120Hz then screen 1 turned.
* * I could still change settings for it in Display settings, but could not choose where it was i relation to the other monitors.
* * I tried changing the Hz back to 59.95Hz and the screen wakes up, but are now mirrored with screen 3.
* * After pressing Win+P and choosing "Join displays" all 3 screen was back on not mirrored.
* * After again aligning them in Display settings, i could change my main screen to 120Hz without issues.