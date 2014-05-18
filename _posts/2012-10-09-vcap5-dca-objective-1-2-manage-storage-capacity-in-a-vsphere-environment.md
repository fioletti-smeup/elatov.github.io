---
title: VCAP5-DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment
author: Karim Elatov
layout: post
permalink: /2012/10/vcap5-dca-objective-1-2-manage-storage-capacity-in-a-vsphere-environment/
dsq_thread_id:
  - 1406610580
categories:
  - Certifications
  - VCAP5-DCA
  - VMware
tags:
  - VCAP5-DCA
---
### Identify storage provisioning methods

For protocols we have Fibre Channel, iSCSI, NFS, FCoE. Check out "<a href="http://blogs.vmware.com/vsphere/2012/02/storage-protocol-comparison-a-vsphere-perspective.html" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://blogs.vmware.com/vsphere/2012/02/storage-protocol-comparison-a-vsphere-perspective.html']);">Storage Protocol Comparison – A vSphere Perspective</a>" to find out what the differences are. Here is a snippet from that page:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/storage_protocols.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/storage_protocols.png']);"><img class="alignnone size-full wp-image-3789" title="storage_protocols" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/storage_protocols.png" alt="storage protocols VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="716" height="786" /></a>

For vmdks we have, Lazy Zeroed Thick, Eager Zeroed Thick, and Thin. From the "<a href="http://pubs.vmware.com/vsphere-50/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-50-storage-guide.pdf" onclick="javascript:_gaq.push(['_trackEvent','download','http://pubs.vmware.com/vsphere-50/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-50-storage-guide.pdf']);">vSphere Storage ESXi 5.0</a>":

> **Thick Provision Lazy Zeroed**
> 
> Creates a virtual disk in a default thick format. Space required for the virtual disk is allocated when the virtual disk is created. Data remaining on the physical device is not erased during creation, but is zeroed out on demand at a later time on first write from the virtual machine. Using the default flat virtual disk format does not zero out or eliminate the possibility of recovering deleted files or restoring old data that might be present on this allocated space. You cannot convert a flat disk to a thin disk.
> 
> **Thick Provision Eager Zeroed**
> 
> A type of thick virtual disk that supports clustering features such as Fault Tolerance. Space required for the virtual disk is allocated at creation time. In contrast to the flat format, the data remaining on the physical device is zeroed out when the virtual disk is created. It might take much longer to create disks in this format than to create other types of disks.
> 
> **Thin Provision**
> 
> Use this format to save storage space. For the thin disk, you provision as much datastore space as the disk would require based on the value that you enter for the disk size. However, the thin disk starts small and at first, uses only as much datastore space as the disk needs for its initial operations. **NOTE** If a virtual disk supports clustering solutions such as Fault Tolerance, do not make the disk thin. If the thin disk needs more space later, it can grow to its maximum capacity and occupy the entire datastore space provisioned to it. Also, you can manually convert the thin disk into a thick disk.

### Identify available storage monitoring tools, metrics and alarms

To check for different storage aspects you can use storage views. Check out "<a href="http://www.virtualizationadmin.com/articles-tutorials/vmware-esx-and-vsphere-articles/storage-management/using-vmware-vsphere-storage-views.html" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://www.virtualizationadmin.com/articles-tutorials/vmware-esx-and-vsphere-articles/storage-management/using-vmware-vsphere-storage-views.html']);">Using VMware vSphere Storage Views</a>". Go to "Datastore" View -> Click on a datastore -> Click on the "Storage View" Tab. It should look like this:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/storage_view_vms.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/storage_view_vms.png']);"><img class="alignnone size-full wp-image-3796" title="storage_view_vms" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/storage_view_vms.png" alt="storage view vms VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="1043" height="776" /></a>

You can also use the Storage Maps to see how you connections look like. Go to the same view as above and then change the view to "Maps". It might look something like this:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/datastore_maps.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/datastore_maps.png']);"><img class="alignnone size-full wp-image-3797" title="datastore_maps" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/datastore_maps.png" alt="datastore maps VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="1039" height="554" /></a>

For Alarms regarding storage go to "Datastore" View -> Select a Datastore -> Then Select the "Alarms" Tab. You will see predefined alarms for datastores. Something like this:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/datastore_alarms.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/datastore_alarms.png']);"><img class="alignnone size-full wp-image-3793" title="datastore_alarms" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/datastore_alarms.png" alt="datastore alarms VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="992" height="296" /></a>

