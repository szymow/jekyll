---
layout: post
title:  "Proxmox set VM to hybrid mode"
date:   2022-12-22 12:00:00 +0100
categories: configuration proxmox
permalink: /:year/:categories/:title.html
---

<h1>VM hibernation while shuting down proxmox</h1>

Here is my solution to hibernate all VMs at reboot or shutdown !

Go in proxmox shell, you need to be root !

      vi /etc/init.d/proxmox

Add this :

Bash:

      #!/bin/sh
      case "$1" in
      start)
         qm list | awk -F'[^0-9]*' '$0=$2' | while read -r vm_id; do qm unlock $vm_id; done;
      ;;
      stop)
         qm list | grep running | awk -F'[^0-9]*' '$0=$2' | while read -r vm_id; do qm suspend $vm_id --todisk 1; done;
         sleep 10
      ;;
      esac
      exit 0

Change file permissions

      chmod 755 /etc/init.d/proxmox

Add symbolic links

      ln -s /etc/init.d/proxmox /etc/rc3.d/S99proxmox
      ln -s /etc/init.d/proxmox /etc/rc0.d/K99proxmox
      ln -s /etc/init.d/proxmox /etc/rc6.d/K99proxmox

Reload daemon

      systemctl daemon-reload
      systemctl start proxmox

Don't forget to add autostart option in proxmox to the VMs that have to boot on start.

Enjoy ! :)

You can reboot or shutown your server from :
- Physical buttons
- Proxmox Gui buttons
- in shell : restart now / shutdow now

<h1>Second solution with bash script</h1>

### suspend_and_shutdown.sh
      qm list | grep running | awk -F'[^0-9]*' '$0=$2' | while read -r vm_id; do qm suspend $vm_id --todisk 1; done; shutdown -h now

### suspend_and_reboot.sh
      qm list | grep running | awk -F'[^0-9]*' '$0=$2' | while read -r vm_id; do qm suspend $vm_id --todisk 1; done; reboot

<h2>References:</h2>
<a href="https://forum.proxmox.com/threads/feature-request-hibernate-vms-on-pve-host-reboot.65529/">Feature request: Hibernate VMs on PVE host reboot</a>