# UPDATE 2017-07：
1. Install and enable spice-webdavd service in guests from https://www.spice-space.org/download/windows/spice-webdavd/
2. Download virt-viewer from http://virt-manager.org, "Run as Administrator" remote-viewer
3. Select folder to be shared.

# 参考 // reference:
http://www.spice-space.org/page/PlannedFeatures
http://www.spinics.net/lists/spice-devel/msg07812.html
http://lists.freedesktop.org/archives/spice-devel/2014-April/016582.html
https://elmarco.fedorapeople.org/

# 需要编译 // compile `spicec` for Windows

[0x05] Create Guest


## a) for windows guest (Depends):
```
$virt-install --connect qemu:///system -n vm_win7 -r 1024 --vcpus=1
--disk path=/var/lib/libvirt/images/vm_win7.img,size=15 -c ./win7.iso
--graphics spice,port=5930,listen=0.0.0.0 --noautoconsole --os-type
windows --accelerate --network=bridge:br0 --hvm
```

After installation:
```
$virsh destroy vm_win7
$virsh edit vm_win7
```

add the following section in `<devices>...</devices>`:
```
<channel type='spiceport'>
        <source channel='org.spice-space.webdav.0'/>
        <target type='virtio' name='org.spice-space.webdav.0'/>
</channel>
```

```
$spicy -h xxxx -p xxx --spice-shared-dir=/path/you/want/to/share
```
Install `http://elmarco.fedorapeople.org/spice-webdavd-x86-0.1.24.msi` for guest
And run `map-drive.bat` from Program Files/Spice webdav/


## b) for linux guest (Depends):
It's similar to windows guest, but you should build webdav-agent by yourself:
```
git clone phodav from upstream
$yum -y install gnome-common  gcc gcc-c++ automake autoconf libtool
intltool gtk-doc glib2-devel libsoup-devel libxml-devel
$./autogen.sh
$make
$make install
```
After these:
`$spice-webdavd -p 8000`
