# RTS2 Setup on Fedora

I found useful to test newest RTS2 sources using dummy devices on my laptop that
is running Fedora.  Since I did not found any documentation for compiling and
installing RTS2 on Fedora, I have decided to add this section.

1. Install dependencies available from Fedora repository

        $ sudo dnf install -y postgresql postgresql-server postgresql-devel \
        automake libtool cfitsio-devel libnova-devel gcc gcc-c++ ncurses-devel \
        GraphicsMagick-c++-devel libX11-devel docbook-style-xsl libxslt
        libxml2-devel libarchive-devel json-glib-devel libsoup-devel pkgconfig \
        wcstools-devel lmfit

3. Clone Git repository

        $ cd ~/src
        $ git clone https://github.com/RTS2/rts2.git


4. Execute compilation

        $ cd rts2
        $ ./autogen.sh
        $ ./configure
        $ make

5. Run tests to verify that code was compiled properly.

        $ make check

6. Install RTS2

        $ sudo make install

---

[Home](../README.md) > [Testing](README.md) > *RTS2 Setup on Fedora*
