---
title: OpenVAS on CentOS
author: Karim Elatov
layout: post
permalink: /2014/05/openvas-centos/
sharing_disabled:
  - 1
dsq_thread_id:
  - 2614800897
categories:
  - Home Lab
  - OS
tags:
  - Arachni
  - CentOs
  - OpenVAS
  - Wapiti
---
## OpenVAS

I wanted to run a vulnerability scan against my home lab to see if snort catches the event (snort setup <a href="http://virtuallyhyper.com/2014/04/snort-debian/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2014/04/snort-debian/']);">here</a>). As I kept reading up on scanners, I ran into <a href="http://www.openvas.org/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://www.openvas.org/']);">OpenVAS</a>. From their site:

> OpenVAS is a framework of several services and tools offering a comprehensive and powerful vulnerability scanning and vulnerability management solution.
    

From their <a href="http://www.openvas.org/software.html" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://www.openvas.org/software.html']);">software</a> page here are the components of OpenVAS:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/openvas-components.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/openvas-components.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/openvas-components.png" alt="openvas components OpenVAS on CentOS" width="758" height="409" class="alignnone size-full wp-image-10592" title="OpenVAS on CentOS" /></a>

The software looked good to me, so I decided to install OpenVAS on CentOS.

### Install OpenVAS on CentOS

Most of the instructions are laid out in <a href="http://www.openvas.org/install-packages.html#openvas_centos_atomic" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://www.openvas.org/install-packages.html#openvas_centos_atomic']);">OpenVAS for CentOS via Atomic</a>. You just need to enable the atomic YUM repository:

    wget -q -O - http://www.atomicorp.com/installers/atomic | sudo sh
    

Get the installer first to see what it does (for those who don't just want to run an unknown script from the web). When I was installing **ossec** (check out the post on that <a href="http://virtuallyhyper.com/2014/04/ossec-freebsd/" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/2014/04/ossec-freebsd/']);">here</a>), I ended up doing that (so I already that had YUM repository enabled). Then to install the software, run the following:

    elatov@ccl:~$sudo yum install openvas
    