You can also check out storage metrics by going to the performance tab. It will look something like this:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/performance_tab_datastore.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/performance_tab_datastore.png']);"><img class="alignnone size-full wp-image-3794" title="performance_tab_datastore" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/performance_tab_datastore.png" alt="performance tab datastore VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="1014" height="406" /></a>

You can also look at the performance from the host view. Go to "Host and Clusters" View -> Select a Host -> Select the Performance Tab -> Select "Advanced" -> Then Switch to "Disk" or "Datastore". It will look something like this:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/host_disk_performance.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/host_disk_performance.png']);"><img class="alignnone size-full wp-image-3795" title="host_disk_performance" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/host_disk_performance.png" alt="host disk performance VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="1037" height="613" /></a>

If you want to really check metrics, fire up esxtop and go to the Disk or LUN view. We will discuss that in detail in later objectives.

### Apply space utilization data to manage storage resources

If you run out of space. You could've discovered this by an alarm, storage view, or maybe just running 'df' on the system. To alleviate the issue, use storage vMotion (cold or live) to make space on the datastore. If you have the ability you should expand the VMFS datastore. From the <a href="http://pubs.vmware.com/vsphere-50/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-50-storage-guide.pdf" onclick="javascript:_gaq.push(['_trackEvent','download','http://pubs.vmware.com/vsphere-50/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-50-storage-guide.pdf']);">vSphere Storage ESXi 5.0</a> guide:

> **Increase a VMFS Datastore**  
> When you need to create virtual machines on a datastore, or when the virtual machines running on a datastore require more space, you can dynamically increase the capacity of a VMFS datastore. Use one of the following methods to increase a VMFS datastore:
> 
> *   Add a new extent. An extent is a partition on a storage device. You can add up to 32 extents of the same storage type to an existing VMFS datastore. The spanned VMFS datastore can use any or all of its extents at any time. It does not need to fill up a particular extent before using the next one.
> *   Grow an extent in an existing VMFS datastore, so that it fills the available adjacent capacity. Only extents with free space immediately after them are expandable.

### Provision and manage storage resources according to Virtual Machine requirements

Check out VCAP5-DCD Objectives <a href="http://virtuallyhyper.com/2012/08/vcap5-dcd-objective-3-3-create-a-vsphere-5-physical-storage-design-from-an-existing-logical-design/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2012/08/vcap5-dcd-objective-3-3-create-a-vsphere-5-physical-storage-design-from-an-existing-logical-design/']);">3.3</a> and <a href="http://virtuallyhyper.com/2012/09/vcap5-dcd-objective-3-5-determine-virtual-machine-configuration-for-a-vsphere-5-physical-design/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2012/09/vcap5-dcd-objective-3-5-determine-virtual-machine-configuration-for-a-vsphere-5-physical-design/']);">3.5</a>. Just plan for Space, I/O Workload, and Redundancy.

### Understand interactions between virtual storage provisioning and physical storage provisioning

I actually discussed this in "<a href="http://virtuallyhyper.com/2012/04/thin-on-thin-with-vmware-thin-provisioned-disks-and-emc-symmetrix-virtual-provisioning/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2012/04/thin-on-thin-with-vmware-thin-provisioned-disks-and-emc-symmetrix-virtual-provisioning/']);">Thin on Thin With VMware Thin Provisioned Disks and EMC Symmetrix Virtual Provisioning</a>". Also from  <a href="http://pubs.vmware.com/vsphere-50/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-50-storage-guide.pdf" onclick="javascript:_gaq.push(['_trackEvent','download','http://pubs.vmware.com/vsphere-50/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-50-storage-guide.pdf']);">vSphere Storage ESXi 5.0</a>:

> **Array Thin Provisioning and VMFS Datastores**
> 
> You can use thin provisioned storage arrays with ESXi. Traditional LUNs that arrays present to the ESXi host, are thick-provisioned. The entire physical space needed to back each LUN is allocated in advance. ESXi also supports thin-provisioned LUNs. When a LUN is thin-provisioned, the storage array reports the LUN's logical size, which might be larger than the real physical capacity backing that LUN. A VMFS datastore that you deploy on the thin-provisioned LUN can detect only the logical size of the LUN. For example, if the array reports 2TB of storage while in reality the array provides only 1TB, the datastore considers 2TB to be the LUN's size. As the datastore grows, it cannot determine whether the actual amount of physical space is still sufficient for its needs. However, when you use the Storage APIs - Array Integration, the host can integrate with physical storage and become aware of underlying thin-provisioned LUNs and their space usage. Using thin provision integration, your host can perform these tasks:
> 
> *   Monitor the use of space on thin-provisioned LUNs to avoid running out of physical space. As your datastore grows or if you use Storage vMotion to migrate virtual machines to a thin-provisioned LUN, the host communicates with the LUN and warns you about breaches in physical space and about out-of-space conditions.
> *   Inform the array about the datastore space that is freed when files are deleted or removed from the datastore by Storage vMotion. The array can then reclaim the freed blocks of space

### Apply VMware storage best practices

Same as <a href="http://virtuallyhyper.com/2012/10/vcap5-dca-objective-1-1-implement-and-manage-complex-storage-solutions/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2012/10/vcap5-dca-objective-1-1-implement-and-manage-complex-storage-solutions/']);">Objective 1.1</a>

### Configure Datastore Alarms

Go To "File" -> "New" -> "Alarm" and then you will see something like this:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/create_new_alarm.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/create_new_alarm.png']);"><img class="alignnone size-full wp-image-3798" title="create_new_alarm" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/create_new_alarm.png" alt="create new alarm VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="735" height="466" /></a>

Go to the "Trigger" tab -> Right Click on the white space -> Select "New Trigger" -> Select a trigger from the drop down menu. It will look something like this:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/select_trigger_for_alarm.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/select_trigger_for_alarm.png']);"><img class="alignnone size-full wp-image-3799" title="select_trigger_for_alarm" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/select_trigger_for_alarm.png" alt="select trigger for alarm VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="732" height="467" /></a>

Select the Warning and Alert limits. Under the Actions Tab, select whether you want to "Send a notification email", "Send a notification trap", or "Run a command". And you are all set.

### Analyze Datastore Alarms and errors to determine space availability

Check under Triggered Alarms (Datastore View -> Select Datastore -> Select Alarms Tab -> Select Triggered View). If there is an alarm under there, fix it

### Configure Datastore Clusters

From "<a href="http://pubs.vmware.com/vsphere-50/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-50-resource-management-guide.pdf" onclick="javascript:_gaq.push(['_trackEvent','download','http://pubs.vmware.com/vsphere-50/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-50-resource-management-guide.pdf']);">vSphere Resource Management ESXi 5.0</a>"

> **Creating a Datastore Cluster**  
> A datastore cluster is a collection of datastores with shared resources and a shared management interface. Datastore clusters are to datastores what clusters are to hosts. When you create a datastore cluster, you can use vSphere Storage DRS to manage storage resources.
> 
> When you add a datastore to a datastore cluster, the datastore's resources become part of the datastore cluster's resources. As with clusters of hosts, you use datastore clusters to aggregate storage resources, which enables you to support resource allocation policies at the datastore cluster level.
> 
> The following resource management capabilities are also available per datastore cluster.
> 
> **Space utilization load balancing** You can set a threshold for space use. When space use on a datastore exceeds the threshold, Storage DRS generates recommendations or performs Storage vMotion migrations to balance space use across the datastore cluster.
> 
> **I/O latency load balancing** You can set an I/O latency threshold for bottleneck avoidance. When I/O latency on a datastore exceeds the threshold, Storage DRS generates recommendations or performs Storage vMotion migrations to help alleviate high I/O load.
> 
> **Anti-affinity rules** You can create anti-affinity rules for virtual machine disks. For example, the virtual disks of a certain virtual machine must be kept on different datastores. By default, all virtual disks for a virtual machine are placed on the same datastore.
> 
> **Initial Placement and Ongoing Balancing** Storage DRS provides initial placement and ongoing balancing recommendations to datastores in a Storage DRS-enabled datastore cluster.
> 
> Initial placement occurs when Storage DRS selects a datastore within a datastore cluster on which to place a virtual machine disk. This happens when the virtual machine is being created or cloned, when a virtual machine disk is being migrated to another datastore cluster, or when you add a disk to an existing virtual machine.
> 
> Initial placement recommendations are made in accordance with space constraints and with respect to the goals of space and I/O load balancing. These goals aim to minimize the risk of over-provisioning one datastore, storage I/O bottlenecks, and performance impact on virtual machines.
> 
> Storage DRS is invoked at the configured frequency (by default, every eight hours) or when one or more datastores in a datastore cluster exceeds the user-configurable space utilization thresholds. When Storage DRS is invoked, it checks each datastore's space utilization and I/O latency values against the threshold. For I/O latency, Storage DRS uses the 90th percentile I/O latency measured over the course of a day to compare against the threshold.
> 
> **Storage Migration Recommendations**vCenter Server displays migration recommendations on the Storage DRS Recommendations page for datastore clusters that have manual automation mode.
> 
> The system provides as many recommendations as necessary to enforce Storage DRS rules and to balance the space and I/O resources of the datastore cluster. Each recommendation includes the virtual machine name, the virtual disk name, the name of the datastore cluster, the source datastore, the destination datastore, and a reason for the recommendation.
> 
> *   Balance datastore space use
> *   Balance datastore I/O load
> 
> Storage DRS makes mandatory recommendations for migration in the following situations:
> 
> *   The datastore is out of space.
> *   Anti-affinity or affinity rules are being violated.
> *   The datastore is entering maintenance mode and must be evacuated.
> 
> In addition, optional recommendations are made when a datastore is close to running out of space or when adjustments should be made for space and I/O load balancing. Storage DRS considers moving virtual machines that are powered off or powered on for space balancing.Storage DRS includes powered-off virtual machines with snapshots in these considerations.

