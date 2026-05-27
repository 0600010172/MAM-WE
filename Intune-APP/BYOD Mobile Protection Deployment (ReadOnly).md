# 📱 BYOD Mobile Protection Deployment Guide (Read-Only Model)

This guide provides step-by-step instructions to transition personal mobile devices (BYOD) from unrestricted access to a **protected, read-only access model** using:

- ✅ Microsoft Intune (App Protection Policies)
- ✅ Conditional Access
- ✅ SharePoint / OneDrive Browser Controls
- 🔮 Optional: Defender for Cloud Apps & Purview

---

# 🧭 STEP 1 — Create App Protection Policies (Intune)

> 🎯 Goal: Protect corporate data within mobile apps and enforce **read-only behavior**

---

## 🔹 1.1 Navigate
1. Open **Microsoft Intune admin center**
2. Go to:  
   **Apps → App protection policies**
3. Click **Create policy**

---

## 🔹 1.2 Basics
- Platform:
  - iOS/iPadOS (create separately)
  - Android (create separately)
- Name:
 
