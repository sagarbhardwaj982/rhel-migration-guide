# RHEL Migration Guide

This repository contains a practical migration playbook for upgrading Red Hat Enterprise Linux between supported major versions.

## Files

- `rhel-migration.md` — detailed migration steps for RHEL 7 → 8, 8 → 9, and 9 → 10
- `README.md` — overview, prerequisites, and usage notes

## Purpose

This guide is intended for system administrators and DevOps engineers who need a safe and repeatable process for major-version migration using Leapp.

It focuses on:

- verifying subscription and entitlement state
- locking the system to a supported source release
- enabling required repositories
- running `leapp preupgrade` and checking inhibitors
- completing the migration and validating the new OS version

## Important notes

- Always validate backups or snapshots before a major-version migration.
- Ensure the current system is on the latest supported minor version of the existing major release.
- Disable or remove unsupported third-party repositories before running Leapp.
- If `leapp-report.txt` contains an inhibitor, fix it before continuing.
- This is a general runbook and should be validated against your organization’s Red Hat subscription and environment.

## Quick access

Open the full guide here:

- [`rhel-migration.md`](./rhel-migration.md)

## Recommended workflow

1. Confirm system registration and subscription
2. Update current version and reboot
3. Enable the proper repo set
4. Install Leapp
5. Run `leapp preupgrade`
6. Review `/var/log/leapp/leapp-report.txt`
7. Resolve all inhibitors
8. Run `leapp upgrade`
9. Reboot and validate `/etc/redhat-release`

## Support

This repository is for operational guidance and should be used alongside your organization’s Red Hat support policy and internal change controls.