After the install is finished, run the following to set it up:

    elatov@ccl:~$sudo openvas-setup
    [i] Initializing CERT advisory database
    [i] Updating /var/lib/openvas/cert-data/dfn-cert-2008.xml
    [i] Updating /var/lib/openvas/cert-data/dfn-cert-2009.xml
    [i] Updating /var/lib/openvas/cert-data/dfn-cert-2010.xml
    [i] Updating /var/lib/openvas/cert-data/dfn-cert-2011.xml
    [i] Updating /var/lib/openvas/cert-data/dfn-cert-2012.xml
    [i] Updating /var/lib/openvas/cert-data/dfn-cert-2013.xml
    
    [i] Initializing scap database
    [i] Updating CPEs
    [i] Updating /var/lib/openvas/scap-data/nvdcve-2.0-2002.xml
    [i] Updating /var/lib/openvas/scap-data/nvdcve-2.0-2003.xml
    [i] Updating /var/lib/openvas/scap-data/nvdcve-2.0-2004.xml
    [i] Updating /var/lib/openvas/scap-data/nvdcve-2.0-2005.xml
    [i] Updating /var/lib/openvas/scap-data/nvdcve-2.0-2006.xml
    [i] Updating /var/lib/openvas/scap-data/nvdcve-2.0-2007.xml
    [i] Updating /var/lib/openvas/scap-data/nvdcve-2.0-2008.xml
    [i] Updating /var/lib/openvas/scap-data/nvdcve-2.0-2009.xml
    [i] Updating /var/lib/openvas/scap-data/nvdcve-2.0-2011.xml
    [i] Updating /var/lib/openvas/scap-data/nvdcve-2.0-2012.xml
    [i] Updating /var/lib/openvas/scap-data/nvdcve-2.0-2013.xml
    [i] Updating /var/lib/openvas/scap-data/nvdcve-2.0-2014.xml
    [i] Updating CVSS scores and CVE counts
    [i] Updating OVAL data
    [i] Updating /var/lib/openvas/scap-data/oval/5.10/org.mitre.oval/c/oval.xml
    [i] Updating /var/lib/openvas/scap-data/oval/5.10/org.mitre.oval/i/oval.xml
    [i] Updating /var/lib/openvas/scap-data/oval/5.10/org.mitre.oval/m/oval.xml
    [i] Updating /var/lib/openvas/scap-data/oval/5.10/org.mitre.oval/p/oval.xml
    [i] Updating /var/lib/openvas/scap-data/oval/5.10/org.mitre.oval/v/family/ios.xml
    [i] Updating /var/lib/openvas/scap-data/oval/5.10/org.mitre.oval/v/family/macos.xml
    [i] Updating /var/lib/openvas/scap-data/oval/5.10/org.mitre.oval/v/family/pixos.xml
    [i] Updating /var/lib/openvas/scap-data/oval/5.10/org.mitre.oval/v/family/unix.xml
    [i] Updating /var/lib/openvas/scap-data/oval/5.10/org.mitre.oval/v/family/windows.xml
    [i] No user data directory '/var/lib/openvas/scap-data/private' found.
    Updating OpenVAS Manager database....
    Step 2: Configure GSAD
    The Greenbone Security Assistant is a Web Based front end
    for managing scans. By default it is configured to only allow
    connections from localhost.
    
    Allow connections from any IP? [Default: yes] no
    Stopping greenbone-security-assistant:                     [  OK  ]
    Starting greenbone-security-assistant:                     [  OK  ]
    
    Step 3: Choose the GSAD admin users password.
    The admin user is used to configure accounts,
    Update NVT's manually, and manage roles.
    
    Enter administrator username [Default: admin] :
    Enter Administrator Password:
    Verify Administrator Password:
    
    ad   main:MESSAGE:8686:2014-04-08 11h55.21 MDT: No rules file provided, the new user will have no restrictions.
    ad   main:MESSAGE:8686:2014-04-08 11h55.21 MDT: User admin has been successfully created.
    
    Step 4: Create a user
    
    Using /var/tmp as a temporary file holder.
    
    Add a new openvassd user
    ---------------------------------
    
    
    Login : user
    Authentication (pass/cert) [pass] :
    Login password :
    Login password (again) :
    
    User rules
    ---------------
    openvassd has a rules system which allows you to restrict the hosts that openvas has the right to test.
    For instance, you may want him to be able to scan his own host only.
    
    Please see the openvas-adduser(8) man page for the rules syntax.
    
    Enter the rules for this user, and hit ctrl-D once you are done:
    (the user can have an empty rules set)
    
    
    Login             : user
    Password          : ***********
    
    Rules             :
    
    
    Is that ok? (y/n) [y] y
    user added.
    
    Starting openvas-administrator...
    Starting openvas-administrator:           [  OK  ]
    
    Setup complete, you can now access GSAD at:
    
    https://localhost:9392
    
    

If you want you can manually get the NVT signatures:

    elatov@ccl:~$sudo openvas-nvt-sync
    [i] This script synchronizes an NVT collection with the 'OpenVAS NVT Feed'.
    [i] The 'OpenVAS NVT Feed' is provided by 'The OpenVAS Project'.
    [i] Online information about this feed: 'http://www.openvas.org/openvas-nvt-feed.html'.
    [i] NVT dir: /var/lib/openvas/plugins
    [i] Will use rsync
    [i] Using rsync: /usr/bin/rsync
    [i] Configured NVT rsync feed: rsync://feed.openvas.org:/nvt-feed
    [w] Private directory '/var/lib/openvas/plugins/private' not found.
    [w] Non-feed NVTs not migrated there will be deleted by rsync.
    Run migration now ([y/n], any other input aborts)? y
    
    [i] Migrating non-OpenVAS files to private sub-directory 'private' of NVT directory '/var/lib/openvas/plugins'. This can take a few minutes.
    [i] Migration done.
    OpenVAS feed server - http://openvas.org/
    This service is hosted by Intevation GmbH - http://intevation.de/
    All transactions are logged.
    Please report problems to admin@intevation.de
    
    receiving incremental file list
    deleting nvt/
    deleting gsf/
    ./
    2013/
    
    sent 64 bytes  received 1365947 bytes  248365.64 bytes/sec
    total size is 185148946  speedup is 135.54
    [i] Checking dir: ok
    [i] Checking MD5 checksum:
    

Before you start the software, we will need to rebuild the database:

    elatov@ccl:~$sudo openvasmd --rebuild
    

After that we can start the scanner:

    elatov@ccl:~$sudo service openvas-scanner start
    Starting openvas-scanner: base gpgme-Message: Setting GnuPG homedir to '/etc/openvas/gnupg'
    base gpgme-Message: Using OpenPGP engine version '2.0.14'
    

