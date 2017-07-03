# Initial System Setup

## Set sudo without password

If your internal network at the observatory is already secured, it should be
safe to set sudo without password for user `pi`.

1. Run `visudo` command:

        $ sudo visudo

2. Add `NOPASSWD:` to the `%sudo` group.

        # Allow members of group sudo to execute any command
        %sudo   ALL=(ALL:ALL) NOPASSWD:ALL


## Improve configuration file of the nano editor

The `nano` is fast and lightweight text editor that actually contains some neat
features.  Unfortunatelly, the default configuration is quite limited, so it's
generally a good idea to look at the global configuration file:

    $ sudo nano /etc/nanorc

I recommend to enable following subset of options.  Note that in case of `set
tabsize` I changed default tabsize from 8 to 4.

    ## Use auto-indentation.
    set autoindent

    ## Backup files to filename~.
    set backup

    ## Use bold text instead of reverse video text.
    set boldtext

    ## Do case sensitive searches by default.
    set casesensitive

    ## Constantly display the cursor position in the statusbar.  Note that
    ## this overrides "quickblank".
    set const

    ## Use cut to end of line by default.
    set cut

    ## Use the blank line below the titlebar as extra editing space.
    set morespace

    ## Enable mouse support, if available for your system.  When enabled,
    ## mouse clicks can be used to place the cursor, set the mark (with a
    ## double click), and execute shortcuts.  The mouse will work in the X
    ## Window System, and on the console when gpm is running.
    ##
    set mouse

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


## Quick setup using raspi-config tool

Raspbian comes with `raspi-config` tool for quick setup.

    $ sudo raspi-config

Now we will go through the individual menu entries


### 1 Change User Password

Default password for the user `pi` is `raspberry`.  Since it is the same for all
Raspberry Pi devices, it is unsafe to keep it unchanged.


### 2 Hostname

Set whatever hostname you would prefer for your observatory controller.  I set
`observatory-controller`.


### 3 Boot Options

I set just the following two boot options.

| Boot Option      | Value   |
| ---------------- | ------  |
| B1 Desktop / CLI | Console |
| B3 Splash Screen | Disable |


### 4 Localisation Options

I set only following three options, while selecting `en_US.UTF-8` as default
locale.

| Localization Option     | Value(s)                              |
| ----------------------- | ------------------------------------- |
| I1 Change Locale        | en_GB.UTF-8, en_US.UTF-8, sk_SK.UTF-8 |
| I2 Change Timezone      | Bratislava                            |
| I4 Change Wi-Fi Country | SK Slovakia                           |


### 5 Interfacing Options

I set single option required for SSH connectivity.

| Interfacing Option | Value |
| ------------------ | ----- |
| P2 SSH             | Yes   |


### 6 Advanced Options

I set following 5 options here.

| Advanced Option      | Value              |
| ---------------------| ------------------ |
| A1 Expand Filesystem | Yes                |
| A2 Overscan          | No                 |
| A3 Memory Split      | 16                 |
| A4 Audio             | 1 Force 3.5mm jack |
| A6 GL Driver         | Legacy             |


## Connect via SSH

Now we should be already able to connect to Raspberry Pi via SSH from the other
computer.

1. It very handy to map IP address of Raspberry Pi to actual hostname on your
   computer.  Open `/etc/hosts` file:

        [gszasz@iris ~]$ sudo nano /etc/hosts

   And add following line (IP address depends on your network):

        192.168.1.108  observatory-controler


2. If you've not done it yet, generate RSA private key with empty passphrase.
   Just hit `ENTER` three times after executing the command.

        [gszasz@iris ~]$ ssh-keygen


3. Copy the key to the Raspberry Pi.

        [gszasz@iris ~]$ ssh-copy-id pi@observatory-controller


4. Now you should be able to SSH into the Raspberry Pi without password.

        [gszasz@iris ~]$ ssh pi@observatory-controller
        pi@observatory-controller ~$


