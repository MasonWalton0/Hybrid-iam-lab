# Hybrid Identity & Access Management Lab

A hands-on cybersecurity home lab simulating a corporate identity and access management environment. Built entirely using free and open-source tools across virtual machines.

---

## Project Overview

This project simulates a real-world enterprise hybrid identity environment. It covers on-premises Active Directory, a self-hosted cloud identity platform (Keycloak), Single Sign-On, Role-Based Access Control, and LDAP Federation — all running inside an isolated virtual network.

The goal was to demonstrate practical IAM skills without relying on paid cloud services like Azure or AWS.

---

## Technologies Used

| Tool | Purpose |
|------|---------|
| Oracle VirtualBox | Virtualization platform |
| Windows Server 2022 | Domain Controller / Active Directory |
| Windows 11 Enterprise | Client workstation |
| Ubuntu Server 22.04 | Keycloak host |
| Active Directory Domain Services | On-premises identity management |
| Keycloak 26.5.6 | Open-source identity and SSO platform |
| Java 17 (OpenJDK) | Keycloak runtime dependency |
| LDAP (Port 389) | Identity federation protocol |

---

## Network Architecture

```
┌──────────────────────────────────────────────────┐
│               Internal Lab Network                │
│                  192.168.1.0/24                   │
│                                                   │
│  DC01         CLIENT01        KEYCLOAK01          │
│  192.168.1.10 192.168.1.20    192.168.1.30        │
│  Windows      Windows 11      Ubuntu 22            │
│  Server 2022  Workstation     Keycloak             │
└──────────────────────────────────────────────────┘
```

All VMs communicate over an isolated VirtualBox internal network called `labnetwork`. No external internet access is required during operation.

---

## Phase 1 — On-Premises Active Directory Environment

### What Was Built
- Deployed **Windows Server 2022** as a Domain Controller (DC01)
- Promoted the server to a Domain Controller for the domain `company.local`
- Created **4 Organizational Units** representing company departments:
  - HR
  - Finance
  - IT
  - Interns
- Created **5 user accounts**:
  - john.doe
  - sarah.hr
  - finance.user
  - it.admin
  - intern.user
- Created **4 Security Groups** and assigned users:
  - HR_Group → sarah.hr
  - Finance_Group → finance.user
  - IT_Admins → it.admin
  - Interns → intern.user
- Deployed **Windows 11 Enterprise** as a client workstation (CLIENT01)
- Joined CLIENT01 to the `company.local` domain
- Verified domain login using `COMPANY\john.doe`

### Key Skills Demonstrated
- Active Directory installation and configuration
- Domain Controller promotion
- Organizational Unit and user management
- Role-Based Access Control (RBAC) with security groups
- Domain join and authentication verification

---

## Phase 2 — Keycloak Identity Platform + LDAP Federation

### What Was Built
- Deployed **Ubuntu Server 22.04** as a dedicated identity server (KEYCLOAK01)
- Installed **Java 17** and **Keycloak 26.5.6**
- Configured Keycloak with a dedicated `company` realm
- Created department groups and assigned users
- Configured an **OpenID Connect client** (`company-app`) for SSO
- Configured **LDAP User Federation** to connect Keycloak directly to Active Directory
- Opened **LDAP port 389** on DC01's firewall to allow identity federation
- Successfully synced Active Directory users into Keycloak via LDAP
- Verified that employee logins through Keycloak authenticate against **Active Directory passwords**

### How LDAP Federation Works in This Lab

```
User logs into Keycloak
        ↓
Keycloak sends credentials to DC01 via LDAP (port 389)
        ↓
Active Directory verifies the password
        ↓
Keycloak grants or denies access
```

Active Directory is the **single source of truth** for all user passwords. Changes made in Active Directory are automatically reflected in Keycloak.

### Key Skills Demonstrated
- Self-hosted identity provider deployment
- Realm, user, and group management in Keycloak
- OpenID Connect SSO configuration
- LDAP federation between Keycloak and Active Directory
- Hybrid identity architecture design
- Firewall rule configuration for identity protocols

---

## Key Concepts Covered

- **Identity & Access Management (IAM)** — controlling who can access what resources
- **Role-Based Access Control (RBAC)** — assigning permissions based on job role
- **Single Sign-On (SSO)** — one login for multiple applications
- **Domain Authentication** — centralized login management via Active Directory
- **Identity Federation** — linking on-premises and cloud-style identity systems via LDAP
- **Hybrid Identity** — combining on-premises AD with a modern identity platform

---

## Lab Environment Specs

| VM | OS | RAM | Disk | IP |
|----|----|-----|------|----|
| DC01 | Windows Server 2022 | 2GB | 60GB | 192.168.1.10 |
| CLIENT01 | Windows 11 Enterprise | 2GB | 50GB | 192.168.1.20 |
| KEYCLOAK01 | Ubuntu Server 22.04 | 4GB | 40GB | 192.168.1.30 |

---

## Screenshots

Screenshots documenting each phase are in the `/screenshots` folder:

```
/screenshots
  /phase1-active-directory
    - dc01-server-manager.png
    - ad-roles-installed.png
    - organizational-units.png
    - users-created.png
    - security-groups.png
    - client01-domain-join.png
    - domain-login-proof.png
  /phase2-keycloak
    - keycloak-dashboard.png
    - company-realm.png
    - users-list.png
    - groups-created.png
    - sso-client-config.png
    - ldap-federation-connected.png
    - ldap-sync-success.png
    - employee-login-proof.png
```

---

## Resume Summary

**Hybrid Identity & Access Management Lab**
- Built a hybrid identity environment connecting Active Directory to Keycloak via LDAP federation
- Implemented RBAC with security groups across HR, Finance, IT, and Intern departments
- Configured Single Sign-On using OpenID Connect on a self-hosted Keycloak instance
- Federated on-premises Active Directory with Keycloak so AD serves as the single source of truth for all authentication
- Managed 3 virtual machines across an isolated lab network
- Documented entire build process with screenshots for portfolio evidence

---

## Author

Mason Walton — Cybersecurity Student
