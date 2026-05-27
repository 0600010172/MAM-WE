# BYOD Mobile Read-Only Access — Workload Summary

| Workload | Required Role | What should be configured | Why it is essential |
|---|---|---|---|
| Intune App Protection Policies | Intune Administrator | Create iOS/Android App Protection Policies for Outlook, Teams, OneDrive, SharePoint, Office apps, and Edge. Configure data protection, copy/paste restrictions, save-as blocking, encryption, PIN, jailbreak/root detection, and conditional launch. | Protects corporate data inside managed apps without requiring full device enrollment. App protection policies control how corporate data is accessed, shared, copied, saved, or moved between apps. |
| Microsoft Entra Conditional Access | Conditional Access Administrator / Security Administrator | Create CA policy for iOS/Android BYOD users. Target Microsoft 365 apps. Require App Protection Policy. Start in Report-only, then enable. | Ensures users can only access corporate data from apps protected by Intune MAM. Microsoft recommends using “Require app protection policy” for mobile access. |
| SharePoint / OneDrive Access Control | SharePoint Administrator / Global Administrator | Configure unmanaged devices to use “Allow limited, web-only access.” Block download, sync, and opening files in desktop apps from unmanaged devices. | Provides read-only browser access for unmanaged/personal devices and prevents local file download or sync. |
| Microsoft Edge Managed Browser | Intune Administrator | Force web links and corporate web content to open in Microsoft Edge as the managed browser. | Keeps browser sessions inside the protected app ecosystem and prevents data leakage through unmanaged browsers. |
| Defender for Cloud Apps | Security Administrator | Optional future phase: enable Conditional Access App Control and session policies such as block download, monitor risky activity, watermark, and real-time access control. | Adds real-time session protection beyond basic CA and SharePoint controls. Useful for higher-risk users or sensitive workloads. |
| Microsoft Purview | Compliance Administrator | Optional future phase: configure sensitivity labels, DLP, auto-labeling, and endpoint DLP. | Protects sensitive content based on classification, not only device/app state. Essential for long-term compliance and data governance. |

Sources: Microsoft Intune App Protection Policies protect organizational data inside managed apps; Conditional Access can require app protection policy for iOS/Android; SharePoint can limit unmanaged devices to web-only access.  


# Recommended Architecture Notes

| Layer | Control | Result |
|---|---|---|
| Identity Layer | Conditional Access | Decides whether access is allowed based on user, device platform, app, and grant controls. |
| App Layer | Intune App Protection Policy | Protects corporate data inside mobile apps without requiring device enrollment. |
| Browser Layer | SharePoint / OneDrive unmanaged device control | Allows browser viewing but blocks download, sync, and desktop app access. |
| Session Layer | Defender for Cloud Apps | Optional future control for real-time monitoring and session restrictions. |
| Data Layer | Microsoft Purview | Optional future control for sensitivity labels, DLP, encryption, and compliance. |

# Target End-State

| User Action | Expected Result |
|---|---|
| Read email in Outlook mobile | Allowed |
| Open Teams mobile | Allowed |
| View SharePoint / OneDrive file in app | Allowed with MAM controls |
| Copy corporate data to personal app | Blocked |
| Save file locally | Blocked |
| Open file in unmanaged app | Blocked |
| Access SharePoint from browser | Limited web-only access |
| Download from browser | Blocked |
| Sync OneDrive from BYOD | Blocked |



# Intune App Protection Policy Architecture

```mermaid
flowchart TD

A[Personal Mobile Device] --> B[Microsoft Outlook]
A --> C[Microsoft Teams]
A --> D[OneDrive]
A --> E[SharePoint App]

B --> F[Intune App Protection Policy]
C --> F
D --> F
E --> F

F --> G[Restrict Copy Paste]
F --> H[Block Local Save]
F --> I[Require PIN]
F --> J[Encrypt Corporate Data]
F --> K[Block Screenshot]

F --> L[Microsoft 365 Data]

L --> M[Exchange Online]
L --> N[SharePoint Online]
L --> O[OneDrive for Business]
L --> P[Microsoft Teams]
```

