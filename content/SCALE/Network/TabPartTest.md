---
title: "Some Article"
weight: 15
---

{{< tabs "Lagg Protocol Options" >}}
{{< tab "LACP" >}}
The most commonly used LAGG protocol and is one part of [IEEE specification 802.3ad](https://www.ieee802.org/3/hssg/public/apr07/frazier_01_0407.pdf). In LACP mode, negotiation is performed with the network switch to form a group of ports that are all active at the same time. The network switch must support LACP for this option to function.
{{< /tab >}}
{{< tab "Failover" >}}
Failover causes traffic to be sent through the primary interface of the group. If the primary interface fails, traffic diverts to the next available interface in the LAGG.
{{< /tab >}}
{{< tab "Tab Part Test" >}}
{{< include file="static/includes/SCALE/TabPartTest.md.part" >}}
{{< /tab >}}
{{< tab "RoundRobin" >}}
Round robin accepts inbound traffic on any port of the LAGG group and sends outbound traffic using a round robin scheduling algorithm. Traffic is sent out in sequence using each LAGG interface in turn.
{{< /tab >}}
{{< tab "None" >}}
This mode disables traffic on the LAGG interface without disabling the LAGG interface.
{{< /tab >}}
{{< /tabs >}}