Now it is no longer necessary to have connected keyboard and screen directly to
the device as long as it is connected to the network.


## Disable Bluetooth

It is very unlikely that anybody would connect any bluetooth devices now.  It is
time to turn it off.

1. Power off bluetooth radio

        $ sudo bluetoothctl
        [bluetooth]# power off
        [bluetooth]# quit

2. Permanently disable bluetooth interface.

        $ sudo nano /boot/config.txt

   Add following line to the file:

        dtoverlay=pi3-disable-bt

3. Reboot Raspberry Pi

        $ sudo reboot


## Extend Swapfile size

Default swap space on Raspbian image is minimalistic. Now we have whole SD card
to go, so now is the time to expand swap space to proper size.

1. Turn off swap

        $ sudo dphys-swapfile swapoff

2. Configure swap size to 2x RAM Size.

        $ sudo nano /etc/dphys-swapfile

   Convince yourself that `CONF_SWAPSIZE` is commented out and uncomment
   `CONF_SWAPFACTOR=2` line

        # set size to absolute value, leaving empty (default) then uses
        #   computed value you most likely don't want this, unless you
        #   have an special disk situation
        #CONF_SWAPSIZE=512

        # set size to computed value, this times RAM size, dynamically
        #   adapts, guarantees that there is enough swap without wasting
        #   disk space on excess
        CONF_SWAPFACTOR=2

        # restrict size (computed and absolute!) to maximally this limit
        #   can be set to empty for no limit, but beware of filled
        #   partitions!  this is/was a (outdated?) 32bit kernel limit (in
        #   MBytes), do not overrun it but is also sensible on 64bit to
        #   prevent filling /var or even / partition
        #CONF_MAXSWAP=2048

3. Reload the configuration file.

        $ sudo dphys-swapfile setup

4. Turn swap on again.

        $ sudo dphys-swapfile swapon


## Set Locales

Default locale settings are still not satisfying enough, at least not for an
observatory located in Slovakia :).

1. Set global locales.

        $ sudo nano /etc/default/locale

   Make sure the file contains following lines:

        LANG=en_US.UTF-8
        LANGUAGE=en_US:en
        LC_TIME=en_GB.UTF-8
        LC_PAPER=en_GB.UTF-8
        LC_MEASUREMENT=en_GB.UTF-8
        LC_MONETARY=sk_SK.UTF-8
        LC_COLLATE=C

2. Set US keyboard

        $ sudo localectl set-keymap us set-x11-keymap us

3. Verify if locales are all set properly.

        $ localectl
           System Locale: LANG=en_US.UTF-8
                          LANGUAGE=en_US:en
                          LC_TIME=en_GB.UTF-8
                          LC_COLLATE=C
                          LC_MONETARY=sk_SK.UTF-8
                          LC_PAPER=en_GB.UTF-8
                          LC_MEASUREMENT=en_GB.UTF-8
               VC Keymap: us
              X11 Layout: us


## Update Raspbian and install essential packages

Note that Raspberry Pi has to be connected to the network.  If it is not online,
proceed with the Network Setup](network-setup.md) first.

    $ sudo apt-get update -y

There are some essential packages that are generally useful for debugging and
maintenance of the observatory controller.

    $ sudo apt-get install -y wget tmux mc


Improve `nano` editor settings:

    $ sudo nano /etc/nanorc

    ## Use auto-indentation.
    set autoindent

    ## Backup files to filename~.
    set backup

    ## Use bold text instead of reverse video text.
    set boldtext


## Install extra RPi tools

I am maintaining a repository of useful scripts for Raspbian Jessie.  It's
generally a good idea to install it.

    $ mkdir -p ~/src && cd ~/src
    $ git clone https://github.com/gszasz/rpi-scripts.git
    $ cd rpi-scripts
    $ sudo ./install.sh

---

[Home](../README.md) > [Installation](README.md) > *Initial System Setup*
