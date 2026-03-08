# Dev.to Cross-Posts — Ready to Publish
## All 6 Articles with Front Matter

---

## Article 1: Multi-Cloud Zero Trust Architecture

```markdown
---
title: "Building Zero Trust Architecture Across AWS, Azure, and GCP"
published: true
tags: zerotrust, aws, azure, devops
canonical_url: https://kogunlowo123.github.io/blog/multi-cloud-zero-trust-architecture.html
cover_image:
description: "A hands-on guide to implementing Zero Trust security across AWS, Azure, and GCP using identity-based access, microsegmentation, and NIST 800-207 principles."
---

Most organizations treat cloud security as an afterthought — bolting on firewalls after the architecture is already built. Zero Trust flips this entirely: **never trust, always verify**.

After implementing Zero Trust architectures across 30+ enterprise environments spanning AWS, Azure, and GCP, I've distilled the approach into repeatable patterns that work at scale.

## What You'll Learn

- How to implement identity-based perimeter across all three clouds
- Cross-cloud IAM federation with Azure Entra ID as the IdP
- Microsegmentation patterns for VPCs, VNets, and VPC Networks
- NIST 800-207 compliance mapping for multi-cloud

## The Multi-Cloud Zero Trust Stack

| Layer | AWS | Azure | GCP |
|-------|-----|-------|-----|
| Identity | IAM + SSO | Entra ID + Conditional Access | Cloud Identity + BeyondCorp |
| Network | Security Groups + NACLs | NSGs + Azure Firewall | Firewall Rules + VPC SC |
| Workload | GuardDuty + Inspector | Defender for Cloud | Security Command Center |
| Data | Macie + KMS | Purview + Key Vault | DLP + Cloud KMS |

## Terraform Implementation

Here's how I federate IAM across clouds using Terraform:

```hcl
# AWS OIDC Provider for Azure AD Federation
resource "aws_iam_openid_connect_provider" "azure_ad" {
  url             = "https://login.microsoftonline.com/${var.azure_tenant_id}/v2.0"
  client_id_list  = [var.azure_app_client_id]
  thumbprint_list = [var.azure_ad_thumbprint]
}

resource "aws_iam_role" "federated_role" {
  name = "azure-federated-access"
  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Effect = "Allow"
      Principal = { Federated = aws_iam_openid_connect_provider.azure_ad.arn }
      Action = "sts:AssumeRoleWithWebIdentity"
      Condition = {
        StringEquals = {
          "${aws_iam_openid_connect_provider.azure_ad.url}:aud" = var.azure_app_client_id
        }
      }
    }]
  })
}
```

## Best Practices

1. **Start with identity** — federate before you segment
2. **Use conditional access policies** for context-aware authentication
3. **Encrypt everything in transit and at rest** across all three clouds
4. **Log every access decision** to a centralized SIEM
5. **Automate compliance checks** with policy-as-code

## Resources

I've open-sourced the Terraform modules used in this guide:

- [terraform-aws-security-baseline](https://github.com/kogunlowo123/terraform-aws-security-baseline) — AWS security foundations
- [terraform-azure-hub-spoke-network](https://github.com/kogunlowo123/terraform-azure-hub-spoke-network) — Azure network segmentation
- [multi-cloud-landing-zone](https://github.com/kogunlowo123/multi-cloud-landing-zone) — Cross-cloud governance

Read the full guide with architecture diagrams at [kogunlowo123.github.io](https://kogunlowo123.github.io/blog/multi-cloud-zero-trust-architecture.html).

---

*Kehinde Ogunlowo is a Principal Multi-Cloud DevSecOps Architect at Citadel Cloud Management. Follow on [GitHub](https://github.com/kogunlowo123) and [LinkedIn](https://linkedin.com/in/kehinde-ogunlowo).*
```

---

## Article 2: Production RAG Pipelines on AWS

