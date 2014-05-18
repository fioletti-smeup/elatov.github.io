---
title: Deploying a Test Windows Environment in a KVM Infrastucture
author: Karim Elatov
layout: post
permalink: /2013/04/deploying-a-test-windows-environment-in-a-kvm-infrastucture/
dsq_thread_id:
  - 1404673767
categories:
  - Home Lab
  - OS
tags:
  - Active Directory
  - KVM
  - SPICE
---
I was recently playing with KVM and needed to setup a domain controller for testing reasons. A great introduction to KVM can be seen in Jarret's "<a href="http://virtuallyhyper.com/2012/07/installing-kvm-as-a-virtual-machine-on-esxi5-with-bridged-networking/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2012/07/installing-kvm-as-a-virtual-machine-on-esxi5-with-bridged-networking/']);">Installing KVM as a Virtual Machine on ESXi 5 with Bridged Networking</a>" post. It has all the steps on how to set deploy and configure it. All of the below instructions are assuming that you already have a KVM server up and running.

### Connect to the KVM Server with Virtual Machine Manager (virt-manager)

If you don't have <a href="http://virt-manager.org/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virt-manager.org/']);">Virtual Machine Manager</a> installed, go ahead and install it:

    sudo yum install virt-manager    
    

Depending on where your ISOs are stored you have a couple of options. If the ISOs are stored on the KVM server then you will have to use SSH X-Forwarding to do the initial setup, and after the initial install you can manage the VMs with **virt-manager** via *qemu+ssh* protocol instead of SSH X-Forwarding. The reason for this is because you can't browse local directories from **virt-manager** remotely. From '<a href="http://doc.opensuse.org/products/draft/SLES/SLES-kvm_sd_draft/cha.libvirt.storage.html" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://doc.opensuse.org/products/draft/SLES/SLES-kvm_sd_draft/cha.libvirt.storage.html']);">Chapter 8. Managing Storage</a>'

> Using the file browser by clicking on Browse is not possible when operating from remote.

It's possible but all the ISOs would have to be in one big directory without sub-directories, from the same page:

> **CD/DVD ISO images**
> 
> In order to be able to access CD/DVD iso images on the VM Host Server from remote, they also need to be placed in a storage pool.

There was an NFS export shared to the KVM server and the ISOs were organized by folders, so adding that directory/export as a storage pool didn't help out since all the ISOs need to be in one big directory. We could add each subdirectory as a storage pool but that would be a lot work. This was discussed in <a href="http://comments.gmane.org/gmane.comp.emulators.libvirt.user/2603" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://comments.gmane.org/gmane.comp.emulators.libvirt.user/2603']);">this</a> forum. If the ISOs were local to the your machine, then you can just launch **virt-manager** locally and connect to the KVM server using SSH and then point to the local ISO.

So let's login to our KVM server with SSH with X-Forwarding enabled:

    [elatov@klaptop ~]$ ssh -X virtuser@kvm
    

Then from the remote machine let's launch **virt-manager**:

    [virtuser@kvm ~]$ virt-manager
    

At this point you should see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/connected_virt-manager.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/connected_virt-manager.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/connected_virt-manager.png" alt="connected virt manager Deploying a Test Windows Environment in a KVM Infrastucture" width="839" height="278" class="alignnone size-full wp-image-7924" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

As you can see it will auto connect to the local instance (if properly configured). You will see a list of the VMs that are currently running and small performance graphs on the right as well. You can go to "Edit" -> "Connection Details":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/virt_manager_connection_details.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/virt_manager_connection_details.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/virt_manager_connection_details.png" alt="virt manager connection details Deploying a Test Windows Environment in a KVM Infrastucture" width="854" height="318" class="alignnone size-full wp-image-7925" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

After selecting that you will see the following window:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/ssh-x-kvm-conn-details_g.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/ssh-x-kvm-conn-details_g.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/ssh-x-kvm-conn-details_g.png" alt="ssh x kvm conn details g Deploying a Test Windows Environment in a KVM Infrastucture" width="746" height="495" class="alignnone size-full wp-image-7926" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

From here you can see: connection details, configured Storage Pools, and Configured Networks.

### Create a New VM with Virt-Manager

From the main **virt-manager** window click on the "New" button:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/virt-manager_create-new-vm-button.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/virt-manager_create-new-vm-button.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/virt-manager_create-new-vm-button.png" alt="virt manager create new vm button Deploying a Test Windows Environment in a KVM Infrastucture" width="854" height="318" class="alignnone size-full wp-image-7927" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

And that will start the "Create New VM" Wizard:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm-wizard_step1.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm-wizard_step1.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm-wizard_step1.png" alt="create new vm wizard step1 Deploying a Test Windows Environment in a KVM Infrastucture" width="481" height="392" class="alignnone size-full wp-image-7928" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

At the first step: name the VM as you desire, select "Local install media" (since we are going to use the ISO that is locally stored on the KVM server), and then click "Forward":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step1_filled_out.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step1_filled_out.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step1_filled_out.png" alt="create new vm wizard step1 filled out Deploying a Test Windows Environment in a KVM Infrastucture" width="481" height="392" class="alignnone size-full wp-image-7929" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then you will see the 2nd step of the wizard: <a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step2.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step2.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step2.png" alt="create new vm wizard step2 Deploying a Test Windows Environment in a KVM Infrastucture" width="481" height="403" class="alignnone size-full wp-image-7930" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Select "Use ISO image", click on "Browse", and then you will see the available Storage Pools:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step2_browse_storage_volumes.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step2_browse_storage_volumes.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step2_browse_storage_volumes.png" alt="create new vm wizard step2 browse storage volumes Deploying a Test Windows Environment in a KVM Infrastucture" width="775" height="195" class="alignnone size-full wp-image-7931" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

