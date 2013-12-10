SELinux-Cobbler
===============

SELinux policy for Cobbler 2.4 on RHEL 6.4

SELinux complains a lot if you install Cobbler, even with Cobbler's own policy, included in the cobbler package.

This policy enables TFTP, rsync, distro/repo importing, the web GUI which is available in the cobbler-web package, and PAM authentication for cobbler-web (Kerberos supported!).

Installation
------------

You only need to load the .pp file with semodule, like so.

    semodule -i cobbler_extra.pp

Developing
----------

If you'd like to edit the policy, be my guest!

Make your changes in the .te file, and build it into a .pp file like so.

First run this to check the .te for any issues, and fix those that come up. The -o parameter specifies the output for the .mod file it generates, and the last parameter is your .te file.

    checkmodule -M -m -o cobbler_extra.mod cobbler_extra.te

Then run this to build a .pp from the .mod. Again, the -o specifies the output .pp file, and -m is your .mod input

    semodule_package -o cobbler_extra.pp -m cobbler_extra.mod

Load it in SELinux with semodule

    semodule -i cobbler_extra.pp

You can temporarily disable the module with 

    semodule -d cobbler_extra

Re-enable it with

    semodule -e cobbler_extra

And remove it for good with

    semodule -r cobbler_extra

Troubleshooting
---------------

If you're developing and running into silent denials (you can't do something, but turning SELinux off let's you do that, with no AVC messages in audit.log) you can disable the dontaudit rules with the following command.

    semodule -DB

Now everything will be logged! This is very useful for developing policy.

Rebuild policy and enable dontaudit rules when you're done.

    semodule -B
