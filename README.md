# dnacenter-ansible

## Background:
Please see these example playbooks to explore in https://sandboxdnac.cisco.com prior to official Ansible module availability for DNA Center. As of March 2019 these playbooks predate the official modules that are "work in progress" according to the BU.
Hense, these playbooks will not be supportrd as roles nor lifecycle managed, as they will be obsolete. 

## Purpose: 
Use these playbooks to explore several ansible technigues to interact with DNA Center APIs with Ansible. These playbooks were written to interact with the devnet always on sandbox. The sandbox credentials are included in the host file. Please note that the playbooks are not intended for production networks. For lab usage the playbooks will need some modification.

## What you will find:
You will find several playbooks that configure DNA Center's APIs similar to the GUI. These playbook examples cover only a subset of available API feature capabilities. Due to parsing JSON for variables, some index items need to be adjusted or added to work outside the Devnet Sandbox.

## Getting Starte:

Install Ansible 2.7 on a MAC or Linux
'''
pip install ansible
'''






