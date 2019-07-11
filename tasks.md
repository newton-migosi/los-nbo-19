## Permissions

1. Create two __groups__: _sales_ and _account_.
2. Create __users__ _lisa_ and _linda_ who are members of the __group__ _sales_.
3. Create __users__ _leo_ and _lea_ who are members of the __group__ _account_.
4. Create the __directories__ ```/data/sales``` and ```/data/account```.
5. Ensure that members of the __group__ _sales_ have all __permissions__ to ```/data/sales```, and members of the __group__ _account_ have all __permissions__ to ```/data/account```.

## IPtables
1. ___Disable___ the _firewall_ service.
2. ___Install___ the ```iptables``` service package.
3. ___Set___ the ```iptables``` _policy_ to ```DROP``` on the ```INPUT``` and the ```OUTPUT``` chain .
4. ___Verify___ nothing works anymore using ```ping localhost```
5. Open the firewall to ___accept___ _incoming_ and _outgoing_ traffic on the _loopback_ interface.
6. __Test__ that you can ```ping localhost```.
7. Open the firewall to ___accept___ _incoming_ __ssh__ traffic.
8. Use the ```state``` module in the ```iptables``` command to ___allow___ all _outgoing_ traffic with a state ```established, related```.
9. ___Test___ that you can use ```ssh``` to connect to your machine (you can test from the host).
10. Open the firewall for _incoming_ __icmp__ traffic to ___enable___ ``ping`` test from an external machine that works
11. Use ```iptables --save``` to ___save___ your _configuration_ to ```/etc/sysconfig/iptables``` 
12. Enable the ```iptables``` _service_ so that your firewall configuration can be used.