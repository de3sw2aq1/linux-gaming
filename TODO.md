Antigravity in Podman/litterbox?

Script/prompt to generate a lutris install yaml
https://github.com/lutris/lutris/blob/master/docs/installers.rst
Use variable substitution for easy config (but any data that needs to preserved should be written to a uniquely named environment variable or json(?) file inside the GAMEDIR)

GAMEDIR is a parent folder for:

prefix/ - wine prefix
install/ - user populated directory of installer zips? auto extract to a new game-vxxx dir and move to install-old/
install-old/ - old/cached install zips files
game-v001 - extracted contents of zip, stripping all but top level
game-v002... - updated versions of game
state/ - overlayfs writable bottom dir for save files/etc inside game dir if any
overlay-001/ overlay-002/ - overlayfs overlays over game dir (game mods)
game/ - empty dir, overlayfs mountpoint (game's exe will be run from below here) Use as working dir for game

how to sandbox wine?
bubblewrap? firejail? etc
block network traffic
block filesystem access except for prefix/ and game/ (or all of GAMEDIR is maybe ok)

Use Lutris "command prefix" to wrap the `wine` command execution with `bwrap`
--overlay-src game-v1/ --overlay-src overlay-001/ --overlay-src overlay-002/ --overlay state/ workdir/ game/
--chdir game/


--symlink SRC DEST

maybe use opensnitch
or some custom firewalling (mitmproxy would be neat)


maybe have some auto-inject modding tools options
inject frida gadget perhaps (linux or windows version?)


autoupdater:
get game url, and do the lazy "has the 3xx redirect changed" test
or \*checker
manual 


run a bash script, that runs podman run to do the install stuff
(chroot-ish podman into dir)
`game-setup new foo/`
interactively prompt for stuff
press `o` open install/ dir in file browser
user press `e` to extract all install/ files, if only one, its the game, if extras, prompt for game in selection list, others are probably overlays
have user press `l` or something to trigger lutris install, show install yaml first?
user press `i` for running an installer exe