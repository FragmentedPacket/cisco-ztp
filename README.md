# Cisco IOS-XE Guestshell ZTP Script

### Overview
This script is part of a workflow that involves Netbox for device variables, Ansible and Jinja2 to create the configuration using static and dynamic information.
Ansible saves the final file with the `serialnumber.cfg`. This helps to ensure that when this script is deployed that the correct configuration is used.

I'm using the management port and a DHCP server to be able to push the script to the device to be executed using the ZTP process on Cat9k's.
DHCP Server with option 67 and 150. Option 67 can be the only option specified if set to `http://fileserver/ztp.py`

### Variables
TFTP server defined in script
Image name and image MD5 are also defined

### Workflow - Upgrade Required
1. Obtain serial number from device
2. Set `config_file` variable to `serialnumber.cfg`
3. Checks to see if image exists on device
3a. If the file exists, verify MD5 hash
3b. If MD5 hash does not match, retransfer
4. If file does not exist, transfer file and verify MD5 hash
5. Deploy EEM script to upgrad device (install mode)
6. Device reboots
7. See Workflow - Upgrade NOT Required

### Workflow - Upgrade NOT Required
1. Obtain serial number from device
2. Set `config_file` variable to `serialnumber.cfg`
3. Checks to see if configuration exists on device
3a. If not, transfer file over
4. Find and remove any existing certs on device
5. Deploy configuration using EEM script (configure replace)

### Things To Note
Configure replace requires commands to be exact which can cause the configuration deployment to fail. Make sure all commands have been tested prior to deployment.