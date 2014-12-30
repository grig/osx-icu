# Updating time zone information for OS X Lion

Time zones change all the time. However, Apple stops shipping time zone updates
for older OSes. This can get very inconvenient if your country has changed its
time zones and you're stuck on an old OS.

These steps will show you how to fix that.

You are going to need Apple Command Line Tools downloadable from
http://developer.apple.com/ (free registration required).

## Updating time zones

0. Open Terminal.app

1. Get ICU code from a proper branch. For OS X Lion that would be 'tzupdates-10.7':

        git clone -b tzupdates-10.7 https://github.com/grig/osx-icu

2. Download the latest `tzdata` package from http://www.iana.org/time-zones. At
   the time of writing, that is `tzdata2014j.tar.gz`:

        wget http://www.iana.org/time-zones/repository/releases/tzdata2014j.tar.gz

3. Change to icuSources directory within the repository

        cd osx-icu/icuSources

4. Copy the tzdata package to `tools/tzcode`:

        cp ../../tzdata2014j.tar.gz tools/tzcode

5. Configure and make the project:

        ./runConfigureICU MacOSX --with-data-packaging=archive
        gnumake

6. Back up old version of time zone data:

        sudo cp /usr/share/icu/icudt46l.dat{,.bak}

7. Install the new time zone data package:

        sudo install -o root -g wheel -m 0644 -Sp data/out/icudt46l.dat /usr/share/icu/icudt46l.dat

8. Re-boot your computer

# Credits

This work is based on HOWTO by `dfox_spb` published in http://geektimes.ru/post/131621/ (in Russian).

ICU is copyright (c) 1995-2011 International Business Machines Corporation and others.

tzdata database is in public domain.