```markdown
---
title: "Building Production RAG Pipelines on AWS with Bedrock and OpenSearch"
published: true
tags: ai, aws, python, machinelearning
canonical_url: https://kogunlowo123.github.io/blog/production-rag-pipelines-aws.html
cover_image:
description: "End-to-end guide to building production RAG pipelines using AWS Bedrock, OpenSearch Serverless, and Terraform — with hallucination mitigation strategies."
---

RAG (Retrieval-Augmented Generation) is how enterprises are deploying LLMs without fine-tuning. But most tutorials stop at the demo stage. Production RAG is a different beast entirely.

Here's what production RAG actually requires — and how to build it on AWS.

## RAG vs Fine-Tuning vs Prompt Engineering

| Approach | Cost | Data Freshness | Accuracy | Complexity |
|----------|------|---------------|----------|------------|
| RAG | Medium | Real-time | High (with good retrieval) | Medium |
| Fine-Tuning | High | Static (retraining needed) | High | High |
| Prompt Engineering | Low | Static | Variable | Low |

## Architecture

The pipeline: Documents → Chunking → Embeddings → Vector Store → Query → Retrieval → LLM → Response.

## Python Implementation

```python
import boto3
import json

bedrock = boto3.client("bedrock-runtime", region_name="us-east-1")
opensearch = boto3.client("opensearchserverless")

def query_knowledge_base(question: str, collection_id: str) -> str:
    # Generate embedding for the question
    embed_response = bedrock.invoke_model(
        modelId="amazon.titan-embed-text-v2:0",
        body=json.dumps({"inputText": question})
    )
    query_embedding = json.loads(embed_response["body"].read())["embedding"]

    # Search OpenSearch vector store
    results = search_vectors(query_embedding, collection_id, k=5)
    context = "\n".join([r["text"] for r in results])

    # Generate answer with context
    prompt = f"""Based on the following context, answer the question.

Context: {context}

Question: {question}

Answer:"""

    response = bedrock.invoke_model(
        modelId="anthropic.claude-3-sonnet-20240229-v1:0",
        body=json.dumps({
            "anthropic_version": "bedrock-2023-05-31",
            "messages": [{"role": "user", "content": prompt}],
            "max_tokens": 1024
        })
    )
    return json.loads(response["body"].read())["content"][0]["text"]
```

## Hallucination Mitigation

1. **Chunk size matters** — 512 tokens with 50-token overlap works best
2. **Hybrid search** — combine semantic + keyword search (BM25)
3. **Citation grounding** — force the model to cite source chunks
4. **Confidence scoring** — filter low-relevance retrievals (cosine similarity < 0.7)
5. **Guardrails** — use Bedrock Guardrails to block off-topic responses

## Open Source Modules

- [terraform-aws-rag-pipeline](https://github.com/kogunlowo123/terraform-aws-rag-pipeline) — Complete RAG infrastructure
- [terraform-aws-bedrock-platform](https://github.com/kogunlowo123/terraform-aws-bedrock-platform) — Bedrock foundation
- [terraform-aws-bedrock-agents](https://github.com/kogunlowo123/terraform-aws-bedrock-agents) — Autonomous Bedrock agents

Full article with architecture diagrams: [kogunlowo123.github.io](https://kogunlowo123.github.io/blog/production-rag-pipelines-aws.html)

---

*Kehinde Ogunlowo — Principal Multi-Cloud DevSecOps Architect at [Citadel Cloud Management](https://citadelcloudmanagement.com)*
```

---

## Article 3: Complete EKS with Terraform

```markdown
---
title: "The Complete Guide to Production EKS with Terraform"
published: true
tags: kubernetes, aws, terraform, devops
canonical_url: https://kogunlowo123.github.io/blog/terraform-eks-complete-guide.html
cover_image:
description: "Production-ready EKS deployment with Terraform — Karpenter autoscaling, self-healing nodes, pod security standards, and multi-AZ high availability."
---

EKS is the most popular managed Kubernetes service, but most deployments I've seen in production audits are dangerously under-configured. Missing node auto-remediation, no pod security standards, manual scaling — the list goes on.

This guide covers everything you need for production EKS.

## EKS vs AKS vs GKE

| Feature | EKS | AKS | GKE |
|---------|-----|-----|-----|
| Control Plane Cost | $0.10/hr | Free | Free (Standard) |
| Autopilot Mode | No (use Karpenter) | No | Yes |
| Node Auto-Repair | Manual/Lambda | Built-in | Built-in |
| Service Mesh | App Mesh / Istio | Istio | Anthos / Istio |
| GPU Support | p4d, g5 | NC, ND series | T4, A100 |

## Terraform Module

```hcl
module "eks" {
  source = "github.com/kogunlowo123/terraform-aws-auto-healing-eks"

