# RTS2 Setup

1. Install dependencies available freom the Raspbian repository

RTS2 need following packages to be installed.  Although part of them are already
installed now, it is safer to reinstall all of them.

        $ sudo apt-get install --reinstall \
        postgresql postgresql-server-dev-9.4 libecpg-dev automake libtool \
        libcfitsio3-dev libnova-dev libecpg-dev gcc g++ libncurses5-dev \
        libgraphicsmagick++1-dev libx11-dev docbook-xsl xsltproc \
        libxml2-dev libarchive-dev libjson-glib-dev libsoup2.4-dev
        pkg-config libwcstools-dev


2. Install dependencies via pip

   The `lmfit` package seems to be too old in Raspbian Jessie.  We need to get
   it from pip.

        $ sudo pip install lmfit


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

[Home](../README.md) > [Installation](README.md) > *RTS2 Setup*