# Conditional Access Policy Architecture

```mermaid
flowchart TD

A[User Sign In] --> B[Microsoft Entra ID]

B --> C[Conditional Access Policy]

C --> D{Device Type}

D -->|Managed App| E[Require App Protection Policy]
D -->|Browser Access| F[Apply Browser Restrictions]
D -->|Unapproved App| G[Block Access]

E --> H[Approved Microsoft Apps]

H --> I[Outlook]
H --> J[Teams]
H --> K[OneDrive]
H --> L[SharePoint]

F --> M[Limited Web Access]

M --> N[SharePoint Online]
M --> O[OneDrive for Business]
```

# SharePoint and OneDrive Browser Control Architecture

```mermaid
flowchart TD

A[Unmanaged Browser] --> B[Microsoft Entra ID]

B --> C[Conditional Access]

C --> D[SharePoint Online Access Control]

D --> E[Allow Web Only Access]

E --> F[View Files in Browser]

E --> G[Block Download]
E --> H[Block Sync]
E --> I[Block Print]
E --> J[Restrict Local Save]

F --> K[SharePoint Online]
F --> L[OneDrive for Business]
```

# End-to-End Mobile Protection Flow

```mermaid
flowchart LR

A[Personal Device] --> B[Microsoft Entra ID]

B --> C[Conditional Access]

C --> D{Access Method}

D -->|Managed Mobile App| E[Intune App Protection]

D -->|Browser Access| F[SharePoint Browser Restriction]

E --> G[Protected Microsoft Apps]

G --> H[Exchange Online]
G --> I[Teams]
G --> J[OneDrive]
G --> K[SharePoint]

F --> L[Web Only Access]

L --> M[Download Blocked]
L --> N[Sync Blocked]
L --> O[Print Blocked]

H --> P[Secure Corporate Data]
I --> P
J --> P
K --> P
```

# Future Architecture with Defender for Cloud Apps

```mermaid
flowchart TD

A[User Access] --> B[Conditional Access]

B --> C[Conditional Access App Control]

C --> D[Microsoft Defender for Cloud Apps]

D --> E[Real Time Session Monitoring]
D --> F[Download Blocking]
D --> G[Risk Detection]
D --> H[Session Control]

D --> I[Microsoft 365 Services]
```

# Future Architecture with Microsoft Purview

```mermaid
flowchart TD

A[Microsoft Purview] --> B[Sensitivity Labels]

A --> C[Data Loss Prevention]

A --> D[Auto Labeling]

B --> E[SharePoint Online]
B --> F[OneDrive]
B --> G[Exchange Online]
B --> H[Teams]

C --> E
C --> F
C --> G
C --> H

E --> I[Intune App Protection]
F --> I
G --> I
H --> I

I --> J[Protected Corporate Data]
```



# 3. Architecture Overview

```mermaid
flowchart TD

    A[User]

    A --> B{Connection Method}

    B -->|Browser| C[Managed Browser<br/>Edge / Safari / Chrome]
    B -->|Mobile App| D[Managed Microsoft Apps<br/>Outlook / Teams / OneDrive]

    subgraph Microsoft365["Microsoft 365 Security Platform"]

        C --> E[SharePoint / OneDrive<br/>Browser Access Control]

        D --> F[Intune App Protection Policy]

        E --> G[Microsoft Entra ID]

        F --> G

        G --> H[Conditional Access Policy]

        H --> I{Access Evaluation}

        I -->|Compliant Protected Access| J[Authorized Session]

        I -->|Unmanaged or Risky Access| K[Restricted Access / Blocked]

        J --> L[SharePoint Online]

        J --> M[OneDrive for Business]

        J --> N[Exchange Online]

        J --> O[Microsoft Teams]

        F --> P[Restrict Copy/Paste]

        F --> Q[Block Local Save]

        F --> R[Require PIN / Biometrics]

        E --> S[Web-Only Access]

        E --> T[Block Download]

        E --> U[Block Sync]

        E --> V[Block Print]

    end
```