This will show you the *img* files for already running VMs. If we had a storage pool with one big directory of ISOs then you could just select the ISO from that Storage pool (but this wasn't the case for us). Next click on "Browse Local" which is only available if we are connecting locally or with SSH with X-Forwarding. Then we will see the "Browse for ISO" dialogue:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_step2_locate_iso_dialogue.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_step2_locate_iso_dialogue.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_step2_locate_iso_dialogue.png" alt="create new vm step2 locate iso dialogue Deploying a Test Windows Environment in a KVM Infrastucture" width="665" height="355" class="alignnone size-full wp-image-7932" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

At this point just browse to the location of the ISOs and select your Win2k8 ISO. After that is done, here how step 2 of the wizard will look like:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_step2_filled_out.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_step2_filled_out.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_step2_filled_out.png" alt="create new vm step2 filled out Deploying a Test Windows Environment in a KVM Infrastucture" width="481" height="403" class="alignnone size-full wp-image-7933" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then click "Forward" and then we will get to step 3 of the wizard:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_step3.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_step3.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_step3.png" alt="create new vm step3 Deploying a Test Windows Environment in a KVM Infrastucture" width="481" height="403" class="alignnone size-full wp-image-7934" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

We have to choose CPU and RAM settings, looking over <a href="http://technet.microsoft.com/en-us/windowsserver/bb414778.aspx" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://technet.microsoft.com/en-us/windowsserver/bb414778.aspx']);">this</a> microsoft page, we can see their minimum requirements:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/microsoft_min_reqs.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/microsoft_min_reqs.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/microsoft_min_reqs.png" alt="microsoft min reqs Deploying a Test Windows Environment in a KVM Infrastucture" width="666" height="416" class="alignnone size-full wp-image-7935" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

I just kept 1CPU and 1GB of RAM and clicked "Forward", at which point I saw step 4 of the wizard:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_step4.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_step4.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_step4.png" alt="create new vm step4 Deploying a Test Windows Environment in a KVM Infrastucture" width="481" height="403" class="alignnone size-full wp-image-7936" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

I did other installs of the Win2K8R2 and after all the windows updates it ended up using about 20GB, so that is what I setup. Here is how my final step 4 looked like:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step4_filled_out.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step4_filled_out.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step4_filled_out.png" alt="create new vm wizard step4 filled out Deploying a Test Windows Environment in a KVM Infrastucture" width="481" height="403" class="alignnone size-full wp-image-7937" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

I then clicked "Forward" and it took to the 5th step (the last step):

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step5.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step5.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_wizard_step5.png" alt="create new vm wizard step5 Deploying a Test Windows Environment in a KVM Infrastucture" width="481" height="538" class="alignnone size-full wp-image-7938" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Here you can select the networking setup for the VM. We were using "bridged" (the same setup that Jarret described in <a href="http://virtuallyhyper.com/2012/07/installing-kvm-as-a-virtual-machine-on-esxi5-with-bridged-networking/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2012/07/installing-kvm-as-a-virtual-machine-on-esxi5-with-bridged-networking/']);">his</a> post). So I left the defaults and clicked "Finish". At that point the console to the VM started up:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_console.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_console.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_console.png" alt="create new vm console Deploying a Test Windows Environment in a KVM Infrastucture" width="825" height="732" class="alignnone size-full wp-image-7939" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Keep going through the install as you would usually do. Here is a screenshot of the install process going:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_windows_install_process.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_windows_install_process.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_new_vm_windows_install_process.png" alt="create new vm windows install process Deploying a Test Windows Environment in a KVM Infrastucture" width="825" height="732" class="alignnone size-full wp-image-7940" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

### Connect to the KVM Server Remotely with virt-manager

After the initial install of the OS is finished you don't need to use SSH X-Forwarding to connect to the KVM server. Install **virt-manager** locally and then fire it up and when it starts up it will try to automatically connect to a hyper-visor:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/searching_for_available_hypervisors.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/searching_for_available_hypervisors.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/searching_for_available_hypervisors.png" alt="searching for available hypervisors Deploying a Test Windows Environment in a KVM Infrastucture" width="314" height="229" class="alignnone size-full wp-image-7941" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Since we are not running one locally you will get error like this:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/failure_to_connect_to_libvirtd.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/failure_to_connect_to_libvirtd.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/failure_to_connect_to_libvirtd.png" alt="failure to connect to libvirtd Deploying a Test Windows Environment in a KVM Infrastucture" width="490" height="211" class="alignnone size-full wp-image-7942" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

You can just close that and then you will see your **virt-manager** in a disconnected state:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/virt-manager_not_connected.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/virt-manager_not_connected.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/virt-manager_not_connected.png" alt="virt manager not connected Deploying a Test Windows Environment in a KVM Infrastucture" width="822" height="244" class="alignnone size-full wp-image-7943" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

At this point go to "File" -> "Add Connection":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/virt-manager_add_connection.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/virt-manager_add_connection.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/virt-manager_add_connection.png" alt="virt manager add connection Deploying a Test Windows Environment in a KVM Infrastucture" width="822" height="244" class="alignnone size-full wp-image-7945" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then go ahead and fill out the necessary information:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/add_connection_filled_out.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/add_connection_filled_out.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/add_connection_filled_out.png" alt="add connection filled out Deploying a Test Windows Environment in a KVM Infrastucture" width="366" height="316" class="alignnone size-full wp-image-7962" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

If you hit connect it will ask for you the virtuser password:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/ssh_password_g.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/ssh_password_g.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/ssh_password_g.png" alt="ssh password g Deploying a Test Windows Environment in a KVM Infrastucture" width="292" height="247" class="alignnone size-full wp-image-7963" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

It will actually keep asking for the password when you launch VMs. The best thing to do is use SSH keys so you don't have to keep typing in the password. First generate your own pair of SSH keys:

    [elatov@klaptop ~]$ ssh-keygen -t rsa
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/elatov/.ssh/id_rsa): 
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in /home/elatov/.ssh/id_rsa.
    Your public key has been saved in /home/elatov/.ssh/id_rsa.pub.
    The key fingerprint is:
    a8:42:e5:7e:fd:99:ab:24:94:96:15:e4:da:21:2e:05 elatov@klaptop
    The key's randomart image is:
    +--[ RSA 2048]----+
    |    E  .o        |
    |     . . .       |
    |    . o +        |
    |   o o O .       |
    |  . o O S        |
    | . . = .         |
    |  . o o o        |
    |   . . o . o     |
    |        ..=.     |
    +-----------------+
    

