# 📱 BYOD Mobile Read-Only Protection (Intune + CA) — Quick Setup Guide

This guide provides **minimal, actionable steps** to configure **read-only access on personal mobile devices** using App Protection Policies.

---

# 🧭 STEP 0 — Create Test Group

1. Go to **Entra admin center**
2. **Groups → New group**
3. Type: **Security**
4. Name: `BYOD-Test`
5. Add test user
6. Click **Create**

---

# 🧭 STEP 1 — Create App Protection Policy

## 🔹 Navigation
1. **Intune admin center**
2. **Apps → Protection**
3. Click **+ Create**

---

## 🔹 Platform
- Select:
  - iOS/iPadOS OR Android

---

## 🔹 Basics
- Name: `APP - BYOD ReadOnly`
- Next

---

## 🔹 Apps
- Add:
  - Outlook
  - Teams
  - OneDrive
  - SharePoint
  - Office
  - (Optional) Edge
- Next

---

## 🔹 Data Protection (READ-ONLY SETTINGS)

- Backup org data → **Block**
- Send org data → **Policy managed apps**
- Save copies → **Block**
- Allow save to services → **None**
- Receive data → **Policy managed apps**
- Open data into org → **Allow**
- Restrict copy/paste → **Blocked**
- Cut/copy limit → **0**
- Third-party keyboard → **Block**

### Encryption
- Encrypt org data → **Require**

### Functionality
- Sync → **Block**
- Printing → **Block**
- Web transfer → **Policy managed apps**
- Notifications → **Block**
- Screen capture → **Block**

---

## 🔹 Access Requirements

- PIN → **Require**
- PIN type → **Numeric**
- Simple PIN → **Block**
- Min length → **6**
- Biometrics → **Allow**
- Override with PIN → **Require**
- Timeout → **5–15 min**
- App PIN when device PIN set → **Require**
- Work account → **Require**

---

## 🔹 Conditional Launch

### App conditions
- Max PIN attempts → **5 → Reset PIN**
- Offline (minutes) → **1440 → Block**
- Offline (days) → **90 → Wipe**

### Device conditions
- Jailbroken/rooted → **Block**

### Additional
- Min OS:
  - iOS → **14.0**
  - Android → **10.0**
- Action → **Block access**

---

## 🔹 Assignments

### Included
- Add group → `BYOD-Test`

### Excluded
- Add:
  - Break-glass admin accounts

---

## 🔹 Create
- Click **Create**

---

# ✅ RESULT

- ✅ Mobile apps = **Read-only**
- ❌ No copy / paste / save / download / print
- ✅ Works on personal devices (no enrollment required)
- ✅ Data stays inside protected apps

---