  cluster_name    = "production-cluster"
  cluster_version = "1.29"
  vpc_id          = module.vpc.vpc_id
  subnet_ids      = module.vpc.private_subnet_ids

  node_groups = [{
    name           = "general"
    instance_types = ["m6i.xlarge", "m6i.2xlarge"]
    min_size       = 3
    max_size       = 20
    desired_size   = 5
  }]

  enable_karpenter                = true
  enable_cluster_autoscaler       = false  # Use Karpenter instead
  enable_node_termination_handler = true
  enable_auto_remediation         = true
}
```

## Best Practices

1. **Use Karpenter over Cluster Autoscaler** for faster scaling and better bin-packing
2. **Enable pod disruption budgets** for every production workload
3. **Use node termination handler** for spot instance graceful shutdown
4. **Implement network policies** with Calico or Cilium
5. **Enable control plane logging** to CloudWatch
6. **Use IRSA** (IAM Roles for Service Accounts) instead of node-level IAM

## Open Source

- [terraform-aws-auto-healing-eks](https://github.com/kogunlowo123/terraform-aws-auto-healing-eks) — Self-healing EKS
- [terraform-aws-eks](https://github.com/kogunlowo123/terraform-aws-eks) — Standard EKS module
- [terraform-aws-vpc-complete](https://github.com/kogunlowo123/terraform-aws-vpc-complete) — VPC for EKS

Full guide: [kogunlowo123.github.io](https://kogunlowo123.github.io/blog/terraform-eks-complete-guide.html)
```

---

## Article 4: Azure Sentinel SOC Automation

```markdown
---
title: "Automating SOC Operations with Microsoft Sentinel and AI-Powered Triage"
published: true
tags: azure, security, devops, python
canonical_url: https://kogunlowo123.github.io/blog/azure-sentinel-soc-automation.html
cover_image:
description: "Deploy AI-powered SOC automation using Microsoft Sentinel, KQL analytics rules, SOAR playbooks, and LLM-based threat triage."
---

SOC teams drown in alerts. The average enterprise SOC receives 10,000+ alerts per day, and 80% are false positives. The solution isn't more analysts — it's smarter automation.

Here's how to build an AI-powered SOC using Microsoft Sentinel.

## Sentinel vs Splunk vs Elastic

| Feature | Sentinel | Splunk | Elastic SIEM |
|---------|----------|--------|-------------|
| Pricing | Pay-per-GB | License-based | Free (self-hosted) |
| Cloud Native | Yes (Azure) | Hybrid | Hybrid |
| Built-in SOAR | Yes (Logic Apps) | SOAR add-on | Limited |
| AI/ML Detection | Built-in UEBA | MLTK add-on | ML jobs |
| MITRE Coverage | 200+ rules | 100+ | 150+ |

## KQL Analytics Rule Example

```kql
// Detect brute force attacks
SecurityEvent
| where EventID == 4625
| summarize FailedAttempts = count() by TargetAccount, IpAddress, bin(TimeGenerated, 5m)
| where FailedAttempts > 10
| extend AccountName = tostring(split(TargetAccount, "\\")[1])
| project TimeGenerated, AccountName, IpAddress, FailedAttempts
```

## Terraform Deployment

Deploy the complete Sentinel stack:
- [terraform-azure-sentinel-ai](https://github.com/kogunlowo123/terraform-azure-sentinel-ai)
- [ai-agent-soc-triage](https://github.com/kogunlowo123/ai-agent-soc-triage)
- [terraform-azure-security-center](https://github.com/kogunlowo123/terraform-azure-security-center)

Full article: [kogunlowo123.github.io](https://kogunlowo123.github.io/blog/azure-sentinel-soc-automation.html)
```

---

## Article 5: AI Agents for Enterprise DevOps

