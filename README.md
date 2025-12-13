# tirdad-patch
A small patch to take the functionality of the "tirdad kernel module" from Kicksecure / Whonix and write it into the kernel TCP stack

The tirdad kernel module that Kicksecure / Whonix uses randomizes TCP ISN's to prevent a side channel attack - more can be read on Kicksecure docs or at this link: https://bitguard.wordpress.com/?p=982

The hardened kernels we run (and will soon release) have live patching turned off for security purposes, thus, I still wanted to be able to include the random ISN capabilities in the "hardened-desktop" kernel, it will not be included in the "hardened-server" kernel, however, you may always pull the source / config and patch it in.

This patch does work but it is not technically formatted properly - I am aware. It was generated with git diff and will work with "patch -p1" or "git apply". As of now, it totally removes the normal kernel ISN generation and replaces it with "get_random_bytes". Eventually I plan to add in the original TCP functionality and the Tirdad-type functionality and use a sysctl setting so the user can choose which ISN generation method they prefer to use if they run the hardened-desktop kernel.
