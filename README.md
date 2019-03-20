# dnacenter-ansible

## Background:
Please see these example playbooks to explore in https://sandboxdnac.cisco.com prior to official Ansible module availability for DNA Center. As of March 2019 these playbooks predate the official modules that are "work in progress" according to the BU.
Hense, these playbooks will not be supportrd as roles nor lifecycle managed, as they will be obsolete. 

## Purpose: 
Use these playbooks to explore several ansible technigues to interact with DNA Center APIs with Ansible. These playbooks were written to interact with the devnet always on sandbox. The sandbox credentials are included in the host file. Please note that the playbooks are not intended for production networks. For lab usage the playbooks will need some modification.

## What you will find:
You will find several playbooks that configure DNA Center's APIs similar to the GUI. These playbook examples cover only a subset of available API feature capabilities. Due to parsing JSON for variables, some index items need to be adjusted or added to work outside the Devnet Sandbox.

## Getting Started:

Install Ansible 2.7 on a MAC or Linux
```
pip install ansible
```
On windows folow this link: http://www.oznetnerd.com/installing-ansible-windows/

Clone this repository:
```
$ git clone https://github.com/CiscoSE/dnacenter-ansible.git
$ cd dnacenter-ansible.git

```
## Device-inventory
Run this playbook to get JSON output for a list of available devices and IP address information. Please note the Sandbox ASR1000 doesn't have enbled password activated.

```
$ ansible-playbook playbooks/device-inventory/dnac-inventory.yml

TASK [Inventory Details] ************************************************************
ok: [localhost] => {
    "msg": [
        {
        
 {
            "apManagerInterfaceIp": "",
            "associatedWlcIp": "",
            "bootDateTime": "2018-10-16 10:37:51",
            "collectionInterval": "Global Default",
            "collectionStatus": "Managed",
            "errorCode": null,
            "errorDescription": null,
            "family": "Switches and Hubs",
            "hostname": "cat_9k_1.marius.x-trem.ro",
            "id": "1a85db61-8bf2-4717-9060-9776f42e4581",
            "instanceTenantId": "5bd3634ab2bea0004c3ebb58",
            "instanceUuid": "1a85db61-8bf2-4717-9060-9776f42e4581",
            "interfaceCount": "41",
            "inventoryStatusDetail": "<status><general code=\"FAILED_FEAT\"/><topCause code=\"UNKNOWN\"/>\n</status>",
            "lastUpdateTime": 1553038763443,
            "lastUpdated": "2019-03-19 23:39:23",
            "lineCardCount": "2",
            "lineCardId": "df065d20-8d9b-4b66-a5ed-30aab545b85b, 766f14fe-8bb6-4ac7-a58c-f456f7e2ab34",
            "location": null,
            "locationName": null,
            "macAddress": "f8:7b:20:67:62:80",
            "managementIpAddress": "10.10.22.66",
```

## Command-Runner
Command Runner allows read-only level CLI commands to run from the DNA Center API.
```
$ ansible-playbook playbooks/command-runner/dnac-command-runner.yml
```
The Devnetsandbox only has a few devices so select from the two options by pasting the deviceId into the prompt. Please review the Playbook/YAML code to learn how prompts provide an option for input of variables.

```
TASK [DeviceId] ****************************************************************************************
ok: [localhost] => {
    "msg": [
        "cat_9k_1.marius.x-trem.ro - deviceId: 1a85db61-8bf2-4717-9060-9776f42e4581",
        "cs3850.marius.x-trem.ro - deviceId: 79d3a90b-1b95-4cd8-a9bd-6d5952814432"
    ]
}
Please enter the device ID: 1a85db61-8bf2-4717-9060-9776f42e4581
```
Enter any read level command. I.e, Show commands , ping etc
```
Please enter CLI cmd: sh ip int br

TASK [Print Results] ***********************************************************************************
ok: [localhost] => {
    "msg": [
        "\"[{  \\\"deviceUuid\\\":\\\"1a85db61-8bf2-4717-9060-9776f42e4581\\\" 
        ,\\\"commandResponses\\\":{\\\"SUCCESS\\\":{\\\"sh ip int br\\\":\\\"sh ip int br\\\\nInterface 
        IP-Address      OK? Method Status                Protocol\\\\nVlan1                  10.10.22.97    
        YES NVRAM  up                    up     
        \\\\nGigabitEthernet0/0     unassigned      YES NVRAM  administratively down down    \\\\nTe1/0
```
Adjust your terminal width to make more readable.

## Path-trace:

Run this playbook to glean the path between two devices in DNA Center. Although the Sandbox is a small, in a larger environment this feature provides multiple hops and other path detailsbeyond CDP etc.

```
$ ansible-playbook playbooks/path-trace/dnac-path-trace.yml

TASK [Inventory Details] ************************************************************
ok: [localhost] => {
    "msg": [
        "cat_9k_1.marius.x-trem.ro 10.10.22.66",
        "cs3850.marius.x-trem.ro 10.10.22.73"
    ]
}

TASK [Inventory Details to File] ****************************************************
changed: [localhost -> localhost]
Please enter source IP address for path trace : 10.10.22.66
Please enter destination IP address for path trace : 10.10.22.73

 "msg": [
        {
            "lastUpdate": "Wed Mar 20 02:10:28 UTC 2019",
            "networkElementsInfo": [
                {
                    "egressInterface": {
                        "physicalInterface": {
                            "id": "8352aa72-4851-4023-a765-d07004f6e524",
                            "name": "TenGigabitEthernet1/1/1",
                            "usedVlan": "NA"
                        }
                    },
                    "id": "1a85db61-8bf2-4717-9060-9776f42e4581",
                    "ip": "10.10.22.66",
                    "linkInformationSource": "OSPF",
                    "name": "cat_9k_1.marius.x-trem.ro",
                    "role": "ACCESS",
                    "type": "Switches and Hubs"
```
### Truncated

## Network Plug and Play
Run this set of playbooks to add new deives and claim them in DNA Center from a .csv spreadsheet. Please note these devices are not connected so they aren't active.

Step 1: Review https://sandboxdnac.cisco.com to make sure their are no existing devices added or claimed. 
user: devnetuser pass: Cisco123!

<img width="1337" alt="Screenshot 2019-03-19 22 18 54" src="https://user-images.githubusercontent.com/11307137/54654817-dc699400-4a95-11e9-8d9f-d4cf1df8ede7.png">


