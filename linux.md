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

### change hostname

```sh
hostnamectl set-hostname maszyna-zaglady
sudo echo 'maszyna-zaglady' > /etc/hostname
```

### ip

```sh
ip -br a
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
```

### tree
```sh
tree -a -I '.git|*~'  # list all files excluding .git folder and files ending with ~
```

### gpg
```sh
gpg --encrypt --armor -R recpient@email.com the_file  # encrypt a file with anonymous recipient
gpg --output decrypted_file --decrypt encrypted_file.asc  # read encrypted file, write output to decrypted_file
gpg --gen-key  # configure new keys, --full-gen-key for more options like RSA length
# --armor for ASCII output, -r known recipient, -R anonymous recipient
# --sign known author, --local-user <ID> to choose author
# good idea to create multiple revocation certificates for various revocation reasons
# make sure its chmod 600
gpg --output revocation.crt --gen-revoke mail@domain.com
gpg --keyserver pgp.mit.edu  --search-keys search_parameters  # search popular keyserver for keys
gpg --import name_of_public_key_file  # import public key into local keychain
gpg --fingerprint mail@domain.com  # brief info about imported key
gpg --sign-key mail@domain.com  # sign/trust imported key, number of signs can be seen when importing
gpg --output my.key --armor --export mail@domain.com
gpg --list-keys  # list fingerprints of imported keys
gpg --send-keys --keyserver pgp.mit.edu --fingerprint mail@domain.com  # send public key to a keyserver
gpg --edit-key mail@domain.com  # open menu for editing the key
gpg --export > public-keys.pgp  # export public keys
gpg --export-secret-keys > private-keys.pgp  # export private keys
gpg --import < public-keys.pgp  # import key from file
```
