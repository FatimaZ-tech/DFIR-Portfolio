# DFIR Case Report – TryHackMe: Secret Recipe

## Case Information
**Case Title:** Secret Recipe  
**Platform:** TryHackMe  
**Analyst:** Fatima Z  
**Date:** 7 Jan 2025  
**Objective:**  
Analyze provided Windows registry hives to determine whether sensitive intellectual property belonging to a coffee shop owner was accessed or discovered on a confiscated employee workstation.

---


## Executive Summary
Jasmine, the owner of a well-known New York coffee shop called *Coffely*, reported potential theft of her confidential coffee recipe after her work laptop was serviced by James from the IT department. The recipe is known to be stored exclusively on her work device.

James’s personal machine was confiscated and analyzed after suspicion arose that he may have accessed or copied the recipe. Although no direct file copies were found on the system, registry artifacts revealed compelling evidence of unauthorized access, sensitive file discovery, reconnaissance activity, and concealment efforts with user account creation and VPN.

The investigation confirms that James intentionally accessed and identified the secret recipe file, demonstrating clear intent to misuse privileged access and potentially exfiltrate the confidential coffee recipe.

---

## Investigation Scope & Evidence Sources
The investigation was conducted **exclusively** on the following Windows registry hives:

| Registry Hive | Purpose |
|-------------|--------|
`SYSTEM` | Computer identification and system configuration  
`SECURITY` | Security policy and audit-related artifacts  
`SOFTWARE` | Installed software and network configuration  
`SAM` | Local user account information  
`NTUSER.DAT` | User activity, searches, and file interaction artifacts  
`UsrClass.dat` | Shell activity and user interaction metadata  

**Primary Tools Used**
- Registry analysis tool Registry Explorer
- Manual artifact correlation
- Timeline reconstruction based on registry data

---

# Detailed Investigation & Findings

---

## (1) System Identification
The system name was identified from SYSTEM hive.

---

## (2) User Account Analysis
User account information was extracted from the `SAM` hive:

A suspicious user account named **bdoor** was identified.

![Suspicious User Account](evidence/bdoor-user.png)

This indicates:
- Potential unauthorized account creation
- Possible persistence or alternate access and risk of misuse
---

## (3) Network Configuration & VPN Evidence
Analysis of the `SOFTWARE` hive revealed network-related artifacts.

A VPN configuration associated with **ProtonVPN** was identified.

This suggests:
- Intentional network anonymization
- Preparation for covert data access or transfer

---

## (4) Shared Resource Awareness
Registry artifacts related to network shares indicated awareness of a shared folder named **RESTRICTED FILES**

![Restricted Files Share](evidence/restricted-files.png)

This suggests that the user was aware of and interacted with a restricted resource likely containing sensitive data.

---

## (5) Sensitive File Discovery via Registry Artifacts
Review of registry-based recent file artifacts revealed access to:

- **secret-recipe.pdf**

The access timestamp correlates with interaction involving restricted resources.

![Secret Recipe PDF Artifact](evidence/secret-recipe-pdf.png)

This indicates that the file was located and accessed on the system at that time.

---

## (6) Additional File Interaction
Registry artifacts also revealed interaction with an additional **TXT file** during the same timeframe.

![TXT File Artifact](evidence/txt-file.png)

This may represent supporting activity related to the sensitive file like notes taken by the user

---

## (7) Reconnaissance Activity Identified
Registry evidence indicated execution of commands used to enumerate:

- Network interfaces
- Connected devices

![Reconnaissance Activity](evidence/recon-commands.png)

This behavior aligns with reconnaissance activity following unauthorized access.

---

## (8) User Search Activity
Analysis of the `NTUSER.DAT` hive, specifically the `WordWheelQuery` key, revealed searches for:

- Network utility tools
- File transfer methods using File Explorer

![Search History Evidence](evidence/search-history.png)

This demonstrates intent to identify mechanisms capable of moving files.

---

## (9) Program Execution Evidence
Registry-based execution artifacts revealed use of the following programs:

- **PowerShell** (multiple executions)
- **Wireshark** (network monitoring)
- **Explorer** (file navigation)
- **everything.exe** (file indexing and discovery)

![Everything.exe Evidence](evidence/powershell.png)
![Everything.exe Evidence](evidence/everything-exe.png)

The presence of `everything.exe` is particularly significant, as it enables rapid system-wide file discovery.

---

---

# Timeline of Events (Registry-Based)
1) System identity confirmed via SYSTEM hive  
2) Suspicious user account **bdoor** identified  
3) VPN configuration (ProtonVPN) detected  
4) Awareness of restricted shared folder established  
5) `secret-recipe.pdf` located and accessed  
6) Additional TXT file accessed  
7) Network and device enumeration observed  
8) Searches for file transfer utilities performed  
9) File discovery tools executed  

---

# Conclusion
Based solely on the analysis of the provided Windows registry hives, the investigation confirms unauthorized and intentional activity consistent with sensitive data discovery.

Although no file system artifacts or confirmed exfiltration evidence were available, registry data clearly demonstrates that James accessed and identified the confidential coffee recipe belonging to Jasmine. The use of VPN software, user creation, reconnaissance, and advanced file discovery tools further supports malicious intent.

The absence of direct file copies does not negate the findings, as registry artifacts provide sufficient evidence of access and preparation.

**Case Outcome:**  
Confirmed unauthorized access and sensitive data discovery involving Coffely’s secret recipe.

---

# Security Recommendations

### Access & Account Controls
- Audit and remove unauthorized local user accounts
- Enforce least-privilege policies

### Monitoring & Detection
- Alert on VPN software installation and usage
- Monitor execution of file discovery and reconnaissance tools

### Data Protection
- Restrict access to sensitive shared resources

### Insider Threat Mitigation
- Implement strict IT maintenance access controls
- Monitor user behavior following privileged access
- Enforce separation of duties

---

## Evidence Folder
All screenshots supporting findings are located in the `evidence/` directory.
