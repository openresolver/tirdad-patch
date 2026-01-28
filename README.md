# Tirdad-sysctl-patch
A small patch to take the functionality of the "tirdad kernel module" from Kicksecure / Whonix add it into the kernel source and add a sysctl setting to toggle between normal and random ISN generation


**Note:** Please use the "0001-net-tcp-add-sysctl-to-toggle-TCP-ISN-randomization-6.18.patch" for kernel releases 6.18 and above as the previous patch will conflict due to a line numbering / ordering issue.

# Info
This is the tirdad kernel module that Kicksecure / Whonix uses to randomize TCP ISN's to prevent a side channel attack - more can be read on Kicksecure docs or at this link: https://bitguard.wordpress.com/?p=982. Thank you to "0xsirus" for the original module.

There are 2 patches in the repo. Please note that this is global and affects both IPv4 and IPv6 TCP ISN generation.

# How to use (only use one or the other):
"0001-net..." is a patch to add a sysctl setting to toggle between the upstream TCP ISN generation and totally random ISN generation. The sysctl setting is `net.ipv4.tcp_random_isn`. You can set via `sysctl -w net.ipv4.tcp_random_isn=1` to enable totally random ISN generation. In an effort to not break upstream code, the default is set to '0' or 'off'. You can override this behavior by creating a `/etc/sysctl.d/99-tcp-isn-override.conf` and adding `net.ipv4.tcp_random_isn = 1`

`cd` into your kernel source directory and run `git am 0001-net-ipv4-add-sysctl-to-toggle-TCP-ISN-generation.patch`

"tirdad.patch" is a simple git diff that totally removes the original secure_seq Sip Hash ISN generation from the kernel and replaces with `"get_random_bytes()"`

`cd` into your kernel source directory and run `patch -p1 < tirdad.patch` or `git apply tirdad.patch`

# TODO for v3 patch
-Fix minor formatting issues

-Possibly rename sysctl or make it more clear that it affects both IPv4 and IPv6?

-Add comment to explain why the setting is global

-Possible change to avoid unecessary cast in `get_random_bytes()`

-Evaluate possible change to `get_random_u32`
