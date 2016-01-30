mdadm-bad-blocks.
=========

mdadm does not currently monitor bad blocks status, meaning that there may no alerts on lack of redundant data. Please see https://marc.info/?l=linux-raid&m=145395120222380&w=2. While the lack of alerts should be patched, changes to mdadm itself may take a long time to propagate.

Only 1.x devices support the bad block list. You can check for whether the bad blocks list is enabled for a given device by using 'mdadm --examine-badblocks <dev>' Use of bad blocks can be changed for an array at assemble time using '--update' and 'bbl' or 'no-bbl' assuming there are not already some bad blocks on the array.

This role performs the following:

* Sets policy for use of the mdadm bad blocks list for new devices if such a policy is supported by the installed version of mdadm.
* Copies a script raid-bad-blocks to /usr/local/bin.
* Adds a cron job for the user "nobody" to run raid-bad-blocks at midnight every night.

raid-bad-blocks checks for a non-empty value of bad_blocks in sysfs for each device and will send an email to the mdadm.conf MAILADDR email using "mail" if any devices with a non-empty value are found.

This cron job will fail to notify if the bad blocks are overwritten in one way or another within 24 hours. But we believe a 24 hour notification is probably good enough for most purpose, and keeps the script raid-bad-blocks extremely simple. If you disagree, pull requests are welcome.

The location of mdadm.conf is set to /etc/mdadm.conf for RedHat families and to /etc/mdadm/mdadm.conf for Debian families.

Requirements
------------

It is assumed you have mail and mdadm installed. 

Role Variables
--------------

mdadm_bad_blocks_use: Valid values are "yes" or "no". Defaults to "no". Sets CREATE bbl=<choice> in mdadm.conf .

Example Playbook
----------------

    - hosts: physical
      roles:
         - { role: ansible-role-mdadm-bad-blocks, mdadm_bad_blocks_use: "no" }

License
-------

GPLv3

Author Information
------------------

Please contact Sarah Newman - srn at prgmr.com - with any feedback.