At this point all the components should be up and running:

    elatov@ccl:~$sudo service openvas-administrator status
    openvas-administrator (pid  25590) is running...
    elatov@ccl:~$sudo service openvas-scanner status
    openvassd (pid  14431) is running...
    elatov@ccl:~$sudo service openvas-manager status
    openvas-manager (pid  14442) is running...
    elatov@ccl:~$sudo service gsad status
    gsad (pid  8681) is running...
    

If you need to make sure the configuration is all good, you can run the following:

    elatov@ccl:~$sudo /usr/bin/openvas-check-setup
    

### Running a scan with OpenVAS

Go to the GreenBone Security Assistant (https://IP:9293) and you should see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/greenbone-assistant.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/greenbone-assistant.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/greenbone-assistant.png" alt="greenbone assistant OpenVAS on CentOS" width="681" height="513" class="alignnone size-full wp-image-10594" title="OpenVAS on CentOS" /></a>

After logging in you should see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-first-page.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-first-page.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-first-page.png" alt="gsa first page OpenVAS on CentOS" width="922" height="496" class="alignnone size-full wp-image-10595" title="OpenVAS on CentOS" /></a>

Then add a target host, by going to **Configuration** -> **Targets**:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-conf-targets.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-conf-targets.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-conf-targets.png" alt="gsa conf targets OpenVAS on CentOS" width="335" height="288" class="alignnone size-full wp-image-10597" title="OpenVAS on CentOS" /></a>

Then click on the *star* to add a target:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-new-target-button.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-new-target-button.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-new-target-button.png" alt="gsa new target button OpenVAS on CentOS" width="155" height="73" class="alignnone size-full wp-image-10598" title="OpenVAS on CentOS" /></a>

Fill out all the information:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-new-target-filled-out.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-new-target-filled-out.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-new-target-filled-out.png" alt="gsa new target filled out OpenVAS on CentOS" width="559" height="280" class="alignnone size-full wp-image-10599" title="OpenVAS on CentOS" /></a>

Then click on "**Create Target**" and you should see your new target added:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-list-of-targets.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-list-of-targets.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-list-of-targets.png" alt="gsa list of targets OpenVAS on CentOS" width="546" height="214" class="alignnone size-full wp-image-10600" title="OpenVAS on CentOS" /></a>

Then let's create a task to run a scan against our newly created target. Go to **Scan Management** -> **New Task**:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-sm-nt.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-sm-nt.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/gsa-sm-nt.png" alt="gsa sm nt OpenVAS on CentOS" width="245" height="144" class="alignnone size-full wp-image-10601" title="OpenVAS on CentOS" /></a>

Then fill out all the information:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/new-task-filledout.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/new-task-filledout.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/new-task-filledout.png" alt="new task filledout OpenVAS on CentOS" width="463" height="425" class="alignnone size-full wp-image-10602" title="OpenVAS on CentOS" /></a>

Click on "**Create Task**" and the task should be ready:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/task-created.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/task-created.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/task-created.png" alt="task created OpenVAS on CentOS" width="341" height="166" class="alignnone size-full wp-image-10603" title="OpenVAS on CentOS" /></a>

BTW here are all the available scan configuration from the default install:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/scan-configuration-gsa.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/scan-configuration-gsa.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/scan-configuration-gsa.png" alt="scan configuration gsa OpenVAS on CentOS" width="748" height="370" class="alignnone size-full wp-image-10604" title="OpenVAS on CentOS" /></a>

Lastly click on the *play* button to start the scan:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/start-scan-button-gsa.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/start-scan-button-gsa.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/start-scan-button-gsa.png" alt="start scan button gsa OpenVAS on CentOS" width="922" height="187" class="alignnone size-full wp-image-10605" title="OpenVAS on CentOS" /></a>

Here is the scan kicked off:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/scan-started.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/scan-started.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/scan-started.png" alt="scan started OpenVAS on CentOS" width="377" height="172" class="alignnone size-full wp-image-10606" title="OpenVAS on CentOS" /></a>

### Getting OpenVAS Scan Results

After the scan is finished, you will see the following under the **Tasks** section:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/scan-finished.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/scan-finished.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/scan-finished.png" alt="scan finished OpenVAS on CentOS" width="339" height="166" class="alignnone size-full wp-image-10609" title="OpenVAS on CentOS" /></a>

Click on the scan and you will see the details:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/task-details-gsa.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/task-details-gsa.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/task-details-gsa.png" alt="task details gsa OpenVAS on CentOS" width="911" height="534" class="alignnone size-full wp-image-10610" title="OpenVAS on CentOS" /></a>

Then click on the "**Magnifying Glass**" (to see the details of the report) and you will see the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/report-details-gsa.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/report-details-gsa.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/report-details-gsa.png" alt="report details gsa OpenVAS on CentOS" width="909" height="292" class="alignnone size-full wp-image-10611" title="OpenVAS on CentOS" /></a>

From there you can download the the PDF version of the scan results.

### Fixing the Watipi and Arachni Warnings

Initially I was see the following warnings on my scan results:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/arachni-missing-in-results.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/arachni-missing-in-results.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/arachni-missing-in-results.png" alt="arachni missing in results OpenVAS on CentOS" width="599" height="258" class="alignnone size-full wp-image-10612" title="OpenVAS on CentOS" /></a>

> Arachni could not be found in your system path.

And also the following:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/wapiti-empty-report.png" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/wapiti-empty-report.png']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/wapiti-empty-report.png" alt="wapiti empty report OpenVAS on CentOS" width="594" height="250" class="alignnone size-full wp-image-10613" title="OpenVAS on CentOS" /></a>

> wapiti report filename is empty.

For the **arachni** one, I actually didn't have that installed. To install that, get the package:

    elatov@ccl:~$wget http://downloads.arachni-scanner.com/arachni-0.4.6-0.4.3-linux-x86_64.tar.gz
    

Then extract the software:

    elatov@ccl:~$tar xzvf arachni-0.4.6-0.4.3-linux-x86_64.tar.gz
    

Lastly install in under **/usr/local**:

    elatov@ccl:~$sudo mv arachni-0.4.6-0.4.3 /usr/local/.
    

For ease of use, let's create a symlink:

    elatov@ccl:~$sudo ln -s /usr/local/arachni-0.4.6-0.4.3 /usr/local/arachni
    

Then lastly symlink the binaries under **/usr/bin**:

    elatov@ccl:~$sudo ln -s /usr/local/arachni/bin/arachni* /usr/bin/
    elatov@ccl:~$sudo ln -s /usr/local/arachni/bin/readlink_f.sh /usr/bin/
    

After that it started working, and I saw the following in my reports:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/arachni-results-included_i.jpg" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/arachni-results-included_i.jpg']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/arachni-results-included_i.jpg" alt="arachni results included i OpenVAS on CentOS" width="691" height="585" class="alignnone size-full wp-image-10614" title="OpenVAS on CentOS" /></a>

For **wapiti**, I had the package for that installed:

    elatov@ccl:~$rpm -qa | grep wapiti
    wapiti-2.3.0-5.el6.art.noarch
    

But it wasn't working for some reason. I ran the scan manually:

    elatov@ccl:~$sudo openvas-nasl -t localhost -T log -X -i /var/lib/openvas/plugins /var/lib/openvas/plugins/remote-web-wapiti.nasl
    

and then I realized I was missing a python module. Not sure why it wasn't installed. Upon installing the following:

    elatov@ccl:~$sudo yum install python-requests
    

and then running the command again, I saw the following:

    elatov@ccl:~$sudo openvas-nasl -t localhost -T log -X -i /var/lib/openvas/plugins /var/lib/openvas/plugins/remote-web-wapiti.nasl
    Here is the wapiti report:
    ********************************************************************************
                         Wapiti 2.3.0 - wapiti.sourceforge.net
                            Report for http://127.0.0.1:80
                  Date of the scan : Tue, 15 Apr 2014 22:21:40 +0000
                              Scope of the scan : folder
    ********************************************************************************
    Summary of vulnerabilities :
    ----------------------------
    ********************************************************************************
    
                                    Anomalies found:
    
    
    --- End of report ---
    

It looks like it worked this time around. Running another scan and the results showed the same:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/watipi-results-included_i1.jpg" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/watipi-results-included_i1.jpg']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/watipi-results-included_i1.jpg" alt="watipi results included i1 OpenVAS on CentOS" width="693" height="307" class="alignnone size-full wp-image-10623" title="OpenVAS on CentOS" /></a>

### Checking out the Progress of a Scan

You can enable extra logging on the scanner. Here is the option for that:

    elatov@ccl:~$grep log_whole_attack /etc/openvas/openvassd.conf
    log_whole_attack = yes
    

After that's enabled, stop the service and then start it back up:

    elatov@ccl:~$sudo service openvas-scanner stop
    elatov@ccl:~$sudo service openvas-scanner start
    

Then if you start another scan, you will see the following in the logs:

    elatov@ccl:~$tail /var/log/openvas/openvassd.log
    [Tue Apr 15 21:47:47 2014][999] user om starts a new scan. Target(s) : 192.168.2.100, with max_hosts = 20 and max_checks = 4
    [Tue Apr 15 21:47:47 2014][999] user om : testing 192.168.2.100 (::ffff:192.168.2.100) [1049]
    [Tue Apr 15 21:47:47 2014][1049] user om : new KB will be saved as /var/lib/openvas/users/om/kbs/192.168.2.100
    

Then if you check out the KB file, you will see all the events:

    elatov@ccl:~$sudo tail -f /var/lib/openvas/users/om/kbs/192.168.2.100
    1397586988 3 Launched/1.3.6.1.4.1.25623.1.0.880714=1
    1397586988 3 Launched/1.3.6.1.4.1.25623.1.0.802182=1
    1397586988 3 Launched/1.3.6.1.4.1.25623.1.0.881070=1
    1397586988 3 Launched/1.3.6.1.4.1.25623.1.0.863027=1
    1397586988 3 Launched/1.3.6.1.4.1.25623.1.0.860931=1
    1397586988 3 Launched/1.3.6.1.4.1.25623.1.0.71512=1
    1397586988 3 Launched/1.3.6.1.4.1.25623.1.0.855447=1
    1397586988 3 Launched/1.3.6.1.4.1.25623.1.0.64795=1
    1397586988 3 Launched/1.3.6.1.4.1.25623.1.0.870281=1
    1397586988 3 Launched/1.3.6.1.4.1.25623.1.0.802779=1
    1397586988 1 SentData/1.3.6.1.4.1.25623.1.0.80110/NOTE=Here is the wapiti report
    :\n*****************************************************************************
    ***\n                     Wapiti 2.3.0 - wapiti.sourceforge.net\n
           Report for http://192.168.2.100:80\n              Date of the scan : Tue
    , 15 Apr 2014 18:36:19 +0000\n                          Scope of the scan : fold
    er\n****************************************************************************
    ****\nSummary of vulnerabilities :\n----------------------------\n**************
    ******************************************************************\n\n
                          Anomalies found:\n\n\n--- End of report ---
    1397586988 3 Launched/1.3.6.1.4.1.25623.1.0.880823=1
    1397586988 3 Launched/1.3.6.1.4.1.25623.1.0.58620=1
    

### Limiting the Cron Jobs for OpenVAS

By default the following rules are added by the openvas install:

    elatov@ccl:~$ls -l /etc/cron.d/openvas*
    -rw-r--r-- 1 root root 119 Apr  9 10:12 /etc/cron.d/openvas-sync-cert
    -rw-r--r-- 1 root root  85 Jun 28  2010 /etc/cron.d/openvas-sync-plugins
    -rw-r--r-- 1 root root 136 Apr 15 09:53 /etc/cron.d/openvas-sync-scap
    

The **rsync** was causing an IO spike on my little VM, so I decided to limit the IO and CPU usage of the sync (by prepending the sync with a **nice** and **ionice** commands):

    elatov@ccl:~$cat /etc/cron.d/openvas-sync-scap
    # start plugin sync daily at 1am
    0 1 * * * root /usr/bin/nice -n 19 /usr/bin/ionice -c2 -n7 /usr/sbin/openvas-scapdata-sync > /dev/null
    

### Snort Catching the OpenVAS Scan

After I ran the scan against a web server, I saw snort catching the scan right away:

<a href="http://virtuallyhyper.com/wp-content/uploads/2014/04/snort-catching-openvas-scan_i2.jpg" onclick="javascript:_gaq.push(['_trackEvent','outbound-article','http://virtuallyhyper.com/wp-content/uploads/2014/04/snort-catching-openvas-scan_i2.jpg']);"><img src="http://virtuallyhyper.com/wp-content/uploads/2014/04/snort-catching-openvas-scan_i2.jpg" alt="snort catching openvas scan i2 OpenVAS on CentOS" width="1021" height="490" class="alignnone size-full wp-image-10624" title="OpenVAS on CentOS" /></a>

I was also running **ossec** with active-responses enabled, that blocked the IP of the scanner right away.
