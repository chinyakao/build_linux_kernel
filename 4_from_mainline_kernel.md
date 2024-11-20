# Build Linux Kernel from Ubuntu Mainline Kernel Tree
https://kernel.ubuntu.com/mainline/

## Build Step
### Copy system config file
```
cp /boot/config-`uname -r` .config
```

### Revise config cert
```
# (Original)
CONFIG_SYSTEM_TRUSTED_KEYS="debian/canonical-certs.pem"
CONFIG_SYSTEM_REVOCATION_KEYS="debian/canonical-revoked-certs.pem"
```
```
# (Revised)
CONFIG_SYSTEM_TRUSTED_KEYS=""
CONFIG_SYSTEM_REVOCATION_KEYS=""
```
### Make config
```
make oldconfig
```

### Create kernel deb package
```
make bindeb-pkg -j $(nproc)
```

### 6 files be generated
```
linux-headers-xxx.deb
linux-image-xxx.deb
linux-image-xxx-dbg-xxx.deb
linux-libc-xxx.deb
linux-upstream_xxx.buildinfo
linux-upstream_xxx.changes
```

### Install deb 
```
sudo dpkg -i linux-image-xxx.deb
```

### Check & boot into Target Kernel
reboot and keep pressing f4 to get into GRUB to select tartget kernel

## Troubleshooting
Follow the page: https://wiki.ubuntu.com/KernelTeam/GitKernelBuild
But fail to build by the command 
```
make -j $(getconf _NPROCESSORS_ONLN) deb-pkg
```
Error message
```
dpkg-source: error: LC_ALL=C patch -t -F 0 -N -p1 -u -V never -E -b -B .pc/diff.patch/ --reject-file=- < mainline-crack.orig.v3iR7o/debian/patches/diff.patch subprocess returned exit status 1
dpkg-buildpackage: error: dpkg-source --compression=gzip -b . subprocess returned exit status 2
```
