# Security and NAT Policies

### Security Policy fundamental concepts
* All traffic must match a session and security policy (stateful firewall)
* Basics are a source and destination zone
* Granular includes Source/Dest Address, ports, application, URL Categories, Source user and HIP profiles
* Sessions are established for bidirectional data flow
* Policies > Security has the current security rules
* Columns on this page can be customized for your preferred information displayed
* Three types of security rules:
    * Intrazone: All traffic within a zone. This traffic is allowed by default.
    * Interzone: All traffic between zones. This traffic is blocked by default.
    * Universal: Allowing all traffic between source and destination. Combines intra and interzone traffic.
* Any created rules have traffic logged by default; system created rules (intra/interzone at the end) are not logged.
* Rules are evaluated from top to bottom; when a match is found, no further eval is done.
* Rule Shadowing is when multiple rules match the same scope of traffic.

### Security Policy Administration
* Security policy must include:
   * Source zone, Destination zone, Action
* Security policies can also include:
   * Source IP
   * Destination IP
   * User
   * Application
   * Service
   * URL's
   * Additional actions (logging, vuln/av/malware profiles, scheduling and QoS)
* Scheduling can set times when a rule is allowed
* Rules can be reordered, disabled, deleted, added, cloned
* Unused rules can be shown by clicking the 'Highlight Unused Rules' checkbox at the bottom of the screen
* Address Objects is used to give a familiar name(s) to a single IP, IP range or an FQDN (functional dns resolution is needed for FQDN)
* Address Groups are a group of Address Objects. Groups are used to help simplify firewall administration.
* Tags are used to help organize and 'tag' rules that are in related policy groups. examples are: mail, web, DC, SQL, etc
* Custom Services can be created to help provide simplification and identity to services. In services, you can specify a single port, multiple ports with commas, or a range of ports with a dash.
* For the pre-defined intra/interzone allow/deny rules, choose override to set logging or other profile settings such as av/mal/vuln profiles.
### DoS Protection Profiles
* Flood protection: Detects and prevents attacks in which the network is flooded with
packets, which results in too many half-open sessions or services being unable to respond to
each request. In this case, the source address of the attack is usually spoofed.
* Resource protection: Detects and prevents session exhaustion attacks. In this type of
attack, many hosts (bots) are used to establish as many fully established sessions as possible
for consuming all of a system’s resources.
* You can configure aggregate and classified DoS Protection Profiles and apply one profile or one of
each type of profile to DoS Protection Policy Rules when you configure DoS Protection.
   * Aggregate — Sets thresholds that apply to the entire group of devices specified in a DoS
   Protection policy rule instead of to each individual device, so one device could receive the
majority of the allowed connection traffic. For example, a Max Rate of 20,000 CPS means the
total CPS for the group is 20,000 and an individual device can receive up to 20,000 CPS if the
other devices don’t have connections. Aggregate DoS Protection policies provide another
layer of broad protection (after the dedicated DDoS device at the Internet perimeter and
Zone Protection profiles) for a particular group of critical devices when you want to apply
extra constraints on specific subnets, users, or services.
   * Classified — Sets flood thresholds that apply to each individual device specified in a DoS
Protection policy rule. For example, if you set a Max Rate of 5,000 CPS, each device specified
in the rule can accept up to 5,000 CPS before it drops new connections. If you apply a
classified DoS Protection policy rule to more than one device, the devices governed by the
rule should be similar in terms of capacity and how you want to control their CPS rates
because classified thresholds apply to each individual device. Classified profiles protect
individual critical resources.
### Zone Protection
* A Zone Protection profile with flood protection configured defends an entire ingress zone against
SYN, ICMP, ICMPv6, UDP, and other IP flood attacks. The firewall measures the aggregate amount
of each flood type entering the zone in new connections per second (CPS) and compares the totals
to the thresholds you configure in the Zone Protection profile.
* Reconnaissance Protection: Similar to the military definition of reconnaissance, the network security definition of
reconnaissance is when attackers attempt to gain information about your network’s vulnerabilities
by secretly probing the network to find weaknesses. Reconnaissance activities are often preludes to
a network attack. Enable Reconnaissance Protection on all zones to defend against port scans and
host sweeps:
   * Port scans discover open ports on a network. A port scanning tool sends client requests to a
range of port numbers on a host, with the goal of locating an active port to exploit in an
attack. Zone Protection profiles defend against TCP and UDP port scans.
   * Host sweeps examine multiple hosts to determine if a specific port is open and vulnerable
