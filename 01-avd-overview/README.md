# 01 — AVD Overview

In this section I describe the high-level architecture of my Azure Virtual Desktop (AVD) environment and how I structured this project. The later folders go into the detailed implementation (identity, networking, session hosts, FSLogix, autoscaling, security, and monitoring), so here I focus on the overall story of what I built and why.

---

## My Goals with This AVD Environment

With this project I wanted to:

- Design a **secure and scalable** AVD environment.
- Use **modern identity** with Microsoft Entra ID (Azure AD).
- Centralize user profiles with **FSLogix** on Azure Files.
- Optimise **cost** with autoscaling and right-sized session hosts.
- Apply **security, hardening, and monitoring** from the start.
- Document everything in a way that I (and others) can reproduce later.

This repo is not just a reference; it’s the exact journey I took to build my own AVD deployment in Azure.

---

## High-Level Architecture

At a high level, my AVD environment consists of:

- **Identity and Access**
  - Microsoft Entra ID for user identities.
  - Conditional Access policies (MFA, compliant devices).
  - Role-based access control (RBAC) for least privilege.

- **Networking**
  - A dedicated virtual network and subnets for AVD.
  - Private connectivity to storage and other services (where applicable).
  - Controlled outbound access via NSGs and (optionally) firewalls.

- **Session Hosts & Images**
  - Windows multi-session session hosts.
  - A base image with the required apps and FSLogix installed.
  - Joined to Entra / hybrid as required for my design.

- **FSLogix Profiles & Storage**
  - FSLogix profile containers stored on Azure Files.
  - Azure AD Kerberos enabled for secure access.
  - Proper IAM roles configured on the file share.

- **Host Pools & App Groups**
  - A pooled host pool.
  - A desktop application group for full desktop access.
  - User and group assignments handled through Entra ID.

- **Scaling and Cost Optimisation**
  - A scaling plan for workday schedules.
  - Ramp-up, peak, ramp-down, and off-peak configuration.
  - Autoscale managing session host power state to reduce cost.

- **Security, Hardening, and Monitoring**
  - Conditional Access for AVD (MFA, device requirements).
  - RBAC review on resource groups, host pools, and workspaces.
  - Log Analytics workspace and AVD diagnostics connected.
  - Baseline security configuration on session hosts.

---

## How I Structured This Project

Each numbered folder in the repo maps to a logical step in my AVD journey:

- `01-avd-overview` – This high-level overview of what I’m building.
- `02-identity-and-access` – How I handle identity, RBAC, and basic access.
- `03-networking-and-connectivity` – My virtual network and connectivity design.
- `04-session-hosts-and-images` – Session host VMs and base image considerations.
- `05-fslogix-profiles-and-storage` – FSLogix profile containers and Azure Files.
- `06-host-pools-and-app-groups` – Host pool and app group configuration.
- `07-scaling-and-cost-optimisation` – Autoscaling and cost control.
- `08-security-hardening-and-monitoring` – Security controls, hardening, and monitoring.

Each folder has its own README where I walk through what I did, show screenshots from the Azure portal, and capture the exact settings I used.

---

## How I Expect Someone to Use This

If someone wants to follow my design, the flow is:

1. Start here in `01-avd-overview` to understand the big picture.
2. Move through the folders in order (`02`, `03`, `04`, …).
3. Recreate the configuration step by step in their own Azure subscription.
4. Adapt naming, regions, and sizes to fit their environment.

The goal is that you can go from **no AVD** to a **documented, secure, cost-optimised AVD environment** using this repository as a guide.



I will keep improving this overview if I change the architecture or add new components in the future.

