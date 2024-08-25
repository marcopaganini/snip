![ShellCheck](https://github.com/marcopaganini/snip/actions/workflows/shellcheck.yml/badge.svg)

# snip - CLI snippets manager in bash

## Description

`snip` is a simple snippet manager for bash, which allows the user to save
useful and important bash code snippets directly from the bash prompt. It is
especially useful to save long one-liners, or rarely used commands which we tend
to forget. `snip` is written in pure bash, but uses
[fzf](https://github.com/junegunn/fzf) to allow the user to perform searches on
the snippet database.

![Screen Cast](https://raw.githubusercontent.com/marcopaganini/snip/master/assets/snip.gif)

## Motivation

Shell scripting is a powerful tool in the right hands. The piping capabilities
coupled with the many readily available utilities make it possible to perform
complex and useful operations on all sorts of data. Most medium to advanced bash
users will have a number of these stored somewhere, from text files to
[Google Keep](http://keep.google.com). Those solutions involve cutting and
pasting, and in the case of web-based solutions, the use of a browser.

Consider for example the following sequence of commands, taken from a real use
case, a command to restart a VNC server using `systemd-run`:

```
if [[ -s "${VNCPASSWD}" ]]; then
  systemctl --user reset-failed x0tigervncserver;
  systemd-run --user --unit=x0tigervncserver --  /usr/bin/x0tigervncserver \
    --verbose --localhost no --fg  --SecurityTypes=TLSVnc \
    --PasswordFile="${VNCPASSWD}" "${DISPLAY}"
fi
```

It took a few tries to get the command "just right", which naturally filled the
history with all sorts of invalid or "almost valid" commands.

With `snip`, it's just be a matter of typing `Ctrl-x` `Ctrl-n` ("N" for "new")
directly at the command-line prompt to save the snippet when the correct command
shows. To find and recall a saved snippet just type `Ctrl-x` `Ctrl-R` ("R" for
"Run"). This will fill-in the command-line prompt with the command, ready to
execute.

## Features

*   Save and recall snippets directly from the bash prompt with keybindings.
*   One line setup, won't slow down your `.bashrc` loading.
*   Uses a simple text file for the database.
*   CLI commands to edit the database manually.
*   CLI commands to list the contents of the database in a formatted way.
*   Automatically saves a time stamp with the command.
*   Written in pure shell. To install just copy one file to `/usr/local/bin` or
    any other location in your `PATH`.
*   Displays full usage with a simple command (`snip help`).

## Installation

Installation is easy. Just cut & paste the two lines below. To "upgrade" snip,
just repeat the same commands:

```
sudo curl -s https://raw.githubusercontent.com/marcopaganini/snip/master/snip >/usr/local/bin/snip
sudo chmod 755 ~/usr/local/bin/snip
```

Then edit your `~/.bashrc` file and add the following:

```
eval "$(/usr/local/bin/snip setup)"
```

Alternatively, if you prefer using a one-liners to setup your `~/.bashrc`, you
can also do:

```
eval '"$(/usr/local/bin/snip setup)"' >> ~/.bashrc
```

## Using snip

The `eval` command above installs two new keybindings to your bash
configuration:

*   `Ctrl-X` `Ctrl-N`: Add the current line to the snippet database. It will
    replace the current line with a `snip add` command to insert your snippet
    into the database. Just hit ENTER afterwards and provide a description.

*   `Ctrl-X` `Ctrl-R`: This starts fzf so the user can choose a command to be
    recalled. The command is inserted in the current command-line input. If no
    modifications are required, just hit ENTER to execute the command.

It's also possible to perform operations directly from the CLI, using `snip
command`. For a list of commands, see the "Appendix: Manual Page" section below.
You can also recall the full documentation directly from the command-line by
typing `snip help`.

## Future changes

`snip` is in its early stages and I have a number of ideas for future features.
Some ideas:

*   A bit more color in the fzf selector for those who prefer it.
*   git integration - commit and pull your snippets automatically from your
    repo.
*   zsh integration.
*   A `snip delete` command (right now, use `snip edit` and delete the snippet
    line directly from your editor.)
*   Other general improvements.

## Similar options

*   [https://github.com/knqyf263/pet](https://github.com/knqyf263/pet) - pet is a snippet manager written in Go.
    if you like snip, you may like pet too.

## Comments and ideas

Feel free to open issues for any problems you find and send me your comments,
ideas, and PRs.

## Appendix: Manual Page

```
NAME
    snip - CLI to manage shell code snippets.

SYNOPSIS
    snip [command] [options]

DESCRIPTION
    The snip utility allows users to easily manage bash code snippets.
    Users can save and execute commands directly from the bash command line,
    with the provided keybindings. It's also possible to perform operations
    on the snippet database directly on the command-line.

OPTIONS
    The first argument is always a command. Commands may or may not have
    options (see below for a summary).

    add [-f, --file FILENAME] [-D, --delete] [command]
        The add command adds a new entry to the snip database. Command must
        be properly quoted and will be added "as-is". The program will ask
        for a description from the keyboard.

        If used with the `--file` option, snip will read the command to be
        saved from a filename. Using `--delete` with the `--file` option
        causes the file to be deleted after its contents are added to the
        database. This is useful when adding content from temporary files.

    edit
        Invoke the text editor on the database file. The database is a simple
        text file using '|' (pipe) as a delimiter. Every line needs to have
        three exact fields: timestamp, description, and command. It is
        acceptable for the command to contain pipe characters, but not for
        the timestamp or description fields.

    find [-q, --query STRING]
        The find command invokes fzf on the database and prints the
        command-line for the snippet chosen by the user. The `--query` flag
        sets the initial query for fzf, if present.

    help
        This helpful message. :)

    list, ls
        The list command issues a formatted listing of the snippet database,
        including the snippet creation timestamps.

    setup
        This command issues the required commands to setup the bash
        command-line bindings to easily add snippets and re-run saved
        snippets. To install snip, run this from your `~/.bashrc` file:

        eval "$(/path/to/snip setup)"

        This will create a few functions in your bash namespace and two
        bindings. By default they are:

        Ctrl-X Ctrl-N
            Add the current line as a new snippet. The program will replace
            the text in the command-line with the appropriate `snip add`
            command.  All the user needs to do is press the ENTER key to
            confirm the action and enter a description.

        Ctrl-X Ctrl-R
            Find and run a saved snippet. This will open an fzf window and
            allow the user to choose a snippet to run. Once selected, snip
            will replace the current bash input buffer with the command to
            run.
    version
        Show the program version.

SETUP
    Just add `eval "$(/path/to/snip setup)"` to your `~/.bashrc`. Depending on your
    setup, you may need to add it to `~/.profile` as well).

CONFIGURATION FILE
    On the first run, snip will create a configuration file with default
    values.

    Currently, it is possible to override a few items in the config file.
    To do that, edit `~/.config/snip/config` and/edit add the following:

    # bash keybindings
    #
    # The two settings below control the bindings for the add and find
    # commands, respectively. C-key means Control+key. Use man bash and
    # look for the "bind" command to find the full syntax for the sequence.

    SNIP_BIND_ADD='"\C-x\C-n"'
    SNIP_BIND_FIND='"\C-x\C-r"'

    # bat (aka batcat) theme. If you have batcat installed, snip will
    # automatically use it to syntax highlight the snippets in the fzf preview
    # window. You can override the theme using the setting below.To see all
    # available themes, run `cat --list-themes` (or `batcat --list-themes`
    # in some distributions.)
    BAT_THEME="gruvbox-dark"

    The lines above show the default keybindings to add and run commands.
    Please keep in mind that this file is sourced directly by the main
    program, so it should be a valid bash file. Look for the the key syntax
    for "bind -x" in the bash manpage for details.

REQUIREMENTS
    This program requires FZF to run (https://github.com/junegunn/fzf). Please note that
    fzf is very popular and available natively in most Linux distributions.

    If installed, snip will use `batcat` (aka bat) to provide syntax highlighting
    in the preview window.

AUTHOR
    (C) Aug/2024 by Marco Paganini <paganini [at] paganini [dot] net>

```
