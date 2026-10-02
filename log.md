# Bundle Update Log

## 2026-10-02
* **Creation**: Added [aws-cli/](aws-cli/index.md), a one-stop guide to the AWS Command Line Interface built from the official v2 User Guide and Command Reference — installing and updating (Linux/macOS/Windows), `aws configure` and named profiles, the config/credentials files and precedence, IAM Identity Center (SSO) and assumed roles, command structure and quoting, `--output`/`--filters`/`--query` (JMESPath), pagination and skeleton input files, `aws s3` vs `aws s3api`, auto-prompt/completion/aliases/scripting, troubleshooting, and best practices — using the "Universal Remote" mnemonic analogy and three original diagrams.

## 2026-09-15
* **Creation**: Added [device-farm/](device-farm/index.md), covering AWS Device Farm end to end — real device testing and device pools, automated testing frameworks (Appium, Instrumentation, XCTest/XCTest UI, built-in Fuzz), Remote Access and the client-side Appium endpoint, desktop browser testing (managed Selenium Grid), test results and debugging, the Private Device Lab, and integrations/workflow — using the "Real Device Farm" (barn) mnemonic analogy and three original diagrams.
* **Creation**: Added [global-infrastructure/](global-infrastructure/index.md), covering Regions, Availability Zones, Edge Locations & CloudFront, Local Zones & Wavelength Zones, and designing for high availability across both — using the "World Map" (cities, boroughs, corner stores) mnemonic analogy and three original diagrams.
* **Creation**: Added [vpc/](vpc/index.md), covering VPCs, subnets, route tables, Internet Gateways, NAT Gateways (in dedicated depth, including the one-per-AZ HA pattern and the per-GB cost trap), Security Groups vs. Network ACLs, VPC Peering & Endpoints, and hybrid connectivity (VPN, Direct Connect, Transit Gateway) — using the "Gated Community" mnemonic analogy and three original diagrams.
* **Addition**: Added [vpc/tenancy.md](vpc/tenancy.md), covering default/dedicated-instance/dedicated-host tenancy and the VPC-level instance tenancy attribute.
* **Creation**: Added [dynamodb/](dynamodb/index.md), covering tables/items/attributes, primary keys & partitions (including the hot-partition problem), Query vs. Scan, Secondary Indexes (GSI/LSI), capacity modes, and Streams/TTL/Global Tables/DAX — using the "Valet Parking Garage" mnemonic analogy and three original diagrams — plus a hands-on tutorial building a real `CustomerOrders` table with Console steps and AWS CLI commands.

## 2026-09-13
* **Creation**: Added [cloud-computing/](cloud-computing/index.md), a fundamentals folder covering why organizations move to the cloud, a short history of AWS, the IaaS/PaaS/serverless spectrum, and decomposing a single server into EC2/RDS/S3, using the "Moving Day" mnemonic analogy and six original diagrams.
* **Creation**: Added [s3/](s3/index.md), covering Amazon S3 end to end — buckets, objects, storage classes, versioning, lifecycle, replication, security, access points, encryption, sharing, static hosting, performance, consistency, event processing, object lock, and monitoring/cost — using the "Self-Storage Facility" mnemonic analogy and eleven original diagrams.

## 2026-09-12
* **Initialization**: Created the AWS Knowledge Bundle following the [OKF v0.2 spec](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md).
* **Creation**: Added the first service folder, [iam/](iam/index.md), covering AWS Identity and Access Management with the "Secure Office Building" mnemonic analogy and eight original diagrams.
