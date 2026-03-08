# Reddit Posts — Batch 2 (Articles 7-16)

---

## Article 7: AKS Production
### r/AZURE
**Title:** Open-sourced a production AKS Terraform module with CNI Overlay, Workload Identity, and Defender for Containers

Sharing a Terraform module for AKS that goes beyond the basics — Azure CNI Overlay networking, Workload Identity federation, Defender for Containers, ephemeral OS disks, and Azure Policy add-on for pod security enforcement.

Includes a networking comparison (kubenet vs CNI vs CNI Overlay vs CNI Cilium) and when to use each. Also covers auto-scaling with KEDA and node pool management.

Module: https://github.com/kogunlowo123/terraform-azure-aks
Full guide: https://kogunlowo123.github.io/blog/terraform-azure-aks-production.html

---

## Article 8: Bedrock Agents
### r/aws
**Title:** Lessons learned building autonomous workflows with AWS Bedrock Agents in production

Sharing patterns from deploying Bedrock Agents with knowledge bases and action groups. Key lessons: always use agent aliases (never DRAFT in production), validate all action group inputs in Lambda before execution, use OpenSearch Serverless over Pinecone for the vector store (cost and latency), and set guardrails to prevent destructive actions.

Includes a comparison of Bedrock Agents vs LangChain vs Azure AI Agents.

Full writeup: https://kogunlowo123.github.io/blog/aws-bedrock-agents-autonomous-workflows.html

---

## Article 9: DevSecOps Pipeline
### r/devops
**Title:** Complete DevSecOps pipeline architecture with security gates at every stage — open-sourced

Wrote up the DevSecOps pipeline architecture we use across multi-cloud deployments. Covers pre-commit (secret scanning, IaC linting), build (SAST, SCA, container scanning), deploy (policy-as-code, signed artifacts), and runtime (DAST, Falco, CSPM).

Includes a tools comparison: Snyk vs SonarQube vs Checkov vs Trivy vs tfsec.

Full guide: https://kogunlowo123.github.io/blog/devsecops-pipeline-multi-cloud.html

### r/netsec
**Title:** DevSecOps pipeline security scanning — SAST, DAST, SCA, and container scanning comparison

Deep-dive into integrating security scanning across CI/CD pipelines for multi-cloud deployments. Covers tool selection (Snyk vs SonarQube vs Semgrep vs Trivy), pipeline stage design, and blocking vs non-blocking gates.

Full guide: https://kogunlowo123.github.io/blog/devsecops-pipeline-multi-cloud.html

---

## Article 10: GKE Autopilot
### r/googlecloud
**Title:** GKE Autopilot in production — what works, what doesn't, and Terraform module

After running Autopilot workloads for 12 months, sharing what actually works in production vs what the docs don't tell you. No DaemonSets, no privileged containers, resource requests must be accurate. But zero node management and pay-per-pod pricing are real advantages.

Includes Standard vs Autopilot vs Cloud Run decision framework.

Module: https://github.com/kogunlowo123/terraform-gcp-gke
Full guide: https://kogunlowo123.github.io/blog/gcp-gke-autopilot-terraform.html

---

## Article 11: Entra Zero Trust
### r/AZURE
**Title:** Microsoft Entra Zero Trust IAM — Conditional Access, PIM, and cross-cloud federation patterns

Wrote up the Entra ID features most enterprises are under-utilizing: Conditional Access with risk detection, PIM for just-in-time admin access, cross-cloud federation to AWS (OIDC) and GCP (Workload Identity), and passwordless with FIDO2.

Includes Entra vs AWS IAM Identity Center vs GCP Cloud Identity comparison.

Full guide: https://kogunlowo123.github.io/blog/microsoft-entra-zero-trust-iam.html

---

## Article 12: AWS VPC Networking
### r/aws
**Title:** AWS VPC networking patterns — subnet architecture, Transit Gateway, and PrivateLink with Terraform

Sharing the VPC architecture pattern I use across 50+ production deployments: public/private/isolated subnets, CIDR planning that doesn't break at scale, Transit Gateway for hub-and-spoke, and PrivateLink for service access without VPC CIDR overlap.

Module: https://github.com/kogunlowo123/terraform-aws-vpc-complete
Full guide: https://kogunlowo123.github.io/blog/terraform-aws-vpc-networking-guide.html

---

## Article 13: K8s Security
### r/kubernetes
**Title:** Kubernetes security hardening checklist — Pod Security Standards, Network Policies, RBAC, and runtime detection

Put together a comprehensive K8s security hardening guide based on the NSA/CISA hardening guide. Covers Pod Security Standards (Restricted mode), default-deny network policies, least-privilege RBAC, OPA Gatekeeper admission control, image scanning with Trivy, and runtime detection with Falco.

Full guide: https://kogunlowo123.github.io/blog/kubernetes-security-hardening.html

---

## Article 14: Azure Landing Zones
### r/Terraform
**Title:** Building Azure Landing Zones with Terraform — management groups, policy inheritance, and hub-spoke networking

Sharing a Terraform-based Azure landing zone implementation with management group hierarchy, Azure Policy at scale, hub-spoke networking with Azure Firewall, and subscription vending automation.

Comparison of CAF Enterprise-Scale vs custom Terraform vs Bicep approaches.

Full guide: https://kogunlowo123.github.io/blog/terraform-azure-landing-zone.html

---

## Article 15: Lambda Serverless
### r/aws
**Title:** AWS Lambda serverless patterns — SQS, DynamoDB Streams, EventBridge, and Step Functions with Terraform

Deep-dive into event-driven Lambda patterns that work at scale. Covers cold start optimization (layers, SnapStart, Graviton2, avoid VPC), DLQ configuration, and when to use Lambda vs Fargate vs EC2.

Module: https://github.com/kogunlowo123/terraform-aws-lambda
Full guide: https://kogunlowo123.github.io/blog/aws-lambda-serverless-patterns.html

---

## Article 16: Disaster Recovery
### r/devops
**Title:** Multi-cloud disaster recovery architecture — from Pilot Light to Active-Active with Terraform

Wrote up DR architecture patterns with real RPO/RTO numbers. Covers Backup/Restore, Pilot Light, Warm Standby, and Multi-Site Active — with Terraform code for Aurora Global Database failover and Route 53 health checks.

Most teams pick a DR strategy without defining RPO/RTO first. This guide starts with requirements.

Full guide: https://kogunlowo123.github.io/blog/cloud-disaster-recovery-terraform.html
