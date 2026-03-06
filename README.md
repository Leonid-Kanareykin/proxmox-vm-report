### PLAYBOOK: Generate Proxmox VM Report
 -----------------------------------------------------------------------------
 Author: Leonid Kanareykin, itmanz@gmail.com, telegram: @itmanz93
 ----------------------------------------------------------------
 This playbook connects to all Proxmox nodes and retrieves detailed VM information:
 Ansible groups,Node,VMID,VM name,Status,CPU (cores),RAM allocated (GB),RAM used (GB),All disks size (GB),OS Type,OS Version (agent),Pool,Tags
 (including OS type from config, OS version from QEMU agent, pool, tags), and compiles a timestamped CSV report.
 
 !!! Tested with inventory file in such yaml format:
 proxmox:
    children:
      proxmox_dc1:
        children:
          proxmox_dc1_cluster1:
             cluster https://proxmox-dc1.infra.company.com/
            hosts:
              proxmox-01.dc1.wb-bank.ru:
         proxmox_dc1_cluster2:
        children:
          proxmox_dc2_cluster1:
             cluster https://proxmox-dc2.infra.company.com/
            hosts:
              proxmox-01.dc2.company.com:

 -----------------------------------------------------------------------------
 Key features:
 - generate CSV files with Ansible groups,Node,VMID,VM name,Status,CPU (cores),RAM allocated (GB),RAM used (GB),All disks size (GB),OS Type,OS Version (agent),Pool,Tags
 - connect using SSH
 - retrieve info from multiple cluster and hosts
 - no needed API keys to simplify many API keys generating in environment with high number of clusters because script uses your SSH account and comand line commands like pvesh which in fact uses API
 - Parallel per‑VM processing on each node (using xargs -P)
 - Get 'ostype' from VMs config if QEMU agent is unavailable
 - Converts memory/disk values from bytes to gigabytes (2 decimals)
 - Adds a header row with user‑friendly column names
 - No temporary files on the control node – everything assembled in memory
 =============================================================================
