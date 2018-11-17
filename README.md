# Strong Screen Intelligence (ssi)

This is a script designed to assist attaching, removing, and changing your multiple-monitor setup under Linux.

Dependencies:
* python3
* sh module (pip install sh)
* xrandr on your path
* randr compatible video card (most nvidia and ATI cards work)

# Usage

    # list your ports from "left to right" of how the screens will be that are plugged into it
    $ echo '{"priority":["DP-2","HDMI-0","DP-0"]}' > conf/priority.json
    # plug in the screens you want active, and run this:
    $ ./bin/ssi --help
    usage: ssi [-h] [--dry-run] [--mode MODE] [--screen-priority SCREEN_PRIORITY]
    
    optional arguments:
      -h, --help            show this help message and exit
      --dry-run             Only print commands that would be run
      --mode MODE           Name of the mode to switch to
      --screen-priority SCREEN_PRIORITY
                            JSON file mapping the port name to screen positions
                            from left to right
    $ ./bin/ssi --dry-run
    Found port DP-0 at resolution 2560x1440 (connected=True, active=True)
    Found port DP-0 has max resolution 2560x1440
    Found port DP-1 at resolution None (connected=False, active=False)
    Found port HDMI-0 at resolution 2560x1440 (connected=True, active=True)
    Found port HDMI-0 has max resolution 2560x1440
    Found port DP-2 at resolution 1920x1080 (connected=True, active=True)
    Found port DP-2 has max resolution 1920x1080
    Found port DP-3 at resolution None (connected=False, active=False)
    Found port DP-4 at resolution None (connected=False, active=False)
    Placing screen 0 on port DP-2 (max resolution 1920x1080) at position 0x0
    Placing screen 1 on port HDMI-0 (max resolution 2560x1440) at position 1920x0
    Placing screen 2 on port DP-0 (max resolution 2560x1440) at position 4480x0
    DRY_RUN: would be running xrandr --output DP-2 --mode 1920x1080 --rotate normal --pos 0x0 --output HDMI-0 --mode 2560x1440 --rotate normal --pos 1920x0 --output DP-0 --mode 2560x1440 --rotate normal --pos 4480x0 --output DP-1 --off --output DP-3 --off --output DP-4 --off
    # you can now run the above command via copy/paste, or rerun the command without --dry-run


# TODO:

Currently, only xrandr bits are implemented, but I hope to do more soon.

The goal is to have a set of configurations you can switch between and also invoke window-manager specific commands (i.e. for ratpoison, bind keys to flip between screens)

For example, when I am in single monitor mode, I might have the following applications running:

* ctrl-t 0 xterm showing main tmux
* ctrl-t 1 main firefox browser window
* ctrl-t 2 xterm showing console chat program
* ctrl-t 3 chrome browser window
* ctrl-t 4 xterm for playing music / alsamixer
* ctrl-t 5 vncviewer for running IntelliJ or other java apps that behave poorly with ratpoison

When I attach a second monitor, I might like to automatically:

* run xrandr to set desktop dimensions and screen layouts
* restart ratpoison to detect new desktop dimensions
* bind ctrl-t ctrl-1 to the laptop screen
* bind ctrl-t ctrl-2 to the new screen
* re-number all windows to match the above, since restarting ratpoison loses it

When I attach a third monitor, I might like to do the following:

* run xrandr to set desktop dimensions and screen layouts
* restart ratpoison to detect new desktop dimensions
* split the laptop screen vertically into two frames
* bind ctrl-t ctrl-1 to the left frame laptop screen
* bind ctrl-t ctrl-2 to the right frame laptop screen
* bind ctrl-t ctrl-3 to the first new screen
* bind ctrl-t ctrl-4 to the second new screen
* re-number all windows to match the above.
* put application 2 in ctrl-t ctrl-1 frame
* put application 3 in ctrl-t ctrl-2 frame
* put application 0 in ctrl-t ctrl-3 frame
* put application 1 in ctrl-t ctrl-4 frame

Currently, I basically do all of these steps manually any time I plug in or remove a monitor.  That's dumb.  I want it to happen automatically.  That's why I wrote this.


