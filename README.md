# Cloud-Cloud Security & SOC Homelab

Overview

This project is a lightweight cloud-security and SOC practice lab built on Kali Linux.

The lab was designed to simulate practical cloud-security activities without using a paid AWS account. Instead, I used Moto Server to create a local AWS-compatible environment and AWS CLI to interact with cloud resources.

The project focused on securing S3 resources, reviewing IAM permissions, applying least privilege, generating suspicious cloud activity, analyzing alerts, and documenting a simulated security incident.

Objectives

* Practice cloud security fundamentals
* Identify and remediate S3 security misconfigurations
* Understand IAM permissions and excessive privileges
* Apply the principle of least privilege
* Generate and investigate cloud activity
* Create and analyze security alerts
* Practice basic SOC incident investigation
* Document a security incident and response workflow

Technologies Used

* Kali Linux
* Docker
* Moto Server
* AWS CLI
* Amazon S3 API
* AWS IAM API
* Linux/Grep

Lab Architecture

Kali Linux
     │
     ▼
   Docker
     │
     ▼
 Moto Server
     │
     ├── S3
     │    └── dmc-lab-bucket
     │
     └── IAM
          └── soc-analyst
     
Cloud Activity
     │
     ▼
Security Alert
     │
     ▼
Log Analysis
     │
     ▼
Incident Investigation

1. S3 Security

A bucket named dmc-lab-bucket was created for the exercise.

I intentionally simulated a public-read configuration and then remediated it.

The final bucket ACL contained only the owner with:

FULL_CONTROL

There was no public:

AllUsers READ

permission.

Security lesson

Cloud storage should not be publicly accessible unless there is a legitimate and documented requirement.

⸻

2. IAM Security

An IAM user named:

soc-analyst

was created.

To simulate an excessive-permission scenario, an intentionally over-privileged policy was created:

LabAdministratorPolicy

The policy granted:

Action: *
Resource: *

This represented an administrator-level permission model and was used only for security testing.

The excessive policy was subsequently removed.

⸻

3. Least Privilege

A more restrictive policy was created:

SOC-S3-ReadOnly

The policy allowed:

s3:GetObject
s3:ListBucket

No delete, write, or administrator permissions were included.

This demonstrated the Principle of Least Privilege.

⸻

4. Cloud Activity

A test object was uploaded:

test.txt

to:

dmc-lab-bucket

The object was later deleted as part of the investigation exercise.

Because Moto does not enforce AWS IAM authorization exactly like production AWS, the deletion result was not treated as proof that the IAM policy permitted deletion.

Instead, the event was used to simulate suspicious cloud activity.

⸻

5. SOC Alert

A local security log was created:

~/soc-alerts.log

The simulated alert contained:

ALERT | S3 object deleted | bucket=dmc-lab-bucket | object=test.txt

The alert was then filtered using:

grep "ALERT" ~/soc-alerts.log

This simulated a basic SOC analyst workflow of identifying relevant security events from logs.

⸻

6. Incident Documentation

An incident record was added to the log:

INCIDENT | Unauthorized S3 object deletion | STATUS=INVESTIGATING

The investigation workflow was:

Cloud Activity
      ↓
Security Alert
      ↓
Log Analysis
      ↓
Investigation
      ↓
Incident Documentation

7. Investigation Approach

For a suspicious cloud event, the investigation process would be:

1. Review the relevant logs.
2. Identify the affected resource.
3. Identify the associated user or identity.
4. Determine whether the activity was authorized.
5. Assess potential impact.
6. Contain or remediate the threat where necessary.
7. Document the incident.

Key Takeaways

This project provided hands-on practice with:

* Cloud security
* S3 security
* IAM
* Least privilege
* Security monitoring
* Log analysis
* Alert triage
* Incident investigation
* Incident documentation

Important Lab Note

This was a local training environment using Moto Server and dummy credentials. It was created for cybersecurity learning and portfolio demonstration and was not intended to represent a production AWS environment.

Future Improvements

Potential future improvements include:

* Connecting cloud logs to a lightweight SIEM
* Automated alert generation
* Detection rules
* Threat-hunting exercises
* Automated incident response
* Integration with a larger SOC homelab

Author

Oluwadamola Ologan

Cybersecurity | Cloud Security | SOC | IT-SOC-Homelab
