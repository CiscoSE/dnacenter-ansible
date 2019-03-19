# dnacenter-ansible
Please see these example playbooks to explore in https://sandboxdnac.cisco.com prior to official Ansible module availability for DNA Center. As of March 2019 these playbooks predate the official modules that are a work in progress according to the BU.
Hense these playbooks will not be supportrd as roles or lifecycle managed as they will become obsolete. 

Purpose: 
Use these playbooks to explore several ansible technigues to interact with DNA Center APIs. The playbooks were written to interact with the devnet always on sandbox with credentials included in the host file. Please note that the playbooks are not intended for production networks and will need to be modified in some cases when ran against your own lab environments. 

What you will see:
You will find several playbooks that configure the DNA Center APIs similar to the GUI. These playbooks are only examples that cover only a subset of available API feature capabilities. Due to parsing JSON and other technigues to provide variables to the playbooks, several index items need to be adjusted or added to work outside of the Devnet Always on Sandbox.





