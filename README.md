# check_synology
> Independant Nagios plugin to monitor Synology NAS

1. Get the Synology official MIB files and put them in the mibs SNMP folder on your Nagios server
```bash
# Download link for MIB
wget https://global.download.synology.com/download/Document/Software/DeveloperGuide/Firmware/DSM/All/enu/Synology_MIB_File.zip
unzip Synology_MIB_File.zip
sudo mv *.txt /usr/share/snmp/mibs/
```
2. Put the `check_synology` plugin in the folder libexec (/usr/local/nagios/libexec) of your Nagios installation and chown it to nagios:nagios
```bash
sudo chown nagios:nagios check_synology
```
3. Give execution permission on the plugin
```bash
sudo chmod +x check_synology
```

## Usage

```text
Usage: ./check_synology -H <SNMP_HOST> [-p {161|Other}] -su <SNMP_USERNAME> -sap <SNMP_AUTH_PASSPHRASE> [-sapr {SHA|MD5}
-spp <SNMP_PRIVACY_PASSPHRASE> [-sppr {AES|DES}] -e <KEYWORD>
```

You have also the possibility to fix some parameters, by default SNMP port is fixed to the default one (161), SNMP auth protocol is fixed to SHA (most secured option) and SNMP privacy protocol is fixed to AES (most secured option).

If you use the same parameters, you don't have to specify these arguments in your commmands.

You can edit the plugin to fix these variables if you prefer:

```bash
# Default values
SNMP_HOST=""
SNMP_PORT="161"
SNMP_USERNAME=""
SNMP_AUTH_PASSPHRASE=""
SNMP_AUTH_PROTOCOL="SHA"
SNMP_PRIVACY_PASSPHRASE=""
SNMP_PRIVACY_PROTOCOL="AES"
```

### Supported keywords

The `-e` argument supports these keywords:

|Keyword|Information|
|-|-|
|systemStatus|System partition status|
|temperature|Temperature of the NAS|
|powerStatus|Returns error if power supplies fail|
|systemFanStatus|Returns error if system fan fails|
|cpuFanStatus|Returns error if CPU fan fails|
|modelName|Model name of the NAS|
|serialNumber|Serial number of the NAS|
|version|DSM version|
|upgradeAvailable|Update availability|
|diskID[0-255]|Disk name from DSM|
|diskModel[0-255]|Disk model|
|diskType[0-255]|Disk type|
|diskStatus[0-255]|Current disk status|
|diskTemperature[0-255]|Disk temperature|
|diskRole[0-255]|Disk role in the system|
|diskRetry[0-255]|Connection retries of the disk|
|diskBadSector[0-255]|Bad sectors count of the disk|
|diskIdentifyFail[0-255]|Identification failed for the disk|
|diskRemainLife[0-255]|Estimate remain life of the disk|
|diskName[0-255]|Name of the disk|
|diskHealthStatus[0-255]|Current disk health status|
|raidName[0-255]|RAID name in DSM|
|raidStatus[0-255]|Current RAID status|
|raidFreeSize[0-255]|RAID free size still available|
|raidTotalSize[0-255]|RAID total size|
|raidHotspareCnt[0-255]|Total of hotspare disks which can protected RAID|
|upsDeviceModel|UPS model|
|upsDeviceManufacturer|UPS manufacturer|
|upsDeviceSerial|UPS serial number|
|upsInfoStatus|Current UPS status|
|upsInfoMfrDate|UPS manufacturing date|
|upsInfoLoadValue|Current load of the UPS|
|upsBatteryChargeValue|Battery charge of the UPS|
|upsBatteryChargeWarning|UPS battery where it switches to warning state|
|serviceNameCIFS|CIFS service name (to use combined associated serviceUsers)|
|serviceNameAFP|AFP service name (to use combined associated serviceUsers)|
|serviceNameNFS|NFS service name (to use combined associated serviceUsers)|
|serviceNameFTP|FTP service name (to use combined associated serviceUsers)|
|serviceNameSFTP|SFTP service name (to use combined associated serviceUsers)|
|serviceNameHTTP_HTTPS|HTTP/HTTPS service name (to use combined associated serviceUsers)|
|serviceNameTELNET|TELNET service name (to use combined associated serviceUsers)|
|serviceNameSSH|SSH service name (to use combined associated serviceUsers)|
|serviceNameOTHER|OTHER service name (to use combined associated serviceUsers)|
|serviceUsersCIFS|Give the number of users using CIFS|
|serviceUsersAFP|Give the number of users using AFP|
|serviceUsersFTP|Give the number of users using FTP|
|serviceUsersSFTP|Give the number of users using SFTP|
|serviceUsersHTTP_HTTPS|Give the number of users using HTTP/HTTPS|
|serviceUsersTELNET|Give the number of users using TELNET|
|serviceUsersSSH|Give the number of users using SSH|
|serviceUsersOTHER|Give the number of users using other services|
|serviceUsers|Give the number of users using the first default service|
> For options with `[0-255]`, it means you can use a number after the keyword. If you use the keyword without any number, it will give you the information of the first disk or the first RAID (So it's OK for  a one bay NAS), if you have a NAS with 2 disks or more or if you have 2 RAIDs or more, you have to add 0 (for the first), 1 (for the second), etc...
> Example : `-e diskID0` will give you the ID of the first disk, `-e diskID1` will give the ID of the second disk, etc... Same for RAIDs. So you can monitor up to 256 disks and 256 RAIDs if needed.

For each keyword, the `OK|WARNING|CRITICAL` logic is applied and you will receive the right status for each situation. Informative keywords are always `OK` by default with their text status (example : modelName).

Technical keywords (like `raidStatus` for exemple) will give you the right status (`OK|WARNING|CRITICAL`) and corresponding text information for each situtation.

### Screenshots
> Sample of some configured controls

![alt text](https://github.com/John4887/check_synology/blob/main/check_synology_sample.png)
