# Project Details — AWS VPC Peering

**Author:** Raj Patil  
**Batch:** AWS Internship Batch1  
**Region:** `ap-south-1` (Mumbai)

## Purpose

Create private connectivity between two non-overlapping VPCs using a VPC peering connection. Use a bastion host for SSH administration and keep the two application instances on private addresses.

## Resource inventory

| Resource | Name | Address/CIDR | ID / notes |
|---|---|---|---|
| VPC A | `ewb-vpc-a` | `10.10.0.0/16` | `vpc-069ab77a8167915a5` |
| VPC B | `ewb-vpc-b` | `10.20.0.0/16` | `vpc-0cac7153b279dc807` |
| Public subnet A | `ewb-a-public` | `10.10.1.0/24` | `subnet-0449d69d8404dddbf` |
| Private subnet A | `ewb-a-private` | `10.10.2.0/24` | `subnet-01c558669ecd54a5d` |
| Private subnet B | `ewb-b-private` | `10.20.1.0/24` | `subnet-0d4ac455f97381f5` |
| Internet gateway | `ewb-igw-a` | VPC A | `igw-0508230ad22b0724f` |
| VPC peering | `ewb-a-b-peer` | Active | `pcx-086d9f1f868e17c94` |
| Public bastion | `ewb-bastion` | Private `10.10.1.179`; public `13.232.37.153` | EC2 |
| App-A | `ewb-app-a` | `10.10.2.165` | Private EC2 |
| App-B | `ewb-app-b` | `10.20.1.42` | Private EC2 |

## Route tables

- **`ewb-rt-a-public`**, associated with public subnet A:
  - `10.10.0.0/16 → local`
  - `0.0.0.0/0 → Internet Gateway`
  - `10.20.0.0/16 → VPC peering`
- **`ewb-rt-a-private`**, associated with private subnet A:
  - `10.10.0.0/16 → local`
  - `10.20.0.0/16 → VPC peering`
- **`ewb-rt-b-private`** (`rtb-0e423296f55a2b12f`), associated with private subnet B:
  - `10.20.0.0/16 → local`
  - `10.10.0.0/16 → VPC peering`

## Security group intent

- Bastion: allow SSH TCP/22 only from the administrator's public IP in CIDR `/32`.
- App-A and App-B: allow SSH TCP/22 from bastion private IP `10.10.1.179/32`.
- App-A: allow ICMP Echo Request from App-B `10.20.1.42/32`.
- App-B: allow ICMP Echo Request from App-A `10.10.2.165/32`.

Confirm actual security-group rules in the AWS console before using this document as an audit record.

## Traffic flow

For App-A to reach App-B, App-A sends traffic to `10.20.1.42`. The subnet's associated route table in VPC A needs a route for `10.20.0.0/16` targeting the active peering connection. VPC B's route table needs the return route `10.10.0.0/16` targeting the same peering connection. Security groups must allow the intended traffic in both directions. The reverse path uses the corresponding routes and source/destination addresses.

## Evidence status

Reported tests:
- App-A → App-B, `ping -c 4 10.20.1.42`: 4 transmitted, 4 received, 0% packet loss.
- App-B → App-A, `ping -c 4 10.10.2.165`: 4 transmitted, 4 received, 0% packet loss.
- SSH access to application instances through the bastion was confirmed by the user.

Evidence still to add if available:
- Clearly labelled screenshots for each test and host.
- A baseline connectivity failure before peering (not captured in the available notes).
- Controlled ICMP failure and recovery screenshots (not captured in the available notes).
- Cleanup evidence / final resource verification (not available yet).

Do not claim missing evidence as completed, and do not upload PEM keys, passwords, AWS access keys, session tokens, or other secrets.
