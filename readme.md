# Ubuntu Setup Memo

## Environment
* Mac Book Pro 2015
	* US Keyboard
* Dual Boot using rEFInd
	* Mac OS X (El Capitan)
	* Ubuntu 15.10

## Install
* refer to lots of document in the internet
* should not connect the internet during install
* should not install 3rd party applications when install
* known problem
	* two fingers click doesn't work...
	* right click is assigned as:
		* two fingers tap
		* three fingers click
		* two fingers click on the right bottom area

## First of all
* Update packages
```
%> sudo apt-get update
%> sudo apt-get upgrade
```
* Adjust display
	* System Settings -> Screen Display -> Scale for menu and title bars = 1.5
* Disable login sound
	* Set volume mute in login screen



## Keyboard
### Enable tilde and enforce fn key for special keys
* subl /etc/rc.local
```
echo 2 > /sys/module/hid_apple/parameters/fnmode
echo 0 > /sys/module/hid_apple/parameters/iso_layout
```

### Setup hotkeys
* Target:
	* Set "Command" key as Control
	* Set "Control" key as Super
	* Set "Caps Lock" key as Meta for emacs like shortcut (e.g. C-k)
* Swap keys
	* It's okay to use your own settings and reflect it using "xkbcomp" command at every boot, but I noticed that default settings are loaded when I switch text entry method. So, I modify the default settings here.
	* ~$ subl /usr/share/X11/xkb/symbols/pc

```
///!modified
///key <CAPS> {	[ Caps_Lock		]	};
key <CAPS> {[ Meta_L ] };

key <NMLK> {	[ Num_Lock 		]	};

key <LFSH> {	[ Shift_L		]	};
///!modified
///key <LCTL> {	[ Control_L		]	};
///key <LWIN> {	[ Super_L		]	};

///!modified
key <META> {	[ NoSymbol, Meta_L	]	};
//modifier_map Mod1   { <META> };
modifier_map Mod2   { <META> };
```

* ~$ subl /usr/share/X11/xkb/symbols/altwin
```
partial modifier_keys 
xkb_symbols "meta_alt" {
    ///!modified
    //key <LALT> { [ Alt_L, Meta_L ] };    -->
    key <LALT> { [ Alt_L ] };
    key <RALT> { type[Group1] = "TWO_LEVEL",
                 symbols[Group1] = [ Alt_R, Meta_R ] };
    ///!modified
    //modifier_map Mod1 { Alt_L, Alt_R, Meta_L, Meta_R }; -->
    modifier_map Mod1 { Alt_L };
//  modifier_map Mod4 {};
};
```

* set hot keys using Autokey
	* install Autokey using software center
	* set hotkeys like the following:
		* keyboard.send_key("<left>")
	* register into gnome-session-properties
	* sample scripts
		* C-b (back). assign the following script to hotkey "meta+b"
```
# Enter script code
keyboard.send_key("<left>")
```
		* C-k (line delete). assign the following script to hotkey "meta+k"
```
# Enter script code
keyboard.send_keys("<shift>+<end>")
keyboard.send_key("<delete>")
```

## Network
### Connect wifi at office/school
* Security: WPA & WPA2 Enterprise
* Authentication: Protected EAS (PEAP)
* Anonymous identity: blank
* CA certificate: None
* check to No CA certificate is required
* PEAP version: Automatic
* Inner authentication: MSCHAPv2
* Username: yours
* Password: yours

## Appearance

### Background color
3476C2

### Terminal
* Hide user name and Host name in terminal prompt
* subl .bashrc
```
if [ "$color_prompt" = yes ]; then
    #PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    #PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
    PS1='${debian_chroot:+($debian_chroot)}\w\$ '
fi
```


## Desktop and Files (nautilus)
### workspaces
* System settings -> Appearance -> Behaviors -> enable workspaces

### Shortcut
* subl .config/nautilus/accels &
```
(gtk_accel_path "<Actions>/ShellActions/Back" "<Primary>bracketleft")
(gtk_accel_path "<Actions>/ShellActions/Forward" "<Primary>bracketright")
(gtk_accel_path "<Actions>/ShellActions/Up" "BackSpace")
```

