
# BYOD Mobile Protection Deployment Guide (Read-Only Model)

## Objective

Deploy Microsoft Intune App Protection Policies (MAM-WE) for BYOD mobile devices to:

- Protect corporate data
- Allow read-only access
- Prevent download to personal storage
- Prevent copy/paste outside managed apps
- Avoid full device enrollment
- Support iPhone and Android devices

This model focuses only on application-level protection.

---

# Architecture Overview

```text
User Personal Device (BYOD)
        │
        ▼
Managed Microsoft Apps
(Outlook / Teams / OneDrive / SharePoint)
        │
        ▼
Microsoft Intune App Protection Policy
        │
        ├── Allow sign-in with corporate account
        ├── Restrict copy/paste outside apps
        ├── Block save to personal storage
        ├── Require PIN / biometrics
        └── Enable selective wipe
```

---

# Target Applications

| Application | Protection |
|---|---|
| Outlook | Managed |
| Teams | Managed |
| OneDrive | Managed |
| SharePoint | Managed |
| Microsoft Edge (Optional) | Managed |

---

# Deployment Flow

```text
Create User Group
        ↓
Configure App Protection Policy
        ↓
Assign Managed Apps
        ↓
Configure Conditional Access
        ↓
Require Approved Apps
        ↓
User Sign-in from Mobile Device
        ↓
Policy Applied Automatically
```

---

# Prerequisites

| Requirement | Details |
|---|---|
| License | Microsoft Intune Suite / EMS E3 / Microsoft 365 E3/E5 |
| Identity | Microsoft Entra ID |
| Apps | Microsoft mobile applications |
| Devices | Personal iOS / Android |
| Enrollment | Not required |

---

# Step 1 — Create User Group

## Purpose

Create a dedicated group for BYOD users.

## Steps

1. Open Microsoft Entra Admin Center
2. Navigate to:
   - Groups
3. Select:
   - New Group
4. Configure:
   - Group Type: Security
   - Group Name: BYOD-Mobile-Users
5. Add target users

---

# Step 2 — Configure Intune MAM Settings

## Steps

1. Open Microsoft Intune Admin Center
2. Navigate to:
   - Apps
   - App protection policies
3. Select:
   - Create policy

---

# Step 3 — Create iOS/iPadOS App Protection Policy

## Platform

- iOS/iPadOS

## Policy Name

```text
BYOD-iOS-ReadOnly
```

---

## Target Apps

Add:

- Outlook
- Teams
- OneDrive
- SharePoint

---

## Data Protection Settings

| Setting | Value |
|---|---|
| Backup org data | Block |
| Send org data to other apps | Policy managed apps |
| Receive data from other apps | Policy managed apps |
| Restrict cut/copy/paste | Policy managed apps |
| Save copies of org data | Block |
| Allow user to save copies to selected services | None |
| Encrypt org data | Require |

---

## Access Requirements

| Setting | Value |
|---|---|
| PIN for access | Required |
| Biometric access | Allowed |
| Work or school account credentials | Required |

---

## Conditional Launch

| Setting | Value |
|---|---|
| Offline grace period | 12 hours |
| Jailbroken devices | Block access |

---

## Assignments

Assign to:

```text
BYOD-Mobile-Users
```

---

# Step 4 — Create Android App Protection Policy

## Platform

- Android

## Policy Name

```text
BYOD-Android-ReadOnly
```

---

## Target Apps

Add:

- Outlook
- Teams
- OneDrive
- SharePoint

---

## Data Protection Settings

| Setting | Value |
|---|---|
| Backup org data | Block |
| Send org data to other apps | Policy managed apps |
| Receive data from other apps | Policy managed apps |
| Restrict cut/copy/paste | Policy managed apps |
| Save copies of org data | Block |
| Encrypt org data | Require |

---

## Access Requirements

| Setting | Value |
|---|---|
| PIN for access | Required |
| Biometric access | Allowed |
| Work or school account credentials | Required |

---

## Conditional Launch

| Setting | Value |
|---|---|
| Rooted devices | Block access |
| Offline grace period | 12 hours |

---

## Assignments

Assign to:

```text
BYOD-Mobile-Users
```

---

# Step 5 — Configure Conditional Access

## Objective

Allow access only from approved protected apps.

---

## Steps

1. Open Microsoft Entra Admin Center
2. Navigate to:
   - Protection
   - Conditional Access
3. Select:
   - New Policy

---

## Policy Name

```text
BYOD-Mobile-ApprovedApps
```

---

## Users

Include:

```text
BYOD-Mobile-Users
```

---

## Cloud Apps

Select:

- Office 365
- SharePoint Online
- Exchange Online

---

## Conditions

### Device Platforms

Include:

- iOS
- Android

---

## Grant Controls

Require:

```text
Require approved client app
```

OR

```text
Require app protection policy
```

Enable:

```text
Grant access
```

---

# Step 6 — Optional SharePoint Read-Only Restriction

## Objective

Prevent browser download from unmanaged devices.

---

## Steps

1. Open SharePoint Admin Center
2. Navigate to:
   - Policies
   - Access Control
3. Configure:

| Setting | Value |
|---|---|
| Unmanaged devices | Allow limited, web-only access |

---

# Step 7 — User Experience

## User Flow

```text
Install Microsoft Apps
        ↓
Sign in with Corporate Account
        ↓
Intune applies protection policy
        ↓
Corporate data becomes protected
        ↓
User can read corporate data only
```

---

# Expected Security Behavior

| Action | Result |
|---|---|
| Copy to personal apps | Blocked |
| Download to local storage | Blocked |
| Screenshot | Allowed unless separately restricted |
| Access from unmanaged browser | Limited |
| Corporate data wipe | Selective wipe only |
| Personal data visibility | Not visible to company |

---

# Validation Checklist

| Validation | Expected Result |
|---|---|
| Outlook access works | Yes |
| Teams access works | Yes |
| Copy/paste outside apps blocked | Yes |
| Save to device blocked | Yes |
| OneDrive local download blocked | Yes |
| Selective wipe available | Yes |

---

# Recommended Pilot Deployment

| Phase | Users |
|---|---|
| Pilot | IT Team |
| Phase 1 | Security Team |
| Phase 2 | General Users |

---

# Rollback Plan

## Steps

1. Remove policy assignment
2. Disable Conditional Access policy
3. Remove users from BYOD group

---

# Monitoring

## Intune Reports

Navigate to:

```text
Apps → Monitor → App protection status
```

Review:

- Policy deployment
- User status
- App status
- Protection failures

---

# Security Recommendations

| Recommendation | Reason |
|---|---|
| Use MFA | Additional identity protection |
| Use app protection only | Minimize privacy concerns |
| Block unmanaged browsers | Prevent uncontrolled downloads |
| Use Microsoft Edge | Better protected web access |
| Use selective wipe | Remove only corporate data |

---

# Final Outcome

This deployment provides:

- BYOD mobile access
- Read-only corporate data protection
- No full device management
- Reduced privacy concerns
- Secure Microsoft 365 mobile access
- App-level protection through Intune MAM
````
