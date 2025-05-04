---
Assignee: Nikita Katyshev
Interview graded: true
Last edited time: 2023-07-23T12:38
Last recall: 2023-07-22
Needs Rework: false
Status: Not started
Topic:
  - "[Google Cloud](Google%20Cloud.md)"
---
## Virtual Private Cloud

Global virtual network that spans all regions. Single VPC for an entire organization, isolated within projects.

There are 2 types of VPC:

- Default
- Custom

VPC is hosted remotely by a public cloud provider. There code is ran, stored data, hosted websites and done everything as in regular cloud.

For every VPC may be configured next objects:

- Projects
- Networks
- Subnetworks
- Regions
- Zones
- IP addresses
- Virtual machines
- Routes
- Firewall rules

## Regions

VPC have different regions, each of them may have several subnets

[![](https://lh6.googleusercontent.com/Nj_nDovAD24GvxypVDmRA4M6IPqA43EowXWEJzYb__fW36co5Ow1LiHGazpJIIXhYhkKZeoOYuKhf5_Ay2LvRFNJJXObOcZJETGA3m2XTQqijSVIZiZ4D8fC48kDs9EQcJ43z1uKJAXABa--gLVFDtlCoAC2rDjgFqnKprJBr_l0sfTrvMfuQXFdq0cWLg)](https://lh6.googleusercontent.com/Nj_nDovAD24GvxypVDmRA4M6IPqA43EowXWEJzYb__fW36co5Ow1LiHGazpJIIXhYhkKZeoOYuKhf5_Ay2LvRFNJJXObOcZJETGA3m2XTQqijSVIZiZ4D8fC48kDs9EQcJ43z1uKJAXABa--gLVFDtlCoAC2rDjgFqnKprJBr_l0sfTrvMfuQXFdq0cWLg)

## Project

Associates objects and services with billings. Contains up to 5 networks that can be shared

## Network

Network has no IP addresses range. It is global and spans all available regions.

### **Types**:

- Default
    - Every project
    - One subnet per region
    - Default firewall rues
- Auto
    - Default network
    - One subnet per region
    - Regional IP allocation
    - Fixed /20 subnetwork per region, expandable up to /16
- Custom
    - No default subnets created
    - Full control of IP ranges
    - Regional IP allocation

## IP

There are

- Internal IP
    - Allocated from subnet range to VMs by DHCP
    - DHCP lease is renewed every 24h
    - VM name + IP is registered with network-scoped DNS
- External IP
    - Assigned from pool(ephemeral)
    - Reserved(static)
    - May Bring Your Own IP addresses(BYOIP)
    - VM doesn’t know external IP. It is mapped to the internal IP

[![](https://lh4.googleusercontent.com/5HCwQR3LjKRiy2PMQpDaGbkdzg1ed_i2QnLwowX49jKUgrYYiOPXCOSlCMyGAEpqz4xzwifmQjOQdscK2UHSZP0zeD_VtGGl3EMRgh9ypBDVILi2IdcZFRHqih60AHrj9UAaUkYohSKhnQRklU-goBUa5l52d7_nr5TTmmIbmDXhqcAtIQFTPb68UgN8Yg)](https://lh4.googleusercontent.com/5HCwQR3LjKRiy2PMQpDaGbkdzg1ed_i2QnLwowX49jKUgrYYiOPXCOSlCMyGAEpqz4xzwifmQjOQdscK2UHSZP0zeD_VtGGl3EMRgh9ypBDVILi2IdcZFRHqih60AHrj9UAaUkYohSKhnQRklU-goBUa5l52d7_nr5TTmmIbmDXhqcAtIQFTPb68UgN8Yg)

[![](https://lh6.googleusercontent.com/a92n0nc2sG88xRAbXppDlFplB8KyXq4CypwY8rzI4Y2DYVu-vuVn0saC8jQVadaoiYXd7zVk3bQvCdFesCdCSFCKNaswKGx9jTWuNhZY796eSrl4yaMwIWaMMBKL39H_JdFyOI27deGSZX91xGqDdJA99qxYIagZGNLqkLXO_j1Ch5WQWTya2LfgGsAZgw)](https://lh6.googleusercontent.com/a92n0nc2sG88xRAbXppDlFplB8KyXq4CypwY8rzI4Y2DYVu-vuVn0saC8jQVadaoiYXd7zVk3bQvCdFesCdCSFCKNaswKGx9jTWuNhZY796eSrl4yaMwIWaMMBKL39H_JdFyOI27deGSZX91xGqDdJA99qxYIagZGNLqkLXO_j1Ch5WQWTya2LfgGsAZgw)

## Routes

[![](https://lh3.googleusercontent.com/_Cm8U7r0MGOeqMawC425xGkoEmK1cfBLg9kZw70hS0_JP8JiV1njSQ9D5LoHD7gDcdZg6YLhjJ0BpNuinfP2Am4-S5AJF0fESxNoVFDuacJUR39VquuE92LZMkcN2aC1xuGxl2TVQbed4Lwf7ghJN0Za5UzzlZtzWRBYSORXCFi-6Uyl_fZoQvhmaDupJQ)](https://lh3.googleusercontent.com/_Cm8U7r0MGOeqMawC425xGkoEmK1cfBLg9kZw70hS0_JP8JiV1njSQ9D5LoHD7gDcdZg6YLhjJ0BpNuinfP2Am4-S5AJF0fESxNoVFDuacJUR39VquuE92LZMkcN2aC1xuGxl2TVQbed4Lwf7ghJN0Za5UzzlZtzWRBYSORXCFi-6Uyl_fZoQvhmaDupJQ)

[![](https://lh4.googleusercontent.com/VhRHkTkPim98oUOWBi_Z1pUxaVS4iftUl5bhjvJNX73jf9Hr9QehA8UYSMvSjiTuXJm75FNxa2mTJE1YnfmpvFdaBoCkYKkdpTin3e8Ejydt5YaYU1go1FdDNW7dWPRBOocYvyXx6kGgstwtFcrjxa1I9qVd5h9lPbIZSZ-sNFJoxf0_PzroPWaX0a50eg)](https://lh4.googleusercontent.com/VhRHkTkPim98oUOWBi_Z1pUxaVS4iftUl5bhjvJNX73jf9Hr9QehA8UYSMvSjiTuXJm75FNxa2mTJE1YnfmpvFdaBoCkYKkdpTin3e8Ejydt5YaYU1go1FdDNW7dWPRBOocYvyXx6kGgstwtFcrjxa1I9qVd5h9lPbIZSZ-sNFJoxf0_PzroPWaX0a50eg)

## Firewall

### View the firewall rules

Each VPC network implements a distributed virtual firewall that you can configure. Firewall rules allow you to control which packets are allowed to travel to which destinations. Every VPC network has two implied firewall rules that block all incoming connections and allow all outgoing connections.

- In the left pane, click **Firewall**. Notice that there are 4 **Ingress** firewall rules for the **default** network:
    - default-allow-icmp
    - default-allow-rdp
    - default-allow-ssh
    - default-allow-internal
- These firewall rules allow **ICMP**, **RDP**, and **SSH** ingress traffic from anywhere (0.0.0.0/0) and all **TCP**, **UDP**, and **ICMP** traffic within the network (10.128.0.0/9). The **Targets**, **Filters**, **Protocols/ports**, and **Action** columns explain these rules.

  

Egress in the world of networking implies traffic that exits an entity or a network boundary, while Ingress is traffic that enters the boundary of a network. While in service provider types of the network this is pretty clear, in the case of datacenter or cloud it is slightly different. In the cloud, Egress still means traffic that’s leaving from inside the private network out to the public internet

[https://aviatrix.com/learn-center/cloud-security/egress-and-ingress/#:~:text=Egress in the world of,the boundary of a network](https://aviatrix.com/learn-center/cloud-security/egress-and-ingress/#:~:text=Egress%20in%20the%20world%20of,the%20boundary%20of%20a%20network).

### Egress

[![](https://lh3.googleusercontent.com/MyCPqRpWcTlxVHX0xUw9CHhKBe5-FEWShAmGVJMgO-Pf_bHrogz3DRP_azhIrCtffhL3q7o0vbR3EIq76ZmBAeKaWbXLcaQ6fFaapR0JRkpnrDM_tE0SPmlpx9F3Q5GCBkvZjjkkuJsZE41l_iZr7ZI_WA3BRKRhtsFt7sQ_kyV15J6fHuFL_mmv3A-2Kg)](https://lh3.googleusercontent.com/MyCPqRpWcTlxVHX0xUw9CHhKBe5-FEWShAmGVJMgO-Pf_bHrogz3DRP_azhIrCtffhL3q7o0vbR3EIq76ZmBAeKaWbXLcaQ6fFaapR0JRkpnrDM_tE0SPmlpx9F3Q5GCBkvZjjkkuJsZE41l_iZr7ZI_WA3BRKRhtsFt7sQ_kyV15J6fHuFL_mmv3A-2Kg)

### Ingress

[![](https://lh3.googleusercontent.com/K4WoACWzWdL1B34mkVO0H_3QiFSdBMHHwuBoGZVrgCsedvToVfVRTJaJ7jkLxpKHE4H1-HglfQtPg0_xjiJKtBOtVzJJSxgr9VzZtqeYxpw5qrCUEozBHWNI4wUniWdW49cq4MK5ssUYPOxHAhnel13ZB1qAJ9siBQRbIHm0Dn3js4e2gp3OD6A11n4rqw)](https://lh3.googleusercontent.com/K4WoACWzWdL1B34mkVO0H_3QiFSdBMHHwuBoGZVrgCsedvToVfVRTJaJ7jkLxpKHE4H1-HglfQtPg0_xjiJKtBOtVzJJSxgr9VzZtqeYxpw5qrCUEozBHWNI4wUniWdW49cq4MK5ssUYPOxHAhnel13ZB1qAJ9siBQRbIHm0Dn3js4e2gp3OD6A11n4rqw)

  

[![](https://lh5.googleusercontent.com/IpaU1zzUGrGzaOKPrKZrqICjrXkaTT3c-UduXbbOtZPeTPsDJ_3htWd45RNUN8RpH5q8yZt8KgWEyNtjIOl2O4V2MfBPd779C72n8IXXUkgCrTaPTI3suYRNrHt_Y_iQb4sh1nafIEzZHpBoHr-OaUt7Zi7RYYP44d8Z5j9sdFBgIu2Rvf4BXPHSowGM6w)](https://lh5.googleusercontent.com/IpaU1zzUGrGzaOKPrKZrqICjrXkaTT3c-UduXbbOtZPeTPsDJ_3htWd45RNUN8RpH5q8yZt8KgWEyNtjIOl2O4V2MfBPd779C72n8IXXUkgCrTaPTI3suYRNrHt_Y_iQb4sh1nafIEzZHpBoHr-OaUt7Zi7RYYP44d8Z5j9sdFBgIu2Rvf4BXPHSowGM6w)

### Cloud NAT

[![](https://lh4.googleusercontent.com/kEO6AjBYIjLEr836MKhtBdYnGQ5EghmhOhMJl8LnTElzRKCncF-K8owYTHfEdkCvMFkUBFj4TGCpPydRskmc4WMr-FwqdKrSpa2NcHbqcyYHMgZSPxngxeJojpUYC6HlMP_2UFRSIayNc_4q1P8X0-m8zJq4UB6Hb5Lu69OiQ2BqfJkZ5YbQs-gsFhVZBA)](https://lh4.googleusercontent.com/kEO6AjBYIjLEr836MKhtBdYnGQ5EghmhOhMJl8LnTElzRKCncF-K8owYTHfEdkCvMFkUBFj4TGCpPydRskmc4WMr-FwqdKrSpa2NcHbqcyYHMgZSPxngxeJojpUYC6HlMP_2UFRSIayNc_4q1P8X0-m8zJq4UB6Hb5Lu69OiQ2BqfJkZ5YbQs-gsFhVZBA)

### Configure a Cloud NAT gateway

Cloud NAT is a regional resource. You can configure it to allow traffic from all ranges of all subnets in a region, from specific subnets in the region only, or from specific primary and secondary CIDR ranges only.

1. In the Cloud Console, on the **Navigation menu** (), click **Network services > Cloud NAT**.
    
    [![](https://lh4.googleusercontent.com/byy3Ka9AJ-mBb0dg4bU1STc3XFGaLeQKmGK_zXEC-N3NzN5sl2k5lwxm00BJ9dsjG9ZK9h-BSEobmh3w2WDL3UroLExBGuaTL3BYiQpVMnypxYh_6z0Z9SnJ3pNevEO9HX1DepIDNhRaBRaP4w2FBc0gUukzbtJeroRdbx2FotMLA1o-I8jxMkEdEiCOOw)](https://lh4.googleusercontent.com/byy3Ka9AJ-mBb0dg4bU1STc3XFGaLeQKmGK_zXEC-N3NzN5sl2k5lwxm00BJ9dsjG9ZK9h-BSEobmh3w2WDL3UroLExBGuaTL3BYiQpVMnypxYh_6z0Z9SnJ3pNevEO9HX1DepIDNhRaBRaP4w2FBc0gUukzbtJeroRdbx2FotMLA1o-I8jxMkEdEiCOOw)
    
2. Click **Get started** to configure a NAT gateway.
3. Specify the following:

|   |   |
|---|---|
|**Property**|**Value (type value or select option as specified)**|
|Gateway name|nat-config|
|Network|privatenet|
|Region|us-central1|

1. For **Cloud Router**, select **Create new router**.
2. For **Name**, type **nat-router**
3. Click **Create**.