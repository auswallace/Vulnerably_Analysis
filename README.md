# Vulnerability Management Data Analysis

## Overview
This project focuses on exploring, analyzing, and modeling vulnerability management data. The dataset simulates vulnerability scan results across various devices, allowing for experimentation with risk scoring, device classification, vulnerability aging, and prioritization techniques.

The dataset is generated using Python, incorporating realistic relationships between vulnerabilities, devices, risk levels, and scanning timelines. This repository showcases data analysis techniques, risk modeling, and reporting best practices in cybersecurity analytics.

---

## Data Overview
The dataset consists of simulated device vulnerability scan results, with each row representing a scanned vulnerability on a specific device.

| Column Name          | Description |
|----------------------|------------------------------------------------|
| device_id           | Unique identifier for a device. |
| device_name         | Hostname of the device. |
| ip_address          | IP address of the device. |
| os                 | Operating system installed on the device. |
| device_type        | Categorized as "Server" or "Workstation", based on OS type. |
| scan_date          | Date when the vulnerability scan was performed. |
| vulnerability_id    | CVE identifier of the detected vulnerability (e.g., CVE-2023-12345). |
| cve_publish_date   | Date when the vulnerability was publicly disclosed. |
| plugin_id          | Unique identifier for the vulnerability detection plugin. |
| cvss_score         | CVSS (Common Vulnerability Scoring System) score (0-10) for the vulnerability. |
| risk_score         | Deterministically calculated based on CVSS score (see Risk Scoring below). |
| exploit_available  | Whether an exploit is publicly available (Yes / No). |
| patch_available    | Whether a patch exists (Yes / No). |
| compliance_issue   | Whether the vulnerability violates compliance policies (Yes / No). |
| vulnerability_age  | Days since scan date (today - scan_date). |

---

## How Key Columns Were Created
- **device_type** → Assigned based on `os`:  
  - `"Windows Server"` / `"Linux RHEL"` → **Server**  
  - `"Windows 10"` / `"Linux Ubuntu"` / `"macOS Ventura"` → **Workstation**  
- **scan_date Adjustment** → If `scan_date` is before `cve_publish_date`, it is pushed forward.  
- **risk_score Calculation** → Determined using a fixed multiplier per CVSS range:  

  | CVSS Score Range | Multiplier | Risk Score Formula |
  |------------------|------------|----------------------|
  | 0.0 - 3.9 (Low) | 50         | `cvss_score * 50` |
  | 4.0 - 6.9 (Medium) | 75      | `cvss_score * 75` |
  | 7.0 - 8.9 (High) | 100       | `cvss_score * 100` |
  | 9.0 - 10.0 (Critical) | 125  | `cvss_score * 125` |

- **vulnerability_age** → Computed as days between `scan_date` and today.

---

## Data Nuances & Considerations
1. **Scan Dates vs. CVE Publish Dates**  
   - A vulnerability cannot be scanned before it was publicly disclosed.  
   - If `scan_date` is earlier than `cve_publish_date`, it is adjusted to ensure logical consistency.  

2. **Risk Score Is Deterministic**  
   - Unlike traditional risk assessments that may use random scaling, this dataset applies fixed multipliers to make risk scores consistent and reproducible.  

3. **Device & Vulnerability Distribution**  
   - The dataset is parameterized to ensure a minimum number of unique devices and vulnerabilities while maintaining a realistic many-to-many mapping between them.  

4. **Use Cases for This Dataset**  
   - **Visualization:** Generate risk dashboards using Python, Tableau, or Power BI.  
   - **Statistical Analysis:** Identify trends in vulnerability aging and risk prioritization.  
   - **Machine Learning:** Train models to predict high-risk vulnerabilities based on attributes.  

---

## Next Steps & How to Use This Data
- Load the dataset into a Python notebook or data visualization tool.
- Analyze trends in risk scores, exploit availability, and vulnerability aging.
- Experiment with different risk scoring models to improve prioritization.
- Use it as a practice dataset for security analytics and data science projects.

