# Simple AppSec Scanner

`security_scanner.py` is a lightweight Python CLI that demonstrates both:

- `SAST` (Static Application Security Testing): scans source files for risky patterns without executing them
- `DAST` (Dynamic Application Security Testing): probes a running web app with simple attack payloads

It is intentionally small and dependency-light, so it is best suited for demos, training, or starter automation rather than full enterprise coverage.

## Features

- Static checks for:
  - string-built SQL queries
  - `eval` / `exec`
  - `subprocess` with `shell=True`
  - hardcoded secrets
  - debug mode left enabled
  - unsafe `pickle` usage
  - potentially unsafe `yaml.load`
  - insecure randomness
  - disabled TLS verification
  - frontend `innerHTML` / `eval`
  - Java string-built SQL and `Runtime.exec`
  - PHP dangerous execution and query patterns
  - insecure configuration like wildcard CORS or disabled SSL validation
  - weak hashes, insecure temp files, Flask debug launch, stored browser tokens, private keys, and more
- Dynamic checks for:
  - reflected XSS via query parameters
  - reflected XSS via HTML forms
  - SQL-error indicators after simple quote injection
  - session-aware scans after optional login workflows
  - CSRF-aware login flows by collecting hidden token fields before authentication
- Output formats:
  - human-readable text
  - JSON for automation or CI parsing
  - HTML reports for sharing or review
  - SARIF for code scanning platforms and security tooling
- Triage support:
  - severity filtering with `--min-severity`
  - accepted-risk suppression via `--ignore-file`
  - baseline diffing with `--baseline-file` and `--new-only`
- Reuse support:
  - named scan profiles from `scan_profiles.json`
- Local browser workflow:
  - built-in web UI for launching scans, previewing reports, and downloading them directly

## Usage

Run SAST on a code directory:

```powershell
python .\security_scanner.py sast .\my_app
```

Write an HTML report to disk:

```powershell
python .\security_scanner.py sast .\my_app --format html --output .\report.html
```

Run DAST on a running app:

```powershell
python .\security_scanner.py dast "http://localhost:8000/search?q=test"
```

Run both:

```powershell
python .\security_scanner.py both --code .\my_app --url "http://localhost:8000/search?q=test"
```

Use JSON output:

```powershell
python .\security_scanner.py sast .\my_app --format json
```

Use SARIF output and only include `high` and above:

```powershell
python .\security_scanner.py sast .\my_app --format sarif --min-severity high --output .\results.sarif
```

Ignore accepted findings with an ignore file:

```powershell
python .\security_scanner.py sast .\my_app --ignore-file .\.scannerignore.json
```

Authenticated DAST with login and protected paths:

```powershell
python .\security_scanner.py dast "http://127.0.0.1:8000/" --login-url "http://127.0.0.1:8000/login" --login-method POST --login-data "username=admin&password=admin123" --csrf-url "http://127.0.0.1:8000/" --csrf-form-action "/login" --csrf-token-field "csrf_token" --success-indicator "Welcome admin" --protected-paths "/account"
```

Run from a reusable profile:

```powershell
python .\security_scanner.py both --code .\demo_app --url "http://127.0.0.1:8000/" --profile authenticated-demo --profiles-file .\scan_profiles.json
```

Show only findings that are new compared with a saved baseline:

```powershell
python .\security_scanner.py both --code .\demo_app --url "http://127.0.0.1:8000/" --profile authenticated-demo --profiles-file .\scan_profiles.json --baseline-file .\baseline_report.json --new-only
```

## Web UI

Start the local scanner UI:

```powershell
python .\security_scanner.py webui --host 127.0.0.1 --port 8088
```

Then open `http://127.0.0.1:8088` in your browser. The UI can run `SAST`, `DAST`, or `both`, apply a minimum severity filter, load named profiles, preview `text`, `json`, `html`, or `sarif` output, compare against a baseline, download reports directly, and generate an ignore-template JSON file from current findings.

## Demo App

An intentionally vulnerable demo target is included at `.\demo_app\vulnerable_demo.py`.

Start it locally:

```powershell
python .\demo_app\vulnerable_demo.py
```

Then run both SAST and DAST against it:

```powershell
python .\security_scanner.py both --code .\demo_app --url "http://127.0.0.1:8000/" --format html --output .\demo-report.html
```

To exercise login/session-aware DAST against the demo app:

```powershell
python .\security_scanner.py both --code .\demo_app --url "http://127.0.0.1:8000/" --profile authenticated-demo --profiles-file .\scan_profiles.json
```

## Notes

- SAST is pattern-based here, so false positives are possible.
- DAST only tests the paths and forms it can observe from the provided page.
- The DAST mode should only be used against applications you are authorized to test.
- The demo app is intentionally insecure and should never be deployed to a real environment.
