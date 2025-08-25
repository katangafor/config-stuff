# config-stuff
rando config, dotfiles n stuff

config.fish
```
# common stuff
fish_add_path /opt/homebrew/bin

# only do this if you successfully installed java 21 with homebrew and linked it
set -gx JAVA_HOME /opt/homebrew/opt/openjdk@21/libexec/openjdk.jdk/Contents/Home
set -gx PATH $JAVA_HOME/bin $PATH

set -x EDITOR /opt/homebrew/bin/hx

function zpls
    hx ~/.config/fish/config.fish
end

function rez
    source ~/.config/fish/config.fish
end

function lg
    lazygit
end

function flushdns
    dscacheutil -flushcache
end

function t2
    tree -L 2
end

function t3
    tree -L 3
end

function dns
    dscacheutil -flushcache
    sudo killall -HUP mDNSResponder
end

# this is copy pasted from the nnn docs
function n --wraps nnn --description 'support nnn quit and change directory'
    if test -n "$NNNLVL" -a "$NNNLVL" -ge 1
        echo "nnn is already running"
        return
    end

    # The behaviour is set to cd on quit (nnn checks if NNN_TMPFILE is set)
    # If NNN_TMPFILE is set to a custom path, it must be exported for nnn to
    # see. To cd on quit only on ^G, remove the "-x" from both lines below,
    # without changing the paths.
    if test -n "$XDG_CONFIG_HOME"
        set -x NNN_TMPFILE "$XDG_CONFIG_HOME/nnn/.lastd"
    else
        set -x NNN_TMPFILE "$HOME/.config/nnn/.lastd"
    end

    # Unmask ^Q (, ^V etc.) (if required, see `stty -a`) to Quit nnn
    # stty start undef
    # stty stop undef
    # stty lwrap undef
    # stty lnext undef

    # The command function allows one to alias this function to `nnn` without
    # making an infinitely recursive alias
    command nnn $argv

    if test -e $NNN_TMPFILE
        source $NNN_TMPFILE
        rm -- $NNN_TMPFILE
    end
end

function p
    cd ~/projects
    n
end

if status is-interactive
    # Commands to run in interactive sessions can go here
end

```

# helix 
config.toml (`~/.config/helix/config.toml`)
```
theme = "dark_plus"

[editor]
line-number = "relative"
# Minimum severity to show a diagnostic after the end of a line:
# enable this and the other one for inline diagnostics which I don't rly need but they're cool so maybe later
# end-of-line-diagnostics = "hint"

[editor.inline-diagnostics]
# Minimum severity to show a diagnostic on the primary cursor's line.
# cursor-line = "error"

[keys.normal.backspace]
t = [":sh cargo test -- --nocapture"]
s = [":w", ":config-reload", ":format"]
r = [":sh cargo run"]
c = [":w", ":config-open"]
y = [":clipboard-yank"]

# idk if I really need thesse
D = ["goto_first_nonwhitespace", "extend_to_line_end"]
d = ["goto_first_nonwhitespace", "extend_to_line_end", "normal_mode"]

b = [":echo %sh{git blame -L %{cursor_line},+1 %{buffer_name}}"]


[keys.insert]
up = "no_op"
down = "no_op"
left = "no_op"
right = "no_op"
```

languages.toml (`~/.config/helix/languages.toml`)
```
[[language]]
name = "typescript"
formatter = { command = 'prettier', args = ["--parser", "typescript"] }
auto-format = true

[[language]]
name = "tsx"
formatter = { command = 'prettier', args = ["--parser", "typescript"] }
auto-format = true

[[language]]
name = "elixir"
scope = "source.elixir"
injection-regex = "elixir"
file-types = ["ex", "exs"]
roots = ["mix.exs"]
auto-format = true
diagnostic-severity = "hint"
comment-token = "#"
indent = {tab-width = 2, unit = " "}
language-servers = [ "elixir-ls" ]


```
