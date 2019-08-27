# Creating a VM with a customization spec. 

It wasn’t very clear in the documentation how to achieve it. https://docs.ansible.com/ansible/latest/modules/vmware_guest_module.html

I downloaded the 2016 trial image of Windows 2016 and made a customization spec that would hard-set an IP address and some other data to test (attached). The base image has 40GB and I expand it to 50, as well as change the portgroup.

Note the version of Ansible. To use this feature you need to be on 2.8+ . You can upgrade Ansible with

[asm@localhost ~]$ ansible --version
ansible 2.5.1

sudo pip uninstall ansible
sudo pip install ansible

[asm@localhost ~]$ ansible --version
ansible 2.8.1


I had an issue that the NIC was always showing disconnected. This turned out to be that the customization spec would fail because I had ran sysprep on my template, and the customization already does that. From https://github.com/ansible/ansible/issues/45834

Note: vmware_guest doesn’t allow all customization of the VM hardware in the same play as the creation of the VM. I was able to set CPU and RAM and change the portgroup, but it would fail at the disk level. That’s why you have 2 plays (in theory it should all be the same play). The “Clone a virtual machine from Windows template” one is the one you want to check out first, that one is reliable at this point.
