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

troubleshooting:
http://inetzeroblog.com/troubleshooting-security-policies/#more-622

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
web / passthrough

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

> show security screen ids-option untrust-screen
Screen object status:

Name                                         Value
  ICMP flood threshold                       1000
  UDP flood threshold                        1000
  TCP winnuke                                enabled
  TCP port scan threshold                    5000
  ICMP address sweep threshold               10000
  IP tear drop                               enabled
  TCP SYN flood attack threshold             200
  TCP SYN flood alarm threshold              1024
  TCP SYN flood source threshold             1024
  TCP SYN flood destination threshold        2048
  TCP SYN flood timeout                      20
  IP spoofing                                enabled
  ICMP ping of death                         enabled
  IP source route option                     enabled
  TCP land attack                            enabled
  TCP SYN fragment                           enabled
  TCP no flag                                enabled
  IP unknown protocol                        enabled
  IP bad options                             enabled
  IP record route option                     enabled
  IP timestamp option                        enabled
  IP security option                         enabled
  IP loose source route option               enabled
  IP strict source route option              enabled
  IP stream option                           enabled
  ICMP fragmentation                         enabled
  ICMP large packet                          enabled
  TCP SYN FIN                                enabled
  TCP FIN no ACK                             enabled
  TCP SYN-ACK-ACK proxy threshold            512
  IP block fragment                          enabled
  Session source limit threshold             1000
  Session destination limit threshold        1000
  Alarm without drop                         enabled

> show security screen statistics zone trust


```
##NAT

##IPsec VPNs

Dynamic vpn

Static vpn

Debug single vpn:
```request security ike debug-enable local 81.246.13.226 remote 91.176.128.1```

Error codes:
- No proposal chosen : 
    - proposal mismatch
    - tunnel interface not in security zone

- 

How to analyze the IKE Phase 1 messages in the Kmd Log for a J Series or SRX Series device : http://kb.juniper.net/InfoCenter/index?page=content&id=KB10101

How to analyze IKE Phase 2 Messages in the Kmd Log for a J Series or SRX Series device :
http://kb.juniper.net/InfoCenter/index?page=content&id=KB10099

##High Availability (HA) Clustering
```
> set chassis cluster cluster-id <0-15> node <0-1> reboot

# set groups node0 system host-name SRX-node0
# set groups node0 interfaces fxp0 unit 0 family inet address 10.32.1.21/24
# set groups node1 system host-name SRX-node1
# set groups node1 interfaces fxp0 unit 0 family inet address 10.32.1.22/24
# set apply-groups "${node}"
# set chassis cluster control-link-recovery
# set chassis cluster reth-count 16
# set chassis cluster redundancy-group 0 node 0 priority 200
# set chassis cluster redundancy-group 0 node 1 priority 100
# set chassis cluster redundancy-group 1 node 1 priority 100
# set chassis cluster redundancy-group 1 node 0 priority 200

# set chassis cluster cluster-id 1 node 0 reboot
# set chassis cluster cluster-id 1 node 1 reboot

# set interfaces fab0 fabric-options member-interfaces ge-X/X/X
# set interfaces fab1 fabric-options member-interfaces ge-X/X/X
```
http://kb.juniper.net/InfoCenter/index?page=content&id=KB15504

##IDP
Install IDP license
```
> start shell
% cd /var/tmp
% scp user@server:dir1/licensefile.txt .
% cd /var/db/idpd
% scp user@server:/dir1/idp.tar.tgz
% tar xzvf idp.tar.tgz
% exit

> request system license add /var/tmp/licensefile.txt
> show system license (look for installed license)
> request security idp security-package install
> request security idp security-package install status (repeat until you get a “done”)
> request security idp security-package install policy-templates (templates from juniper)
> configure
# set system scripts commit file templates.xsl
# commit
# set security idp active-policy ? (to view available policies)
```
Enable Policies
```
# edit security idp
# delete idp-policy DMZ_Services
# delete idp-policy DNS_Services
# delete idp-policy File_Server
# delete idp-policy Getting_Started
# delete idp-policy IDP_Default
# delete idp-policy Web_Server
# show idp-policy Recommended (only template left after deletes above)
# delete idp-policy Recommended rulebase-ips rule 1 (or any rule you do not want 1 – 9 )
# set active-policy Recommended (sets the IPS policy)
```
apply ips policy to a security policy
```
top edit security policies from-zone untrust to-zone zone_name
set policy webservers then permit application-services idp (choose your then stmt and put in idp)
delete system scripts (delete the templates.xsl script from above)
```

##Unified Threat Management (UTM)

##Var


http://rtoodtoo.net/category/jncip-sec/