If you have ssh-agent running then you will just have to type in that password once and you will be set. Now go ahead and add your SSH public key to the KVM server:

    [elatov@klaptop ~]$ ssh-copy-id virtuser@kvm
    virtuser@kvm's password: 
    Now try logging into the machine, with "ssh 'virtuser@kvm'", and check in:
    
      ~/.ssh/authorized_keys
    
    to make sure we haven't added extra keys that you weren't expecting.
    

Now if you try to connect to the KVM server with **virt-manager** via *qemu+ssh* you won't have to enter the virtuser password every time. When you use **virt-manager** remotely you will only see CPU usage like so:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/remote_virt-manager.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/remote_virt-manager.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/remote_virt-manager.png" alt="remote virt manager Deploying a Test Windows Environment in a KVM Infrastucture" width="822" height="244" class="alignnone size-full wp-image-7964" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

### Setup a Windows Active Directory Server

Most of the instructions are laid out <a href="http://www.rackspace.com/knowledge_center/article/installing-active-directory-on-windows-server-2008-r2-enterprise-64-bit" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://www.rackspace.com/knowledge_center/article/installing-active-directory-on-windows-server-2008-r2-enterprise-64-bit']);">here</a> and <a href="http://www.petri.co.il/installing-active-directory-windows-server-2008.htm" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://www.petri.co.il/installing-active-directory-windows-server-2008.htm']);">here</a>. After Windows 2008 R2 is installed, launch the Server Manager by entering:

    servermanager.msc
    

In the run dialog and you will see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/server_manager_started.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/server_manager_started.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/server_manager_started.png" alt="server manager started Deploying a Test Windows Environment in a KVM Infrastucture" width="794" height="550" class="alignnone size-full wp-image-7977" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then go to "Roles" and click on "Add Roles":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/blank_roles_win2k8.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/blank_roles_win2k8.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/blank_roles_win2k8.png" alt="blank roles win2k8 Deploying a Test Windows Environment in a KVM Infrastucture" width="774" height="568" class="alignnone size-full wp-image-7978" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

From the list select "Active Directory Domain Services":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/dc_role_selected.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/dc_role_selected.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/dc_role_selected.png" alt="dc role selected Deploying a Test Windows Environment in a KVM Infrastucture" width="772" height="569" class="alignnone size-full wp-image-7979" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then click "Next" and couple of times, followed by a "Finish". After the install is done you will see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/dc_install_finished.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/dc_install_finished.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/dc_install_finished.png" alt="dc install finished Deploying a Test Windows Environment in a KVM Infrastucture" width="775" height="569" class="alignnone size-full wp-image-7980" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Now let's actually configure it. To do that run:

    dcpromo.exe
    

from the Run dialog and you will see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/ad_setup_wizard.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/ad_setup_wizard.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/ad_setup_wizard.png" alt="ad setup wizard Deploying a Test Windows Environment in a KVM Infrastucture" width="497" height="471" class="alignnone size-full wp-image-7981" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Click "Next" a couple of times and you see the "Choose Deployment Configuration" window:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/choose_deployment_conf_ad_wizard.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/choose_deployment_conf_ad_wizard.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/choose_deployment_conf_ad_wizard.png" alt="choose deployment conf ad wizard Deploying a Test Windows Environment in a KVM Infrastucture" width="498" height="469" class="alignnone size-full wp-image-7982" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

I didn't have an existing domain, so I selected "Create a New Domain in a new forest" and clicked next:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/name_forest_room_domain_ad_wizard.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/name_forest_room_domain_ad_wizard.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/name_forest_room_domain_ad_wizard.png" alt="name forest room domain ad wizard Deploying a Test Windows Environment in a KVM Infrastucture" width="497" height="472" class="alignnone size-full wp-image-7983" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