```markdown
---
title: "Building Enterprise AI Agent Systems for DevOps Automation"
published: true
tags: ai, devops, python, automation
canonical_url: https://kogunlowo123.github.io/blog/ai-agents-enterprise-devops.html
cover_image:
description: "Design and deploy multi-agent AI systems for DevOps automation using LangChain, AWS Bedrock Agents, and self-healing infrastructure patterns."
---

AI agents are transforming DevOps from reactive firefighting to proactive automation. Unlike chatbots that answer questions, agents take action — they diagnose issues, execute runbooks, and remediate infrastructure problems autonomously.

## Agent Framework Comparison

| Framework | Best For | Language | Cloud Integration |
|-----------|----------|----------|------------------|
| LangChain/LangGraph | Custom orchestration | Python | Any |
| CrewAI | Role-based teams | Python | Any |
| AutoGen | Research/coding | Python | Any |
| AWS Bedrock Agents | AWS-native workflows | Any | AWS |

## Multi-Agent Orchestrator Example

```python
from langgraph.graph import StateGraph, END

def create_devops_team():
    workflow = StateGraph(AgentState)
    workflow.add_node("supervisor", supervisor_agent)
    workflow.add_node("monitor", monitoring_agent)
    workflow.add_node("diagnose", diagnostic_agent)
    workflow.add_node("remediate", remediation_agent)

    workflow.set_entry_point("supervisor")
    workflow.add_conditional_edges("supervisor", route_task)

    return workflow.compile()
```

## Open Source

- [langchain-multi-agent-framework](https://github.com/kogunlowo123/langchain-multi-agent-framework)
- [terraform-aws-bedrock-agents](https://github.com/kogunlowo123/terraform-aws-bedrock-agents)
- [terraform-aws-auto-healing-eks](https://github.com/kogunlowo123/terraform-aws-auto-healing-eks)

Full guide: [kogunlowo123.github.io](https://kogunlowo123.github.io/blog/ai-agents-enterprise-devops.html)
```

---

## Article 6: Multi-Cloud FinOps

```markdown
---
title: "Multi-Cloud FinOps: Automated Cost Optimization Across AWS, Azure, and GCP"
published: true
tags: cloud, devops, aws, azure
canonical_url: https://kogunlowo123.github.io/blog/finops-multi-cloud-cost-optimization.html
cover_image:
description: "Automate cloud cost optimization across AWS, Azure, and GCP with AI-powered anomaly detection, rightsizing, and waste elimination."
---

The average enterprise wastes 30% of its cloud spend. Across three clouds, that waste compounds into millions annually. FinOps isn't just dashboards — it's automated action.

## Cost Management Tools Comparison

| Capability | AWS | Azure | GCP |
|-----------|-----|-------|-----|
| Cost Explorer | Cost Explorer | Cost Management | Billing Reports |
| Recommendations | Compute Optimizer | Advisor | Recommender |
| Budgets/Alerts | AWS Budgets | Budget Alerts | Budget Alerts |
| Savings Plans | Yes | Reservations | CUDs |

## Anomaly Detection with Python

```python
import boto3
from datetime import datetime, timedelta

def detect_cost_anomalies(threshold_pct: float = 20.0):
    ce = boto3.client("ce")
    today = datetime.utcnow()

    current = ce.get_cost_and_usage(
        TimePeriod={"Start": (today - timedelta(days=1)).strftime("%Y-%m-%d"),
                     "End": today.strftime("%Y-%m-%d")},
        Granularity="DAILY", Metrics=["UnblendedCost"]
    )

    baseline = ce.get_cost_and_usage(
        TimePeriod={"Start": (today - timedelta(days=30)).strftime("%Y-%m-%d"),
                     "End": (today - timedelta(days=1)).strftime("%Y-%m-%d")},
        Granularity="DAILY", Metrics=["UnblendedCost"]
    )

    avg_cost = sum(float(d["Total"]["UnblendedCost"]["Amount"])
                   for d in baseline["ResultsByTime"]) / 29
    current_cost = float(current["ResultsByTime"][0]["Total"]["UnblendedCost"]["Amount"])

    if current_cost > avg_cost * (1 + threshold_pct / 100):
        return {"anomaly": True, "current": current_cost, "average": avg_cost}
    return {"anomaly": False}
```

## Open Source

- [ai-finops-optimization-agent](https://github.com/kogunlowo123/ai-finops-optimization-agent)
- [terraform-aws-eks](https://github.com/kogunlowo123/terraform-aws-eks)

Full article: [kogunlowo123.github.io](https://kogunlowo123.github.io/blog/finops-multi-cloud-cost-optimization.html)
```
