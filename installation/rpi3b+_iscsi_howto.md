# RPi 3B+ iscsi how-to

Inspiration from [hiroom2.com](https://www.hiroom2.com/2017/07/11/debian-9-tgt-en/).
Kudos!

## Foreword 

These steps are meant to be completed on a Linux computer that serves as a 
netboot server of the MicroMEC cluster. The steps can of course be adopted for 
other use cases. 

* Our netboot server (ie. iscsi target server) is called `bootserv`. 

* The netboot server is available on the LAN and can be pinged using the name `bootserv`. 

* We refer to the RPi 3B+ with an id: `07f32691` and a name: `rpi3-1`.

* We use sudo and start the preparations in /tmp on the netboot server. 

__Security Note__

The setup is meant for a private lab. Security considerations for exposing the 
rootfs via the LAN are not discussed in this how-to as of now. 

### Prerequisites

* A computer running Debian Linux. Any Linux flavor can be used, but
our instructions refer to Debian.

* 5GB free space on the hard drive. 

* Installed packages: tgt, open-iscsi.

* Fast LAN (preferably Gigabit Ethernet).

### Steps

1. Download a ready made rootfs from the [Open Build Service](https://build.opensuse.org/).

    We will use openSUSE Tumbleweed on our RPi 3B+:

    * [the latest openSUSE-Tumbleweed-ARM-JeOS.aarch64-rootfs tar.xz file](https://download.opensuse.org/ports/aarch64/tumbleweed/images)

2. Extract the downloaded rootfs

        $ cd /tmp

        $ mkdir openSUSE_Tumbleweed

        $ xz -d /tmp/openSUSE-Tumbleweed-ARM-JeOS.aarch64-rootfs.aarch64-2020.05.10-Snapshot20200512.tar.xz

        $ cd openSUSE_Tumbleweed

        $ tar xvf /tmp/openSUSE-Tumbleweed-ARM-JeOS.aarch64-rootfs.aarch64-2020.05.10-Snapshot20200512.tar.xz


3. Create a virtual loop back device that will hold the rootfs:

        $ cd /tmp

        $ dd if=/dev/zero of=07f32691-opensuse-rootfs.img bs=400M count=10

        $ sudo losetup -fP 07f32691-opensuse-rootfs.img

        $ sudo mkfs.ext4 07f32691-opensuse-rootfs.img

4. Check which loopback devices are allocated by the kernel:

        $ losetup -a
        /dev/loop0: []: (/tmp/07f32691-opensuse-rootfs.img)

5. Mount the virtual block device

        $ mkdir 07f32691-rootfs-mount
        
        $ sudo mount -o loop /dev/loop0 07f32691-rootfs-mount

6. Copy the content of an existing rootfs image to the mounted block device

        $ cp -R /tmp/downloaded_rootfs/*  07f32691-rootfs-mount/

7. Unmount the file and move it to the place where it will be served. 

        $ sudo umount 07f32691-rootfs-mount

        $ sudo losetup -D

        $ sudo mkdir /srv/iscsi

        $ sudo mv 07f32691-opensuse-rootfs.img /srv/iscsi

8. Prepare the iscsi target and publish it

        $ sudo tgtadm --lld iscsi --op new --mode target --tid 1 -T iqn.org.micromec:rpi3-1-opensuse-rootfs

        $ sudo tgtadm --lld iscsi --op new --mode logicalunit --tid 1 --lun 1 -b /srv/iscsi/07f32691-opensuse-rootfs.img

        $ sudo tgtadm --lld iscsi --op bind --mode target --tid 1 -I ALL

    At this point the rootfs is available on the local network.

9. Save the configuration on the netboot server to remain persistent

        $ sudo tgt-admin --dump | tee  /etc/tgt/conf.d/micromec-cluster.conf

### Local Testing

1. Check the iscsi target locally

    Discover the iscsi target 
    
        $ sudo iscsiadm --mode discovery --op update --type sendtargets --portal localhost
        127.0.0.1:3260,1 iqn.org.micromec:rpi3-1-opensuse-rootfs

    Login to the iscsi target

        $ sudo iscsiadm -m node --targetname iqn.org.micromec:rpi3-1-opensuse-rootfs  -p localhost -l
        Logging in to [iface: default, target: iqn.org.micromec:rpi3-1-opensuse-rootfs, portal: 127.0.0.1,3260] (multiple)
        Login to [iface: default, target: iqn.org.micromec:rpi3-1-opensuse-rootfs, portal: 127.0.0.1,3260] successful.

    Check if a new partition appears in the list:

        $ cat /proc/partitions
        major minor  #blocks  name
        .....
           8       32    4096000 sdc

    Mount the partition to a mount point

        $ mkdir /tmp/test-rootfs

        $ sudo mount /dev/sdc /tmp/test-rootfs

        $ ls -alrt /tmp/test-rootfs

    Umount the partition
    
        $ sudo umount /tmp/test-rootfs
        
    Logout from all iscsi targets
    
        $ sudo iscsiadm -m node -U all
        

### Remote Testing

1. Check the iscsi target remotely

    Login to an other Linux computer which also has the open-iscsi tools installed.

    Discover the iscsi target 
    
        $ sudo iscsiadm --mode discovery --op update --type sendtargets --portal bootserv
        192.168.4.1:3260,1 iqn.org.micromec:rpi3-1-opensuse-rootfs
    
    Login to the iscsi target
    
        $ sudo iscsiadm -m node --targetname iqn.org.micromec:rpi3-1-opensuse-rootfs -p bootserv -l
        Logging in to [iface: default, target: iqn.org.micromec:rpi3-1-opensuse-rootfs, portal: 192.168.4.1,3260]
        Login to [iface: default, target: iqn.org.micromec:rpi3-1-opensuse-rootfs, portal: 192.168.4.1,3260] successful.
        
    Check the available partitions
    
        $ cat /proc/partitions
        major minor  #blocks  name
        .....
           8       16    4096000 sdb

    Mount the partition to a mount point

        $ mkdir /tmp/test-rootfs

        $ sudo mount /dev/sdb /tmp/test-rootfs

        $ ls -alrt /tmp/test-rootfs

    Umount the partition

        $ sudo umount /tmp/test-rootfs

    Logout from all iscsi targets
    
        $ sudo iscsiadm -m node -U all
  
