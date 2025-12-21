# tirdad-patch
A small patch to take the functionality of the "tirdad kernel module" from Kicksecure / Whonix and write it into the kernel TCP stack

# info
This is the tirdad kernel module that Kicksecure / Whonix uses to randomize TCP ISN's to prevent a side channel attack - more can be read on Kicksecure docs or at this link: https://bitguard.wordpress.com/?p=982. Thank you to "0xsirus" for the original module.

There are 2 patches in the repo. Please note that this is global and affects both IPv4 and IPv6 TCP ISN generation.

# How to use:
"0001-net..." is a patch to add a sysctl setting to toggle between the upstream TCP ISN generation and totally random ISN generation. The sysctl setting is `net.ipv4.tcp_random_isn`. You can set via `sysctl -w net.ipv4.tcp_random_isn=1` to enable totally random ISN generation. In an effort to not break upstream code, the default is set to '0' or 'off'. You can overwrite this behavior by creating a `99-tcp-isn-override.conf` and adding `net.ipv4.tcp_random_isn = 1`

`cd` into your kernel source directory and run `git am 0001-net-ipv4-add-sysctl-to-toggle-TCP-ISN-generation.patch`

"tirdad.patch" is a simple git diff that totally removes the original secure_seq SIPHASH ISN generation from the kernel and replaces with `"get_random_bytes()"`

`cd` into your kernel source directory and run `patch -p1 tirdad.patch` or `git apply tirdad.patch`
