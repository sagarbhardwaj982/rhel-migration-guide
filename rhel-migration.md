# RHEL Migration Guide — 7 / 8 / 9 / 10

This guide explains how to upgrade Red Hat Enterprise Linux between supported major versions using Leapp.

Important:
- Always take a backup or snapshot before a major OS migration.
- Make sure the host is on the latest supported minor release of the current major version.
- Confirm the system is registered and subscribed.
- Review `/var/log/leapp/leapp-report.txt` before continuing.
- If an `inhibitor` is present, fix it before continuing.

---

# Part 1: RHEL 7 → RHEL 8

Update the current system and reboot:

```bash
sudo yum update -y
sudo reboot
```

Check the subscription state:

```bash
sudo subscription-manager list --installed
sudo subscription-manager status
```

If the system is not registered, register it:

```bash
sudo subscription-manager register --auto-attach
```

Lock the release to the latest supported 7.x version:

```bash
sudo subscription-manager release --set=7.9
```

Enable the repo that contains Leapp and install it:

```bash
sudo subscription-manager repos --enable=rhel-7-server-extras-rpms
sudo yum install leapp-upgrade -y
```

Run the pre-upgrade validation:

```bash
sudo leapp preupgrade
```

Check the report for blockers:

```bash
grep -i "inhibitor" /var/log/leapp/leapp-report.txt
```

If `inhibitor` appears:
- read the report carefully
- fix the issue
- rerun `leapp preupgrade`

When the report is clean, perform the migration:

```bash
sudo leapp upgrade
sudo reboot
```

Verify the OS version:

```bash
cat /etc/redhat-release
uname -r
```

Expected result:
- RHEL 8.x

---

# Part 2: RHEL 8 → RHEL 9

If this server had previously been upgraded from RHEL 7, remove any stale temporary Leapp files:

```bash
sudo rm -Rf /root/tmp_leapp_py3
```

Update the current system and reboot:

```bash
sudo dnf update -y
sudo reboot
```

Check the subscription state:

```bash
sudo subscription-manager list --installed
sudo subscription-manager status
```

Enable the required repositories:

```bash
sudo subscription-manager repos \
  --enable rhel-8-for-x86_64-baseos-rpms \
  --enable rhel-8-for-x86_64-appstream-rpms
```

Set the supported 8.x release:

```bash
sudo subscription-manager release --set=8.10
sudo dnf update -y
```

Install Leapp and run the pre-check:

```bash
sudo dnf install leapp-upgrade -y
sudo leapp preupgrade
```

Check for blockers:

```bash
grep -i "inhibitor" /var/log/leapp/leapp-report.txt
```

Fix any inhibitors before continuing.

When the report is clear:

```bash
sudo leapp upgrade
sudo reboot
```

Verify the version:

```bash
cat /etc/redhat-release
uname -r
```

Expected result:
- RHEL 9.x

---

# Part 3: RHEL 9 → RHEL 10

Check the current release and subscription:

```bash
cat /etc/redhat-release
sudo subscription-manager list --installed
```

Install the EL9-to-EL10 migration package:

```bash
sudo dnf install leapp-upgrade-el9toel10 -y
```

Run the pre-upgrade check with the target version:

```bash
sudo leapp preupgrade --target 10.0
```

Review blockers:

```bash
grep -i "inhibitor" /var/log/leapp/leapp-report.txt
```

Fix any reported inhibitors before continuing.

When the report is clear:

```bash
sudo leapp upgrade --target 10.0
sudo reboot
```

Verify the upgraded OS:

```bash
cat /etc/redhat-release
uname -r
```

Expected result:
- RHEL 10.x

---

# Reading the Leapp report

The report may look like:

```text
Risk Factor: high (inhibitor)
Title: ...
Summary: ...
Remediation:
[hint] ...
```

Check for blockers with:

```bash
grep -i "inhibitor" /var/log/leapp/leapp-report.txt
```

If there is no output:
- the system is clear to continue

If there is output:
- review the issue
- fix the cause
- rerun the pre-upgrade process

---

# Best practices

- Always take a full backup or VM snapshot before an OS upgrade
- Keep the current system on the latest supported minor version
- Disable unsupported third-party repositories
- Ensure the target version is supported by your subscription
- Validate all services after the reboot
- Use a maintenance window for production systems

---

# Final validation

After each migration, validate the host:

```bash
cat /etc/redhat-release
uname -r
```

This confirms the machine is running the expected RHEL major version.

---

# Notes

This guide is intended as a practical operational reference. Always confirm compatibility with Red Hat documentation, your support contract, and your internal change-management policy.
