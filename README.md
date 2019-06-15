# runescape-3-ubuntu-install

## How to get the Runescape 3 installer to work in Ubuntu 16.04 / 18.04

If you use the Runescape 3 Linux installer today (March/June of 2019) you will notice that it will both not install due to lacking dependencies, but also will crash due to lacking dependencies after you manage to get it installed.

This guide makes it simple to get it up and running correctly on your Ubuntu machines. I got tired after having set the client up on both my laptop and desktop to remember all the steps, so that's why this guide exists.

You may have to prepend your queries with `sudo` in some cases to get them to run. This is to gain superuser permissions during installation. I recommend against becoming superuser via `su`, mainly because it is easy to forget to go back to your normal user, but that is up to you.

## Prerequisites:
- Ubuntu 16.04/18.04 (other versions may work as well)
- A terminal open
- Internet connection

## Installation commands from Runescape

1. Grab the install commands from Runescape:
[Click on the linux button](https://www.runescape.com/download)

## Extra dependencies for installation

2. To get the launcher to install:
- Install Libglew1.10-3 via `wget http://launchpadlibrarian.net/161405671/libglew1.10_1.10.0-3_amd64.deb && dpkg -i libglew1.10_1.10.0-3_amd64.deb`
That fetches the libglew1.10-3 debian package, and then dpkg installs it.

## To get the game not to crash

3. To get the game to run and not crash:
- You are going to have to install both libpng12-0 and libcurl3. Libpng12-0 will likely not be in your software sources list, so we will add that source.

Open you software sources file via your terminal by typing `sudo nano /etc/apt/sources.list`, then add the line `deb http://security.ubuntu.com/ubuntu xenial-security main` in that file. Hit `ctrl-X` and `y` to save. You can replace nano with vim, emacs, code etc if you prefer another text editor.

Run `sudo apt-get update` to update your software sources and make it possible to find libpng.
Then run `sudo apt install libpng12-0 libcurl3 libcurl-openssl1.0-dev`.

Now you should be able to launch the game. However! If you launch via the terminal, you will notice an error saying that the libcanberra-gtk-module is missing.

## Fixing missing runtime dependencies

To install the missing module, run `sudo apt install libcanberra-gtk-module libcanberra-gtk3-module`.

## Fixing the glitchy / static sound

One final thing you will most likely notice is that the sound is messed up. We can however fix this as well.
Open your runescape launcher file by typing `sudo nano /usr/share/applications/runescape-launcher.desktop`. Now this is where your launcher is most-likely located. You may have to go to your file explorerer / terminal and search for its actual location if it does not exist there. A quick google search could also help if things really change in the future, others will probably have documented it.

Now we are going to add an environmental variable to the `Exec` line. I have seen a few different versions of the Exec line online, but this should work on 90% of systems.
- Right after `Exec=` insert `env PULSE_LATENCY_MSEC=100` with a space between 100 and the path to your launcher. Then add `%u %F` to the end of the line if not already there.

Example of how it can look (don't worry if your path is a bit different or if you have more options):
`Exec=env PULSE_LATENCY_MSEC=100 /usr/bin/runescape-launcher %u %F`

Save that with `Ctrl-X` and `y`.

## All done

Now you are good to go, all dependencies met and your sound should be fixed. Just click on the Runescape launcher.


### If things change, feel free to open a PR and I'll get it merged.

