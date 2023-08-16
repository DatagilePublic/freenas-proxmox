# TrueNAS ZFS over iSCSI interface  [![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=TCLNEMBUYQUXN&source=url)

## 📢: ATTENTION 2023-08-16 📢: New repos are now online at Cloudsmith.

### Updates 2023-02-12<br/>  - Added `systemctl restart pvescheduler.service` command to the package.
#### Roadmap
* Fix automated builds.
  * Production - 'main' repo component.
* Package the patches with the deb package.
  * Remove the need for the git dependency.
* Change to LWP::UserAgent
  * Remove depenancy of the REST::Client because LWP::UserAgent is already installed and used by Proxmox VE.
* Change from FreeNAS to TrueNAS.
  * Cleanup the FreeNAS repo and name everything to TrueNAS to be inline with the product.
* Add API key for direct TrueNAS services.
  * Will be a new enable field and API key and will only be used by the plugin.
  * You will still need the SSH keys, username, and password because of Proxmox VE using `iscsiadm` to get the list of disks.
    * This is tricky because the format needs to be that of the output of 'zfs list' which is not part of the LunCmd but that of the backend Proxmox VE system and the API's do a bunch of JSON stuff.

## Thank you for all that have recently donated to the project - Updated 2022-06-04
    Alexander Finkhäuser - Recurring
    Bjarte Kvamme - Recurring
    Jonathan Schober - Recurring
    Frederic Silvi
    Security Camera
    Vincent Cui
    Jakub Jochec
    
    Mark Komarinski
    Jesse Bryan
    Maksym Vasylenko
    Daniel Most
    Velocity Host
    Robert Hancock
    Clevvi Technology
    Mark Elkins
    Marc Hodler
    Martin Gonzalez

### 'truenas-proxmox' repo (Follows a release branch - Current 2.x) Currently unavailable.
Will be production ready code that has been tested (as best as possible) from the 'testing' repo.

### 'truenas-proxmox-testing' repo (Follows the master branch)
Will be 'beta' code for features, bugs, and updates.

## New Installs - truenas-proxmox.

### For stable releases, issue the following from a command line:
```bash
keyring_location=/usr/share/keyrings/ksatechnologies-truenas-proxmox-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/ksatechnologies/truenas-proxmox/gpg.284C106104A8CE6D.key' |  gpg --dearmor >> ${keyring_location}
```

Copy the following to /etc/apt/sources.list.d/ksatechnologies-repo.list
```bash
# Source: KSATechnologies
# Site: https://cloudsmith.io
# Repository: KSATechnologies / truenas-proxmox
# Description: TrueNAS plugin for Proxmox VE - Production
deb [signed-by=/usr/share/keyrings/ksatechnologies-truenas-proxmox-keyring.gpg] https://dl.cloudsmith.io/public/ksatechnologies/truenas-proxmox/deb/debian any-version main
```

### For development releases, issue the following from a command line:
```bash
keyring_location=/usr/share/keyrings/ksatechnologies-truenas-proxmox-testing-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/ksatechnologies/truenas-proxmox-testing/gpg.CACC9EE03F2DFFCC.key' |  gpg --dearmor >> ${keyring_location}
```

Copy the following to /etc/apt/sources.list.d/ksatechnologies-repo.list
```bash
# Source: KSATechnologies
# Site: https://cloudsmith.io
# Repository: KSATechnologies / truenas-proxmox-testing
# Description: TrueNAS plugin for Proxmox VE - Testing
#deb [signed-by=/usr/share/keyrings/ksatechnologies-truenas-proxmox-testing-keyring.gpg] https://dl.cloudsmith.io/public/ksatechnologies/truenas-proxmox-testing/deb/debian any-version main
```
Then enable it from the Proxmox UI if you wish to use it.

### Update apt
Then issue the following to install the package
```bash
apt update
apt install freenas-proxmox
```

Before freenas-proxmox-2.2.0-0-beta8 please issue the following:
```
systemctl restart pvescheduler.service
```
due to this post https://github.com/TheGrandWazoo/freenas-proxmox/issues/109#issuecomment-1367527917

Then just do your regular upgrade via apt at the command line or the Proxmox Update subsystem; the package will automatically issue all commands to patch the files.
```bash
apt update
apt [full|dist]-upgrade
```

If you wish not to use the package you may remove it at anytime with
```
apt [remove|purge] freenas-proxmox
```
This will place you back to a normal and non-patched Proxmox VE install.

#### NOTE: Please be aware that this plugin uses the TrueNAS APIs but still uses SSH keys due to the underlying Proxmox VE perl modules that use the ```iscsiadm``` command.

You will still need to configure the SSH connector for listing the ZFS Pools because this is currently being done in a Proxmox module (ZFSPoolPlugin.pm). To configure this please follow the steps at https://pve.proxmox.com/wiki/Storage:_ZFS_over_iSCSI that have to do with SSH between Proxmox VE and TrueNAS. The code segment should start out `mkdir /etc/pve/priv/zfs`.

1. Remember to follow the instructions mentioned above for the SSH keys.

2. Refresh the Proxmox GUI in your browser to load the new Javascript code.

3. Add your new TrueNAS ZFS-over-iSCSI storage using the TrueNAS-API.

4. Thanks for your support.