* Packet-Based Attack Protection
  * Packet-based attacks take many forms. Zone Protection profiles check IP, TCP, ICMP, IPv6, and
   ICMPv6 packet headers and protect a zone by:
  * Dropping packets with undesirable characteristics.
  * Stripping undesirable options from packets before admitting them to the zone.
   Select the drop characteristics for each packet type when you Configure Packet Based Attack
   Protection. The best practices for each IP protocol are:
      *  IP Drop—Drop Unknown and Malformed packets. Also drop Strict Source Routing and
Loose Source Routing because allowing these options allows adversaries to bypass Security
policy rules that use the Destination IP address as the matching criteria. For internal zones
only, check Spoofed IP Address so only traffic with a source address that matches the
firewall routing table can access the zone.
      * TCP Drop—Retain the default TCP SYN with Data and TCP SYN-ACK with Data drops, drop
Mismatched overlapping TCP segment and Split Handshake packets, and strip the TCP
Timestamp from packets.
      * ICMP Drop—There are no standard best practice settings because dropping ICMP packets
depends on how you use ICMP (or if you use ICMP). For example, if you want to block ping
activity, you can block ICMP Ping ID 0.
      * IPv6 Drop—If compliance matters, ensure that the firewall drops packets with
non-compliant routing headers, extensions, etc.
      * ICMPv6 Drop—If compliance matters, ensure that the firewall drops certain packets if the
packets don’t match a Security policy rule.
* Protocol Protection 
  * In a Zone Protection profile, Protocol Protection defends against non-IP protocol-based attacks.
Enable Protocol Protection to block or allow non-IP protocols between security zones on a Layer 2
VLAN or on a virtual wire, or between interfaces within a single zone on a Layer 2 VLAN (Layer 3
interfaces and zones drop non-IP protocols so non-IP Protocol Protection doesn’t apply). Configure
Protocol Protection to reduce security risks and facilitate regulatory compliance by preventing less
secure protocols from entering a zone, or an interface in a zone.
* Ethernet SGT Protection - Cisco TrustSec network
  
### Network Address Translation
* NAT policy is evaluated after the destination zone route lookup
* NAT policy is applied just before packet is forwarded.
* NAT types are Source and Destination, and these are from the perspective of the firewall.
* Source NAT configuration
   * Source NAT is generally used from traffic on private internal IP's to a publicly routable IP (user inside to server outside). Types of Source NAT's include:
      * Static IP: fixed 1-to-1 translation; used when a NAT IP needs to remain the same IP and Port. This can be done with an IP address range, but the translation will always be static 1:1; in a 10-IP range, a x.y.z.2 source will always translate to z.y.x.2 destination.
      * Dynamic IP: 1-to-1 ip address translation only (no port number); can be used with a single or pool of IP's. Generally used for a set number of internal hosts with a matching number of external IP's, but static isn't required.
      * Dynamic IP and port (DIPP): This is used for multiple IP's to one or a few IP's, by allowing the connection to use another port than the default service. This is generally used for internet access outbound from home and business connections.
   * To use Source NAT:
      * Create a NAT policy rule: Original packets (IP's of client(s)) that will be using the NAT (source address), destination address (if needed), and the type of source translation (Static, Dyn, DynDIPP), and the IP to translate to.
      * A security policy will be needed to allow the traffic. The security policy will include:
           * Source Address (private/internal client IP's/subnet)
           * Source Zone where client IP's reside
           * Destination Zone (where IP to NAT to exists)
           * Any application or services
           * Allow the traffic on the policy
   * Bidirectional NAT's can be configured (only available for static NAT).
      * To configure, check the 'bidirectional' checkbox in the NAT configuration source nat translated packet tab.
   * DIPP NAT oversubscription is when a DIPP Source NAT uses more than the available 64,000 ports available per ip address. This is done by using the destination IP of new sessions outbound to 'ride' the same active port as other traffic going to that IP address.
* Destination NAT configuration
   * Destination NAT is used for external traffic coming into a private or secured location inside your network.
   * Types of Destination NAT:
      * Static IP: 1:1 translation of inbound traffic
      * Optional Port Forwarding: Can route to multiple internal servers based on Port number (25 to mail, 80 to web, etc)
   * Configuration:
      * Create a Destination NAT policy, defining the source and destination (pre-nat on both)
           * Incoming: (any or specific) to Destination (external routable IP) - Untrust to Untrust typically for example, with a destination translation to the internal IP address
      * Create a security policy that permits the post NAT zone (and IP's if needed) to the Pre-Nat destination IP/app/service/action
      * Security Policy does pre-nat source/dest, post-nat destination zone.
      * Destination NAT Port Forwarding Configuration:
          * When configuring the Destination NAT address under the 'Translated Packet' tab, put in the translated port of the destination
          * A destination NAT can be set with different destination translation IP's and ports from the same external facing IP, as long as the service is specified.
