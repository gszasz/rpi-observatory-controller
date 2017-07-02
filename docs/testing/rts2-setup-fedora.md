# RTS2 Setup on Fedora

I found useful to test newest RTS2 sources using dummy devices on my laptop that
is running Fedora.  Since I did not found any documentation for compiling and
installing RTS2 on Fedora, I have decided to add this section.

1. Install dependencies available from Fedora repository.

        $ sudo dnf install -y postgresql postgresql-server postgresql-devel \
        automake libtool cfitsio-devel libnova-devel gcc gcc-c++ ncurses-devel \
        GraphicsMagick-c++-devel libX11-devel docbook-style-xsl libxslt \
        libxml2-devel libarchive-devel json-glib-devel libsoup-devel pkgconfig \
        wcstools-devel lmfit

3. Clone Git repository.

        $ cd ~/src
        $ git clone https://github.com/RTS2/rts2.git

4. Execute compilation.

        $ cd rts2
        $ ./autogen.sh
        $ ./configure
        $ make

5. Run tests to verify that code was compiled properly.

        $ make check

6. Install RTS2.

        $ sudo make install

7. Create and populate `/etc/rts2` directory.

        $ cd rts2
        $ sudo bash rts2-init

8. Now we need to edit `/etc/rts2/rts2.ini`.

        $ sudo nano /etc/rts2/rts2.ini

   Replace all `xxxx` strings by actual values.

9. Initialize and configure PostgreSQL database.  First we have to populate the
   database and start `postgresql` server:

        $ sudo postgresql-setup --initdb
        $ sudo systemctl start postgresql
        $ sudo systemctl enable postgresql

   Then you can create a database user and initialize the `stars` database for
   the user.  Note that you have to use the username *identical* with the one
   you normally use when login to the system.  The following sequence of
   commands starts at `/home/gszasz/src/rts2/` directory and `gszasz` is my
   username.

        [gszasz@iris rts2]$ sudo -i -u postgres
        -bash-4.3$ createuser --interactive gszasz
        Shall the new role be a superuser? (y/n) y
        -bash-4.3$ exit
        [gszasz@iris rts2]$ createdb stars
        [gszasz@iris rts2]$ cd src/sql
        [gszasz@iris sql]$ ./rts2-builddb stars
        Would you like to create Landolt calibration fields targets [Y|n]? y
        Would you like to create targets for planets [Y|n]? y
        [gszasz@iris sql]$ cd data
        [gszasz@iris sql]$ psql stars < dummy.sql

   The last two commands are necessary only if you are going to run RTS2 with
   dummy devices.  Now you can try to run following command. `1` is dark-frame
   target.

        $ rts2-targetinfo 1
        1 d         nan         nan +00:00 nan          nan         nan transiting Dark frames
           C0:'D 0'
            |-- expected light time: 0 # images 1
            \-- expected duration: 0
         Expected maximal script duration: 0

   If it do not see this output, something is wrong.

   Add telescope Please configure database with rts2-configdb tool for camera,
   telescope and filters.

   TODO

---

[Home](../README.md) > [Testing](README.md) > *RTS2 Setup on Fedora*
