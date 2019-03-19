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
## Command-Runner
Command Runner allows read-only level CLI commands to run from the DNA Center API.
```
ansible-playbook playbooks/command-runner/dnac-command-runner.yml
```
The sandbox only has a few devices so select from the options by pasting the deviceId to the prompt

```
TASK [DeviceId] ****************************************************************************************
ok: [localhost] => {
    "msg": [
        "cat_9k_1.marius.x-trem.ro - deviceId: 1a85db61-8bf2-4717-9060-9776f42e4581",
        "cs3850.marius.x-trem.ro - deviceId: 79d3a90b-1b95-4cd8-a9bd-6d5952814432"
    ]
}
Please enter the device ID: 1a85db61-8bf2-4717-9060-9776f42e4581
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