### List View
* Files-> Edit -> Preferences -> Views. List

### Hide unnecessary bookmarks
* subl .config/user-dirs.dirs &
```
# XDG_DESKTOP_DIR="$HOME/Desktop"
# XDG_DOWNLOAD_DIR="$HOME/Downloads"
# XDG_TEMPLATES_DIR="$HOME/Templates"
# XDG_PUBLICSHARE_DIR="$HOME/Public"
# XDG_DOCUMENTS_DIR="$HOME/Documents"
# XDG_MUSIC_DIR="$HOME/Music"
# XDG_PICTURES_DIR="$HOME/Pictures"
# XDG_VIDEOS_DIR="$HOME/Videos"
XDG_DESKTOP_DIR="$HOME/Desktop"
XDG_DOWNLOAD_DIR="$HOME/Downloads"
XDG_TEMPLATES_DIR="$HOME"
XDG_PUBLICSHARE_DIR="$HOME"
XDG_DOCUMENTS_DIR="$HOME"
XDG_MUSIC_DIR="$HOME"
XDG_PICTURES_DIR="$HOME"
XDG_VIDEOS_DIR="$HOME"
```

### Add applications (sublime text) to "open with" applications
* subl /usr/share/applications$ subl sublime_text.desktop &
	* modify from
```
Exec=/opt/sublime_text/sublime_text %F
```
to
```
Exec=/opt/sublime_text/sublime_text %U
```

* subl .local/share/applications/sublime_text.desktop &
```
[Desktop Entry]
Encoding=UTF-8
Version=1.0
Type=Application
Name=Sublime Text
Icon=sublime_text.png
Exec=/opt/sublime_text/sublime_text %F
StartupNotify=false
StartupWMClass=Sublime_text
OnlyShowIn=Unity;
X-UnityGenerated=true
```

## Others

### Japanese
* System settings->Language Support->Install Japanese
* Logout
* Text Entry setting -> Add (Japanese(Anthy))

### Enable exFat
```
%> sudo apt-get install exfat-utils exfat-fuse
```


## tools
### Sublime text
* https://packagecontrol.io/installation
* Ctrl+Shift+p -> package controll:install package
	* Flatland
	* brackethighlighter
	* converttoutf8
	* simpleclone
	* sublimecodeintel
	* terminal
	* trailingspaces
	* xaml

### chrome

### acrobat

### oracle jdk
```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
sudo add-apt-repository -r ppa:webupd8team/java
sudo apt-get update
```

### Android Studio
* android studio http://developer.android.com/sdk/installing/index.html?pkg=studio
* extract into ~/bin/android-studio
* ~/bin/android-studio/bin/inspect.sh

### Command line tools
```
%> sudo apt-get install git gitk
%> ssh-keygen
%> subl .ssh/id_rsa.pub &
%> git config --global user.name "take"
%> git config --global user.email take@gmail.com
```

### astah

## Meld
* from software center
* add this into .gitconfig
```
[diff]
	tool = meld
```
* %> git difftool -d

## php
```
%> sudo apt-add-repository ppa:ondrej/php5-5.6
%> sudo apt-get update
%> sudo apt-get install php5
```

## Pythoon
* should not change the default python
	* use "python3" command to run python3
* install pip
	* sudo apt-get -y install python-pip

## VirtualBox
* To access USB
	* install virtualbox extension pack
	* enable USB controller (should use USB3 on VirtualBox 5)
	* sudo adduser USERNAME vboxusers
	*reboot

## backup
```
sudo add-apt-repository ppa:nemh/systemback
sudo apt-get update
sudo apt-get install systemback
```
start from launcher  
need external drive (ext4)

## UART Terminal
GtkTerm

## VNC Viewer
* https://www.realvnc.com/download/vnc/
* Debian-compatible installer contains both server and client

# Cheat command

## recovery from freeze
* ctrl+alt+f1
```
sudo service lightdm restart
sudo reboot
sudo shutdown -h now
```
