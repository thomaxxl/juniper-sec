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
* Security
* Functional
* Junos-Host
* Null

```
root# show security-zone trust
tcp-rst;
address-book {
    address lan 10.32.32.0/24;
    address company {
        dns-name company.com;
    }
}
host-inbound-traffic {
    system-services {
        all;
        telnet {
            except;
        }
    }
}
interfaces {
    ge-0/0/0.0;
}
```



##Security Policies
```
# set security forwarding-options family inet6 mode flow-based
> show security flow session [ extensive ]
> show security flow status
> show security policies from-zone trust to-zone untrust detail

#show groups junos-defaults [ applications ]

[edit applications]
root# show
application junos-ftp {
    application-protocol ignore;
    protocol tcp;
    destination-port 6021;
}

# set schedulers scheduler working-hours [ monday daily ... ] [ time ]

root# show security policies from-zone trust to-zone untrust
policy default-permit {
    match {
        source-address any;
        destination-address any;
        application any;
    }
    then {
        permit {
            firewall-authentication {
                web-authentication {
                    client-match contractors;
                }
            }
        }
        log {
            session-init;
        }
    }
    scheduler-name working-hours;
}

# set security policies policy-rematch
# insert policy

```
##Firewall User Authentication
```
# set access profile admin-access session-options client-idle-timeout 120
# set access profile contractors client ext1 client-group C1 firewall-user password blah
> show security firewall-authentication [ history users ]

```
##Screens

##NAT

##IPsec VPNs

##High Availability (HA) Clustering

##Unified Threat Management (UTM)

#


