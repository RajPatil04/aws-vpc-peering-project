# AWS VPC Peering Project

### Connecting Two Private Networks Using AWS VPC Peering

An AWS networking project demonstrating private IPv4 connectivity between two Virtual Private Clouds (VPCs) using VPC peering, Amazon EC2, custom route tables, security groups, and a bastion host.

## Project Overview

The objective of this project is to establish secure communication between two separate AWS VPCs while keeping the application servers private.

A bastion host in VPC A provides an administrative entry point. The two application instances communicate through a VPC peering connection using their private IPv4 addresses.

## Architecture Overview

The project consists of:

- Two non-overlapping VPCs.
- Three subnets across the two VPCs.
- One public bastion host.
- Two private EC2 application instances.
- One VPC peering connection.
- An internet gateway for the public subnet.
- Custom route tables and security groups.

The architecture diagram will be added after the network design is prepared.

## Network Addressing Plan

| Resource | CIDR / Private IP |
|---|---|
| VPC A | `10.10.0.0/16` |
| Public subnet A | `10.10.1.0/24` |
| Private subnet A | `10.10.2.0/24` |
| VPC B | `10.20.0.0/16` |
| Private subnet B | `10.20.1.0/24` |
| Bastion host | `10.10.1.10` |
| App-A | `10.10.2.10` |
| App-B | `10.20.1.10` |

These are the planned addresses from the project workbook. Actual resource IDs and test results will be recorded during implementation.

## AWS Services

- Amazon VPC
- Amazon EC2
- VPC Peering
- Internet Gateway
- Route Tables
- Security Groups
- EC2 Key Pairs

## Security Design

- Keep App-A and App-B without public IPv4 addresses.
- Restrict SSH access to the required administrative hosts.
- Use private IP addresses for inter-VPC communication.
- Configure routes in both directions.
- Never publish private keys, passwords, or access tokens.

## Testing and Validation

The implementation will be tested for:

1. Connectivity failure before peering.
2. Successful communication from App-A to App-B.
3. Successful reverse communication from App-B to App-A.
4. SSH access to App-B through the bastion host.
5. Controlled ICMP failure and recovery.
6. Private addressing of both application instances.

Test results and screenshots will be added after the tests have been performed.

## Documentation

Detailed network configuration, routing, security rules, troubleshooting, and cleanup procedures will be documented in the `docs/` directory.

## Project Status

**Status:** In Progress

The repository is being prepared before the AWS infrastructure is implemented.

## Learning Outcomes

This project aims to develop practical knowledge of AWS networking, CIDR addressing, route tables, VPC peering, security groups, private connectivity, SSH administration, and network troubleshooting.
