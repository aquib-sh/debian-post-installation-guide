## DEBIAN POST INSTALLATION GUIDE

Author: Shaikh Aquib


### Installation of some must have tools and software
Note: There is no sudo privilege for user, so we have to switch to superuser using
`su` command. It is recommended that you use `su` and to enter the super user to do super user work instead of using sudo before every command that requires elevated privilege.
```bash
apt install -y git vim htop neofetch curl zip python3-pip default-jdk gimp obs-studio kamoso
```
to install Git, VIM, htop, neofetch, curl, zip, pip, JDK, GIMP, OBS-Studio


### Add important binary paths to PATH environment variable
1) open `vim ~/.bashrc`
2) add append below line to the end of file 
```bash
export PATH=$PATH:/bin:/sbin
```
3) save the file and launch a new instance of terminal and do `su` to enter super user.


### Add user to sudo
enter super user using `su` command.
```bash
usermod -aG sudo <user>
```
where replace the <user> with the user you want to add to sudo
(example: `usermod -aG sudo aquib`),
restart your computer for changes to take effect


### Add .vimrc
- open ~/.vimrc file (it will create one) using `vim ~/.vimrc`
- copy paste the below contents into file<br/>

```
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
```


### Add non-free and (backports to the sources list or upgrade to Debian testing)
NOTE: You don`t need to add backports if you are upgrading to Debian testing/sid

1) Open sources list file using `vim /etc/apt/sources.list`
2) Add `contrib non-free` to the end of each source line to add non-free repos
3) Add a new line with `<debian_codename>-backports`
   example: `deb http://deb.debian.org/debian bullseye-backports main non-free`

   The final sources.list would similar as below except for the codename bullseye (Debian 11)
   Note: ignore the last two lines if switching to Debian testing
```
deb http://deb.debian.org/debian/ bullseye main contrib non-free
deb-src http://deb.debian.org/debian/ bullseye main contrib non-free

deb http://security.debian.org/debian-security bullseye-security main contrib non-free
deb-src http://security.debian.org/debian-security bullseye-security main contrib non-free

deb http://deb.debian.org/debian/ bullseye-updates main contrib non-free
deb-src http://deb.debian.org/debian/ bullseye-updates main contrib non-free

deb http://deb.debian.org/debian bullseye-backports main non-free
deb-src http://deb.debian.org/debian/ bullseye-backports main non-free
```
4) run `apt update` to fetch the updates from sources.


### Switch to Debian testing/sid 
1) Replace the code_name (example: bullseye) with testing in /etc/apt/sources.list
   Your /etc/apt/sources.list should look like below:
```
deb http://deb.debian.org/debian/ testing main contrib non-free
deb-src http://deb.debian.org/debian/ testing main contrib non-free

deb http://security.debian.org/debian-security testing-security main contrib non-free
deb-src http://security.debian.org/debian-security testing-security main contrib non-free

deb http://deb.debian.org/debian/ testing-updates main contrib non-free
deb-src http://deb.debian.org/debian/ testing-updates main contrib non-free
```
Now,
- update the repo
- upgrade the system
- upgrade to the testing distribution </br>

```bash
apt update && apt upgrade && apt dist-upgrade && reboot
```



### Installing non-free firmwares and drivers
1) add nomodeset to grub to avoid conflicts.
   goto grub file using
   `vim /etc/default/grub`
 
   Change<br>`GRUB_CMDLINE_LINUX_DEFAULT="quiet"`
   <br>to</br>
   `GRUB_CMDLINE_LINUX_DEFAULT="nomodeset"`

   Now, save the file and quit the editor.
   run `update-grub`

Now, To
- Install linux kernel headers
- Install nvidia driver
- Install realtek firmware for wifi, bluetooth, and other non free firmwares
- Install printer drivers
- Reboot after installation is complete

```bash
apt install -y linux-headers-amd64 firmware-realtek firmware-misc-nonfree printer-driver-all cups nvidia-driver && reboot
```

### Changing resolution
Resolution can be changed through display settings in GNOME
but after setting `nomodeset` the gdm login screen is of lower resolution
to solve this we will copy the monitors.xml file to gdm configuration. <br/>

```bash
cp /home/<user>/.config/monitors.xml /var/lib/gdm3/.config/
```
<br>replace the `<user>` with your username.


### Force GNOME to alt + tab only in current workspace
```bash
gsettings set org.gnome.shell.app-switcher current-workspace-only true
```


### Pinkish display problem after upgrading to Debian Testing with Nvidia
In some rare cases you would observed the monitor color to be pinkish after upgrading to testing
the solution to this is 
- open color settings from gnome-settings
- click on the the device to add profile
- select `Colorspace: sRGB` and set it as default
- After the color profile is added the colors should be normal, the profile would appear as 
`Standard Space - sRGB`


### Install some necessary and cool fonts
```bash
apt install -y fonts-indic fonts-cantarell fonts-comfortaa fonts-firacode fonts-recommended
```

### Install some useful Python3 libraries
```bash
pip3 install pandas jupyter requests bs4 selenium lxml openpyxl
```

### Important references:
1) How to add fonts manually?
- Copy the font files to /usr/share/fonts</br>
- run `fc-cache -f -v` to update fonts</br>
- run `fc-list` to verify your installed font
