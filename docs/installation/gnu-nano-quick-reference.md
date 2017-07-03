# GNU nano Quick Reference

*GNU nano* is a lightweight text editor that emulates look and feel of the
legendary *Pico* editor.  Although *GNU nano* undoubtedly cannot compete with
complexity and flexibility of *Vim* or *Emacs*, it is powerful enough to become
first choice of sysadmins and a default editor in several Linux distributions,
including Raspbian.


## Keyboard shortcuts

Nano does not provide any menus, so you need to memorize a couple of keyboard
combinations to use it.  Two lines at the bottom serve as contextual help that
shows you the important keyboard shortcuts.  Note that they are written in UNIX
notation (e.g. `^X` translates to `Ctrl-X`).  Following table summarizes the
most important of them:

| Keystroke | Function          | Description                     |
| --------- | ------------------| ------------------------------- |
| `Ctrl-G`  | `Get help`        | Help                            |
| `Ctrl-X`  | `Exit`            | Save file and exit              |
| `Ctrl-O`  | `Write Out`       | Save file as                    |
| `Ctrl-W`  | `Where Is`        | Search                          |
| `Alt-W`   | `WhereIs Next`    | Search next                     |
| `Alt-R`   | `Replace`         | Search and replace              |
| `Ctrl-K`  | `Cut`             | Cut line(s) / Cut selection     |
| `Ctrl-U`  | `UnCut`           | Paste line(s) / Paste selection |
| `Ctrl-^`  | `Set mark`        | Start selection                 |
| `Alt-^`   | `Copy`            | Copy text of the selection      |
| `Alt-G`   | `Go To Line`      | Move to given position          |
| `Ctrl-R`  | `Open new buffer` | Open another file               |
| `Alt->`   | `Next buffer`     | Switch opened files forward     |
| `Alt-<`   | `Previous buffer` | Switch opened files backward    |

The last two keystrokes are actually `Alt-.` and `Alt-,`.  There is no need to
hold `Shift`.  The same applies to shortcuts `Ctrl-^` and `Alt-^` for selecting
and copying text.  Vast majority of people press `Ctrl-6` and `Alt-6` instead.

It's generally a good idea to tweak the global configuration file a little bit:

    $ sudo nano /etc/nanorc

I recommend to enable following options.  Note that in case of `set tabsize`
option I prefer to change tab size 4 instead of default 8.

    ## Use auto-indentation.
    set autoindent

    ## Backup files to filename~.
    set backup

    ## Do case sensitive searches by default.
    set casesensitive

    ## Constantly display the cursor position in the statusbar.  Note that
    ## this overrides "quickblank".
    set const

    ## Use the blank line below the titlebar as extra editing space.
    set morespace

    ## Allow multiple file buffers (inserting a file will put it into a
    ## separate buffer).  You must have configured with --enable-multibuffer
    ## for this to work.
    ##
    set multibuffer

    ## Make the Home key smarter.  When Home is pressed anywhere but at the
    ## very beginning of non-whitespace characters on a line, the cursor
    ## will jump to that beginning (either forwards or backwards).  If the
    ## cursor is already at that position, it will jump to the true
    ## beginning of the line.
    # set smarthome

    ## Use smooth scrolling as the default.
    set smooth

    ## Use this tab size instead of the default; it must be greater than 0.
    set tabsize 4

    ## Convert typed tabs to spaces.
    set tabstospaces

    ## Enable the new (EXPERIMENTAL) generic undo code, not just for line cuts
    set undo

---

[Home](../README.md) > [Installation](README.md) > *GNU nano Quick Reference*