Now to the actual process:

> **Create a Datastore Cluster** You can manage datastore cluster resources using Storage DRS. **Procedure**
> 
> 1.  In the Datastores and Datastore Clusters view of the vSphere Client inventory, right-click the Datacenter object and select New Datastore Cluster.
> 2.  Follow the prompts to complete the Create Datastore Cluster wizard.

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/right_click_datastore_view.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/right_click_datastore_view.png']);"><img class="alignnone size-full wp-image-3801" title="right_click_datastore_view" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/right_click_datastore_view.png" alt="right click datastore view VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="618" height="427" /></a>

Fill Out the Name:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/Name_Storage_DRS_Cluster.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/Name_Storage_DRS_Cluster.png']);"><img class="alignnone size-full wp-image-3802" title="Name_Storage_DRS_Cluster" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/Name_Storage_DRS_Cluster.png" alt="Name Storage DRS Cluster VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="942" height="708" /></a>

Set the Automation Level:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Automation_level.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Automation_level.png']);"><img class="alignnone size-full wp-image-3803" title="SDRS_Automation_level" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Automation_level.png" alt="SDRS Automation level VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="942" height="710" /></a>

Set the Threshold Limits:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Threshold_limits.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Threshold_limits.png']);"><img class="alignnone size-full wp-image-3804" title="SDRS_Threshold_limits" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Threshold_limits.png" alt="SDRS Threshold limits VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="943" height="707" /></a>

Select Cluster to enable Storage DRS on:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Select_Hosts.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Select_Hosts.png']);"><img class="alignnone size-full wp-image-3805" title="SDRS_Select_Hosts" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Select_Hosts.png" alt="SDRS Select Hosts VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="945" height="710" /></a>

Select Datastores to participate in the Storage DRS Cluster:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Select_Datastore.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Select_Datastore.png']);"><img class="alignnone size-full wp-image-3806" title="SDRS_Select_Datastore" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Select_Datastore.png" alt="SDRS Select Datastore VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="943" height="709" /></a>

Click Finish on the Summary Page:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Summary_page.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Summary_page.png']);"><img class="alignnone size-full wp-image-3807" title="SDRS_Summary_page" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/SDRS_Summary_page.png" alt="SDRS Summary page VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="943" height="707" /></a>

You will see the datastore cluster that you created, like so:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/datastore_cluster_view_with_dscl.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/datastore_cluster_view_with_dscl.png']);"><img class="alignnone size-full wp-image-3870" title="datastore_cluster_view_with_dscl" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/datastore_cluster_view_with_dscl.png" alt="datastore cluster view with dscl VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="610" height="272" /></a>

Now going back to guide:

