# Ransomware-Simulation

## Overview
This lab simulates an initial attack scenario (Initial Access) based on social engineering, where a user executes a seemingly legitimate file (e.g., an invoice) that triggers suspicious behavior on the system.

The goal is to analyze and detect these behaviors using tools like Sysmon and Event Viewer, without using real malware.

## Objectives
- Simulate execution of a disguised malicious file  
- Observe process chain (process tree)  
- Identify suspicious PowerShell execution  
- Detect abnormal file creation  
- Analyze logs using Sysmon

## Lab Environment

- 1 Windows Virtual Machine
- Sysmon installed
- Event Viewer

## Lab Steps

1. Create the simulated phishing file
Create a file with a convincing name:

invoice_april_2026.bat
File content:
```
@echo off
echo Opening document...

start notepad.exe

powershell -WindowStyle Hidden -Command "Get-ChildItem C:\\Users\\Public\\Documents | Out-File C:\\Users\\Public\\log.txt"

timeout /t 2 >nul

echo executed > C:\\Users\\Public\\executed.txt
```
## Execute the scenario
Place the file in the Downloads folder
Execute it manually (simulating user behavior)

## Security Analysis
🔗 Expected Process Chain
```
explorer.exe → cmd.exe → powershell.exe → notepad.exe
```

## Indicators of Compromise (IOCs)
PowerShell execution with hidden window
Script executed from Downloads directory
Unexpected file creation:
- C:\Users\Public\log.txt
- C:\Users\Public\executed.txt

## Using Sysmon
Relevant Events
Log Path
```
Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational
```
<img width="1364" height="255" alt="image" src="https://github.com/user-attachments/assets/e61fda31-3a1f-4985-8e5d-7d8c8b1d4d91" />

<img width="624" height="367" alt="image" src="https://github.com/user-attachments/assets/5facd93f-8126-4625-82fd-58ecaf5aaecc" />


## Critical Analysis

This behavior is not typical of a normal user because:

An "invoice" file should not execute PowerShell
Hidden execution indicates possible evasion techniques
Files are created outside expected user context

## Security Best Practices
Avoid executing unknown files
Monitor script execution (PowerShell)
Use logging tools like Sysmon
Implement behavior-based detection

## conclusion

This lab demonstrates how simple social engineering attacks can lead to suspicious command execution, reinforcing the importance of behavioral detection and continuous monitoring.
