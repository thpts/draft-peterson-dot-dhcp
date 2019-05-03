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

# The DNS over TLS Option

## IPv4 DHCP Option

The format of the IPv4 DoT DHCP option is shown below.

~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|      Code     |      Len      |                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+                               |
|                                                               |
|                          DNS Servers                          |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

Code:
 : The DoT DHCPv4 option (one octet)

Len:
 : Length of the DNS Servers list in octects. If the length is set to 0 the DHCP
   server MUST transmit a Domain Name Server Option message {{RFC2132}}, which
   the client will infer as all hosts provided in that option as capable of
   answering queries with DoT.

DNS Servers:
 : IPv4 addresses of DNS servers. This field is optional and MUST not be set if
   the Len field is set to 0.

## IPv6 DHCP Option

The format of the IPv6 Captive-Portal DHCP option is shown below.

~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          option-code          |           option-len          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
|                           DNS Servers                         |
|                                                               |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

option-code:
 : TODO (two octets)

option-len:
 : Length of the list of DNS servers in octects, which MUST be a multiple of 16,
   or set to 0. When this value is 0 the DHCP server MUST transmit a DNS
   Recursive Name Server option {{RFC3646}}, which the client will infer as all
   hosts provided in that option as capable of answering queries with DoT.

DNS Servers:
 : IPv6 addresses of DNS servers. This field is optional and MUST not be set if
   the option-len field is set to 0.

## The DoT IPv6 RA Option

The format of the DoT Router Advertisement option is shown below.

~~~
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|      Type     |      Len      |                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+                               |
|                                                               |
|                          DNS Servers                          |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
~~~

Type:
 : TODO (one octet)

Len:
 : 8-bit unsigned integer representing the entire length of all fields, in units
   of 8 bytes. The minimum value is 0, and 3 if one DNS server is contained in
   the option. Every additional DNS server increases the length by 2. This field
   is used by the receiver to determine the number of DNS server addresses in
   the option. Where the value is set to 0, The IPv6 Router MUST transmit a
   Recursive DNS Server Option {{RFC8106}}, which the client will infer as all
   hosts provided in that option as capable of answering queries with DoT.

DNS Servers:
 : One or more IPv6 addresses of DNS servers. When the Len value is not 0, the
   number of addresses is determined by the Length field. That is, the number of
   addresses is equal to (Length - 1) / 2. If the Len value is 0 this field MUST
   not be set.

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

TODO
