### LFCS Exam Notes

1. ___Containers___: start a containerwith a specific name, make sure it autostarts
2. ```libvirt``` VM: change VM memory assignment, make sure it autostarts
3. ```ssh```: secure SSH, allow users, no password login, no X-Forwarding, Log level INFO
4. ```diff```: find differences between files, find a file that exists in directory a and not in b
5. ```find```: find files based on permissions, size, not owned by user xyz
6. ```tar```: create/extract archives, using XZ and other archivers
7. ___Symbolic Links___
8. ___Permissions___: delete a file ```chattr -j```, set an ACL, set SUID on a file
9. ___Users___: make a user member of a primary/secondary group
10. ___RAID___: create a level 1 RAID device with 3 devices and one spare and write the configuration to ```/etc/mdadm.conf```.
11. Create an __ext3__ _file system_, mount it by label
12. Create an _Apache virtual host_ (another virtual host already exists)
13. Use ```iptables``` to allow traffic coming in on _interface_ ```lo0```, source _IP_ ```a.b.c.d```, _target port_ ```3456``` and insert it before any other lines. Write the output to ```/root/somefile```.
14. Command redirection: __STDOUT__, __STDERR__