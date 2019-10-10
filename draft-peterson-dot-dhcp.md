---
title: "DNS over Transport Layer Security announcements using DHCP or Router Advertisements"
abbrev: "DoT using DHCP or Router Advertisements"
docname: draft-peterson-dot-dhcp-latest
date: {DATE}
author:
    -
      ins: T. Peterson
      name: Thomas Peterson
      email: nosretep.samoht@gmail.com

ipr: trust200902
category: std
area: General
workgroup:
keyword: Internet-Draft
stand_alone: yes
pi: [toc, sortrefs, symrefs]

normative:
    RFC2119:
    RFC2132:
    RFC3646:
    RFC6125:
    RFC8106:
    RFC8174:

informative:
    RFC2131:
    bootp-registry:
        title: "Dynamic Host Configuration Protocol (DHCP) and Bootstrap Protocol (BOOTP) Parameters"
        target: http://www.iana.org/assignments/bootp-dhcp-parameters
    dhcpv6-registry:
        title: "Dynamic Host Configuration Protocol for IPv6 (DHCPv6)"
        target: http://www.iana.org/assignments/dhcpv6-parameters
    icmpv6-registry:
        title: "Internet Control Message Protocol version 6 (ICMPv6) Parameters"
        target: http://www.iana.org/assignments/icmpv6-parameters


--- abstract

This specification describes a DHCP option and Router Advertisement (RA)
extension to inform clients of the presence of DNS resolvers with Transport
Layer Security (TLS).

--- middle

# Introduction

DHCPv4 {{RFC2131}}, DHCPv6 {{RFC3646}}, and IPv6 Router Announcements
{{RFC8106}} all provide means to inform clients of available resolvers using
the incumbent DNS protocol for querying, however there is no means of specifying
alternate protocols to perform DNS queries.

# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in BCP 14 {{RFC2119}} {{RFC8174}}
when, and only when, they appear in all capitals, as shown here.

"DoT", "Do53" and other related abbreviations follow the definitions as
defined in {{!I-D.draft-hoffman-dns-terminology-ter-01}}.

# The DNS over TLS Option

The DoT DHCP/RA option informs the client that a DoT service is available for
use for answering DNS queries using the same IP address(es) returned in Do53 DNS
Server DHCP/RA options. Thus networks which announce DoT services MUST announce
DNS resolver availability via their respective options, and provide a TLS
certificate on the DoT service which passes verification ({{!RFC6125}}) against
the DNS Host Name provided in the DoT DHCP/RA option.

## IPv4 DHCP Option

The format of the IPv4 DoT DHCP option is shown below.

~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|      Code     |      Len      |                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+                               |
|                                                               |
|                          DNS Host Name                        |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

Code:
 : The DoT DHCPv4 option (one octet)

Len:
 : Length of the DNS Host Name

DNS Host Name:
 :  The DNS Host Name

## IPv6 DHCP Option

The format of the IPv6 DHCP option is shown below.

~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          option-code          |           option-len          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                           DNS Host Name                       |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

option-code:
 : TODO (two octets)

option-len:
 : Length of the DNS Host Name, in octects

DNS Servers:
 : The DNS Host Name

## The DoT IPv6 RA Option

The format of the DoT Router Advertisement option is shown below.

~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|      Type     |      Len      |                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+                               |
|                                                               |
|                          DNS Host Name                        |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

Type:
 : TODO (one octet)

Len:
 : The length of the DNS Host Name, in octets

DNS Server:
 : The DNS Host Name

## Trust Anchoring

TODO: Put in considerations for using DANE and/or existing certificate
authorities for trust anchoring.

# Security Considerations

An attacker with the ability to inject DHCP messages could include this option
and present a malicious resolver.

TODO: Further risk and threat assessments.

# IANA Considerations

TODO: This section must be updated after assignments have been issued.

This document requires the assignment of an option code assigned under the
"BOOTP Vendor Extensions and DHCP Options" {{bootp-registry}}, in
addition to an option code assigned under the "Option Codes" registry under
DHCPv6 parameters {{dhcpv6-registry}}.

Also, an assignment for an IPv6 RA Option Type from the "IPv6 Neighbor Discovery
Option Formats" registry under ICMPv6 paramters {{icmpv6-registry}}.

--- back

# Acknowledgments
{:numbered="false"}

The author would like to acknowledge the extensive feedback from Martin Thomson.