For my domain I chose "elatov.local":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/elatov_local_name_ad_wizard.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/elatov_local_name_ad_wizard.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/elatov_local_name_ad_wizard.png" alt="elatov local name ad wizard Deploying a Test Windows Environment in a KVM Infrastucture" width="497" height="470" class="alignnone size-full wp-image-7984" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

After clicking "Next" I was presented with the following screen:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/set_forest_functionality_lelel_ad_wiz.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/set_forest_functionality_lelel_ad_wiz.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/set_forest_functionality_lelel_ad_wiz.png" alt="set forest functionality lelel ad wiz Deploying a Test Windows Environment in a KVM Infrastucture" width="497" height="470" class="alignnone size-full wp-image-7985" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

I wasn't planning on using anything below Server 2003, so I left the default option and clicked "Next". At this point I saw the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/set_domain_functional_level_ad_wiz.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/set_domain_functional_level_ad_wiz.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/set_domain_functional_level_ad_wiz.png" alt="set domain functional level ad wiz Deploying a Test Windows Environment in a KVM Infrastucture" width="497" height="468" class="alignnone size-full wp-image-7986" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Same thing here, I left the default and clicked "Next". I was then asked to setup a DNS server:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/additional_domain_controller_options.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/additional_domain_controller_options.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/additional_domain_controller_options.png" alt="additional domain controller options Deploying a Test Windows Environment in a KVM Infrastucture" width="497" height="469" class="alignnone size-full wp-image-7987" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

I didn't have a local DNS server in the environment so I decided to set one up. I left the "DNS server" selected and clicked "Next" and saw the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/location_of_db_log_ad_wiz.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/location_of_db_log_ad_wiz.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/location_of_db_log_ad_wiz.png" alt="location of db log ad wiz Deploying a Test Windows Environment in a KVM Infrastucture" width="499" height="470" class="alignnone size-full wp-image-7988" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

I left the defaults here as well and clicked "Next", at this point I was asked to enter the "Restore Domain Administrator" Password:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/dir_ser_rest_mode_admin_pass_ad_wiz.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/dir_ser_rest_mode_admin_pass_ad_wiz.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/dir_ser_rest_mode_admin_pass_ad_wiz.png" alt="dir ser rest mode admin pass ad wiz Deploying a Test Windows Environment in a KVM Infrastucture" width="498" height="469" class="alignnone size-full wp-image-7989" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

After that I was presented with the "Summary" page:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/summary_ad_wiz.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/summary_ad_wiz.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/summary_ad_wiz.png" alt="summary ad wiz Deploying a Test Windows Environment in a KVM Infrastucture" width="496" height="469" class="alignnone size-full wp-image-7990" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

After clicking "Next" the install process started, after the install was finished I saw the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/ad_setup_finished_ad_wizard.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/ad_setup_finished_ad_wizard.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/ad_setup_finished_ad_wizard.png" alt="ad setup finished ad wizard Deploying a Test Windows Environment in a KVM Infrastucture" width="499" height="472" class="alignnone size-full wp-image-7991" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Selecting "Finish" from the above window prompted for a restart. After the restart I saw the following at the login screen:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/ad_server_joined_to_domain.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/ad_server_joined_to_domain.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/ad_server_joined_to_domain.png" alt="ad server joined to domain Deploying a Test Windows Environment in a KVM Infrastucture" width="1042" height="886" class="alignnone size-full wp-image-7992" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Indicating that I was part of the "elatov" domain :) After I logged in I checked the IP settings and I saw the following:

    C:\Users\Administrator>ipconfig /all
    
    Windows IP Configuration
    
       Host Name . . . . . . . . . . . . : dc
       Primary Dns Suffix  . . . . . . . : elatov.local
       Node Type . . . . . . . . . . . . : Hybrid
       IP Routing Enabled. . . . . . . . : No
       WINS Proxy Enabled. . . . . . . . : No
       DNS Suffix Search List. . . . . . : elatov.local
    
    Ethernet adapter Local Area Connection:
    
       Connection-specific DNS Suffix  . :
       Description . . . . . . . . . . . : Realtek RTL8139C+ Fast Ethernet NIC
       Physical Address. . . . . . . . . : 52-54-00-07-BA-A3
       DHCP Enabled. . . . . . . . . . . : No
       Autoconfiguration Enabled . . . . : Yes
       IPv4 Address. . . . . . . . . . . : 192.168.250.47(Preferred)
       Subnet Mask . . . . . . . . . . . : 255.255.255.0
       Default Gateway . . . . . . . . . : 192.168.250.1
       DNS Servers . . . . . . . . . . . : 127.0.0.1
       NetBIOS over Tcpip. . . . . . . . : Enabled
    

### SETUP AN IIS SERVER

Now let's install an IIS Server. From the Run Dialogue, enter

    servermanager.msc
    

