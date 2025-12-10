# 04. Session Hosts and Images

In this section I deploy my Azure Virtual Desktop (AVD) session host VM, configure Microsoft Entra join and single sign-on (SSO), create a workspace, assign users, and validate the end-to-end connection.

---

## 1. Add a Virtual Machine to the Host Pool

From Azure Virtual Desktop → Host pools, I open my host pool hp-avd-demo and select:

Session hosts → Add

Basics

I keep the defaults that link this VM to my existing host pool:

- Subscription: Azure subscription 1
- Resource group: rg-avd-demo
- Host pool: hp-avd-demo
- Location: East US
- Host pool type: Pooled
- Preferred app group type: Desktop

![Validation Passed](../images/13.add-vm.png)

---

## 2. Configure the Virtual Machine

I continue to the Virtual Machines tab and configure the session host.

General VM Settings

- Add virtual machines: Yes
- Name prefix: sh-avd-demo
- Virtual machine type: Azure virtual machine
- Image: Windows 11 Enterprise multi-session, 24H2 + Microsoft 365 Apps
- VM size: Standard D2as v5
- Number of VMs: 1

![Validation Passed](../images/14.1.vm.png)

Networking & Identity

- Virtual network: vnet-avd
- Subnet: AVD-SessionHosts
- NSG type: Advanced
- NSG: nsg-avd-sessionhosts

Domain Join

- Join directory: Microsoft Entra ID
- Enroll in Intune: No

Local Admin Account

- Username: avdadmin

![Validation Passed](../images/14.2.vm.png)

---

## 3. Verify the Session Host Deployment

After deployment completes, I return to the host pool → Session hosts, where I confirm:

- VM Name: sh-avd-demo-0
- Status: Running
- Health state: Available

![Validation Passed](../images/15.vm-confirmation.png)

---

## 4. Create a Workspace

Next, I create a workspace that users will subscribe to.

Azure Virtual Desktop → Workspaces → Create

Basics

- Name: ws-avd-demo
- Resource group: rg-avd-demo
- Location: East US

![Validation Passed](../images/18.vreate-workspace.png)

---

## 5. Register Application Groups

I register the Desktop Application Group (DAG) that belongs to the host pool.

- Register application groups: Yes
- Add: hp-avd-demo-DAG

![Validation Passed](../images/19.application-groups.png)

---

## 6. Assign Users to the Desktop Application Group

I open:

Azure Virtual Desktop → Application groups → hp-avd-demo-DAG → Assignments

Then I add my user: Erhan Fuchedzhiev

![Validation Passed](../images/16.assignments.png)

---

## 7. Enable Microsoft Entra Single Sign-On (SSO)

Within the host pool I configure SSO for seamless authentication:

Host pool → RDP Properties → Connection Information

- Microsoft Entra single sign-on: Connections will use Microsoft Entra authentication to provide single sign-on

![Validation Passed](../images/17.rdp-properties.png)

---

## 8. User Experience – Connecting From Windows App
Workspace Discovery

In the Windows App, after signing in with my Entra ID, I see:

Workspace: ws-avd-demo
Resource: SessionDesktop

![Validation Passed](../images/20.session-desktop-created.png)

---

Authentication Prompt: When launching the session, I confirm the remote desktop connection request:

![Validation Passed](../images/21.sign-in.png)

---

Successful Connection: The Remote Session Desktop opens, and I am signed in automatically using SSO.

![Validation Passed](../images/22.sign-in-confirmation.png)
