# Cisco IOS-XE Guestshell ZTP Script for Catalyst 9000 Series Switches

## Overview

This script will be pushed to the device and then executed on-box. There are static variables set within the script that will need to be adjusted to match your environment.

## Requirements

1. Catalyst 9000 series switch
2. Management port
3. DHCP server
   * Option 67 (only) if http/https server is available, ex. `http://fileserver/ztp.py`
   * Option 150 specifies TFTP server
   * Option 67 specifies the name of the file `ztp.py`

## Variables

* TFTP Server
* Image name
* Image MD5
* File system
  * Default of `bootflash:/`
* Config file
  * Uses serial number obtained from the device

## Workflow - Upgrade Required

1. Obtain serial number from device
2. Set `config_file` variable to `serialnumber.cfg`
3. Checks to see if image exists on device
   * If the file exists, verify MD5 hash
   * If MD5 hash does not match, retransfer
4. If file does not exist, transfer file and verify MD5 hash
5. Deploy EEM script to upgrade device (install mode)
6. Device reboots
7. See **Workflow - Upgrade NOT Required**

## Workflow - Upgrade NOT Required

1. Obtain serial number from device
2. Set `config_file` variable to `serialnumber.cfg`
3. Checks to see if configuration exists on device
   * If not, transfer file over
4. Find and remove any existing certs on device
5. Deploy configuration using EEM script (configure replace)

## Things To Note

Configure replace requires commands to be exact which can cause the configuration deployment to fail. Make sure all commands have been tested prior to deployment.
