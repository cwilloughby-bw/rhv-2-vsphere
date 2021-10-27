# rhv-2-vsphere

This tool will migrate VMs from RHV/oVirt, to vSphere 7 (And probably others).

To continue, you'll need the following:
| Requirement | Purpose |
|--|--|
| Root/sudo-capable credentials | Used for the source VM, installs open-vm-tools for VMWare guest tools, and reconfigures dracut to support VMWare paravirtual + LSILogic disk controller |
| RHV/oVirt | Used to collect VM info (vCPUs/RAM/NICs/Disks)
| vCenter Account | Used to create target VM |
| ESXi Account | Used to log into a chosen HV to run vmkfstools, to validate the new VMDK disk images, and inflate them |

This tool can operate in two modes:
|Mode|Pros|Cons|Use Cases
|--|--|--|--|
|Hot|Minimal downtime for the host, roughly 5m between shutdown of source VM, and bootup of target VM|Based on a snapshot taken at the start of the migration, any changes after the snapshot are not transferred|Semi-Stateless machines, IE webservers or DBs with all workloads on NFS storage|
|Cold|All data transferred, no snapshots required|Extended downtime, varied based on amount of data. Expect ~45m for 25GB of actual data|Machines with workloads being run from the local disk|

For safety, the tool will default to running in "Cold" mode. To ensure this, "Hot" mode hasn't been implemented yet :)