You will then start up the Server Manager:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/servermanager_started1.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/servermanager_started1.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/servermanager_started1.png" alt="servermanager started1 Deploying a Test Windows Environment in a KVM Infrastucture" width="796" height="552" class="alignnone size-full wp-image-8047" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Click on "Roles":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/servermanager_roles-selected.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/servermanager_roles-selected.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/servermanager_roles-selected.png" alt="servermanager roles selected Deploying a Test Windows Environment in a KVM Infrastucture" width="795" height="551" class="alignnone size-full wp-image-7994" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then click on "Add Roles", and you will see the "Add Roles Wizard":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/add_roles_wizard.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/add_roles_wizard.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/add_roles_wizard.png" alt="add roles wizard Deploying a Test Windows Environment in a KVM Infrastucture" width="773" height="568" class="alignnone size-full wp-image-7995" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Select "Web Server (IIS)":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/Web_server_selected_roles_wiz.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/Web_server_selected_roles_wiz.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/Web_server_selected_roles_wiz.png" alt="Web server selected roles wiz Deploying a Test Windows Environment in a KVM Infrastucture" width="775" height="570" class="alignnone size-full wp-image-7996" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then click "Next" until you get to the "Select Role Services":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/web_server_role_services.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/web_server_role_services.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/web_server_role_services.png" alt="web server role services Deploying a Test Windows Environment in a KVM Infrastucture" width="773" height="569" class="alignnone size-full wp-image-7997" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

From here you can select the necessary components of IIS that you need. After you have selected the necessary components, click "Next" and then finally click "Install" to start the install:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/iis_installing.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/iis_installing.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/iis_installing.png" alt="iis installing Deploying a Test Windows Environment in a KVM Infrastucture" width="772" height="568" class="alignnone size-full wp-image-7998" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

After the install is finished, open Internet Explorer and point it to **http://localhost** you should see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/IE_IIS_Installed.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/IE_IIS_Installed.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/IE_IIS_Installed.png" alt="IE IIS Installed Deploying a Test Windows Environment in a KVM Infrastucture" width="794" height="549" class="alignnone size-full wp-image-7999" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Now let's go ahead and enable SSL on our IIS Server. From the Run Dialog enter

    inetmgr
    

and you will see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/inetmgr_started.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/inetmgr_started.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/inetmgr_started.png" alt="inetmgr started Deploying a Test Windows Environment in a KVM Infrastucture" width="763" height="528" class="alignnone size-full wp-image-8000" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then click on IIS Instance and on the right side you will see a lot of options. Among those options you will see "Server Certificates":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/inetmgr_server_certificates.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/inetmgr_server_certificates.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/inetmgr_server_certificates.png" alt="inetmgr server certificates Deploying a Test Windows Environment in a KVM Infrastucture" width="907" height="610" class="alignnone size-full wp-image-8001" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then double click on "Server Certificates" and you will see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/server_certificated_from_iis.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/server_certificated_from_iis.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/server_certificated_from_iis.png" alt="server certificated from iis Deploying a Test Windows Environment in a KVM Infrastucture" width="906" height="609" class="alignnone size-full wp-image-8002" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then from the "Right Pane" select "Create Self-Signed Certificate" and the Wizard will start up. Enter the name of the site:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_self_signed_certificate.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/create_self_signed_certificate.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/create_self_signed_certificate.png" alt="create self signed certificate Deploying a Test Windows Environment in a KVM Infrastucture" width="576" height="439" class="alignnone size-full wp-image-8003" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then click "OK". You will then see the following under the "Server Certificates":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/server_certificates_self_signed_cert_created.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/server_certificates_self_signed_cert_created.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/server_certificates_self_signed_cert_created.png" alt="server certificates self signed cert created Deploying a Test Windows Environment in a KVM Infrastucture" width="906" height="611" class="alignnone size-full wp-image-8004" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Now that we have an SSL certificate, we need to enable IIS to listen on port 443. This is done by expanding IIS Instance then expanding the Sites folder and selecting "Default Web Site":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/inetmgr_default_site_selected.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/inetmgr_default_site_selected.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/inetmgr_default_site_selected.png" alt="inetmgr default site selected Deploying a Test Windows Environment in a KVM Infrastucture" width="907" height="609" class="alignnone size-full wp-image-8005" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

On the right you will see an option called "Binding". Click on that and the following will show up:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/inetmgr_site_bindings.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/inetmgr_site_bindings.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/inetmgr_site_bindings.png" alt="inetmgr site bindings Deploying a Test Windows Environment in a KVM Infrastucture" width="482" height="223" class="alignnone size-full wp-image-8006" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then click "add" and you see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/add_site_binding_inetmgr.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/add_site_binding_inetmgr.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/add_site_binding_inetmgr.png" alt="add site binding inetmgr Deploying a Test Windows Environment in a KVM Infrastucture" width="399" height="214" class="alignnone size-full wp-image-8007" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then change the type to 'https" and select the SSL certificate that we created. In the end it will look like this:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/add_site_binding_filled_out_inetmgr.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/add_site_binding_filled_out_inetmgr.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/add_site_binding_filled_out_inetmgr.png" alt="add site binding filled out inetmgr Deploying a Test Windows Environment in a KVM Infrastucture" width="400" height="216" class="alignnone size-full wp-image-8008" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then click "OK" and "Close" and you should be all set. From Internet Explorer go to **https://localhost** and make you see the same page as before:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/IE_with_https_IIS.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/IE_with_https_IIS.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/IE_with_https_IIS.png" alt="IE with https IIS Deploying a Test Windows Environment in a KVM Infrastucture" width="797" height="552" class="alignnone size-full wp-image-8009" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

