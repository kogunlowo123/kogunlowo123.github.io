# Reddit Posts — Ready to Submit
## All 6 Articles — Platform-Native Tone

---

## Article 1: Multi-Cloud Zero Trust

### r/devops
**Title:** Open-sourced Terraform modules for Zero Trust architecture across AWS, Azure, and GCP

After spending a lot of time building Zero Trust architectures across multi-cloud environments, I decided to open-source the Terraform modules and write up the patterns that actually work in production.

The guide covers IAM federation using Azure Entra ID as the IdP across all three clouds, microsegmentation patterns for VPCs/VNets/VPC Networks, and how to map everything back to NIST 800-207. Includes working Terraform code for cross-cloud OIDC federation.

Modules: https://github.com/kogunlowo123/terraform-aws-security-baseline

Full writeup: https://kogunlowo123.github.io/blog/multi-cloud-zero-trust-architecture.html

### r/netsec
**Title:** NIST 800-207 Zero Trust implementation patterns for multi-cloud (AWS/Azure/GCP) with Terraform

Wrote a technical guide on implementing Zero Trust security across three clouds simultaneously. Covers identity-based perimeters, microsegmentation, cross-cloud IAM federation, and continuous compliance validation using policy-as-code.

The approach uses Azure Entra ID as the centralized IdP with OIDC federation to AWS and GCP, Terraform for infrastructure automation, and centralized SIEM logging for every access decision.

Full guide with architecture diagrams and code: https://kogunlowo123.github.io/blog/multi-cloud-zero-trust-architecture.html

---

## Article 2: Production RAG Pipelines

### r/aws
**Title:** Lessons from running RAG pipelines in production on AWS Bedrock + OpenSearch Serverless

Sharing some patterns from running production RAG systems on AWS. The biggest differences from tutorials: chunk sizing (512 tokens with overlap), hybrid search (semantic + BM25), confidence scoring to filter low-relevance results, and Bedrock Guardrails for hallucination prevention.

Includes Terraform modules for the complete infrastructure and Python code for the pipeline. Open-sourced everything.

Full guide: https://kogunlowo123.github.io/blog/production-rag-pipelines-aws.html

### r/MachineLearning
**Title:** Production RAG architecture patterns — hallucination mitigation, hybrid search, and scaling to 50K+ daily queries

Most RAG tutorials get you to a working demo. Production RAG needs chunking strategies, hybrid search (vector + BM25), citation grounding, confidence thresholds, and guardrails. Wrote up the patterns that survived real production workloads on AWS Bedrock.

Full writeup with architecture and code: https://kogunlowo123.github.io/blog/production-rag-pipelines-aws.html

---

## Article 3: Production EKS

### r/kubernetes
**Title:** Open-sourced a self-healing EKS Terraform module with Karpenter, node auto-remediation, and termination handler

Built a Terraform module for production EKS that includes Karpenter (instead of Cluster Autoscaler), Lambda-based node auto-remediation via EventBridge, Node Termination Handler for spot graceful shutdown, and all the EKS addons configured properly.

After auditing a bunch of production EKS clusters, most were missing basic things like pod disruption budgets and IRSA. This module bakes all of that in.

Module: https://github.com/kogunlowo123/terraform-aws-auto-healing-eks

Full guide: https://kogunlowo123.github.io/blog/terraform-eks-complete-guide.html

### r/Terraform
**Title:** Production EKS module with Karpenter, self-healing nodes, and all the addons configured

Open-sourced a Terraform module for EKS that handles the things most modules skip: Karpenter provisioner, node termination handler, Lambda-based node remediation triggered by EventBridge, proper IRSA configuration, and CloudWatch alarms for node health.

Module: https://github.com/kogunlowo123/terraform-aws-auto-healing-eks

Writeup with the full architecture: https://kogunlowo123.github.io/blog/terraform-eks-complete-guide.html

---

## Article 4: Azure Sentinel SOC Automation

### r/cybersecurity
**Title:** Built an AI-powered SOC triage system using Microsoft Sentinel + LLM-based alert classification

Sharing an approach to SOC automation that reduced alert fatigue by 70%: Sentinel analytics rules for detection, SOAR playbooks for auto-triage, and an LLM-based classifier that categorizes alerts by severity and maps to MITRE ATT&CK.

The Python triage agent connects to Sentinel, Splunk, or Elastic and uses LLMs to analyze alerts, extract IOCs, and correlate events. Open-sourced both the Terraform module and the Python agent.

Triage agent: https://github.com/kogunlowo123/ai-agent-soc-triage

Full guide: https://kogunlowo123.github.io/blog/azure-sentinel-soc-automation.html

### r/AZURE
**Title:** Terraform module for Microsoft Sentinel with analytics rules, SOAR playbooks, and threat intelligence

Open-sourced a Terraform module that deploys Sentinel with scheduled analytics rules (brute force, impossible travel, suspicious PowerShell), data connectors (AAD, ASC, MDATP), automation rules, and Logic App playbooks for automated triage.

Module: https://github.com/kogunlowo123/terraform-azure-sentinel-ai

Full writeup: https://kogunlowo123.github.io/blog/azure-sentinel-soc-automation.html

---

## Article 5: AI Agents for DevOps

### r/devops
**Title:** Open-sourced a multi-agent framework for DevOps automation — monitor, diagnose, remediate, report

Built a LangGraph-based multi-agent system where specialized agents collaborate: Monitor watches metrics, Diagnostic correlates symptoms to root causes using RAG over runbooks, Remediation executes fixes with rollback, and Report generates summaries.

Also open-sourced Terraform modules for AWS Bedrock Agents and a self-healing EKS cluster that uses Lambda-based auto-remediation.

Framework: https://github.com/kogunlowo123/langchain-multi-agent-framework

Full guide: https://kogunlowo123.github.io/blog/ai-agents-enterprise-devops.html

---

## Article 6: Multi-Cloud FinOps

### r/devops
**Title:** Open-sourced a multi-cloud FinOps agent that finds waste and recommends optimizations across AWS/Azure/GCP

Built a Python agent that connects to AWS Cost Explorer, Azure Cost Management, and GCP Billing to detect anomalies, recommend rightsizing, find unused resources, and calculate reserved instance break-even points.

Uses z-score anomaly detection against 30-day baselines and actual CloudWatch/Monitor metrics for rightsizing recommendations. Reports via Slack or email.

Agent: https://github.com/kogunlowo123/ai-finops-optimization-agent

Full guide: https://kogunlowo123.github.io/blog/finops-multi-cloud-cost-optimization.html

### r/aws
**Title:** Python script for automated AWS cost anomaly detection using Cost Explorer + z-score analysis

Sharing a Python tool that pulls daily costs from Cost Explorer, calculates z-scores against a 30-day baseline, and alerts when spending deviates beyond a configurable threshold. Also identifies oversized instances, unattached EBS volumes, and idle load balancers.

Part of a larger multi-cloud FinOps agent: https://github.com/kogunlowo123/ai-finops-optimization-agent

Full writeup: https://kogunlowo123.github.io/blog/finops-multi-cloud-cost-optimization.html
