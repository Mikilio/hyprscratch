# Hyprscratch

A scratchpad utility for Hyprland

## Installation
Using cargo (Make sure `~/.cargo/bin` is in $PATH):

```
cargo install hyprscratch
```

## Usage
In `hyprland.conf`:

```bash
exec-once = ~/.cargo/bin/hyprscratch [OPTIONS] #start the hyprscratch daemon

bind = $MOD, $KEY, exec, ~/.cargo/bin/hyprscratch $WINDOW_TITLE "$HYPRLAND_EXEC_COMMAND" [OPTIONS] #configure scratchpads
```

Example scratchpad:

```bash
bind = $mainMod, b, exec, ~/.cargo/bin/hyprscratch btop "[float;size 70% 80%;center] kitty --title btop -e btop" onstart
```

### Daemon options:

* `clean [spotless]`: starts the daemon and hides all scratchpads on workspace change. The `spotless` option also hides them on losing focus to non-floating windows.

### Scratchpad options:

* `stack`: makes it so that the scratchpad doesn't hide one that is already present. This can be used to group multiple scratchpads by binding them to the same key and using `stack` on all except the first one. 

* `shiny`: makes it so that the scratchpad is not hidden by `clean spotless`.

* `onstart`: spawns the scratchpad at the start of the hyprland session.

* `special`: uses the special workspace. Ignores `stack`, `shiny` and is ignored by `clean` and `cycle`.

### Extra hyprscratch commands:

* `cycle`: cycles between non-special scratchpads in the order they are defined in the config file.

* `hideall`: hides all scratchpads, useful mostly for stacked ones.

* `reload`: reparses changes to the config file without restarting the daemon.

* `get-config`: prints out the parsed config, useful for debugging potential syntax issues.

## Other Relevant information
To find the title needed for a scratchpad, run `hyprctl clients` and check the `initialTitle` field. An incorrect title results in the scratchpad not being hidden and a new one being spawned instead.

Terminal applications often all use the title of the terminal emulator. Usually the title can be set with the `--title` flag to differentiate them.

If there are multiple scratchpads with the same initial title, the program just grabs the first one it finds.

Scratchpads don't have to be floating. This can also be used to just spawn a specific window, where the binding also hides it or grabs it from another workspace. Non-floating scratchpads are ignored by `clean`.

The program doesn't use hyprland's special workspace by default, it uses workspace 42.
