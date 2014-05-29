jncis-sec
=========

[Detailed Objectives](http://www.juniper.net/us/en/training/certification/resources_jno_332.html)

##Junos Security Overview
![alt text](http://forums.juniper.net/t5/image/serverpage/image-id/826i45301B7500F0E35C/image-size/original?v=mpbl-1&px=-1 "Logo Title Text 1")

```
[edit firewall family inet]
root# show
filter packet-mode {
    interface-specific;
    term 1 {
        then {
            count packets;
            packet-mode;
            accept;
        }
    }
}
```

##Zones

* Security Policies
* Firewall User Authentication
* Screens
NAT
IPsec VPNs
High Availability (HA) Clustering
Unified Threat Management (UTM)

#


