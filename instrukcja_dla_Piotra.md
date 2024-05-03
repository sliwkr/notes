# Instalacja Linux na PEAQ (VALE Notebook Slim S132)

## Ściągnięcie systemu

* Wybrałem dystrubycje Debian w wersji testing. Ściągnięta z https://www.debian.org/
* Szczegóły: https://www.debian.org/devel/testing.

## Przygotowanie instalatora

Kwestia sformatowania dysku USB. Pewnie masz już na to sposób, ale opisuje by było kompletnie

### Na Windows

Pamiętam, że kiedyś używałem Rufus do wypalenia .iso na USB, i działało. https://rufus.ie/en/
Jest też Etcher, na Windows i Linux. Nie korzystałem, https://etcher.balena.io/

### Na Linux

#### Bez instalowania dodatkowych rzeczy, z terminala 

Potrzebujemy dowiedzieć się, jak nazywa się USB które podłączyliśmy; później, potrzebujemy zgrać .iso na USB

```sh
$ lsblk  # by dowiedzieć się, jak nazywa się USB. W przypadku poniżej, to `sdb` z jedną partycją `sdb1`. Każda z pozycji jest widoczna pod `/dev/`
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda      8:0    0 223.6G  0 disk
├─sda1   8:1    0   100M  0 part /boot/efi
├─...
└─sda6   8:6    0  56.6G  0 part /home
sdb      8:16   0   4.3G  0 disk  # w moim przypadku, to ten
└─sdb1   8:17   0   4.3G  0 part /mnt/Pny
```

```sh
# by zgrać .iso na USB. Zwróć uwagę, że w `of` jest nazwa dysku, a nie partycji (sdb, nie sdb1)
sudo dd if=debian-testing-amd64-netinst.iso of=/dev/sdb bs=4M status=progress && sync
```

## Instalacja

Będzie potrzebny internet: wifi (łączy się bez problemu w moim przypadku) albo LAN. Później kwestia przeklikania przez różne menu instalatora. Uwagi:

### root password

Root password może zostać puste: root user będzie wyłączony, a główny user będzie miał uprawnienia sudo
	* Można też ustawić hasło. Wtedy, by uzyskać root priviledges, powinno dodać się usera do grupy `sudo` lub korzystać z `su - ` by przelogować się na root

### Partycjonowanie

* Wbudowany dysk ma niewiele ponad 60gb; polecam dla wygody opcje z LVM, by w przypadku dodania kolejnego dysku móc wygodnie rozszerzyć istniejące partycje bez utraty danych.
	* Jeśli nie zamierzasz dodawać kolejnego dysku, to wersja bez LVM jest (niewiele) prostsza w ustawieniu
* Kwestia wszystkich plików na 1 partycji vs. osobnych partycji `/home` lub osobnych `/home`, `/var`, `/tmp`:
	* Najbardziej przydatne w przypadku reinstalacji systemu; jeśli nie zamierzasz instalować innej dystrybucji Linuxa na komputerze lub chcesz samemu zrobić backup rzeczy do przeniesienia, to opcja z wszystkimi plikami na 1 partycji będzie najwygodniejsza.

### Wybranie oprogramowania na początek

* Zaznaczone tylko 2:
	* KDE Plasma (środowisko graficzne)
	* standard system utilities (opcjonalne, prawdopodobnie niepotrzebne jeśli nie będziesz korzystał z terminala, można doinstalować później w miarę potrzeby)

## Kwestia problemu z dźwiękiem

Pierwszym środowiskiem graficznym jakie wybrałem do sprawdzenia, było KDE. Wydaje mi się, że miałem po prostu pecha by wybrać środowisko, na którym wszystko działa. Przy sprawdzaniu innych opcji w szczególności zauważyłem, że "Debian graphical environment" (innymi słowy GNOME), nie odtwarza dźwięku jak opisywałeś.

* Nie szukałem konkretnej przyczyny, dlaczego nie działa na GNOME.

## Po instalacji

### Coś ode mnie

* Mamy zainstalowaną dystrybucje Debian ze środowiskiem graficznym KDE Plasma.
* Aktualizacje: KDE będzie powiadamiać, gdy coś jest do zaktualizowania. Kwestia kliknięcia i gotowe
* Instalacja oprogramowania: przez "Discover" lub za pomocą `apt` z terminala 
* We "Virtual Desktops" można ustawić coś, na czym Windows wzorował Task View (efekt Windows+Tab)
* Coś, co chciałbym przeczytać zaczynając z linuxem, konkretnie dla Debian: https://wiki.debian.org/DontBreakDebian
* ArchWiki jest dobrym źródłem informacji, mimo że dotyczy innej dystrybucji. https://wiki.archlinux.org/

### Informacje o systemie

