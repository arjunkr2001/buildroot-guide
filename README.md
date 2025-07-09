# buildroot-guide
making a linux distro with buildroot

```bash
sudo apt update
sudo apt install g++ make libncurses-dev unzip bc bzip2 libelf-dev libssl-dev extlinux

wget https://buildroot.org/downloads/buildroot-2025.02.4.tar.gz

tar xf buildroot-2025.02.4.tar.gz

rm *.gz

cd buildroot-2025.02.4

make menuconfig

  Target ptions -> Target Architecture: x86_64
  Kernel -> Linux Kernel -> Kernel configurations: Use the architecture default configuration
  System configuration -> Init system: BusyBox
  Save

make

<..wait for build to complete..>

cd output/images

mkdir distro
mv bzImage rootfs.tar distro/
cd distro

tar xf rootfs.tar
rm rootfs.tar

cd ..

truncate -s 100MB boot.img

mkfs boot.img

sudo mount boot.img /mnt/

sudo extlinux --install /mnt/

sudo cp -r distro/* /mnt/

sudo umount /mnt/

############################################################

if no GTK:

  qemu-system-x86_64 -hda boot.img -nographic

  /bzImage root=/dev/sda console=ttyS0

else:

  qemu-system-x86_64 -hda boot.img

  /bzImage root=/dev/sda
```
