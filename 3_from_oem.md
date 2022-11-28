# Build Linux Kernel from Ubuntu Linux kernel OEM source tree
## Check system kernel & Download the related source
```
uname -r
git clone https://git.launchpad.net/ubuntu/+source/linux-oem-5.14
```

## Create config file

### Create default config file
```
make x86_64_defconfig
```

### Copy system config file
```
cp /boot/config-`uname -r` linux-oem-5.14/.config
```
Revise the following line in the config
```
...
CONFIG_SYSTEM_TRUSTED_KEYS=""
...
CONFIG_SYSTEM_REVOCATION_KEYS=""
...
```

## Create kernel deb package
```
make bindeb-pkg -j $(nproc)
```

5 files generates includes 3 deb files

```
linux-headers-5.14.21_5.14.21-1_amd64.deb
linux-image-5.14.21_5.14.21-1_amd64.deb
linux-libc-dev_5.14.21-1_amd64.deb
linux-upstream_5.14.21-1_amd64.buildinfo
linux-upstream_5.14.21-1_amd64.changes
```

## Install deb package
```
sudo dpkg -i linux-headers-5.14.21_5.14.21-1_amd64.deb
sudo dpkg -i linux-image-5.14.21_5.14.21-1_amd64.deb
sudo dpkg -i linux-libc-dev_5.14.21-1_amd64.deb
```

## Check /boot
```
u@u-Vostro-3420:/boot$ ll
total 292236
drwxr-xr-x  4 root root     4096 十一 28 14:28 ./
drwxr-xr-x 20 root root     4096 十一 28 09:23 ../
-rw-r--r--  1 root root   259375  六   4 02:00 config-5.14.0-1042-oem
-rw-r--r--  1 root root   259368  十  14 20:45 config-5.14.0-1054-oem
-rw-r--r--  1 root root   127447 十一 28 13:27 config-5.14.21
-rw-r--r--  1 root root   136099 十一 28 12:30 config-6.1.0-rc7
drwx------  3 root root    16384  一   1  1970 efi/
drwxr-xr-x  4 root root     4096 十一 28 14:28 grub/
lrwxrwxrwx  1 root root       26 十一 28 09:49 initrd.img -> initrd.img-5.14.0-1054-oem
-rw-r--r--  1 root root 95624194 十一 28 09:49 initrd.img-5.14.0-1042-oem
-rw-r--r--  1 root root 95629710 十一 28 09:50 initrd.img-5.14.0-1054-oem
-rw-r--r--  1 root root 22785959 十一 28 14:28 initrd.img-5.14.21
-rw-r--r--  1 root root 22786040 十一 28 13:05 initrd.img-6.1.0-rc7
lrwxrwxrwx  1 root root       26 十一 28 09:12 initrd.img.old -> initrd.img-5.14.0-1042-oem
-rw-r--r--  1 root root   182704  八  18  2020 memtest86+.bin
-rw-r--r--  1 root root   184380  八  18  2020 memtest86+.elf
-rw-r--r--  1 root root   184884  八  18  2020 memtest86+_multiboot.bin
-rw-------  1 root root  5980141  六   4 02:00 System.map-5.14.0-1042-oem
-rw-------  1 root root  5972996  十  14 20:45 System.map-5.14.0-1054-oem
-rw-r--r--  1 root root  5454990 十一 28 13:27 System.map-5.14.21
-rw-r--r--  1 root root  4617901 十一 28 12:30 System.map-6.1.0-rc7
lrwxrwxrwx  1 root root       23 十一 28 09:49 vmlinuz -> vmlinuz-5.14.0-1054-oem
-rw-------  1 root root  8933280  六   4 02:01 vmlinuz-5.14.0-1042-oem
-rw-------  1 root root  8933920  十  14 20:50 vmlinuz-5.14.0-1054-oem
-rw-r--r--  1 root root 10056224 十一 28 13:27 vmlinuz-5.14.21
-rw-r--r--  1 root root 11066688 十一 28 12:30 vmlinuz-6.1.0-rc7
lrwxrwxrwx  1 root root       23 十一 28 09:49 vmlinuz.old -> vmlinuz-5.14.0-1042-oem
```

###### REF
- https://code.launchpad.net/ubuntu/+source/linux-oem-5.14
- Solve the error: `No rule to make target ‘debian/canonical-certs.pem‘, needed by ‘certs/x509_certificate_list‘`
https://unix.stackexchange.com/questions/293642/attempting-to-compile-kernel-yields-a-certification-error/649484#649484
- Solve the error: `/bin/sh: 1:zstd: not found`
https://forum.openwrt.org/t/ubuntu-20-04-compile-zstd-error/66619
