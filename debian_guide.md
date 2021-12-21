## DEBIAN POST INSTALLATION GUIDE

Author: Shaikh Aquib


### Installation of some must have packages
1) There is no sudo privilege for user so enter super user using
`su` command. It is recommended that you use `su` and enter the super user to do super user work instead of using sudo before every command that requires priviledge.
2) `apt install git vim vim-gtk curl zip python3-pip default-jdk` <br/>to install git, VIM, curl, zip, pip, JDK


### Add .vimrc
1) open ~/.vimrc file (it will create one) using `vim ~/.vimrc`
2) copy paste the below contents into file<br/>

<pre>
filetype plugin indent on
syntax on

" enable copy pasting from mouse using shift
set mouse=a

" show existing tab with 4 spaces width
set tabstop=4

" when indenting with '>', use 4 spaces width
set shiftwidth=4

" On pressing tab, insert 4 spaces
set expandtab

" show line numbers
set number

" Highlight search results
set hlsearch

" Show matching brackets when text indicator is over them
set showmatch

" Turn backup off, since most stuff is in SVN, git etc. anyway...
set nobackup
set nowb
set noswapfile

set ai "Auto indent
set si "Smart indent
</pre>

### Add user to sudo
1) enter super user using `su` command.
2) `usermod -aG sudo <user)` <br/> where replace the <user> with the user you want to add to sudo
   (example: `usermod -aG sudo aquib`)
3) restart your computer for changes to take effect


### Add important binary paths to PATH environment variable
1) open `vim ~/.bashrc`
2) add this line to the end of file `export PATH=$PATH:/bin:/sbin`
3) save the file and launch a new instance of terminal and do `su` to enter super user.


### Add non-free and (backports to the sources list or upgrade to Debian testing)
NOTE: You don`t need to add backports if you are upgrading to Debian testing/sid

1) Open sources list file using `vim /etc/apt/sources.list`
2) Add `contrib non-free` to the end of each source line to add non-free repos
3) Add a new line with `<debian_codename>-backports`
   example: `deb http://deb.debian.org/debian bullseye-backports main non-free`

   The final sources.list would similar as below except for the codename bullseye (Debian 11)
   Note: ignore the last two lines if switching to Debian testing
<pre>
deb http://deb.debian.org/debian/ bullseye main contrib non-free
deb-src http://deb.debian.org/debian/ bullseye main contrib non-free

deb http://security.debian.org/debian-security bullseye-security main contrib non-free
deb-src http://security.debian.org/debian-security bullseye-security main contrib non-free

deb http://deb.debian.org/debian/ bullseye-updates main contrib non-free
deb-src http://deb.debian.org/debian/ bullseye-updates main contrib non-free

deb http://deb.debian.org/debian bullseye-backports main non-free
deb-src http://deb.debian.org/debian/ bullseye-backports main non-free
</pre>
4) run `apt update` to fetch the updates from sources.


### Switch to Debian testing/sid 
1) Replace the code_name (example: bullseye) with testing in /etc/apt/sources.list
   Your /etc/apt/sources.list should look like below:
<pre>
deb http://deb.debian.org/debian/ testing main contrib non-free
deb-src http://deb.debian.org/debian/ testing main contrib non-free

deb http://security.debian.org/debian-security testing-security main contrib non-free
deb-src http://security.debian.org/debian-security testing-security main contrib non-free

deb http://deb.debian.org/debian/ testing-updates main contrib non-free
deb-src http://deb.debian.org/debian/ testing-updates main contrib non-free
</pre>
2) update the repo and system using `apt update && apt upgrade`

3) run `apt dist-upgrade` to upgrade your Debian to testing release.


### Installing Nvida and other drivers and firmwares
1) add nomodeset to grub to avoid conflicts.
   goto grub file using
   `vim /etc/default/grub`
 
   Change<br>`GRUB_CMDLINE_LINUX_DEFAULT="quiet"`
   <br>to</br>
   `GRUB_CMDLINE_LINUX_DEFAULT="nomodeset"`

   Now, save the file and quit the editor.
   run `update-grub`

2) run `apt install linux-headers-amd64` to install linux kernel headers
3) run `apt install nvidia-driver firmware-misc-nonfree` to install nvidia driver
4) run `apt install firmware-realtek` to install realtek firmware for wifi, bluetooth, etc
5) run `apt install printer-driver-all cups` to install printer drivers
6) restart your system using `shutdown -r now`


### Changing resolution
Resolution can be changed through display settings in GNOME
but after setting `nomodeset` the gdm login screen is of lower resolution
to solve this we will copy the monitors.xml file to gdm configuration. <br/>
`cp /home/<user>/.config/monitors.xml /var/lib/gdm3/.config/`
<br>replace the <user> with your username.


### Force GNOME to alt + tab only in current workspace
`gsettings set org.gnome.shell.app-switcher current-workspace-only true'


## Pinkish display problem after upgrading to Debian Testing with Nvidia
In some rare cases you would observed the monitor color to be pinkish after upgrading to testing
the solution to this is 
- open color settings from gnome-settings
- click on the the device to add profile
- select `Colorspace: sRGB` and set it as default
- After the color profile is added the colors should be normal, the profile would appear as 
`Standard Space - sRGB`


## Install some necessary and cool fonts
1) run `apt install fonts-indic fonts-cantarell fonts-comfortaa fonts-recommended`

## Install some needed Python3 libraries
1) run `pip3 install requests bs4 selenium lxml openpyxl`


## Important references:
1) How to add fonts manually?
- Copy the font files to /usr/share/fonts</br>
- run `fc-cache -f -v` to update fonts</br>
- run `fc-list` to verify your installed font
