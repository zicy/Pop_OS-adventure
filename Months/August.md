# August

### Tasks
- [X] Update from Pop!_OS 21.04 => 22.04.
- [ ] Make sure all installed software is up to date.
- [ ] Make Razer Keyboard/ mouse lights static instead of breath
- [ ] Disable middlemouse click not doing scroll
- [ ] Getting Steam up and running.
- [ ] Getting non linux game PlateUp to run in Steam via Proton
- [ ] Get ie Sea of Thives to run using Steam and Proton

---

### Update Pop!_OS
Updated through Settings => OS Upgrade & Recovery 

### Disable middlemouse click past
* Install 'gnome-tweaks' from Pop!_Shop
* Open 'GNOME Tweaks'
* Under 'Keyboard & Mouse' untick 'Middle Click Paste'
* * For me this only disabled it in GNOME, hmmm.

### Razer 'Support'
Have allready installed OpenRazer LINK

OpenRazer mentions 4 GUI on their website.
* [Polychromatic](https://polychromatic.app/)
* [RazerGenie](https://github.com/z3ntu/RazerGenie)
* [razerCommander](https://gitlab.com/gabmus/razercommander)
* [Snake](http://bithatch.co.uk/snake.html)

I are trying out 'Polychromatic' as it seems to be the biggest and also the first in the list :)

After installing i simply opened 'Polychromatic' and set my colors to my usal color with static, and the keyboard and mouse changed color as specified.

Just need to test that it's saved between restarts.

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
* Press 'Apply' on the lefthand side.

Need to test if stil works without problem after OS restart.