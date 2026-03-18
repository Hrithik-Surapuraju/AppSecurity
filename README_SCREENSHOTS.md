# AppSecurity Visual Guide

This README is a screenshot-based walkthrough of the AppSecurity project.

For the full technical guide, see [README.md](C:\AppSecurity\README.md).  
For the beginner-friendly explanation, see [README_LAYMAN.md](C:\AppSecurity\README_LAYMAN.md).

## What This Project Is

AppSecurity is a Python-based application security tool that combines:

- `SAST` to scan source code and configuration files
- `DAST` to test a running web application
- a local `Web UI` for running scans without staying in the terminal

It supports:

- severity filtering
- reusable profiles
- ignore files for accepted findings
- baseline comparison for new-only findings
- authenticated DAST with session and CSRF-aware login handling
- report export in `Text`, `JSON`, `HTML`, and `SARIF`

## Screenshot 1: AppSecurity Web UI

This is the local browser UI used to run scans and download reports.

![AppSecurity UI](C:\AppSecurity\screenshots\appsecurity-ui.png)

What you can see here:

- scan mode selection (`SAST`, `DAST`, `Both`)
- code and URL target fields
- reusable profile support
- severity filtering
- ignore file and baseline file options
- login and CSRF fields for authenticated testing
- download buttons for reports and ignore templates

## Screenshot 2: Vulnerable Demo App

This is the intentionally vulnerable demo application used for testing the scanner.

![Demo App](C:\AppSecurity\screenshots\demo-app.png)

This demo app includes:

- reflected input handling
- login form
- searchable data
- protected route testing
- CSRF-aware login behavior

It exists only for local testing and should never be deployed as a real application.

## Screenshot 3: Generated HTML Security Report

This is an example of the generated HTML report after running a scan.

![HTML Report](C:\AppSecurity\screenshots\full-report.png)

What this report shows:

- total scanned targets
- total findings
- high-severity findings summary
- detailed vulnerability entries
- evidence and remediation guidance

## Typical Workflow

### 1. Start the UI

Run:

```powershell
& 'C:\Users\Surap\AppData\Local\Python\pythoncore-3.14-64\python.exe' .\security_scanner.py webui --host 127.0.0.1 --port 8088
```

Then open:

```text
http://127.0.0.1:8088
```

### 2. Start the Demo App

Run in a separate PowerShell window:

```powershell
& 'C:\Users\Surap\AppData\Local\Python\pythoncore-3.14-64\python.exe' .\demo_app\vulnerable_demo.py
```

### 3. Use the Saved Authenticated Profile

In the UI:

- `Mode`: `both`
- `Code Target`: `C:\AppSecurity\demo_app`
- `URL Target`: `http://127.0.0.1:8000/`
- `Profile Name`: `authenticated-demo`
- `Profiles File`: `scan_profiles.json`

Then click:

- `Run Scan`

### 4. Review or Download the Results

From the UI you can:

- preview the report
- download the current scan report
- download an ignore-template JSON file

## Files Referenced

- [security_scanner.py](C:\AppSecurity\security_scanner.py)
- [scan_profiles.json](C:\AppSecurity\scan_profiles.json)
- [demo_full_report.html](C:\AppSecurity\demo_full_report.html)
- [demo_sast_report.html](C:\AppSecurity\demo_sast_report.html)
- [baseline_report.json](C:\AppSecurity\baseline_report.json)

## Screenshot Files

- [appsecurity-ui.png](C:\AppSecurity\screenshots\appsecurity-ui.png)
- [demo-app.png](C:\AppSecurity\screenshots\demo-app.png)
- [full-report.png](C:\AppSecurity\screenshots\full-report.png)
