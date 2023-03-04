# Linux snippet cookbook

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
- [ansible](#ansible)
- [vim](#vim)
- [colorized tree | less output](#colorized-tree-less-output)
- [colorized jq | less output](#colorized-jq-less-output)
- [associating application with a given extension through xdg mimetypes](#xdg)
- [tar](#tar)
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

## snippets

### new user

```sh
useradd -G sudo tom  # add user named tom and add tom to sudoers
usermod -a -G secret tom  # add tom to secret group
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
```

### curl

```sh
-H "x-api-key: 1234" -H "x-api-user: admin"  # each header given separately
-X POST -H "Content-Type: Application/Json" -d '{"key": "value"}'  # json post
```

### grep

```sh
grep -E 'pattern1|pattern2' log_file  # search multiple patterns, words
docker-compose logs | grep -E 'ERROR|INFO'  # output can be piped into
grep -e pattern1 -e pattern2 log_file  # patterns can be passed separately without quotes
grep -E 'pattern' -C 3  # print 3 lines around the found pattern, use -A for above and -B for below
grep -r 'pattern' dir  # search for a pattern through all files in a given directory
```

### tree

```sh
tree -a -I '.git|*~'  # list all files excluding .git folder and files ending with ~
```

### gpg

#### glossary

key name - refers to the key ID visible on `--list-keys`

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

```sh

fdisk /dev/sdX
  p - list existing partitions
  i - info about partitions
  n - new partition
  t - change partition type
    HPFS/NTFS/exFAT - exFAT, FAT32
  w - save changes

mkfs.exfat -n Devicelabel /dev/sdXY
mkfs.ext4 /dev/sdXY

e2label /dev/sdXY device-label
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

#### strip metadata

```sh
convert -strip input.png output.png
identify -verbose output.png  # to see what's been removed
```

#### join multiple images horizontally

```sh
montage input-1.png input-2.png -tile 1x2 -geometry +0+0 output.png
```

#### convert pdf to png

```sh
convert -density 300 input.pdf -resize 25% output.png
```

#### rotate image

```sh
convert -rotate {90,180,270} input.jpg output.jpg
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

### colorized tree less output

tree -C - do not reset text colouring when passing output through a pipe
less -R - interpret color sequences

```sh
tree -C | less -R
```

### colorized jq less output

jq -C - do not reset text colouring
less -R - interpret color sequences

```sh
ip -j a | jq -C .[0] | less -R
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
$ xdg-mime query filetype <a-file-that-has-the-extension-of-interest.json>
application/json

# associate another .desktop file with the mimetype
$ xdg-mime default <TAB-to-autocomplete> application/json
```

### tar

```sh
tar -xf file.tar.xz
tar -xzvf file.tar.gz
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
```

### mpv

#### Run with subtitles

```sh
mpv video.mkv --sub-file=video-subtitles.txt
```
