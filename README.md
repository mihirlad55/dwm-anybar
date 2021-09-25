# dwm-anybar
![Main CI](https://github.com/mihirlad55/dwm-ipc/workflows/Main%20CI/badge.svg)

**dwm-anybar** is a patch for dwm that enables dwm to manage external status
bars such as lemonbar and polybar. Dwm treats the external bar as it would its
own, so all regular dwm commands such as togglebar affect the external bar in
the same way.


## Requirements
This patch has no additional requirements beyond regular dwm.


## Appling the Patch
The patch can be found on the
[Releases](https://github.com/mihirlad55/dwm-anybar/releases) page. Download the
latest version of the patch that matches your version of dwm.

Update patches can also be found on the same page which can be applied on top of
previous `anybar` patches to fix bugs, improve compatability, etc.


## Configuration
```c
static const int showbar       = 1;          /* 0 means no bar */
static const int topbar        = 1;          /* 0 means bottom bar */
static const int usealtbar     = 1;          /* 1 means use non-dwm status bar */
static const char *altbarclass = "Polybar";  /* Alternate bar class name */
static const char *altbarcmd   = "$HOME/bar.sh"; /* Alternate bar launch command */
```
`showbar` and `topbar` affect the external status bar as it would dwm's status
bar. `showbar` must be `1` to show the external bar. `topbar` must be set
appropriately as well based on if the external bar is docked at the bottom or
the top of the screen. The patch only supports bars docked at the top/bottom of
the monitor.

`usealtbar` must be set to `1` to use an external status bar, otherwise dwm's
own bar will be enabled.

`altbarclass` must be set to the class name of the external status bar for dwm
to differentiate it from regular windows. The class name of the bar can be found
using `xprop`

```
xprop(1):
 WM_CLASS(STRING) = instance, class
                              ^^^^^
                              altbarclass should be set to this
 WM_NAME(STRING) = title
```

`altbarcmd` can be set to the path to a script that launches your bar.
Environment variables are supported. This method to starting your bar is
preferred since it ensures that the bar launches once dwm is ready rather than
launching too early. Make sure this script checks for running bars before
launching, else restarting dwm will cause duplicate bars. One way to do this
would be to add something like this at the beginning of your script:
```
if pgrep '^polybar' > /dev/null; then
  exit 0
fi
```


## Patch Compatability
If the patch is in some way incompatible with any other patches, feel free to
create an issue.

The `anybar` patch has been successfully applied together with the following
patches:
* activetagindicatorbar
* cfacts
* combo
* emptyview
* ewmhtags
* fullgaps
* gaplessgrid
* hide-vacant-tags
* ipc
* movestack
* pertag
* restartsig
* scratchpad
* statuspadding (merge conflict, but solveable)
* sticky
* swallow
* title color
* viewontag
* warp
* xrdb


## Polybar Tray Fix
Since polybar's tray is handled as a separate window and is populated slowly, it
is difficult to manage. There is a `polybar-tray-fix` version of the patch that
allows dwm to manage the tray. The tray isn't actually managed until the
togglebar command is called, but it fixes the issue where toggling the bar would
not hide the tray.

This version of the patch adds `alttrayname` to config.def.h which is already
set to the correct value.


## Related Projects
See [dwm-ipc](https://github.com/mihirlad55/dwm-ipc)

See [polybar-dwm-module](https://github.com/mihirlad55/polybar-dwm-module)
