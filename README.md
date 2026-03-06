### PLAYBOOK: Generate Proxmox VM Report
 -----------------------------------------------------------------------------
 Author: Leonid Kanareykin, itmanz@gmail.com, telegram: @itmanz93
 ----------------------------------------------------------------
 This playbook connects to all Proxmox nodes and retrieves detailed VM information:
 Ansible groups  with all children (usually: group, DC, cluster), Proxmox Node name, VMID, VM name, Power status, CPU (cores), RAM allocated (GB), RAM used (GB), All disks size (GB), OS Type, OS Version (agent), Pool,Tags
 (including OS type from config, OS version from QEMU agent, pool, tags), and compiles a timestamped CSV report.
 
 !!! Tested with example inventory file in such yaml format: https://github.com/Leonid-Kanareykin/proxmox-vm-report/blob/main/inventory-example.yml
 
 Key features:
 - connecting using SSH to one or many hosts and cluster from yaml inventory file
 - generate CSV file with Ansible groups,Node,VMID,VM name,Status,CPU (cores),RAM allocated (GB),RAM used (GB),All disks size (GB),OS Type,OS Version (agent),Pool,Tags
 - retrieve info from multiple cluster and hosts
 - no needed API keys to simplify many API keys generating in environment with high number of clusters because script uses your SSH account and comand line commands like pvesh which in fact uses API
 - Parallel per‑VM processing on each node (using xargs -P)
 - Get 'ostype' from VMs config if QEMU agent is unavailable
 - Converts memory/disk values from bytes to gigabytes (2 decimals)
 - Adds a header row with user‑friendly column names
 - No temporary files on the control node – everything assembled in memory
 =============================================================================

### To run on all inventory
ansible-playbook -i inventory-example.yml /playbooks/pve-vm-report-latest.yml

### To run on some clusters or one cluster
ansible-playbook -i inventory-example.yml /playbooks/pve-vm-report-latest.yml --limit proxmox_dc1_cluster1,cluster2_pve_dc2

