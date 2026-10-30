# Screenshot index

All screenshots were taken on 30 October 2026 in `ap-south-1` (Mumbai), in build order.

## Phase 1 — VPC A

| # | File | What it proves |
|---|---|---|
| 01 | [01-vpc-a-created.png](../screenshots/01-vpc-a-created.png) | `ewb-vpc-a` created with CIDR `10.10.0.0/16` |
| 02 | [02-subnet-a-public.png](../screenshots/02-subnet-a-public.png) | `ewb-a-public` = `10.10.1.0/24` in `ap-south-1a` |
| 03 | [03-subnet-a-private.png](../screenshots/03-subnet-a-private.png) | `ewb-a-private` = `10.10.2.0/24` in `ap-south-1a` |
| 04 | [04-igw-attached.png](../screenshots/04-igw-attached.png) | `ewb-igw-a` state Attached to VPC A |
| 05 | [05-rt-a-public-routes.png](../screenshots/05-rt-a-public-routes.png) | Public table: `0.0.0.0/0 → igw` + local |
| 06 | [06-rt-a-public-associations.png](../screenshots/06-rt-a-public-associations.png) | Public table explicitly associated with `ewb-a-public` |
| 07 | [07-rt-a-private-routes.png](../screenshots/07-rt-a-private-routes.png) | Private table: local route only (no internet) |
| 08 | [08-rt-a-private-associations.png](../screenshots/08-rt-a-private-associations.png) | Private table associated with `ewb-a-private` |

## Phase 2 — VPC B

| # | File | What it proves |
|---|---|---|
| 09 | [09-vpc-b-created.png](../screenshots/09-vpc-b-created.png) | `ewb-vpc-b` created with CIDR `10.20.0.0/16` |
| 10 | [10-rt-b-private-created.png](../screenshots/10-rt-b-private-created.png) | `ewb-rt-b-private`: local route only |
| 11 | [11-rt-b-private-associations.png](../screenshots/11-rt-b-private-associations.png) | Associated with `ewb-b-private` (`10.20.1.0/24`) |

## Phase 3 — Security groups

| # | File | What it proves |
|---|---|---|
| 12 | [12-sg-bastion.png](../screenshots/12-sg-bastion.png) | Bastion SG: SSH from my IP only |
| 13 | [13-sg-app-a.png](../screenshots/13-sg-app-a.png) | App-A SG: SSH from bastion + ICMP from App-B |
| 14 | [14-sg-app-b.png](../screenshots/14-sg-app-b.png) | App-B SG: SSH from bastion + ICMP Echo Request from App-A |

## Phase 4 — Instances

| # | File | What it proves |
|---|---|---|
| 15 | [15-instances-running.png](../screenshots/15-instances-running.png) | 3 × t3.micro running; only bastion has a public IP |
| 16 | [16-bastion-details.png](../screenshots/16-bastion-details.png) | Bastion: private `10.10.1.10` + public IP, subnet `ewb-a-public` |
| 17 | [17-app-a-details.png](../screenshots/17-app-a-details.png) | App-A: `10.10.2.10`, **no** public IPv4 |
| 18 | [18-app-b-details.png](../screenshots/18-app-b-details.png) | App-B: `10.20.1.10` in VPC B, **no** public IPv4 |

## Phase 5 — SSH + baseline

| # | File | What it proves |
|---|---|---|
| 19 | [19-ssh-bastion.png](../screenshots/19-ssh-bastion.png) | Laptop → bastion SSH works, `hostname -I` = `10.10.1.10` |
| 20 | [20-baseline-ping-fail.png](../screenshots/20-baseline-ping-fail.png) | **Before peering:** App-A → App-B ping 100% loss |

## Phase 6–7 — Peering + routes

| # | File | What it proves |
|---|---|---|
| 21 | [21-peering-active.png](../screenshots/21-peering-active.png) | `ewb-a-b-peer` Active, CIDRs `10.10.0.0/16` ↔ `10.20.0.0/16` |
| 22 | [22-rt-b-private-peer-route.png](../screenshots/22-rt-b-private-peer-route.png) | VPC B: `10.10.0.0/16 → pcx` (return path) |
| 23 | [23-rt-a-private-peer-route.png](../screenshots/23-rt-a-private-peer-route.png) | VPC A private: `10.20.0.0/16 → pcx` |
| 24 | [24-rt-a-public-peer-route.png](../screenshots/24-rt-a-public-peer-route.png) | VPC A public: `10.20.0.0/16 → pcx` (bastion → App-B SSH hop) |

## Phase 8 — Tests

| # | File | What it proves |
|---|---|---|
| 25 | [25-forward-ping-success.png](../screenshots/25-forward-ping-success.png) | App-A → App-B: 4/4, 0% loss |
| 26 | [26-ssh-app-b-reverse-ping.png](../screenshots/26-ssh-app-b-reverse-ping.png) | SSH to App-B via bastion + App-B → App-A: 4/4, 0% loss |
| 27 | [27-sg-app-b-icmp-removed.png](../screenshots/27-sg-app-b-icmp-removed.png) | Controlled failure: ICMP rule removed (SSH kept) |
| 28 | [28-icmp-removed-ping-fail.png](../screenshots/28-icmp-removed-ping-fail.png) | Ping now 100% loss |
| 29 | [29-sg-app-b-icmp-restored.png](../screenshots/29-sg-app-b-icmp-restored.png) | ICMP rule restored |
| 30 | [30-icmp-restored-ping-success.png](../screenshots/30-icmp-restored-ping-success.png) | Ping recovered: 4/4, 0% loss |

## Phase 9 — Cleanup

| # | File | What it proves |
|---|---|---|
| 31 | [31-cleanup-vpcs.png](../screenshots/31-cleanup-vpcs.png) | Both lab VPCs deleted, only default VPC remains |
| 32 | [32-cleanup-peering-deleted.png](../screenshots/32-cleanup-peering-deleted.png) | Peering connection status Deleted |
| 33 | [33-cleanup-instances-terminated.png](../screenshots/33-cleanup-instances-terminated.png) | All 3 instances Terminated |
