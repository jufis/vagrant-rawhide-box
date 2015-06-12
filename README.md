#Building with vagrant a Fedora Rawhide BOX

Quick example on how to build a rawhide linux to be used by vagrant on virtual box.

##Requirements

Please install the following packages to your local linux environment:

<pre>
yum install vagrant
yum install VirtualBox
</pre>

##Fetch distribution

We consider the latest fedora rawhide 64-bit boot image available on:

http://download.fedoraproject.org/pub/fedora/linux/development/rawhide/x86_64/os/images/boot.iso

##VirtualBox Distro Setup

Create a new VM on virtual box with the following properties:

1. Install with 2GB RAM
2. A single CPU is ok for now
3. Remove USB devices
4. Remove AUDIO devices
5. Enable clipboard bidirectional support from device's advanced settings
6. Insert boot image in the virtual cdrom device
7. Start VM
8. Install minimal fedora
9. Add user vagrant and make sure you check that this user acts like administrator as well
10. Finish install

##VirtualBox Distro Configuration

1. Reboot in rescue mode:
<pre>
Once in boot screen press *e* on the rescue selection
At the end of the kernel line put *rw init=/bin/bash*
CTRL+x to boot
</pre>
2. Change root user password to *vagrant*
<pre>
passwd root
</pre>
3. Change vagrant user password to *vagrant*
<pre>
passwd vagrant
</pre>
4. visudo -f /etc/sudoers.d/vagrant and put this to not allow passwd:
<pre>
vagrant ALL=(ALL) NOPASSWD:ALL
</pre>
5. Test sudo:
<pre>
sudo ls
</pre>
6. If it prompts you for passwd visudo and check that the wheel group requires nopasswd as well and retest.
7. vi /etc/sysconfig/selinux and set selinux to DISABLED.
8. Install vagrant ssl:
<pre>
mkdir -p /home/vagrant/.ssh
chmod 0700 /home/vagrant/.ssh
wget --no-check-certificate \
    https://raw.github.com/mitchellh/vagrant/master/keys/vagrant.pub \
    -O /home/vagrant/.ssh/authorized_keys
chmod 0600 /home/vagrant/.ssh/authorized_keys
chown -R vagrant /home/vagrant/.ssh
</pre>
9. /sbin/reboot -f
10. Check that you can login to the system.

##VirtualBox Distro Fine-Tunning

1. Install the following packages:

<pre>
yum install openssh-server
yum install docker
yum install ansible
yum install cowsay
yum install bind-utils
yum install kernel-devel
yum install bzip2
yum install dkms
yum install wget
yum install curl
yum install gcc
</pre>

2. vi /etc/ssh/sshd_config and put UseDns=NO

3. Install virtual box guest tools:
<pre>
From device menu -> Click *Insert Guest Additions CD Image*
mount /dev/cdrom <to_a_path>
cd <to_a_path>
sudo ./VBoxLinuxAdditions.run
</pre>

##VirtualBox Distro Packaging

1. Avoid defragmentation:

<pre>
sudo dd if=/dev/zero of=/EMPTY bs=1M
sudo rm -f /EMPTY
</pre>

2.  Change directory to the parrent dir that contains your virtual box image.

3. vagrant package --base <your_vm_folder_name>

4. That's it!

##References

http://www.virtualbox.org/manual/ch04.html

http://www.virtualbox.org/manual/ch02.html

https://fedoraproject.org/wiki/Releases/Rawhide

https://blog.engineyard.com/2014/building-a-vagrant-box

http://www.skoblenick.com/vagrant/creating-a-custom-box-from-scratch/

http://shop.oreilly.com/product/0636920026358.do
