# Changelog

## Version 0.1.0

*   NEW: New flags to alter the behavior of find and list. By default, both
    find and list will only show the description and the command for snippets
    in the database for the current host. If you want to see commands for all
    hosts, use the "-a" or "--all" flag to either of these commands. To see all
    the details in the database (including hostname and snippet creation
    timestamp), use the "-l", or "--long" flag.

## Version 0.0.5

*   BUGFIX: v0.0.4 didn't update the version number inside the script.

## Version 0.0.4

*   BUGFIX: Check if database exists before attempting to migrate it
    from older versions.

## Version 0.0.3

*   NEW: git repository sync support: To use, create a free repository on your
    git provider of choice (github, gitlab, etc...) and use `snip repo
    <your_repo_url>` to initialize the repository sync. This will also run the
    first sync. From this point on, use `snip sync` to synchronize changes.
    Both your configuration file and database will be synced.

*   NEW: multi-host support: When using git sync, you can use the same
    repository across multiple hosts. There's a new field, "HOST", in the
    database and FZF will also search by host when recalling a snippet with
    `Ctrl-X/Ctrl-R` or `snip find`. V0.0.3 should automatically migrate the
    database from previous versions.

*   NEW: Besides `snip repo` and `snip sync` (described above), there's now a
    `snip log` command that shows you the history of your repository sync
    changes.

*   NEW: Changed the way `Ctrl-X/Ctrl-R` works: Now, you won't see a `snip add`
    command on the command-line that requires ENTER to execute. Instead, `snip
    add` will be called directly, for a cleaner experience.

*   NEW: You can now set the default editor in your config file (`SNIP_EDITOR`).
    Thanks hrfried for the PR!

*   NEW: Config file changes: Previously, we used a file called `config` for
    all configurations. This file is now suffixed by the hostname. This was
    changed so that we can easily sync multiple host configs without git
    conflicts.  Also, on the first run, snip will now generate a fully
    commented out config file, and rely on internal defaults.

*   Added emojis to some messages, for your viewing pleasure. :)

*   Better input sanitization: Explicitly checks for "|" (pipe characters) in
    the description and rejects the input. Pipes cannot be used in the
    description as they're also used as field separators in the database.

## Version 0.0.2

*   NEW: [bat](https://github.com/sharkdp/bat) support. If you have `bat`
    installed (or `batcat` as Debian installs it), snip will use it to provide
    syntax highlight of your shell snippet in the `fzf find` preview window.
    You can configure the theme used by cat by changing the `BAT_THEME`
    variable in your config file (`~/.config/snip/config`). # Version 0.0.1

*   NEW: Added `snip ls` as an alias to `snip list`.

*   NEW: `snip list` will now wordwrap the command to the match the width of the
    terminal, making it easier to see the command. If you need to get the
    unwrapped snippet, use `snip find`.

*   BUGFIX: Reject empty descriptions or descriptions containing the pipe character.

*   BUGFIX: Fix truncation inside fzz preview window. Also fix incomplete command
    display when command contained pipes ("|").

*   Make snip create a default configuration file on the first run. This makes
    it easier to find and edit this file later (`~/.config/snip/config`).

*   Added `set -o nounset` to the code (makes things tidier).

*   Lots of internal code improvements.

## Version 0.0.1

Initial Release (Aug/2024)
