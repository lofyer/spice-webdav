# UPDATE 2019-04：
1. Install and ENABLE spice-webdavd SERVICE in WINDOWS guest from https://www.spice-space.org/download/windows/spice-webdavd/
2. Download virt-viewer in client from http://virt-manager.org, "Run as Administrator" remote-viewer
3. Select folder to be shared.

# Guest libvirt xml
Add the following section in `<devices>...</devices>`:
```
<channel type='spiceport'>
        <source channel='org.spice-space.webdav.0'/>
        <target type='virtio' name='org.spice-space.webdav.0'/>
</channel>
```

## a) in Windows guest OS:
1. Disable Windows Firewall

2. As Administrator, Install spice-webdavd in Windows guest from https://www.spice-space.org/download/windows/spice-webdavd/, and enable spice-webdavd service from task manager.

3. If not as a service, please manually run `map-drive.bat` from Program Files/Spice webdav/

4. You MAY need re-compile the https://gitlab.gnome.org/GNOME/phodav via mingw or just change another version of spice-webdav.msi.

## b) in Linux guest OS:
It's similar to Windows guest, but you should build and run webdav-agent by yourself:
```
git clone phodav from upstream
yum -y install gnome-common  gcc gcc-c++ automake autoconf libtool
intltool gtk-doc glib2-devel libsoup-devel libxml-devel
./autogen.sh
make
make install
spice-webdavd -p 8000
```
## In your virt-viewer client
The Spice client can share a folder with the remote guest. By default folder sharing is disabled. Use the remote-viewer "File" → "Preferences" menu to enable it. The default shared directory is the XDG Public Share directory (ie ~/Public if you use a regular system). You may specify a different folder with --spice-share-dir client option.

# Reference:
http://www.spice-space.org/page/PlannedFeatures

http://www.spinics.net/lists/spice-devel/msg07812.html

http://lists.freedesktop.org/archives/spice-devel/2014-April/016582.html

https://elmarco.fedorapeople.org/
