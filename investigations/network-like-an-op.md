# Week 5 — Network Like an Op

## Objective
Investigate how an application workload communicates with Azure Storage through a private endpoint. Review the virtual network, security rules, routing, private endpoint, and DNS configuration to understand the network path.

## Environment

- **Platform:** Microsoft Azure
- **Environment:** Live multi-user Azure training tenant
- **Access Level:** Reader access
- **Tools:** Azure Portal
- **Services Investigated:**
  - Azure Virtual Network (VNet)
  - Network Security Groups (NSGs)
  - User-Defined Routes (UDRs)
  - Azure Private Endpoint
  - Azure Private DNS (Domain Name System)
  - Azure Storage

## The Ticket
The application team wants to understand how its workload can reach Azure Storage privately and why traffic may bypass the firewall despite a forced-egress route.

## Network Trace

### Hop 1 — Virtual Network (VNet)

**Checked:** `vnet-lab-chap5` → Subnets → `snet-workload`

**Found:** The workload subnet uses `10.60.1.0/24`.

**What it means:** The application operates within this subnet. Its address range identifies the network where the workload originates.

### Hop 2 — Network Security Group (NSG)

**Checked:** `nsg-lab-workload` → Inbound security rules

**Found:** The `allow-vpn-https` rule permits inbound HTTPS (Hypertext Transfer Protocol Secure) traffic from `203.0.113.50`.

**What it means:** The rule allows a specific source address to connect using HTTPS rather than allowing unrestricted inbound access. This is an inbound rule; outbound access to Storage must be evaluated separately.

### Hop 3 — Route Table

**Checked:** `rt-lab-workload` → Routes → `force-egress`

**Found:** The route points to next-hop IP (Internet Protocol) address `10.60.100.4`.

**What it means:** The route directs matching outbound traffic toward a network appliance for inspection. However, Azure selects routes using longest-prefix matching, so a more specific route can take precedence.

### Hop 4 — Private Endpoint

**Checked:** `pe-lab-storage` → DNS configuration

**Found:** The private endpoint uses IP address `10.60.1.62`.

**What it means:** Azure Storage can be accessed through a private IP inside the virtual network rather than through its public endpoint.

### Hop 5 — Domain Name System (DNS)

**Checked:** `privatelink.blob.core.windows.net` → A record

**Found:** `stmadhatlabchap5.privatelink.blob.core.windows.net`

**What it means:** The private DNS zone maps the Storage service's hostname to its private endpoint IP, allowing applications to use a hostname instead of hardcoding an address.

## Answer to the Application Team

The application operates in `snet-workload` within `vnet-lab-chap5`. The network security group controls permitted traffic, while the route table directs matching outbound traffic toward `10.60.100.4`.

Azure Storage is accessible through a private endpoint at `10.60.1.62`. Private DNS allows the application to resolve the Storage service to its private address.

A private endpoint's specific `/32` route can take precedence over a general `0.0.0.0/0` forced-egress route, explaining why Storage traffic may bypass the firewall.

## Security Observations

The inbound HTTPS rule restricts access to a specific source IP. However, the network team should verify that Storage traffic bypassing the firewall is intentional and that public network access to the Storage account is disabled.

## Lessons Learned

- Network Security Groups control permitted traffic.
- Route tables determine where packets are forwarded.
- More specific routes take precedence over broader routes.
- Private Endpoints provide private access to Azure services.
- DNS resolves service hostnames to private IP addresses.
