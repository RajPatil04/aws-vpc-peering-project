# AWS VPC Peering Project — Overview

## 1. Introduction

This project demonstrates how to establish private network connectivity between two Amazon Virtual Private Clouds (VPCs) using AWS VPC Peering.

The implementation uses Amazon EC2 instances, custom route tables, security groups, an internet gateway, and a bastion host to demonstrate secure network communication.

## 2. Problem Statement

Applications deployed in separate VPCs may need to communicate privately without exposing their application servers directly to the public internet.

This project addresses that requirement by establishing a VPC peering connection and configuring the necessary routes and security rules.

## 3. Objectives

- Create two VPCs with non-overlapping IPv4 CIDR ranges.
- Configure public and private subnets.
- Launch a bastion host and two private EC2 instances.
- Establish a VPC peering connection.
- Configure forward and return routes.
- Restrict network access using security groups.
- Verify private connectivity using ICMP and SSH.
- Demonstrate a controlled connectivity failure and recovery.
- Document the implementation using screenshots and test results.

## 4. Proposed Architecture

The architecture contains two VPCs connected through a VPC peering connection.

### VPC A

- CIDR: `10.10.0.0/16`
- Public subnet: `10.10.1.0/24`
- Private subnet: `10.10.2.0/24`
- Bastion private IP: `10.10.1.10`
- App-A private IP: `10.10.2.10`

### VPC B

- CIDR: `10.20.0.0/16`
- Private subnet: `10.20.1.0/24`
- App-B private IP: `10.20.1.10`

The bastion host provides administrative access. App-A and App-B communicate through private IP addresses over the peering connection.

These addresses represent the planned configuration and must be checked against the actual AWS resources.

## 5. AWS Services Used

| Service | Purpose |
|---|---|
| Amazon VPC | Provides isolated virtual networks |
| Amazon EC2 | Hosts the bastion and application instances |
| Internet Gateway | Provides internet connectivity to the public subnet |
| VPC Peering | Connects the two VPCs privately |
| Route Tables | Direct traffic to the appropriate destinations |
| Security Groups | Control inbound and outbound instance traffic |
| EC2 Key Pairs | Authenticate SSH connections |

## 6. Security Considerations

- Keep both application instances without public IPv4 addresses.
- Allow SSH only from the required administrative source.
- Restrict ICMP traffic to the required peer hosts.
- Configure routing in both directions.
- Keep private keys and credentials outside the GitHub repository.
- Remove lab resources after collecting the required evidence.

## 7. Validation Plan

The project will be validated using the following tests:

1. Confirm that connectivity fails before peering is configured.
2. Verify forward connectivity from App-A to App-B.
3. Verify reverse connectivity from App-B to App-A.
4. Connect to App-B through the bastion host.
5. Remove the specified ICMP rule and verify the expected failure.
6. Restore the rule and verify connectivity recovery.
7. Confirm that App-A and App-B have no public IPv4 addresses.

Actual outcomes and screenshots will be added after the tests are performed.

## 8. Expected Learning Outcomes

This project provides practical experience with AWS networking, IPv4 addressing, subnet design, route tables, VPC peering, security groups, SSH, connectivity testing, and cloud resource cleanup.

## 9. Project Status

**Status:** In Progress

The AWS infrastructure, validation results, screenshots, and final report will be documented as the project progresses.
