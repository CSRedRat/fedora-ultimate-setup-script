# Fedora Xfce Setup Script

Using only the [official Fedora 30 Xfce ISO](https://spins.fedoraproject.org/ru/xfce/)
create your perfect Fedora experience and save it to a USB drive to preserve
forever!

Use this script to update the system, install all your favourite programs,
remove the ones you don't want, and setup your computer exactly the way you
like. Optionally save all the .rpm files that are downloaded for later offline
use. By doing this you can recreate the **exact same system without having
access to the internet.**

As default it has my custom tweaked setup that is biased towards front end web
development, but it also has things like setting up mpv for GPU acceleration,
Pulse Audio for high sound quality and some great Gnome desktop settings.

![screenshot](https://github.com/David-Else/fedora-ultimate-setup-script/blob/master/script-screenshot.png)

# How to Use

## Installation

Download this repository using git, CD into the directory, and run:

```
git clone https://github.com/CSRedRat/fedora-xfce-setup-script
cd fedora-xfce-setup-script
./fedora-ultimate-setup-script.sh
```

Now follow the on-screen instructions.

## Offline Mode (optional)

To use this script offline requires you have used it before previously online to
generate the .rpm files needed.

**IMPORTANT** the time to create these files is after a totally fresh install of
Fedora. This way all the updates follow on from the official ISO image, doing so
at a later point will not give the correct results.

First you run the script with option 2 which includes 'create offline install
files for future use'.

These files will have been stored by default in:

```
$HOME/offline-system-updates
$HOME/offline-user-packages
```

Copy these directories along with the exact same script that was run to create
them onto a USB stick. On the new computer install the original Fedora ISO, and
then insert the USB key and run the script choosing option 3.

When offline install mode is used any functionality that requires access to the
internet is skipped, including setting up repositories. If you want updates in
the future to any of the programs that were downloaded from non standard
repositories you will need to add the repos at a later date. This can be done by
re-running the script in offline mode with internet access, or manually.

# Customization

### Customize the packages you want to install or remove

The `create_package_list()` function contains all the packages you want to
install. It contains an associative array in the following format:

```
['category of package']='package-name-1 package-name-2'
```

The `'category of package'` string is only used for categorization and printing
the results to the console, the exact wording is not important. The
`'package-name'` string must be exactly the same as the name you type when using
`dnf install package-name`.

To remove packages just edit the `REMOVE_LIST` array.

```
REMOVE_LIST=(gnome-photos gnome-documents rhythmbox totem cheese)
```

### Customize the adding of repositories

In the `add_repositories()` function you will see
[RPM Fusion](https://rpmfusion.org/) and [Flathub](https://flathub.org/home) are
installed by default. After that certain packages trigger certain repositories
to be installed. You can add your own here.

### Customize the setting up of programs

Later in the script in the `main()` function certain packages trigger certain
actions. This functionality is contained in a case statement in the following
format (note the spaces around the package names):

```
    case " ${PACKAGES_TO_INSTALL[*]} " in
    *' code '*)
        # do something
        ;;&
    *' nodejs '*)
        # do something
        ;;&
    esac
```

This is where you can add custom commands or functions to setup the packages you
have chosen to install. There are also a lot of universal default settings in
`main()` you may want to edit.

### Setting up Visual Studio Code (optional)

Inside the `setup_vscode()` there is an array called `code_extensions`, here you
can add all your favourite extensions to be downloaded and installed.

To obtain the names of currently installed extensions to add to the list you can
use:

```
code --list-extensions
```

My entire user settings file is stored here, please copy and paste your own.

### FAQ

**Q**: Does this script disable the caps lock key? I've noticed that it works
during login but after that it stops working all together.

**A**: It makes the caps lock into a delete for touch typing purposes, the line
that does it is:

```
 gsettings set org.gnome.desktop.input-sources xkb-options "['caps:backspace', 'terminate:ctrl_alt_bksp']"
```

You can delete the `'caps:backspace',` to rectify the situation.
