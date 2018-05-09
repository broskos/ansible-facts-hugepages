# Add custom fact to gather hugepages info from with ansible

Operators may want to review hugepages utilization from time to time to ensure they have sufficient capacity.

Ansible facts don't currently show hugepages information.

This playbook shows how you can create a custom fact to gather hugepages information from your computes.

Once the custom fact is in place on your computes any playbook can use it.

## Example
The get_hugepages.yaml example shows how to distribute the custom fact and use it in a playbook.

```commandline
[stack@pod4-undercloud ~]$ ansible-playbook ansible-facts-hugepages/get_hugepages.yaml 

PLAY [compute] *********************************************************************************************************

TASK [ensure custom facts directory exists] ****************************************************************************
ok: [overcloud-compute-1]
ok: [overcloud-compute-0]

TASK [install custom fact module for hugepages] ************************************************************************
ok: [overcloud-compute-1]
ok: [overcloud-compute-0]

PLAY [compute] *********************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [overcloud-compute-1]
ok: [overcloud-compute-0]

TASK [output our custom facts] *****************************************************************************************
ok: [overcloud-compute-1] => {
    "ansible_local.hugepages": {
        "HugePages_Free": "7", 
        "HugePages_Rsvd": "0", 
        "HugePages_Surp": "0", 
        "HugePages_Total": "32"
    }
}
ok: [overcloud-compute-0] => {
    "ansible_local.hugepages": {
        "HugePages_Free": "13", 
        "HugePages_Rsvd": "0", 
        "HugePages_Surp": "0", 
        "HugePages_Total": "32"
    }
}

TASK [Show me compute servers where my hugepages are more than 75% consumed] ***********************************************
skipping: [overcloud-compute-0]
ok: [overcloud-compute-1] => {
    "ansible_local.hugepages": {
        "HugePages_Free": "7", 
        "HugePages_Rsvd": "0", 
        "HugePages_Surp": "0", 
        "HugePages_Total": "32"
    }
}

PLAY RECAP *************************************************************************************************************
overcloud-compute-0        : ok=4    changed=0    unreachable=0    failed=0   
overcloud-compute-1        : ok=5    changed=0    unreachable=0    failed=0
```  