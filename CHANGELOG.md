# Version 0.0.2

*   Added [bat](https://github.com/sharkdp/bat) support. If you have `bat`
    installed (or `batcat` as Debian installs it), snip will use it to provide
    syntax highlight of your shell snippet in the `fzf find` preview window. You
    can configure the theme used by cat by changing the `BAT_THEME` variable in
    your config file (`~/.config/snip/config`). # Version 0.0.1

*   Make snip create a default configuration file on the first run. This makes
    it easier to find and edit this file later (`~/.config/snip/config`).

*   Added `snip ls` as an alias to `snip list`.

*   Added `set -o nounset` to the code (makes things tidier).

*   Reject empty descriptions or descriptions containing the pipe character.

*   Fix truncation inside fzz preview window. Also fix incomplete command
    display when command contained pipes ("|").

*   `snip list` will now wordwrap the command to the match the width of the
    terminal, making it easier to see the command. If you need to get the
    unwrapped snippet, use `snip find`.

*   Lots of internal code improvements.

# Version 0.0.1

Initial Release (Aug/2024)