### Setup A Windows 7 Client

Install Windows 7 on another VM. Give it an IP and make sure the primary DNS points to the new DC server that we setup. Here is how the IP configuration looked like on my Windows 7 Client:

    C:\Users\elatov>ipconfig /all
    
    Windows IP Configuration
    
       Host Name . . . . . . . . . . . . : client
       Primary Dns Suffix  . . . . . . . :
       Node Type . . . . . . . . . . . . : Hybrid
       IP Routing Enabled. . . . . . . . : No
       WINS Proxy Enabled. . . . . . . . : No
    
    Ethernet adapter Local Area Connection:
    
       Connection-specific DNS Suffix  . :
       Description . . . . . . . . . . . : Realtek RTL8139C+ Fast Ethernet NIC
       Physical Address. . . . . . . . . : 52-54-00-5D-FB-1D
       DHCP Enabled. . . . . . . . . . . : No
       Autoconfiguration Enabled . . . . : Yes
       IPv4 Address. . . . . . . . . . . : 192.168.101.47(Preferred)
       Subnet Mask . . . . . . . . . . . : 255.255.255.0
       Default Gateway . . . . . . . . . : 192.168.101.1
       DNS Servers . . . . . . . . . . . : 192.168.250.47
                                           192.168.101.1
       NetBIOS over Tcpip. . . . . . . . : Enabled
    

Do a test on the client, run **nslookup** to make sure it works:

    C:\Users\elatov>nslookup dc.elatov.local
    Server:  UnKnown
    Address:  192.168.250.47
    
    Name:    dc.elatov.local
    Address:  192.168.250.47
    

Now let's join our Windows 7 client to our domain. From the Run dialogue type in

    sysdm.cpl
    

and then you will see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/sys_properties_client.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/sys_properties_client.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/sys_properties_client.png" alt="sys properties client Deploying a Test Windows Environment in a KVM Infrastucture" width="412" height="458" class="alignnone size-full wp-image-8013" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then click "Change" and fill out the Computer Name and the domain you want to join:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/filled_out_join_domain.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/filled_out_join_domain.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/filled_out_join_domain.png" alt="filled out join domain Deploying a Test Windows Environment in a KVM Infrastucture" width="325" height="387" class="alignnone size-full wp-image-8014" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then click "OK" and you will need to enter the Domain Administrator's credentials to allow this machine to join:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/join_domain_security_creds.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/join_domain_security_creds.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/join_domain_security_creds.png" alt="join domain security creds Deploying a Test Windows Environment in a KVM Infrastucture" width="426" height="255" class="alignnone size-full wp-image-8015" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

If all is successful you will see the following prompt:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/successful_join_to_domain.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/successful_join_to_domain.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/successful_join_to_domain.png" alt="successful join to domain Deploying a Test Windows Environment in a KVM Infrastucture" width="305" height="159" class="alignnone size-full wp-image-8016" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

And then it will ask you restart the machine. While the machine is restarting go the DC Server and run:

    dsa.msc
    

That should show you the "Active Directory Users and Computers" dialogue:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/dsa_msc.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/dsa_msc.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/dsa_msc.png" alt="dsa msc Deploying a Test Windows Environment in a KVM Infrastucture" width="764" height="533" class="alignnone size-full wp-image-8017" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Expand your domain (elatov.local), then go to "Computers" and you should see the newly added computer there:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/dsa_comp_added.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/dsa_comp_added.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/dsa_comp_added.png" alt="dsa comp added Deploying a Test Windows Environment in a KVM Infrastucture" width="760" height="533" class="alignnone size-full wp-image-8018" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

In the same window add a new user by right clicking on the "Users" folder and selecting "Add" -> "User":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/add_user_from_dsa_msc_g.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/add_user_from_dsa_msc_g.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/add_user_from_dsa_msc_g.png" alt="add user from dsa msc g Deploying a Test Windows Environment in a KVM Infrastucture" width="761" height="532" class="alignnone size-full wp-image-8022" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Here is an example of one user I added:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/add_user_dialogue.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/add_user_dialogue.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/add_user_dialogue.png" alt="add user dialogue Deploying a Test Windows Environment in a KVM Infrastucture" width="434" height="364" class="alignnone size-full wp-image-8019" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

After we are done adding the user we will see it in the User's list:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/dsa_msc_user_addedd.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/dsa_msc_user_addedd.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/dsa_msc_user_addedd.png" alt="dsa msc user addedd Deploying a Test Windows Environment in a KVM Infrastucture" width="761" height="530" class="alignnone size-full wp-image-8024" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then from the client log in with that user, like so:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/login_to_domain_from_windows7_client.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/login_to_domain_from_windows7_client.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/login_to_domain_from_windows7_client.png" alt="login to domain from windows7 client Deploying a Test Windows Environment in a KVM Infrastucture" width="1058" height="881" class="alignnone size-full wp-image-8020" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

To login to the local user, hit switch user and then type: **.\elatov** along the password:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/login_to_local_windows7_client.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/login_to_local_windows7_client.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/login_to_local_windows7_client.png" alt="login to local windows7 client Deploying a Test Windows Environment in a KVM Infrastucture" width="1058" height="881" class="alignnone size-full wp-image-8021" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Another check you can do is make sure the machine was added to DNS. From the DC/DNS Server run:

    dnsmgmt.msc
    

