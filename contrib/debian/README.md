
Debian
====================
This directory contains files used to package merged/point-qt
for Debian-based Linux systems. If you compile merged/point-qt yourself, there are some useful files here.

## MERGE: URI support ##


point-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install point-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your MERGEqt binary to `/usr/bin`
and the `../../share/pixmaps/MERGE128.png` to `/usr/share/pixmaps`

point-qt.protocol (KDE)

