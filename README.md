SELinux-Cobbler
===============

SELinux policy for Cobbler 2.4 on RHEL 6.4

SELinux complains a lot if you install Cobbler, even with Cobbler's own policy, included in the cobbler package.

This policy enables TFTP to work properly, as well as the web GUI which is available in the cobbler-web package.

Installation
------------

You only need the .pp files to load with semodule, like so.
  semodule -i cobbler_extra.pp

That's enough effort for now, go take a break.

Developing
----------

If you'd like to edit the policy, be my guest!

Make your changes in the .te file(s), and build them into a .pp file like so.

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
