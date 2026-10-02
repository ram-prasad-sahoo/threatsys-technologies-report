# Cloud Security — domain reference

**Section label:** rename `Vulnerable URL` to `Affected Cloud Resource`.

**Value format:** the exact resource identifier — ARN, bucket name, resource group, or console
path, e.g. `arn:aws:s3:::example-invoices-bucket` or the IAM role
`arn:aws:iam::123456789012:role/AppDeployRole`. List all affected resources if more than one was
demonstrated.

**Identifier style in Description:** name the exact bucket/role/policy/security-group/instance
affected — e.g. "the `example-invoices-bucket` S3 bucket", "the `AppDeployRole` IAM role's
attached policy", "security group `sg-0a1b2c3d` allowing inbound `0.0.0.0/0` on port 22" — not
generic terms like "a resource" or "a setting".

**Typical evidence to look for:** cloud console screenshots, CLI output (`aws s3api`, `az`,
`gcloud`), IAM policy JSON, security group/firewall rules, bucket ACLs, metadata endpoint
responses, cloud config scanner output.

**Classification guidance:**
- Give the most specific CWE supported (e.g. CWE-284 Improper Access Control, CWE-732 Incorrect
  Permission Assignment for Critical Resource, CWE-200 Exposure of Sensitive Information, CWE-16
  Configuration).
- Add the relevant CIS Benchmark control (e.g. CIS AWS Foundations 2.1.x for public S3 buckets) or
  CSA Cloud Controls Matrix domain when the evidence maps cleanly to one. Do not force a mapping
  that isn't a good fit.
- CVE only for a known, specifically identified CVE in cloud-provider or managed-service software.

**Recommendation patterns to draw from:** restrict the resource policy/ACL to least-privilege
principals only, disable public access at the bucket/account level (e.g. S3 Block Public Access),
scope IAM policies to specific actions/resources instead of wildcards, restrict security group
ingress to known CIDR ranges, enable logging/monitoring (e.g. CloudTrail, GuardDuty) on the
resource, rotate any credentials exposed during testing.
