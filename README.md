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
# show security zones security-zone trust
tcp-rst;
address-book {
    address lan 10.32.32.0/24;
    address company {
        dns-name company.com;
    }
}
screen myscreen;
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
```
# show security screen
ids-option myscreen {
    icmp {
        fragment;
        large;
        ping-death;
    }
    ip {
        bad-option;
        security-option;
        source-route-option;
        strict-source-route-option;
        unknown-protocol;
        block-frag;
        tear-drop;
    }
    tcp {
        syn-fin;
        fin-no-ack;
        tcp-no-flag;
        syn-frag;
        syn-ack-ack-proxy;
        syn-flood {
            attack-threshold 100;
        }
        land;
    }
}
ids-option untrust-screen {
    alarm-without-drop;
    icmp {
        ip-sweep;
        fragment;
        large;
        flood;
        ping-death;
    }
    ip {
        bad-option;
        record-route-option;
        timestamp-option;
        security-option;
        stream-option;
        spoofing;
        source-route-option;
        loose-source-route-option;
        strict-source-route-option;
        unknown-protocol;
        block-frag;
        tear-drop;
    }
    tcp {
        syn-fin;
        fin-no-ack;
        tcp-no-flag;
        syn-frag;
        port-scan;
        syn-ack-ack-proxy;
        syn-flood {
            alarm-threshold 1024;
            attack-threshold 200;
            source-threshold 1024;
            destination-threshold 2048;
            queue-size 2000; ## Warning: 'queue-size' is deprecated
            timeout 20;
        }
        land;
        winnuke;
    }
    udp {
        flood;
    }
    limit-session {
        source-ip-based 1000;
        destination-ip-based 1000;
    }
}
```
##NAT

##IPsec VPNs

##High Availability (HA) Clustering

##Unified Threat Management (UTM)

#