and you will see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/dnsmgmt_started.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/dnsmgmt_started.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/dnsmgmt_started.png" alt="dnsmgmt started Deploying a Test Windows Environment in a KVM Infrastucture" width="760" height="528" class="alignnone size-full wp-image-8025" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Expand the "Forward Lookup Zones" and then select your domain (elatov.local), on the right side you will see an "A" record for the newly joined machine:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/dnsmgmt_added_machine.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/dnsmgmt_added_machine.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/dnsmgmt_added_machine.png" alt="dnsmgmt added machine Deploying a Test Windows Environment in a KVM Infrastucture" width="829" height="465" class="alignnone size-full wp-image-8026" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

### Connect To KVM Virtual Machines from a Local Machine Using VNCViewer

If you don't want to go through the **virt-manager** to open up a console, you can use **virsh** to determine what *vncdisplay* a VM is using and then connect to that from your local machine. First determine the *vncdisplay* of the desired VM:

    [elatov@klaptop ~]$ virsh -c qemu+ssh://virtuser@kvm/system list
    Id Name State
    ----------------------------------------------------
    1  VM1                 running
    2  VM2                 running
    16 kelatov_win7_client running
    20 kelatov_Win2k8_DC   running
    23 kelatov_win2k8_iis  running
    

and then this:

    [elatov@klaptop ~]$ virsh -c qemu+ssh://virtuser@kvm/system vncdisplay kelatov_win2k8_iis
    :15
    

now we know the VNC display is **:15**, so let's connect to that VNC display:

    [elatov@klaptop ~]$ vncviewer kvm:15
    

If don't want to open up a the firewall to allow the port range for VNC Connections (ie 5900 - 5999), you could use the KVM Host as an SSH tunnel. Here is how the command would look for that:

    [elatov@klaptop ~]$ vncviewer -via virtuser@kvm 127.0.0.1:15
    

### Enabling Copy and Paste on a Windows KVM VM

To enable Copy and paste within any KVM VM, we need to use **Spice**, more information on Spice can be seen at their home <a href="http://spice-space.org/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://spice-space.org/']);">page</a>. The first thing to check is to make sure your KVM server supports spice (more information regarding spice and libvirt can be seen <a href="http://wiki.centos.org/HowTos/Spice-libvirt" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://wiki.centos.org/HowTos/Spice-libvirt']);">here</a>). This is done by checking if the following RPMs are installed:

    [virtuser@kvm ~]$ rpm -qa | grep spice
    spice-gtk-python-0.6-2.el6.x86_64
    spice-server-0.8.2-5.el6.x86_64
    spice-glib-0.6-2.el6.x86_64
    spice-gtk-0.6-2.el6.x86_64
    

If those are installed then we can enable Spice on our VMs. To enable Spice on a KVM machine, first shut off the VM. Then from Virt-Manager select the VM, and then go to "Edit" -> "Virtual Machine Details":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/edit_vm_details.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/edit_vm_details.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/edit_vm_details.png" alt="edit vm details Deploying a Test Windows Environment in a KVM Infrastucture" width="979" height="379" class="alignnone size-full wp-image-7965" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Notice the VM is a "Shutoff" state, at that point the Console to the VM will open:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_vm_off.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_vm_off.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_vm_off.png" alt="vm details vm off Deploying a Test Windows Environment in a KVM Infrastucture" width="825" height="725" class="alignnone size-full wp-image-7966" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Right Next to the "Console" button there is a "Details" button, by clicking that you will see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_window.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_window.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_window.png" alt="vm details window Deploying a Test Windows Environment in a KVM Infrastucture" width="825" height="725" class="alignnone size-full wp-image-7967" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

From the left pane, select "Video" and you will see this:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_video_panel.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_video_panel.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_video_panel.png" alt="vm details video panel Deploying a Test Windows Environment in a KVM Infrastucture" width="825" height="725" class="alignnone size-full wp-image-7968" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Change the Model from "vga" to "qxl":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_change_video_to_glx.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_change_video_to_glx.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_change_video_to_glx.png" alt="vm details change video to glx Deploying a Test Windows Environment in a KVM Infrastucture" width="825" height="725" class="alignnone size-full wp-image-7969" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then select "Apply" and then select "Display VNC" and you will see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_vnc_display_panel.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_vnc_display_panel.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_vnc_display_panel.png" alt="vm details vnc display panel Deploying a Test Windows Environment in a KVM Infrastucture" width="825" height="725" class="alignnone size-full wp-image-7970" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then change the "Type" from "VNC" to "Spice", and then click on "Apply", as soon as you hit apply you will see the following pop up:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_change_from_vnc_spice_pop_up.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_change_from_vnc_spice_pop_up.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_change_from_vnc_spice_pop_up.png" alt="vm details change from vnc spice pop up Deploying a Test Windows Environment in a KVM Infrastucture" width="382" height="152" class="alignnone size-full wp-image-7971" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Click "Yes", after you click "Yes" you will see a new device added called "Channel" like so:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_spice_channel_added.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_spice_channel_added.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/vm_details_spice_channel_added.png" alt="vm details spice channel added Deploying a Test Windows Environment in a KVM Infrastucture" width="825" height="725" class="alignnone size-full wp-image-7972" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

From there you can click "Run" and the VM will start booting, don't forget to switch from "Details" to "Console" to see the VM's boot process.

After the VM is booted up we need to install the Spice Guest Tools. Open a browse from within the VM and go to **www.spice-space.org/download.html**:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/spice_download_page_from_vm.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/spice_download_page_from_vm.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/spice_download_page_from_vm.png" alt="spice download page from vm Deploying a Test Windows Environment in a KVM Infrastucture" width="814" height="712" class="alignnone size-full wp-image-7973" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Scroll down until you see the "Windows guest tools":

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/spice_guest_windows_tools_from_vm_in_IE.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/spice_guest_windows_tools_from_vm_in_IE.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/spice_guest_windows_tools_from_vm_in_IE.png" alt="spice guest windows tools from vm in IE Deploying a Test Windows Environment in a KVM Infrastucture" width="814" height="712" class="alignnone size-full wp-image-7974" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Download the tools onto the desktop:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/spice_tools_downloaded_on_desktop_in_VM.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/spice_tools_downloaded_on_desktop_in_VM.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/spice_tools_downloaded_on_desktop_in_VM.png" alt="spice tools downloaded on desktop in VM Deploying a Test Windows Environment in a KVM Infrastucture" width="814" height="712" class="alignnone size-full wp-image-7975" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

Then double click on the installer and follow the onscreen instructions:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/spice_installer_in_VM.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/spice_installer_in_VM.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/spice_installer_in_VM.png" alt="spice installer in VM Deploying a Test Windows Environment in a KVM Infrastucture" width="814" height="712" class="alignnone size-full wp-image-7976" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

After the install is done, restart the VM one more time. After the VM reboots you should be able to copy and paste from the console of virt-manager. If you check for running tasks you will see the following processes:

    C:\Users\Administrator>tasklist | findstr vd
    vdservice.exe                  612 Services                   0      3,512 K
    vdagent.exe                    912 Console                    1      3,488 K
    

I actually copied that from the guest :) 

