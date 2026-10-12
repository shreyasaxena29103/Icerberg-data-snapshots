# Icerberg-data-snapshots
A hands-on project exploring Apache Iceberg snapshot management, metadata inspection, and safe snapshot expiration using PyIceberg.

Overview

Apache Iceberg uses snapshots to track the state of a table over time. These snapshots enable time travel, support consistent reads, and help manage changes to table data.

However, retaining snapshots indefinitely can increase metadata overhead and leave older data files eligible for cleanup. This project explores how to inspect snapshot history, evaluate retention policies, and identify snapshots that may be expired.

Objectives

Inspect Iceberg table snapshots and their metadata.

Understand snapshot IDs, timestamps, parent snapshots, and current snapshots.

Implement age-based snapshot retention rules.

Preserve a configurable number of recent snapshots.

Generate a JSON audit report before performing any destructive operation.

Explore safer maintenance workflows for Iceberg tables.

Tech Stack

Apache Iceberg — table format and snapshot-based metadata management

PyIceberg — Python interface for working with Iceberg tables

Python — audit logic and automation

SQLite catalog — local catalog for development and testing

PyArrow — columnar data representation

Project Structure

iceberg-data-snapshots/
├── src/
│   └── iceberg_snapshot_auditor/
│       ├── __init__.py
│       └── auditor.py
├── tests/
│   └── test_auditor.py
├── create_demo_table.py
├── requirements.txt
├── README.md
└── .gitignore

The structure above represents the intended organization; adjust it to match the files in your repository.

How It Works

The auditor follows a simple workflow:

Connect to an Iceberg catalog.

Load the specified Iceberg table.

Retrieve the table's snapshot history.

Evaluate snapshots against the configured age threshold and retention count.

Produce an audit report identifying potential expiration candidates.

Keep the operation in dry-run mode unless execution is explicitly requested.

Example

Run an audit against a local demo table:

python -m iceberg_snapshot_auditor.auditor `
  --catalog default `
  --table audit_demo.events `
  --older-than-days 7 `
  --retain-last 1 `
  --json-out snapshot-audit.json

The example assumes your package is installed or your PYTHONPATH is configured and your local catalog is set up correctly.

Example Audit Output

{
  "catalog": "default",
  "table": "audit_demo.events",
  "mode": "dry-run",
  "older_than_days": 7,
  "retain_last": 1,
  "snapshot_count": 3,
  "candidate_count": 0,
  "expiration_committed": false
}

Illustrative output only. Actual counts depend on the table's snapshot history and retention policy.

Safety First

The project prioritizes reviewing proposed changes before committing expiration.

Dry run by default: inspect the candidates before taking action.

Retention controls: specify an age threshold and the number of recent snapshots to retain.

Local testing: validate behavior against a disposable table before production use.

Reference awareness: verify snapshot tags, branches, and other references before expiration.

Data cleanup awareness: snapshot expiration and orphan-file cleanup are distinct maintenance operations.

Always verify the installed PyIceberg version and its supported maintenance API before enabling actual expiration.

Getting Started

Clone the repository.

Create and activate a Python virtual environment.

Install the dependencies listed in requirements.txt.

Configure a local Iceberg catalog and warehouse.

Create the demo table.

Run the auditor in dry-run mode.

Review the JSON report and test the retention logic.

Future Enhancements

Add scheduled snapshot audits.

Export audit history to a persistent metadata table.

Support configurable retention policies per table.

Add structured logging and monitoring.

Expand automated tests for edge cases and snapshot references.

Integrate with a production catalog when required.

Learning Outcomes

This project provides practical experience with:

Iceberg snapshot metadata and table history

PyIceberg catalog configuration

Retention policy design

Safe data lakehouse maintenance

Python-based data engineering automation

Disclaimer

This is a learning and development project. Test all maintenance operations in a non-production environment first. Never expire snapshots or remove data files without validating retention requirements, table references, and recovery needs.

Author

Shreya Saxena

Exploring data engineering, lakehouse architecture, and Apache Iceberg.
