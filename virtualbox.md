# VirtualBox #

## Create a new VM via Command line ##

	VBoxManage createvm --name "<name>" --ostype <ostype> --register
	VBoxManage modifyvm "<name>" --memory 512 --acpi on --boot1 dvd --nic1 nat
	VBoxManage createhd --filename "<filename>.vdi" --size 10240
	VBoxManage storagectl "<name>" --name "IDE Controller" --add ide --controller PIIX4
	VBoxManage storageattach "<name>" --storagectl "IDE Controller" \
	      --port 0 --device 0 --type hdd --medium "<filename>.vdi"
	VBoxManage storageattach "<name>" --storagectl "IDE Controller"
	      --port 0 --device 1 --type dvddrive --medium /full/path/to/iso.iso
	VBoxHeadless --startvm "<name>"

## Managment via console ##

List virtual machines

	VBoxManage list vms

List available os types

	VBoxManage list ostypes
	
List port forwardings

	VBoxManage getextradata "<name>" enumerate | grep VBoxInternal 
	
Get ip adress from guest

	VBoxManage guestproperty enumerate <name> | grep IP

## Headless ##

To run a virtual machine headless

	VBoxHeadless --startvm <uuid|name>

To also disable VRDP (VirtualBox Remote Desktop Protocol) and access the machine via `ssh` only

	VBoxHeadless --startvm <uuid|name> --vrde=off
	
## SSH into a VirtualBox ##

- install OpenSSH-Server on the guest system
- leave network mode on default `NAT`
- create a port forwarding for port 22 (see [PortForwarding][])

## Port forwarding [PortForwarding] ##

- `<vmName>` the name of your virtual machine
- `<serviceName>` arbitrary but unique name for the service you want to setup port forwarding
- `<cardname>` name of virtual network adapter - `pcnet` for `pcnet-FAST III`, `e1000` for `Intel PRO/1000 MT Desktop`

For instance to setup a port forwarding for a ssh port

	VBoxManage setextradata "<vmName>" "VBoxInternal/Devices/<cardname>/0/LUN#0/Config/<serviceName>/Protocol" TCP
	VBoxManage setextradata "<vmName>" "VBoxInternal/Devices/<cardname>/0/LUN#0/Config/<serviceName>/GuestPort" 22
	VBoxManage setextradata "<vmName>" "VBoxInternal/Devices/<cardname>/0/LUN#0/Config/<serviceName>/HostPort" 2222

To remove a port forwarding use	the same commands without values

	VBoxManage setextradata "<vmName>" "VBoxInternal/Devices/<cardname>/0/LUN#0/Config/<serviceName>/Protocol"
	VBoxManage setextradata "<vmName>" "VBoxInternal/Devices/<cardname>/0/LUN#0/Config/<serviceName>/GuestPort"
	VBoxManage setextradata "<vmName>" "VBoxInternal/Devices/<cardname>/0/LUN#0/Config/<serviceName>/HostPort"