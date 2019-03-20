# dnacenter-ansible

## Background:
These example playbooks were developed to explore in https://sandboxdnac.cisco.com prior to official Ansible module availability for DNA Center. As of March 2019 these playbooks predate the official Cisco modules that are "work in progress" according to the BU.
Hense, these playbooks should become superseded by Cisco supported modules and not supported long term, or converted to roles. 

## Purpose: 
Use these playbooks to explore several ansible technigues to interact with DNA Center APIs with Ansible. These playbooks were written to interact with the devnet always on sandbox. The sandbox credentials are included in the var file. Please note, the playbooks are not intended for production networks, rather the upcoming supported modules are necessary for production. For lab usage these  playbooks are doable outside of the Devnet Sandbox but in some cases require modifications. 

## What you will find:
You will find several playbooks that configure DNA Center's APIs similar to the GUI. These playbook examples cover only a subset of available API feature capabilities. Due to parsing JSON for variables, some index items need to be adjusted or added to work outside the Devnet Sandbox.

## Issue with Sandbox
If someone has deleted the Sandbox devices, you can restore the connected devices from a backup. See the system settings. Select the ansiblebackup or if not available, select the baseline backup for restore.

<img width="1351" alt="Screenshot 2019-03-19 23 14 40" src="https://user-images.githubusercontent.com/11307137/54656683-01153a00-4a9d-11e9-8885-a6a529e79ba6.png">


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
Run this set of playbooks to add new deives and claim them in DNA Center from a .csv spreadsheet. Please note these devices are not connected so they're not contacted. If the devices were added later they would use a default workflow and template for the PNP configuration. The playbook code is adjustable to support additional workflow selections.

Step 1: Review https://sandboxdnac.cisco.com to make sure their are no existing devices added or claimed. 
user: devnetuser pass: Cisco123!

<img width="1337" alt="Screenshot 2019-03-19 22 18 54" src="https://user-images.githubusercontent.com/11307137/54654817-dc699400-4a95-11e9-8d9f-d4cf1df8ede7.png">

Step 2: Run this Playbook to remove any existing devices.
```
$ ansible-playbook playbooks/path-trace/dnac-path-trace.yml
```
It's normal to receive an error after playbook iterates through the last device during deletion. 

Step 3: Verify https://sandboxdnac.cisco.com that Network Plug and Play is empty.

<img width="1281" alt="Screenshot 2019-03-19 22 32 17" src="https://user-images.githubusercontent.com/11307137/54655096-f8216a00-4a96-11e9-95c1-5711e4c757ea.png">

Step 4: Review pnp.csv for list of devices. The current playbook supports adding ten devices. Modify code to increase rows. Also review the parse_csv.py file to understand how to import .csv file into Ansible using JSON.

<img width="265" alt="Screenshot 2019-03-19 22 37 42" src="https://user-images.githubusercontent.com/11307137/54655271-afb67c00-4a97-11e9-838f-4e901cfbbba1.png">

Step 5: Run the following playbook to add new devices from the .csv spreadsheet and claim them in DNA Center.
```
$ ansible-playbook playbooks/pnp/dnac-pnp.yml

TASK [PNP Device Import] ************************************************************
ok: [localhost] => (item={'pnp_name': 'sw1', 'pnp_pid': 'ws-c9300', 'pnp_sn': 'A1234567890'})
ok: [localhost] => (item={'pnp_name': 'sw2', 'pnp_pid': 'ws-c9300', 'pnp_sn': 'B1234567890'})
ok: [localhost] => (item={'pnp_name': 'sw3', 'pnp_pid': 'ws-c9300', 'pnp_sn': 'C1234567890'})
ok: [localhost] => (item={'pnp_name': 'sw4', 'pnp_pid': 'ws-c9300', 'pnp_sn': 'D1234567890'})
ok: [localhost] => (item={'pnp_name': 'sw5', 'pnp_pid': 'ws-c9300', 'pnp_sn': 'E1234567890'})
ok: [localhost] => (item={'pnp_name': 'sw6', 'pnp_pid': 'ws-c9300', 'pnp_sn': 'F1234567890'})
ok: [localhost] => (item={'pnp_name': 'sw7', 'pnp_pid': 'ws-c9300', 'pnp_sn': 'G1234567890'})
ok: [localhost] => (item={'pnp_name': 'sw8', 'pnp_pid': 'ws-c9300', 'pnp_sn': 'H1234567890'})
ok: [localhost] => (item={'pnp_name': 'sw9', 'pnp_pid': 'ws-c9300', 'pnp_sn': 'I1234567890'})
ok: [localhost] => (item={'pnp_name': 'sw10', 'pnp_pid': 'ws-c9300', 'pnp_sn': 'J1234567890'})
```
Step 6: Verify the devices were added and claimed on https://sandboxdnac.cisco.com

<img width="1337" alt="Screenshot 2019-03-19 22 18 54" src="https://user-images.githubusercontent.com/11307137/54654817-dc699400-4a95-11e9-8d9f-d4cf1df8ede7.png">

## SWIM 
The following playbook is used to distirbute image updates to existing devices. This play book is set to work with the Cat 9K golden image. You will find that everything works in DNA Center but ultimately fails becuase of a configuration issue and lack of SFTP server for the Devnet Sanbox. Other playbooks ideas for uploading new images from your laprop using the API as well as activating the images on devices might be added later.

```
$ ansible-playbook playbooks/swim/dnac-swim-image-distribution.yml

TASK [SWIM Details Physical] ********************************************************
ok: [localhost] => {
    "msg": [
        "deviceId: []",
        "imageId: 9f0c005b-33d2-4e07-9227-90ce79ef06ff",
        "name: cat9k_iosxe.16.06.04a.SPA.bin ",
        "deviceFamily: Cisco Catalyst 9300 Switch ",
        "View the entire JSON response at dnac-swim-physical-images-all.json"
 ```
 check the device update status at https://sandboxdnac.cisco.com 
 






