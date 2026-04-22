# OpenShift Health Check Report Tool

This repository contains a fresh Python-based OpenShift health check project built to resemble a more complete reporting tool. It generates a real Excel workbook (`.xlsx`) with multiple worksheets so beginners can explore cluster and workload health in a structured way.

## What this project does

The tool connects to an OpenShift cluster through the `oc` CLI and collects health data for:

- cluster information
- nodes
- pods
- deployments
- services
- routes
- events
- container probe configuration

It then writes the results into a multi-sheet Excel report inside the `reports/` folder.

## Output

Reports are generated in:

```text
reports/
```

Example output:

```text
reports/openshift-health-report-20260422_162000.xlsx
```

## Features

- Python-based CLI
- real Excel workbook generation with `openpyxl`
- multiple worksheets for easier review
- namespace filtering
- label selector filtering
- repo-local report output
- beginner-friendly structure

## Worksheets in the Excel report

The generated workbook currently includes:

1. `Summary`
2. `Cluster`
3. `Nodes`
4. `Pods`
5. `Deployments`
6. `Services`
7. `Routes`
8. `Events`
9. `Probes`

## Prerequisites

Before running the tool, make sure you have:

- Python 3.8 or later
- OpenShift CLI installed (`oc`)
- access to an OpenShift cluster
- permission to view the resources you want to inspect
- logged in using:

```bash
oc login <cluster-url>
```

## Installation

Install Python dependency:

```bash
python3 -m pip install -r requirements.txt
```

## Usage

### Run against all namespaces

```bash
python3 openshift_health_check.py
```

### Run for a specific namespace

```bash
python3 openshift_health_check.py --namespace my-namespace
```

### Run with a label selector

```bash
python3 openshift_health_check.py --namespace my-namespace --label-selector app=myapp
```

### Custom output directory

```bash
python3 openshift_health_check.py --output-dir reports
```

### Custom report filename

```bash
python3 openshift_health_check.py --report-name demo-health.xlsx
```

## Command line options

- `--namespace`  
  Target one namespace. If omitted, the tool scans all namespaces.

- `--label-selector`  
  Optional label filter such as `app=myapp`.

- `--output-dir`  
  Directory for generated reports. Default: `reports`

- `--report-name`  
  Custom Excel filename.

## What each worksheet means

### Summary
High-level counts for:
- ready nodes
- running pods
- unhealthy pods
- deployments needing review
- services
- routes
- events

### Cluster
Basic cluster metadata such as:
- current OpenShift user
- client version
- server version

### Nodes
Node inventory with:
- node name
- status
- roles
- CPU
- memory
- operating system
- kernel version

### Pods
Pod-level review with:
- namespace
- pod name
- phase
- readiness
- restart count
- assigned node
- failure or waiting reason

### Deployments
Deployment health based on:
- desired replicas
- ready replicas
- available replicas

### Services
Service exposure details:
- service type
- cluster IP
- ports

### Routes
OpenShift route details:
- host
- path
- backing service

### Events
Recent cluster or namespace events that help explain:
- failures
- warnings
- scheduling issues
- image pull problems

### Probes
Deployment container probe details:
- liveness probes
- readiness probes
- startup probes
- HTTP path
- port
- initial delay

## Why this is useful for learning

This project helps beginners understand how OpenShift health is usually reviewed:

1. verify cluster access
2. inspect node health
3. inspect pod state
4. compare desired vs available deployments
5. verify service and route exposure
6. inspect events for warnings
7. review readiness, liveness, and startup probes

## Example beginner workflow

1. Log in:
   ```bash
   oc login <cluster-url>
   ```

2. Run the health check:
   ```bash
   python3 openshift_health_check.py --namespace demo
   ```

3. Open the generated `.xlsx` file from the `reports/` folder.

4. Review:
   - pods not in `Running`
   - deployments not fully available
   - warning events
   - missing or incomplete probes

## Project structure

```text
openshift-healthcheck/
├── openshift_health_check.py
├── requirements.txt
├── README.md
└── reports/
```

## Notes

- This implementation is newly written for this repository and not copied from the reference folder.
- It follows a similar idea: collect health data and present it in a structured Excel report.
- The current version focuses on Excel output and beginner readability.

## Suggested next improvements

You can extend this project by adding:

- PDF report generation
- cluster operators worksheet
- storage worksheet
- namespace worksheet
- Prometheus-based capacity analysis
- health score calculation
- setup scripts
- troubleshooting guide
- example usage scripts

## Main command to use

```bash
python3 openshift_health_check.py
```
