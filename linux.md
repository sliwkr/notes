# Linux snippet cookbook

## Heads up

Hello there, thanks for stopping by.
You may get a better mileage by using https://www.mankier.com/ or https://tldr.sh/

## table of contents

- [New user](#New-user)
- [/etc/passwd file](#/etc/passwd-file)
- [change hostname](#change-hostname)
- [ip address](#ip)
- [reboot](#reboot)
- [raspberry cpu & gpu temperature](#raspberry-pi-temperature)
- [create swap file](#create-swap-file)
- [ssh config](#ssh)
- [find](#find)
- [curl](#curl)
- [grep](#grep)
- [tree](#tree)
- [gpg](#gpg)
- [cups](#cups)
- [logging](#log)
- [pdb](#python-pdb)
- [format disk](#disk)
- [package managers](#package-managers)
- [wake-on-lan](#wake-on-lan)
- [bc (calculator)](#bc)
- [qemu](#qemu)
- [NetworkManager](#networkmanager)
- [bluetooth](#bluetooth)
- [ImageMagick](#imagemagick)
- [Working with pdf's](#pdf)
- [ansible](#ansible)
- [vim](#vim)
- [colorized less output when piping](#colorized-less-output-through-pipe)
- [associating application with a given extension through xdg mimetypes](#xdg)
- [tar](#tar)
- [gzip](#gzip)
- [X11](#X11)
- [rsync](#rsync)
- [7zip](#7zip)
- [irssi](#irssi)
- [Linux-wifi](#linux-wifi-management-interfaces)
- [Device details](#device-details)
- [Battery](#battery)
- [rclone, s3fs](#rclone)
- [alpine - build broadcom wifi driver](alpine-build-broadcom-wifi-driver)
- [monitoring](#monitoring)
- [wget](#wget)
- [mpv](#mpv)
- [git](#git)
- [icloudpd](#icloudpd)
- [full system info](#inxi)
- [tmux](#tmux)
- [screen](#screen)
- [reset terminal settings](#reset-terminal-settings)
- [bash](#bash)
- [jq](#jq)
- [expect](#expect)
- [systemd](#systemd)
- [clipboard](#clipboard)
- [fonts](#fonts)
- [sed](#sed)
- [cryptsetup](#cryptsetup)
- [firewalld](#firewalld)
- [iso creation](#iso-cd)
- [dpkg](#dpkg)
- [dig](#dig)
- [creating files](#files)
- [kernel parameters](#kernel)
- [nmap](#nmap)
- [lvm](#lvm)
- [network](#network)
- [x11 display and resolution](#x11screen)
- [date and time](#dates)
- [xargs](#xargs)

## snippets

### new user

```sh
adduser tom --disabled-password  # passwordless user, able to login with ssh key
getent group  # list groups created so far
useradd -G sudo tom  # add user named tom and add tom to sudoers. exit 9 if the user exists
gpasswd -a tom secret  # add tom to secret group; also usermod -a -G secret tom
gpasswd -d tom secret  # remove tom from secret group
newgrp secret  # log into a new group, removes the need to relogin after modifying groups
passwd -e tom  # choose password for tom; -e for expire
cat /etc/passwd  # tom should be here now
```

### /etc/passwd file

```sh
|username |password |UID |GID  |user info |home dir |shell    |
|root     :x        :0   :0    :root      :/root    :/bin/bash
```

#### disable account (e.g. disable root account)

```sh
diff
< root:x:0:0:root:/root:/bin/bash
---
> root:x:0:0:root:/root:/sbin/nologin
```

### change hostname

```sh
hostnamectl set-hostname maszyna-zaglady
sudo echo 'maszyna-zaglady' > /etc/hostname
```

### ip

```sh
ip -br a
hostname -I
```

### reboot

```sh
systemctl reboot
shutdown -r
init 6
```

### raspberry pi temperature

```sh
#!/bin/bash
# Script: my-pi-temp.sh
# Purpose: Display the ARM CPU and GPU  temperature of Raspberry Pi 2/3
# Author: Vivek Gite <www.cyberciti.biz> under GPL v2.x+
# -------------------------------------------------------
cpu=$(</sys/class/thermal/thermal_zone0/temp)
echo "$(date) @ $(hostname)"
echo "-------------------------------------------"
echo "GPU => $(vcgencmd measure_temp | egrep -o '[0-9]*\.[0-9]*')"
echo "CPU => $((cpu/1000))'C"
```

### create swap file

```sh
# as root:
dd if=/dev/zero of=/swapfile count=4096 bs=1MiB  # create the file
chmod 600 /swapfile  # allow only root access
mkswap /swapfile  # set up swap space
swapon /swapfile  # start using swapfile as a new swap space
swapon -s  # display swap spaces summary
free -h  # display memory info
```

- To make it permanent, append to `/etc/fstab`:

```sh
/swapfile   swap    swap    sw  0   0
```

### ssh

- `/etc/ssh/sshd_config`:
  - `PasswordAuthentication no`
  - `PermitEmptyPasswords no`
  - `PermitRootLogin no`
  - add `Protocol 2` line
  - `AllowUsers username-that-is-used-when-sshing other-username`

- see active connections with `ss -n -o state established '( dport = :22 or sport = :22 )'`

- `~/.ssh/config` entry syntax:

```sh
Host maszyna-zaglady
  HostName 192.168.1.86
  User minecraft
```

- add ssh key & send it to other host so it won't ask for password next time

```sh
ssh-keygen  # create new key, only if needed
ssh-copy-id -i ~/.ssh/id_rsa.pub user@host
```

#### jumpbox config

```sh
Host jumpbox
    Hostname 12.34.56.78
    User username
    IdentityFile ~/.ssh/key.pem

Host priv
    Hostname 172.17.0.91
    User username
    IdentityFile ~/.ssh/key.pem
    ProxyJump jumpbox
```

#### escape stuck connection

* `<enter>~.`
* `~?` in a live terminal for more

### find

```sh
find folder  # all files in . and subdirectories
find folder -type f  # files only (no directories)
find folder -type d  # directories only (no files)
find -iname 'name.txt'  # files with case-insensitive name.txt
find -iname 'name*'  # files which name starts with 'name'
find -iname '*.txt' -or -iname '*.png'  # txt or png files
find -iname 'sebastian' -and -type d  # directories named 'sebastian'
find -iname '*.js' -delete  # delete all files that end with .js
find -iname '*.py' | xargs wc -l  # starts wc -l ./foo.py; wc -l ./bar.py; etc
find -iname *.mp4 -printf '%s %p \n'  # find all .mp4 in the current dir, print a list of sizes in bytes and filenames
# find all .txt files conaining 'hello', and read them; https://unix.stackexchange.com/a/389706
find . -type f -name '*.txt'      \
   -exec grep -q 'hello' {} ';'   \
   -exec cat {} ';'
# all files excluding 'foo' and '.git' directory
find . -type f -not -path '*/foo/*' -not -path '*/.git/*'
find . -type d -name tests -exec rm -r {} +  # find tests directory underneath current, delete the folder
```

### curl

```sh
-H "x-api-key: 1234" -H "x-api-user: admin"  # each header given separately
-X POST -H "Content-Type: Application/Json" -d '{"key": "value"}'  # json post
```

#### download, follow redirects

```sh
# -L - follow 3xx redirects
# -o - output file
# --insecure - skip TLS verification
curl -Lo file.yml https://example.org/file.yml
curl -LO https://example.org/file.yml # when no need to rename
```

#### use ssh bastion host as a proxy (socks5)

```sh
curl --proxy socks5h://localhost:8081 https://rabbit.mq.us-east-1.amazonaws.com/
```

#### get request headers

```sh
curl -v -s hostname.com  # be verbose. request & response headers
curl -i hostname.com  # response headers only
```

### grep

```sh
grep -E 'pattern1|pattern2' log_file  # search multiple patterns, words
docker-compose logs | grep -E 'ERROR|INFO'  # output can be piped into
grep -e pattern1 -e pattern2 log_file  # patterns can be passed separately without quotes
grep -E 'pattern' -C 3  # print 3 lines around the found pattern, use -A for above and -B for below
grep -r 'pattern' dir  # search for a pattern through all files in a given directory
ls -a | xargs grep 'pattern' {} \; 2>/dev/null  # limit the search to the current dir
```

### tree

```sh
tree -a -I '.git|*~'  # list all files excluding .git folder and files ending with ~
```

### gpg

* Don't, https://www.latacora.com/blog/2019/07/16/the-pgp-problem/

#### glossary

key name - refers to the key ID visible on `--list-keys`

#### encrypt a file with a passphrase

```sh
gpg --batch -c --cipher-algo AES256 --passphrase qwe123 my_file.txt
```

#### create a new key

The first step is to have a key to sign files with. It's a good idea to have multiple
revocation certificates depending on the reason of revocation.  Make sure that the certificate is `chmod 600`.

```sh
gpg --gen-key  #  pass in your name & email and use the defaults
gpg --full-gen-key  # more options: RSA length, expiry date, key comment
```

#### read created and imported keys

```sh
gpg --list-keys  # list fingerprints of imported keys
```

#### sign a file

```sh
gpg --encrypt --armor -R recpient@email.com the_file  # encrypt a file with anonymous recipient
```


```sh
gpg --output decrypted_file --decrypt encrypted_file.asc  # read encrypted file, write output to decrypted_file
gpg --output revocation.crt --gen-revoke mail@domain.com
gpg --keyserver pgp.mit.edu --search-keys mail@domain.com  # search popular keyserver for keys
gpg --import name_of_public_key_file  # import public key into local keychain
gpg --fingerprint mail@domain.com  # brief info about imported key
gpg --sign-key mail@domain.com  # sign/trust imported key, number of signs can be seen when importing
gpg --output my.key --armor --export mail@domain.com
gpg --send-keys --keyserver pgp.mit.edu --fingerprint mail@domain.com  # send public key to a keyserver
gpg --edit-key mail@domain.com  # open menu for editing the key
gpg --export > public-keys.pgp  # export public keys
gpg --export-secret-keys > private-keys.pgp  # export private keys
gpg --import < public-keys.pgp  # import key from file
--armor  # ASCII output
-r <recipent> # known recipient
-R  # anonymous recipient
--sign  # known author
--local-user <ID>  # choose author
```

### cups

- For HP LaserJet 1020, just use hplip-gui or hp-setup, honestly, avoid the pain.

```sh
sudo apt install printer-driver-foo2zjs cups  # driver & interface for laserjet 1020
system-config-printer  # grahical interface for cups
lpstat -t  # show cups status information, play with the arguments to see other info
cancel $(lpstat | cut -f 1 -d ' ') # cancel currently pending print jobs
lpadmin
lpinfo -m  # extensive list of all printers possible to handle
cupsctl  # configure cups daemon options
http://localhost:631/help/options.html  # info about printing from the console
https://wiki.archlinux.org/index.php/CUPS/Troubleshooting  # good troubleshooting ref
sudo usermod -a -G lp $USER && newgrp lp  # add user to lp group and login to it
```

#### command-line printing

```sh
lpstat -t  # get device name of the printer, e.g. HP_LaserJet_1020
sudo lpadmin -d HP_LaserJet_1020  # set a given printer as default
lp somefile.pdf  # print the file
```

#### allow access to cups interface

- open /etc/cups/cupsd.conf
- change Listen localhost:631 to Port 631
- scroll down to sections <Location> for /, /admin/ and /admin/conf
- extend access for these sections like so:

```html
<Location />
    Order allow,deny
    Allow all
</Location>
```

- restart cups service


### log

```sh
journalctl -f  # logs from running daemons afaiu, /var/log/daemon.log
journalctl -ep {emerg, alert, crit, err, warning, notice, info, debug}  # get all errors of particular category
journalctl -xefb 0 -p err  # x explanations, e jump to end pager, f follow, b 0 last boot, -p err errors only
tail -f /var/log/{messages, syslog}  # verbose
tail -f /var/log/kern.log  # kernel logs
grep "Failed password" /var/log/auth.log  # login attempts, commands requiring sudo
```

### pdb

```sh
s - step into function
n - next line
r - return from function
c - continue execution
l - show code context
a - args for the current function

ipdb - debugger with auto completion
```

### disk

#### format

```sh

fdisk /dev/sdX
  p - list existing partitions
  i - info about partitions
  n - new partition
  t - change partition type
    7 - HPFS/NTFS/exFAT - exFAT, FAT32, NTFS

  w - save changes

mkfs.exfat -n Devicelabel /dev/sdXY
mkfs.vfat /dev/sdXY
mkfs.ext4 /dev/sdXY
mkfs.ntfs /dev/sdXY
mkfs.ntfs -f /dev/sdXY # skip writing 0's at the start

e2label /dev/sdXY device-label

tune2fs -m 1 /dev/sda1  # allocate 2% of storage for root-reserved blocks on ext4 filesystem
```

#### mount permanently (fstab)

* `blkid` to know the disk UUID an filesystem type
* edit /etc/fstab afterwards, e.g. like so:

```fstab
# <file system>           <mount point>  <type>  <options>    <dump>  <pass>
UUID=123-123-123-123-123  /mnt/storage   ext4    user,noauto  0       0
```

### package managers

```sh
apt install --only-upgrade <packagename>  # upgrade single package
apt-get clean  # clear /var/cache/apt/archives folder
apt-cache depends <packagename>  # list dependencies
apt-cache rdepends <packagname>  # list reverse dependencies
```

### wake-on-lan

The feature has to be enabled in BIOS/UEFI first.

#### host setup - NetworkManager

https://networkmanager.dev/docs/api/1.32.8/settings-802-3-ethernet.html

```sh
# on host:
nmcli connection show <name>  # get wired connection properties

# '802-11-wireless.wake-on-wlan' or '802-3-ethernet.wake-on-lan'
nmcli connection modify <name> 802-11-wireless.wake-on-wlan 64  # or just 'magic'
# important, shutting down or rebooting will put the card in whatever mode it was before and you'll need to power up the host manually (once)
service NetworkManager restart
```

#### host setup - ethtool

```sh
# on host
ethtool -s <device-name> wol g
# you'll need a way to preserve the setting on debian-based distros.
# Probably a script with the command above in /etc/rc.0 or /etc/rc.6 will do, but haven't checked
```

#### client setup - wakeonlan

```sh
wakeonlan <mac-address>
```

### bc

```sh
scale=2  # set calculation precision to 2 digits
```

### qemu

#### create a virtual machine

```sh
qemu-img create -f qcow2 test-img.qcow2 2G  # create image file
qemu-system-x86_64 -nographic -enable-kvm -cdrom ISO_IMAGE -hda test-img.qcow2 -m 2G # install the OS
qemu-system-x86_64 -cdrom ISO_IMAGE -cpu host -enable-kvm -m RAM_SIZE -smp NUMBER_OF_CORES -drive file=test-img.qcow2,format=qcow2  # fancy
```

#### networking

10.0.2.2 - gateway, host ip from guest perspective

##### ssh from host to guest

```sh
# guest accessible from host on localhost:5555, external network accessible from guest
qemu-system-x86_64 \
  -device e1000,netdev=net0 \
  -netdev user,id=net0,hostfwd=tcp::5555-:22 \
  -nographic -enable-kvm -hda test-alpine.qcow2 -m 2G
```

##### list network interfaces

```sh
iwconfig
```

#### keyboard shortcuts

`Ctrl` + `Alt` + `G` - release grab/focus
`Ctrl` + `Alt` + `+/-` - zoom/unzoom

### networkmanager

#### power off the wifi card (rfkill)

```sh
nmcli radio wifi off
```

#### show wifi password for the current network

```sh
nmcli d wifi show-password
```

#### show wifi password for a known network

```sh
nmcli c show <SSID> --show-secrets \
  | grep wireless-security.psk: \
  | tr -d ' ' \
  | cut -d ':' -f 2
```

### bluetooth

```sh
/var/lib/bluetooth/<listening-device-id>/<paired-device-id>
```

### imagemagick

policy.xml location: `identify -list configure | grep CONFIGURE_PATH` (usually /etc/ImageMagick-6)

#### see metadata (geometry, resolution, date taken if present, geolocation if present, etc.)

```sh
identify -verbose output.png
```

#### take a screenshot of a window

```sh
xwininfo  # determine window id on X11
import -window <window id> "screenshot.png"
```

#### strip metadata

```sh
convert -strip input.png output.png
```

#### join multiple images horizontally

```sh
montage input-1.png input-2.png -tile 1x2 -geometry +0+0 output.png
```

#### convert pdf to png

```sh
convert -density 300 input.pdf -resize 25% output.png
```

#### convert png to jpg

```sh
convert picture.png picture.jpg
```

#### rotate image

```sh
convert -rotate {90,180,270} input.jpg output.jpg
```

#### resize - make smaller

```sh
convert input.jpg -resize 640x480 output.jpg  # resize to a certain size
convert input.jpg -resize 640 output.jpg  # resize and preserve the aspect ratio
convert input.jpg -resize 50% output.jpg  # make smaller by 50%
convert input.jpg -resize 640x480\< output.jpg  # make large images smaller, leave smaller untouched
```
### pdf

#### rotate

* GUI `pdfarrange`, CTRL+arrow key for rotating the page

#### crop

* GUI `krop`, painful

#### compress/optimize/reduce size

https://www.digitalocean.com/community/tutorials/reduce-pdf-file-size-in-linux

```sh
ghostscript -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 -dPDFSETTINGS=/screen -dNOPAUSE -dQUIET -dBATCH -sOutputFile=output.pdf input.pdf
```

#### merge

https://pikepdf.readthedocs.io/en/latest/topics/pages.html

```python
from pikepdf import Pdf
from glob import glob

pdf = Pdf.new()

for file in sorted(glob('*.pdf')):
    src = Pdf.open(file)
    pdf.pages.extend(src.pages)

pdf.save('output.pdf')
```


### ansible

https://docs.ansible.com/ansible/2.9/modules/modules_by_category.html

```ssh
ansible-doc --type connection --list  # list modules for given category
ansible-config list
```

#### run ad-hoc command

`--diff` can be used as sort of a dry-run to see command outcome

```sh
ansible localhost -m ping
ansible localhost -m gather_facts
ansible --diff -m copy -a "src=master.gitconfig dest=~/.gitconfig" localhost
```

#### inventory

https://docs.ansible.com/ansible/latest/network/getting_started/first_inventory.html#build-your-inventory

##### see the inventory as ansible sees it

```sh
ansible-inventory -i inventory.yml --list
ansible-inventory -i inventory.yml --graph  # tree-like
```

#### vault

https://docs.ansible.com/ansible/latest/network/getting_started/first_inventory.html#network-vault

vault id, vault identity - ?

##### Encrypt a variable with a prompt for encryption password

```sh
ansible-vault encrypt_string \
  --vault-id $USER@prompt 'thepassword' \
  --name 'variable_name'
```

##### Encrypt a variable with encryption password stored as text in a file

```sh
ansible-vault encrypt_string \
  --vault-id $USER@path_to_the_file 'thepassword' \
  --name 'variable_name'
```

#### playbook

```sh
- hosts: localhost
  gather_facts: false
  handlers:
    - name: restart stuff
      become: yes
      service:
        name: stuff
        state: restarted
      listen: "restart stuff"
  roles:
    - foo
    - role: baz
      variable: bar
  tasks:
    - name: do stuff
      ping:
      notify: "restart stuff"
    - fail: msg="Bailing out"
      when: bar is undefined
```

##### Errors

```sh
[WARNING]: Platform linux on host host1 is using the discovered Python interpreter at /usr/bin/python3.9, but future installation of another Python interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-
core/2.12/reference_appendices/interpreter_discovery.html for more information.
```

Option 1: Set `ANSIBLE_PYTHON_INTERPRETER=auto_silent` (https://docs.ansible.com/ansible/latest/reference_appendices/config.html#interpreter-python)
Option 2: At ansible.cfg, set `interpreter_python = auto_silent` or to a specific python path



#### console

```sh
# execute 'setup' module on localhost with arguments 'filter=ansible_selinux' to filter the output
ansible -m setup localhost -a 'filter=ansible_selinux'
```


##### using sudo results in SUDO_USER variable

```sh
$ sudo su
# ansible -m setup localhost | grep USER
            "SUDO_USER": "sliwkr",
            "USER": "root",
```

##### changing user to root doesn't

```sh
$ su root
Password:
# ansible -m setup localhost | grep USER
[WARNING]: No inventory was parsed, only implicit localhost is available
            "USER": "sliwkr",
```


### vim

newline character: `\r`

#### format .json

```sh
  :%!jq .
  :%!python -m json.tool
```

#### run vimscript for a given file

```sh
vim -s script.vim file.txt
```

#### open man page for highlighted text

* Highlight the phrase
* Press Shift+K

#### start with no config

```sh
vim --clean
HOME=$(mktemp -d) vim -u NONE  # prior to 8.0.1554
```

#### clipboard / registers

Note: On [X11](#X11), a clipboard usually refers to *CLIPBOARD selection*

* `"+y` in visual mode, copy selection to clipboard register
* `"+p` paste from clipboard register

#### edit a file on a remote host

# just so you know - if you find yourself doing this on prd,
# you're doing something wrong

```sh
nvim scp://sliwkr@192.168.0.1//etc/nginx/conf.d/file.conf
```

### colorized less output through pipe

* A matter of telling the first program not to reset text colouring when passing its output through a pipe,
then telling the second program to interpret color sequences

```sh
tree -C | less -R
```

```sh
jq -C '.' file.json | less -R
```


### xdg

#### Find out which application handles a given filetype

```sh
# the .desktop files can be found in /usr/share/applications and ~/.local/share/applications
$ xdg-mime query default application/json
code.desktop
```

#### Change default application for a given file extension

```sh
$ xdg-mime query filetype file.json  # determine filetype (mimetype)
application/json

# associate another .desktop file with the mimetype
$ xdg-mime default <TAB-to-autocomplete> application/json
```

### tar

```sh
tar -xf file.tar.xz  # extract .tar.xz
tar -xzvf file.tar.gz  # extract .tar.gz
tar -xjvf file.tar.bz  # extract .tar.bz
tar -xaf file.tar.xz  # extract .tar.zst
tar --zstd -xvf file.tar.zst  # extract .tar.zst

tar -czvf name-of-archive.tar.gz /path/to/directory-or-file  # -c = create archive, -z = compress with gz, -v = show progress, -f = specify filename
find . -maxdepth 1 -iname "*.html" | xargs tar -czvf tw.tar.gz  # find all html files in $PWD without recursing & tar 'em

tar -tvf name-of-archive.tar.gz  # list archive contents
tar -tf name-of-archive.tar.gz --wildcards '*Filename*' # search for file in archive
```

### gzip

```sh
gzip -d file.gz  # unzip file.gz into the file which was archived & remove the .gz
```

### X11

#### clipboard

* Note: X11 clipboard consists of a few parts called `selections`.
There's `PRIMARY`, `SECONDARY`, and `CLIPBOARD`:
    * `PRIMARY` a default on linux. Try highlighting a text in the console, then pressing the middle mouse button - you're using `PRIMARY` that way
    * `SECONDARY` no idea. I don't think it's being used often
    * `CLIPBOARD` the one we're all used to from Windows. If you're using a virtual machine, this is the one to go for having a clipboard that's shared with the host.

```sh
# xclip alternative
echo 'stuff to be copied' | xsel -ib  # pipe stdout of a command into the CLIPBOARD selection
```

### rsync

```sh
rsync -r local_dir remote_location  # copy local_dir with contents to remote_location
rsync -r local_dir/ remote_location  # copy contents of local_dir, not the directory itself
rsync -p original copy  # keep permissions
rsync -t original copy  # keep timestamps
rsync local_dir/ remote_location --exclude=secrets  # exclude file
rsync local_dir/ remote_location --exclude-from=secrets_list  # exclude files from list
```

#### useful flags & arguments

```sh
--progress
--remove-source-files  # remove source after copying to remote
-n  # --dry-run

-a # --archive mode:
   # recurse directories
   # copy symlinks
   # preserve permissions
   # preserve timestamps
   # preserve file group
   # preserve owner
   # preserve device files
```

### 7zip

#### Archive a folder

```sh
7z a archive.7z ./folder_name
7a a -p file.7z file.txt
```

### irssi

#### Connect to a new network through soju bouncer

```sh
/network add -user sliwkr/irc.libera.chat libera
/server add -auto -tls -network libera chat.sr.ht 6697 <the-oauth-token>
```

#### Connect to a channel on a given network

```sh
/join -libera #irssi,#python
```

### linux wifi management interfaces

- NetworkManager
- iwd - iNet Wireless Daemon
   - relies on functionality integrated into the kernel, avoids usage of external libraries
   - can be used together with NetworkManager
   - can be a substitute of wpa_supplicant
   - can be used in standalone mode, without neither NetworkManager nor wpa_supplicant
- wpa_supplicant


### device details

#### hard drive info

```sh
blkid
hdparm -I /dev/sdX
```

#### hard drive - change label

```sh
e2label /dev/sdX "NEW_LABEL"
blkid  # to see the label
```

### battery

#### Check status

```sh
/sys/class/power_supply/BAT1  # directory with various battery property files
upower --dump  # output contains other devices than the battery 
upower --dump | grep 'Device.*BAT' | cut -f 2 -d ' '  # get upower device name from the output
upower -i <device-name>  # output about a particular device
```

### rclone

#### list buckets available on a remote

```sh
rclone lsd remotename:
```

#### mount a s3-compatible bucket as a filesystem in userspace

```sh
# find s3ApiUrl
curl https://api.backblazeb2.com/b2api/v2/b2_authorize_account -u "<keyID>:<applicationKey>"
# mount s3 as a filesystem
s3fs 'bucket-name' 'mount/point' -o passwd_file=$HOME/.passwd-s3fs -o url="<s3ApiUrl>" -o use_path_request_style
```

#### copy files from local directory to the bucket, show progress

```sh
rclone copy Pictures backblaze:pudelko/Pictures -P --dry-run
```

### alpine - build broadcom wifi driver

```sh
doas apk add alpine-sdk git
doas addgroup $(whoami) abuild
git clone git://git.alpinelinux.org/aports
```

### monitoring

#### cpu

```sh
uptime | mailx -s "cpu" root
```

#### memory

```sh
free | mailx -s "mem" root
```

#### disk

```sh
(df -h; du -sh /home/*) | mailx -s "disk" root
```

#### process aliveness

```sh
ps -ef | grep theprocess | mailx -s "theprocess" root
```

#### system aliveness

```sh
ping -c 4 wp.pl | mailx -s "aliveness" root
```

### wget

#### Download a file

```sh
wget https://website.com/file.txt
wget -O filename.txt https://website.com/file.txt
wget -i fielist.txt  # download a list of files
```

### mpv

#### Run with subtitles

```sh
mpv video.mkv --sub-file=video-subtitles.txt
```

### git

#### worktrees / multiple branches checked out at the same time

```sh
# --bare could be used, but git isn't really designed to work this way
git clone ssh://git@forge.com/myrepo.git myrepo/main  # main as in the 'main' branch
cd myrepo/main
git worktree add ../ci ci  # adds 'ci' worktree which tracks 'ci' branch
git worktree list
git worktree remove ../ci  # remove local linked 'ci' worktree
```

#### what's the upstream the branch is tracking?

```sh
git remote show origin  # porcelain/slow
git remote get-url origin  # show the url of this repo
git rev-parse --abbrev-ref branchname@{upstream}  # returns branch if set, errors if not
git branch -u originname/foo foo  # set local branch to track a remote branch originname/foo
```

#### Show filenames modified by a diff

```sh
git diff --compact-summary HEAD~1 HEAD
```

#### Remove a directory

```sh
git rm -r directoryname  # removes from the filesystem
git rm -r --cached directoryname  # removes from git repository, keeps the directory in the filesystem
git commit -m 'delete directoryname' && git push origin mybranchname
```

#### Tag a commit

```sh
git tag tagname commit  # create a new tag locally
git push origin tag tagname  # push the tag to remote
git tag --delete tagname  # remove a wrong tag locally
git push --delete origin tagname  # remove a wrong tag on remote
git fetch --tags  # get a list of tags from remote
git tag  # list tags on local
```

#### Compare feature branch $HEAD with a common ancestor of the master and feature branch

```sh
git diff master...feature
```

#### Sparse checkout

```sh
git clone --no-checkout https://therepo.git
cd therepo
git sparse-checkout init --cone
git sparse-checkout set onedir/ofinterest another/dir
git checkout master

```

#### List branches containing / not containing a specific commit id

```sh
git branch --contains <commit>
git branch --no-contains <commit>
```

#### See commit history for a given file

```sh
git log --follow -- path/to/file
```

#### See diff with more surrounding lines

```sh
git diff -U6  # show 6 lines surrounding a change
```

#### Get root path of a repository

```sh
git rev-parse --show-toplevel
git rev-parse --show-superproject-working-tree  # submodules only
```

#### create a patch

```sh
git diff > a_file.patch
```

#### apply a patch

```sh
git apply --stat a_file.patch  # see stats
git apply --check a_file.patch # see if it applies

git apply a_file.patch  # apply & add to staging without committing
git am --signoff < a_file.patch  # apply & commit & signoff
```
### icloudpd

#### Sync all photos and videos from iCloud to a directory

```sh
icloudpd -u my.email@mail.com -d target_dir
# move to "Recently deleted" after downloading
icloudpd -u my.email@mail.com -d target_dir --delete-after-download
```

### inxi

#### Get full system information

```sh
inxi -Fxxxzrc0
```

### tmux

#### Windows / panes

```sh
# Rotate panes in the current window
:rotate-window
# Switch pane split layout (e.g. horiontal -> vertical)
C-b space
:next-layout
```

#### Sessions

```sh
tmux new-session -s ansible  # create a new session called 'ansible' (C-, to rename)
C-), C-(  # switch between sessions
C-d  # detach
tmux ls  # list active sessions
:attach -c ~  # change default new window directory to ~
```

### screen

#### split screen

* `Ctrl+a |` split vertically
* `Ctrl+a S` split horizontally
* `Ctrl+a TAB` switch between splits
* `Ctrl+a Q` unsplit

#### new window

* `Ctrl+a c` create new window
* `Ctrl+a n` / `Ctrl+a p` / `Ctrl+a Ctrl+a` switch between windows

#### scroll

* `Ctrl+a [` enter copy mode, then Pgup and Pgdn

### bash

#### set multiple variables at once

```sh
read -r foo bar baz <<< $(echo fooval barval bazval)
```

#### reset terminal settings

```sh
printf "\033c"  # <ESC>c
# \033 == \x1B == 27 == ESC
# https://web.archive.org/web/20191222201924/http://www.termsys.demon.co.uk/vtansi.htm
```

#### Useful flags

```bash
set -e  # exit immediately on error
set -x  # be verbose (also set -v), e.g. print commands being executed
set +x  # stop being verbose
set -u  # throw error on undefined variable instead of assuming it's empty
set -o pipefail  # if a command in a pipeline fails, makes the pipeline return the exit status of the failed command (as opposed to returning the last command status by default)
```

#### Read file line by line

```sh
# IFS is blank so filenames with spaces won't get treated as separate files
while IFS= read -r line; do
    echo "Text read from file: $line"
done < my_filename.txt
```

#### replace file extension on mass

```sh
for f in *.pem; do mv -- "$f"  "${f%.pem}.crt"; done
```

#### tables, arrays

* Arrays cannot be passed as function arguments; instead, an array would be passed expanded as a list of arguments - which can be concatenated back through `$@`.

```bash
# declare an "associative array variable" ACCOUNT_IDS
declare -A ACCOUNT_IDS=(
    ["ci"]="123"
    ["stg"]="456"
    ["prd"]="789"
)

# declare an "indexed array variable" ACCOUNT_LIST
declare -a ACCOUNT_LIST=("123" "456" "789")
# another way
ACCOUNT_LIST=("123" "456" "789")

# get all values of an array
echo ${ACCOUNT_IDS[@]}  # '*' can be used and differ in case of values within double quotes, see 'Arrays' in man

# get value of array member
echo ${ACCOUNT_IDS[ci]}
echo ${ACCOUNT_LIST[1]}
echo ${ACCOUNT_LIST}  # equal to ${ACCOUNT_LIST[0]}

# get keys of an array
echo ${!ACCOUNT_IDS[@]}

# delete an array member
unset ACCOUNT_IDS[ci]

# delete an array
unset ACCOUNT_LIST

# iterate over an array
for account in "${ACCOUNT_IDS[@]}"; do
    echo "${account}"
done

# copy an array
arr=$("123" "456")
anotherarr=( "${arr[@]}" )

```

### jq

https://zwischenzugs.com/2023/06/27/learn-jq-the-hard-way-part-i-json/

#### Get an object that has a key with a matching value

```sh
cat ecr.output | jq '.repositories[] | select(.repositoryName=="shenanigans/takeover")'
```

#### Get multiple keys from a list of dictionaries

```sh
jq '.ResourceRecordSets[]
  | [.Name, .Type]' sample.json
```

#### colorized jq | less output

jq -C - do not reset text colouring
less -R - interpret color sequences

```sh
ip -j a | jq -C .[0] | less -R
```

#### replace key value in place (sort of)

https://stackoverflow.com/a/42718624

```sh
tmp=$(mktemp)
jq '.address = "abcde"' test.json > "$tmp"
mv "$tmp" test.json
```

#### get a key that contains a value

```sh
aws iam list-roles --profile myprofile \
    | jq -r '.Roles[].Arn | select(contains("power-user-access"))'

aws iam list-roles | jq '.Roles[] | select(.RoleName=="rolename") | .Arn'
```

#### do not wrap output in quotes

```sh
jq .items[].metadata.name stuff.json -r
```

#### grab just top-level keys

```
jq '.|keys' stuff.json
jq 'keys[]' stuff.json
```

### sort

#### get tab-separated text sorted by given field

```sh
cat file.txt | sort --key=3  # also see KEYDEF in man for additional sorting options
```

### expect

`send "\r"` - Enter
`expect "text"` - wait for "text" being visible on the terminal screen

#### Automate an action executed in a TUI over ssh session

```tcl
#!/usr/bin/expect -f

log_user 0
spawn ssh myremotehost
expect "myusername"
send "botany \r"
expect "options"
send "\r"
send "q\r"
expect "myusername"
send "exit\r"
```

### systemd

#### Oneshot service

```sh
# $HOME/.config/systemd/user/foo.service

[Unit]
Description=Unit to Fooify the Bazoor
Wants=foo.timer

[Service]
Type=oneshot
ExecStart=/home/username/scripts/script.sh

[Install]
WantedBy=multi-user.target
```

#### Timer

```sh
# $HOME/.config/systemd/user/foo.timer

[Unit]
Description=Timer which starts foo.service unit every 24h
Requires=foo.service

[Timer]
Unit=foo.service
OnUnitInactiveSec=86400

[Install]
WantedBy=timers.target
```

### fonts

#### Debian (https://wiki.debian.org/Fonts)

* `/usr/share/fonts` / `~/.local/share/fonts` font location
* `fc-cache -v` update font cache, (config at /etc/fonts/fonts.conf)


### sed

#### Find a line that starts with a word. Add another word to the end of that line

```sh
sed 's/^line_starts_with_me.*/& line_will_end_with_me/' file.txt  #  -i for in-place
```

#### Replace a word in a line

```sh
# Replace <Flag>false</Flag> to <Flag>true</Flag>
sed -i 's/<Flag>false<\/Flag>/<Flag>true<\/Flag>/' config.xml
```

### cryptsetup

#### mount & unmount an encrypted partition

```sh
# as root
blkid | grep crypto_LUKS  # find out the partition
cryptsetup open --type luks /dev/sdc3 the_partition_name  # setup unencrypted mapping to encrypted partition; requires passphrase
mount /dev/mapper/the_part_name /mnt/the_mountpoint  # mount an unencrypted partition
umount /dev/mapper/the_part_name
cryptsetup close the_part_name  # remove the unencrypted mapping and wipe the encryption key from memory
```

### firewalld

#### Get list of open ports

```sh
for s in $(firewall-cmd --list-services); do firewall-cmd --permanent --service "$s" --get-ports; done;
```

#### Get list of predefined services

```sh
firewall-cmd --get-services
```

#### Enable predefined service

```sh
firewall-cmd --add-service grafana  # --permanent for persistence after --reload
```

#### Open a selected port if there's no predefined service for it

```sh
firewall-cmd --add-port=8096/tcp  # --permanent for persistence after --reload
```

#### List enabled predefined services

```sh
firewall-cmd --list-services
```

### iso cd

#### flash an .iso to an USB drive

```sh
# use lsblk or ls /dev/sd* to find out the device
sudo dd if=image.iso of=/dev/sdb bs=1024k status=progress
```

#### Create an .iso from a CD or a DVD

```sh
# cat can be actually used to create one
cat /dev/sr0 > iso_name.iso
```

### dpkg

#### What's the name of that package from which my tool comes from?

```sh
dpkg -S ip | grep '/bin/ip'
```

### dig

#### Fetch the new value for the record, ignoring TTL cache (recursive lookup being done)

```sh
dig ddg.co +trace
```

#### Ask a resolver for the cached value of a record

```sh
dig @9.9.9.9 ddg.co +norecurse
```

#### better output

```sh
dig ddg.co +short
dig ddg.co +yaml
```

#### how long do i have to wait for a negative value to expire?

```sh
$ dig +all void.ddg.co
(...)
;; AUTHORITY SECTION:
ddg.co.  5  IN  SOA  dns1.p03.nsone.net. hostmaster.nsone.net. 1617736126 7200 7200 1209600 3600
         ^                                                                                    ^
    A minimum value of this...                                                      ...and this

(...)
```


### files

#### Update modified timestamp

```sh
touch -a -m -t 201604301451.58 file.jpg  # YYYYMMDDHHmm.ss
```

#### Renaming / filenames using parameter expansion

* https://mywiki.wooledge.org/BashGuide/Parameters#Parameter_Expansion
* Works for simple names, sucks for complex ones; prepare your files first
* Keep in mind filenames with spaces, filenames with no extension, filenames with dots in the name

```sh
VAR=2024.05.tar.gz
echo "$VAR"             # 2024.05.tar.gz
echo "${VAR%.*}"        # 2024.05.tar
echo "${VAR%.*}.log"    # 2024.05.tar.log
echo "${VAR#*.}"        # 05.tar.gz
echo "${VAR##*.}"       # gz
echo "newname.${VAR#*.}"    # newname.05.tar.gz
echo "newname.${VAR##*.}"   # newname.gz
```

#### Given an absolute path to a file, get the directory / filename

```sh
VAR=/mnt/disk/foo/2022/11/12/file.png
basename $VAR  # file.png
dirname $VAR  # /mnt/disk/foo/2022/11/12
```

#### Get absolute path of a file

```sh
readlink -f file.txt
realpath -f file.txt
```

#### Create a 1KiB file

```sh
truncate --size 1K file.txt  # empty 1KiB file (du -k = 0)
tr -dc A-Za-z0-9 </dev/urandom | head -c 1K > file.txt  # 1KiB file with alphanumeric content
```

#### montior / watch for filesystem changes

```sh
# inotify-tools
inotifywait -m -r /path/to/your/directory  # -m monitor, -r recursive
```

### kernel

#### sysctl

https://linuxconfig.org/how-to-read-and-change-the-value-of-kernel-parameters-using-sysctl

```sh
sysctl -a  # list all kernel parameters and their values
sysctl -w fs.inotify.max_user_watches=800000  # set a value, don't persist
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p  # set a value, persist after reboot
```

### nmap

#### ICMP (ping) scan

* When only ip needed

```sh
sudo nmap -sn 192.168.0.0/24
```

* Not nmap, but clever

```sh
#!/bin/sh
# originally posted by Sergiy Kolodyazhnyy at https://askubuntu.com/a/655942
#set -x
pingf(){
    if ping -w 2 -q -c 1 192.168.0."$1" > /dev/null ;
    then
        printf "IP %s is up\n" 192.168.0."$1"
    fi
}

main(){

    NUM=1
    while [ $NUM -lt 255  ];do
        pingf "$NUM" &
        NUM=$(expr "$NUM" + 1)
    done
    wait
}
main
```

#### TCP SYN scan

* Quite fast, 15s for /24 subnet

```sh
sudo nmap -sS 192.168.0.0/24
```


### lvm

* https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/configuring_and_managing_logical_volumes/overview-of-logical-volume-management#lvm-architecture
* https://www.baeldung.com/linux/lvm-add-disk

```sh
pvdisplay  # list physical volumes
vgdisplay  # list volume groups
lvdisplay  # list logical volumes
```

#### Add a physical volume to LVM

```sh
lsblk # get the device identifier
pvcreate /dev/sdX
pvdisplay  # see it added
vgextend rl /dev/sdX  # add physical volume /dev/sdX to volume group rl (expanding claimable space)
```

#### Resize - expand existing logical volume

```sh
# expand existing-lv with 95% of the available free space
lvresize --extents +95%FREE --resizefs rl/existing-lv
# expand existing-lv by 10GB
lvresize --size +10G --resizefs rl/existing-lv
```

#### Resize - shrink existing logical volume

* xfs filesystem: reducing logical volumes is not supported.
Workarounds are described https://dannyda.com/2021/10/08/how-to-shrink-reduce-xfs-partition-size-on-lvm/

```sh
# reduce size of a volume by 10GB
lvresize --size -10G --resizefs rl/existing-lv
```

### network

#### List all tcp connections

```sh
conntrack -L
conntrack -E -s 192.168.1.100 # listen for connections from a given ip address
conntrack -E -d 192.168.1.200 # listen for connections towards a given ip address
```


### x11screen

```sh
xrandr --addmode VIRTUAL1 1280x720
xrandr --output VIRTUAL1 --mode 1280x720 --left-of HDMI1
xrandr --delmode VIRTUAL1 1280x720
```


### dates

#### Convert unix timestamp to human-readable format

```sh
date -d @1743078901
```

#### Create unix timetamp of a given date

```python
import datetime
int(
  (datetime.datetime.now() + datetime.timedelta(days=3)).timestamp()
)
```

### xargs

#### (subshell) execute a command within each directory that's under pwd

```sh
find . -maxdepth 1 -type d | tail -n +2 | xargs -I '{}' bash -c 'cd {}; git status'
```
