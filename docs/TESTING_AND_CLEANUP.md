# Testing, Troubleshooting, and Cleanup

## Test checklist

Before testing, verify that the VPC peering connection is **Active**, each route table is associated with the intended subnet, and both VPCs have routes for the other VPC CIDR through the peering connection.

### 1. Forward private connectivity

Run on App-A:

```bash
ping -c 4 10.20.1.42
```

Reported result: **4 packets transmitted, 4 received, 0% packet loss**.

### 2. Reverse private connectivity

Run on App-B:

```bash
ping -c 4 10.10.2.165
```

Reported result: **4 packets transmitted, 4 received, 0% packet loss**.

### 3. SSH through the bastion

Connect to the bastion using the approved administrator IP and private key. From the bastion, SSH to App-A at `10.10.2.165` and App-B at `10.20.1.42` using the relevant operating-system username and the key-forwarding or key-transfer method configured for the lab. Do not place private keys in this repository.

The user confirmed SSH access through the bastion, but screenshots should be added to the `evidence/` folder to demonstrate it in the submitted report.

## Troubleshooting checklist

If ping fails:
1. Confirm peering status is Active.
2. Confirm VPC CIDRs do not overlap.
3. Check route table associations and routes in both directions.
4. Check security-group ICMP rules and any network ACL rules.
5. Confirm the destination private IP is correct and the instance is running.
6. Verify the OS firewall permits ICMP Echo Request.

If SSH fails:
1. Confirm the bastion is reachable on TCP/22 from the authorized public IP.
2. Confirm application security groups allow SSH from the bastion private IP.
3. Confirm the target private IP, username, and key permissions.
4. Check instance status checks and OS firewall settings.

## Evidence capture

Use clear filenames, for example:
- `01-vpc-peering-active.png`
- `02-route-tables.png`
- `03-app-a-to-app-b-ping.png`
- `04-app-b-to-app-a-ping.png`
- `05-ssh-via-bastion.png`
- `06-controlled-icmp-test.png`
- `07-cleanup-verification.png`

Add only screenshots you actually captured. Crop out account IDs or other information you do not want to publish. Never add `.pem` files, credentials, access keys, session tokens, or secrets.

## Cleanup checklist

Perform cleanup only when you no longer need the lab. Confirm the AWS region is Mumbai (`ap-south-1`) before deleting resources.

1. Terminate the three EC2 instances (bastion, App-A, App-B).
2. Check EBS volumes and snapshots; remove lab-only resources that are no longer needed.
3. Release any Elastic IP allocated specifically for this lab, if present.
4. Remove routes that target the peering connection, then delete the VPC peering connection.
5. Delete lab security groups after dependent network interfaces are gone.
6. Delete custom subnets and custom route tables.
7. Detach and delete the internet gateway from VPC A.
8. Delete both lab VPCs.
9. Re-check the region for remaining lab resources and confirm that no unintended billable resources remain.

AWS console resource states can change, so verify each resource before deleting it. Do not delete resources that belong to other projects.
