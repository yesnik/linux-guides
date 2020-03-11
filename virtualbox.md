# VirtualBox

## Mount CD-Rom with GuestAdditions

```bash
sudo mkdir /media/cdrom
sudo mount /dev/cdrom /media/cdrom
```

## Add existing .vdi disk to Linux

See more info in the [article](https://www.vitalsofttech.com/add-disk-storage-to-oracle-virtualbox-with-linux/).

Let's suppose that we have VirtualBox image with Fedora. And we want to attach to this system existing .vdi disk. 

Steps:

1. In the Fedora VirtualBox settings add new SATA .vdi disk

2. Load Fedora operating system

3. Get list of hard drives: 

```bash
sudo fdisk -l
```
You'll see the attached *.vdi* disk at `/dev/sdb`

4. Use the following command to manipulate this disk's partition table:

```bash
fdisk /dev/sdb
```
```
Enter commands:
n
p
w
```

5. If we want to mount `.vdi` under `/mydisk`.

```bash
sudo mkdir -p /mydisk
sudo mount /dev/sdb1 /mydisk
```