### Using Spice Clients to Connect to Spice-Enabled VMs

If you don't want to use the **virt-manager** console, we can use other Spice clients. First we need to determine what port is used for the Spice connection:

    [elatov@klaptop ~]$ virsh -c qemu+ssh://virtuser@kvm/system dumpxml kelatov_win7_client | grep spice | grep port
    ## <graphics type='spice' port='5913' tlsPort='-1' autoport='yes'></graphics>
    

So we are on port **5913**, now let's setup an SSH tunnel:

    [elatov@klaptop ~]$ ssh -L 5913:localhost:5913 virtuser@kvm
    

Then we can use a spice client:

*   spicec (from the *spice-client* package)
*   spicy (from the *spice-gtk-tools* package)
*   remote-viewer (from the *virt-viewer* package)

and connect to **localhost:5913**

Here are examples of each:

    [elatov@klaptop ~]$ spicec -h localhost -p 5913 
    

Here is how it looks like:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/spicec_logged_in.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/spicec_logged_in.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/spicec_logged_in.png" alt="spicec logged in Deploying a Test Windows Environment in a KVM Infrastucture" width="1026" height="797" class="alignnone size-full wp-image-7946" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

or with **remote-viewer**:

    [elatov@klaptop ~]$ remote-viewer spice://localhost:5913
    

Here is how that will look:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/remote-viewer_spice.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/remote-viewer_spice.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/remote-viewer_spice.png" alt="remote viewer spice Deploying a Test Windows Environment in a KVM Infrastucture" width="1026" height="818" class="alignnone size-full wp-image-7947" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a> and lastly with **spicy**:

    [elatov@klaptop ~]$ spicy 
    

That will launch a GUI and then you can fill out the necessary information, like so:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/sipcy_filled_out.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/sipcy_filled_out.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/sipcy_filled_out.png" alt="sipcy filled out Deploying a Test Windows Environment in a KVM Infrastucture" width="428" height="448" class="alignnone size-full wp-image-7949" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

and then you will see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2013/04/spicy_connected.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2013/04/spicy_connected.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2013/04/spicy_connected.png" alt="spicy connected Deploying a Test Windows Environment in a KVM Infrastucture" width="1026" height="903" class="alignnone size-full wp-image-7950" title="Deploying a Test Windows Environment in a KVM Infrastucture" /></a>

<div class="SPOSTARBUST-Related-Posts">
  <H3>
    Related Posts
  </H3>
  
  <ul class="entry-meta">
    <li class="SPOSTARBUST-Related-Post">
      <a title="Configure AD Replication with Windows 2008" href="http://virtuallyhyper.com/2013/06/configure-ad-replication-with-windows-2008/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2013/06/configure-ad-replication-with-windows-2008/']);" rel="bookmark">Configure AD Replication with Windows 2008</a>
    </li>
    <li class="SPOSTARBUST-Related-Post">
      <a title="Enabling LDAPS on Windows 2008 Active Directory Server" href="http://virtuallyhyper.com/2013/06/enabling-ldaps-on-windows-2008-active-directory-server/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2013/06/enabling-ldaps-on-windows-2008-active-directory-server/']);" rel="bookmark">Enabling LDAPS on Windows 2008 Active Directory Server</a>
    </li>
  </ul>
</div>