```sh
$ inxi -Fxxxzrc0
System:
  Kernel: 6.7.12-amd64 arch: x86_64 bits: 64 compiler: gcc v: 13.2.0
    clocksource: tsc
  Desktop: KDE Plasma v: 5.27.10 tk: Qt v: 5.15.10 wm: kwin_wayland vt: 1
    dm: SDDM Distro: Debian GNU/Linux trixie/sid
Machine:
  Type: Laptop System: VALE product: Notebook Slim S132 v: N/A
    serial: <superuser required>
  Mobo: VA_inet model: GMLR_V11 serial: <superuser required>
    part-nu: PNB_S132-CGR-1O uuid: <superuser required>
    UEFI: American Megatrends v: S132CGMLR_3.0.5 date: 01/13/2022
Battery:
  ID-1: BAT0 charge: 36.5 Wh (100.0%) condition: 36.5/36.5 Wh (100.0%)
    volts: 7.6 min: N/A model: N/A type: Unknown serial: <filter>
    status: charging
CPU:
  Info: dual core model: Intel Celeron N4020 bits: 64 type: MCP
    smt: <unsupported> arch: Goldmont Plus rev: 8 cache: L1: 112 KiB L2: 4 MiB
  Speed (MHz): avg: 1016 high: 1196 min/max: 800/2800 cores: 1: 1196 2: 837
    bogomips: 4377
  Flags: ht lm nx pae sse sse2 sse3 sse4_1 sse4_2 ssse3 vmx
Graphics:
  Device-1: Intel GeminiLake [UHD Graphics 600] driver: i915 v: kernel
    arch: Gen-9.5 ports: active: eDP-1 empty: DP-1,HDMI-A-1 bus-ID: 00:02.0
    chip-ID: 8086:3185 class-ID: 0300
  Device-2: USB Camera driver: uvcvideo type: USB rev: 2.0 speed: 480 Mb/s
    lanes: 1 bus-ID: 1-7:4 chip-ID: 32e6:9005 class-ID: 0e02
  Display: wayland server: X.org v: 1.21.1.11 with: Xwayland v: 23.2.6
    compositor: kwin_wayland driver: X: loaded: modesetting unloaded: fbdev,vesa
    dri: iris gpu: i915 display-ID: 0
  Monitor-1: eDP-1 model: BOE Display 0x09ec res: 1920x1080 dpi: 166
    size: 294x165mm (11.57x6.5") diag: 337mm (13.3") modes: 1920x1080
  API: EGL v: 1.5 hw: drv: intel iris platforms: device: 0 drv: iris
    device: 1 drv: swrast gbm: drv: iris surfaceless: drv: iris wayland:
    drv: iris x11: drv: iris
  API: OpenGL v: 4.6 compat-v: 4.5 vendor: intel mesa v: 23.3.5-1 glx-v: 1.4
    direct-render: yes renderer: Mesa Intel UHD Graphics 600 (GLK 2)
    device-ID: 8086:3185 display-ID: :1.0
Audio:
  Device-1: Intel Celeron/Pentium Silver Processor High Definition Audio
    driver: sof-audio-pci-intel-apl bus-ID: 00:0e.0 chip-ID: 8086:3198
    class-ID: 0401
  API: ALSA v: k6.7.12-amd64 status: kernel-api
  Server-1: PulseAudio v: 16.1 status: active
Network:
  Device-1: Realtek RTL8821CE 802.11ac PCIe Wireless Network Adapter
    driver: rtw_8821ce v: N/A pcie: speed: 2.5 GT/s lanes: 1 port: e000
    bus-ID: 01:00.0 chip-ID: 10ec:c821 class-ID: 0280
  IF: wlp1s0 state: up mac: <filter>
Bluetooth:
  Device-1: Realtek Bluetooth Radio driver: btusb v: 0.8 type: USB rev: 1.1
    speed: 12 Mb/s lanes: 1 bus-ID: 1-4:3 chip-ID: 0bda:c821 class-ID: e001
    serial: <filter>
  Report: hciconfig ID: hci0 rfk-id: 1 state: up address: <filter> bt-v: 4.2
    lmp-v: 8 sub-v: f098 hci-v: 8 rev: 75b8 class-ID: 7c010c
Drives:
  Local Storage: total: 58.24 GiB used: 6.27 GiB (10.8%)
  ID-1: /dev/mmcblk0 model: MMC64G size: 58.24 GiB type: Removable tech: SSD
    serial: <filter> fw-rev: 0x8 scheme: GPT
Partition:
  ID-1: / size: 55.12 GiB used: 6.08 GiB (11.0%) fs: ext4 dev: /dev/dm-0
    mapped: peaq--vg-root
  ID-2: /boot size: 455.1 MiB used: 155.3 MiB (34.1%) fs: ext2
    dev: /dev/mmcblk0p2
  ID-3: /boot/efi size: 511 MiB used: 4.4 MiB (0.9%) fs: vfat
    dev: /dev/mmcblk0p1
Swap:
  ID-1: swap-1 type: partition size: 980 MiB used: 36 MiB (3.7%) priority: -2
    dev: /dev/dm-1 mapped: peaq--vg-swap_1
Sensors:
  System Temperatures: cpu: 55.0 C mobo: N/A
  Fan Speeds (rpm): N/A
Repos:
  Packages: pm: dpkg pkgs: 1994
  Active apt repos in: /etc/apt/sources.list
    1: deb http://deb.debian.org/debian/ trixie main non-free-firmware
    2: deb-src http://deb.debian.org/debian/ trixie main non-free-firmware
    3: deb http://security.debian.org/debian-security trixie-security main non-free-firmware
    4: deb-src http://security.debian.org/debian-security trixie-security main non-free-firmware
    5: deb http://deb.debian.org/debian/ trixie-updates main non-free-firmware
    6: deb-src http://deb.debian.org/debian/ trixie-updates main non-free-firmware
Info:
  Memory: total: 4 GiB available: 3.67 GiB used: 1.96 GiB (53.3%)
  Processes: 167 Power: uptime: 23m states: freeze,mem suspend: deep
    wakeups: 0 hibernate: disabled Init: systemd v: 255 target: graphical (5)
    default: graphical
  Compilers: N/A Shell: Bash v: 5.2.21 running-in: konsole inxi: 3.3.34
```