> **Enable and Disable Storage DRS**
> 
> Storage DRS allows you to manage the aggregated resources of a datastore cluster. When Storage DRS is enabled, it provides recommendations for virtual machine disk placement and migration to balance space and I/O resources across the datastores in the datastore cluster. When you enable Storage DRS, you enable the following functions.
> 
> *   Space load balancing among datastores within a datastore cluster.
> *   I/O load balancing among datastores within a datastore cluster.
> *   Initial placement for virtual disks based on space and I/O workload.
> 
> The Enable Storage DRS check box in the Datastore Cluster Settings dialog box enables or disables all of these components at once. If necessary, you can disable I/O-related functions of Storage DRS independently of space balancing functions.
> 
> When you disable Storage DRS on a datastore cluster, Storage DRS settings are preserved. When you enable Storage DRS, the settings for the datastore cluster are restored to the point where Storage DRS was disabled.
> 
> **Procedure**
> 
> 1.  In the vSphere Client inventory, right-click a datastore cluster and select Edit Settings.
> 2.  Click General.
> 3.  Select Turn on Storage DRS and click OK.
> 4.  (Optional) To disable only I/O-related functions of Storage DRS, leaving space-related controls enabled, perform the following steps. 
>     1.  Select SDRS Runtime Rules.
>     2.  Deselect the Enable I/O metric for Storage DRS check box.
> 5.  Click OK

It will look something like this:

<a href="http://virtuallyhyper.com/wp-content/uploads/2012/09/enable_datastore_cluster.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2012/09/enable_datastore_cluster.png']);"><img class="alignnone size-full wp-image-3871" title="enable_datastore_cluster" src="http://virtuallyhyper.com/wp-content/uploads/2012/09/enable_datastore_cluster.png" alt="enable datastore cluster VCAP5 DCA Objective 1.2 – Manage Storage Capacity in a vSphere Environment " width="721" height="624" /></a>

After that, whatever Storage DRS settings you setup will be applied.

<div class="SPOSTARBUST-Related-Posts">
  <H3>
    Related Posts
  </H3>
  
  <ul class="entry-meta">
    <li class="SPOSTARBUST-Related-Post">
      <a title="VCAP5-DCA Objective 2.3 – Deploy and Maintain Scalable Virtual Networking" href="http://virtuallyhyper.com/2012/10/vcap5-dca-objective-2-3-deploy-and-maintain-scalable-virtual-networking/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2012/10/vcap5-dca-objective-2-3-deploy-and-maintain-scalable-virtual-networking/']);" rel="bookmark">VCAP5-DCA Objective 2.3 – Deploy and Maintain Scalable Virtual Networking</a>
    </li>
    <li class="SPOSTARBUST-Related-Post">
      <a title="VCAP5-DCA Objective 2.2 – Configure and Maintain VLANs, PVLANs and VLAN Settings" href="http://virtuallyhyper.com/2012/10/vcap5-dca-objective-2-2-configure-and-maintain-vlans-pvlans-and-vlan-settings/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2012/10/vcap5-dca-objective-2-2-configure-and-maintain-vlans-pvlans-and-vlan-settings/']);" rel="bookmark">VCAP5-DCA Objective 2.2 – Configure and Maintain VLANs, PVLANs and VLAN Settings</a>
    </li>
    <li class="SPOSTARBUST-Related-Post">
      <a title="VCAP5-DCA Objective 2.1 – Implement and Manage Complex Virtual Networks" href="http://virtuallyhyper.com/2012/10/vcap5-dca-objective-2-1-implement-and-manage-complex-virtual-networks/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2012/10/vcap5-dca-objective-2-1-implement-and-manage-complex-virtual-networks/']);" rel="bookmark">VCAP5-DCA Objective 2.1 – Implement and Manage Complex Virtual Networks</a>
    </li>
    <li class="SPOSTARBUST-Related-Post">
      <a title="VCAP5-DCA Objective 1.3 – Configure and Manage Complex Multipathing and PSA Plug-ins" href="http://virtuallyhyper.com/2012/10/vcap5-dca-objective-1-3-configure-and-manage-complex-multipathing-and-psa-plug-ins/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2012/10/vcap5-dca-objective-1-3-configure-and-manage-complex-multipathing-and-psa-plug-ins/']);" rel="bookmark">VCAP5-DCA Objective 1.3 – Configure and Manage Complex Multipathing and PSA Plug-ins</a>
    </li>
  </ul>
</div>
