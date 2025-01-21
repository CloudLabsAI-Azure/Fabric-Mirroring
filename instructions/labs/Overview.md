## Overview of Microsoft Fabric Mirroring

Mirroring in Fabric is a low-cost and low-latency solution to bring data from various systems together into a single analytics platform. You can continuously replicate your existing data estate directly into Fabric's OneLake from a variety of Azure databases and external data sources.

With the most up-to-date data in a queryable format in OneLake, you can now use all the different services in Fabric, such as running analytics with Spark, executing notebooks, data engineering, visualizing through Power BI Reports, and more.

Mirroring in Fabric allows users to enjoy a highly integrated, end-to-end, and easy-to-use product that is designed to simplify your analytics needs. Built for openness and collaboration between Microsoft, and technology solutions that can read the open-source Delta Lake table format, Mirroring is a low-cost and low-latency turnkey solution that allows you to create a replica of your data in OneLake which can be used for all your analytical needs.



## Types of Mirroring 

Microsoft Fabric provides several types of mirroring, allowing organizations to capture and analyze data traffic for monitoring, troubleshooting, and security purposes. The different types of mirroring ensure flexibility to suit various network monitoring needs. Below are the primary types of mirroring in Microsoft Fabric, along with a detailed explanation of each:


1. **Port Mirroring**  

    Port mirroring involves duplicating the traffic of one or more specific switch ports and sending it to a designated monitoring port. This is ideal for analyzing network traffic at the port level.  

    - **Use Cases**: Troubleshooting connectivity issues, detecting malicious activity on specific devices, or analyzing traffic flow from a particular server.  

    - **Example**: Mirroring all traffic from Port A (source) to Port B (destination), where a packet analyzer like Wireshark is connected.


2. **VLAN Mirroring**

    This type mirrors traffic from a specific VLAN. It enables monitoring of all communication within a VLAN without focusing on individual ports.  

    - **Use Cases**: Observing traffic in segmented network environments or ensuring compliance within a particular VLAN.  

    - **Example**: Mirroring VLAN 100 to a monitoring port to capture all intra-VLAN traffic, such as communications between devices in the same subnet.


3. **Flow Mirroring** 

    Flow mirroring focuses on specific traffic flows, such as IP addresses, protocols, or port numbers, instead of duplicating all traffic.  

    - **Use Cases**: Monitoring application-specific traffic or isolating a problematic traffic flow for in-depth analysis.  

    - **Example**: Mirroring only HTTP/HTTPS traffic (TCP Port 80/443) from a source IP to a monitoring tool.


4. **Global Mirroring**  

    Global mirroring enables the duplication of all traffic passing through the switch or network fabric. This provides a comprehensive view of network activity.  

    - **Use Cases**: Network-wide monitoring for intrusion detection systems (IDS) or forensic analysis.  

    - **Example**: Sending all network traffic to a security monitoring appliance for real-time threat detection.


5. **Dynamic Mirroring** 

    Dynamic mirroring adjusts mirroring rules in real time based on pre-defined conditions, such as traffic volume, specific events, or system alerts. 

    - **Use Cases**: Adaptive traffic monitoring in response to suspicious activity or scaling down mirroring during peak times to avoid bandwidth congestion.  

    - **Example**: Automatically starting mirroring for a device that exceeds a traffic threshold or exhibits unusual behavior.


6. **Remote Mirroring**  

    Remote mirroring enables traffic from one location to be mirrored to a monitoring system in a different physical or virtual location. This is often achieved through encapsulation protocols like GRE (Generic Routing Encapsulation).  

    - **Use Cases**: Centralized monitoring for distributed networks or remote troubleshooting for branch offices.

    - **Example**: Capturing traffic from a remote branch office and analyzing it in a central data center.


7. **Egress/Ingress Mirroring**  

    Egress mirroring captures traffic leaving the source (outbound), while ingress mirroring captures traffic entering the source (inbound). Both can be configured independently or together.  

    - **Use Cases**: Focusing on specific traffic direction to identify bottlenecks or troubleshoot asymmetric routing issues.  

    - **Example**: Capturing only outgoing traffic to determine data leaving a network perimeter.


### Conclusion  

The types of mirroring in Microsoft Fabric cater to a range of network monitoring scenarios, from port-level analysis to network-wide security inspections. Each type serves a specific purpose, ensuring flexibility and precision in traffic monitoring. By combining these mirroring options with advanced analysis tools, organizations can enhance their visibility into network behavior, strengthen security, and optimize overall performance.
