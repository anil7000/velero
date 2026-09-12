# Backup evidence and restore-readiness checklist

A completed backup is not, by itself, proof that an application can be restored.
Use this checklist to distinguish backup status from recovery evidence.

## Inspect an existing backup

With the Velero CLI installed and an authorized context selected, replace
sample-backup with an existing backup name:

```sh
kubectl config current-context
velero backup describe sample-backup --details
velero backup logs sample-backup
```

These commands inspect existing backup information; they do not create a backup
or restore. Logs may contain sensitive resource names, so scrub before sharing.

## Review coverage

| Area | Evidence to collect |
| --- | --- |
| Resource selection | Included/excluded namespaces and resource types |
| Storage | Snapshot or filesystem backup coverage for the actual volumes |
| Consistency | Application-specific quiescing or backup procedure |
| Encryption | Access to required keys through the approved recovery process |
| Dependencies | External databases, DNS, secrets and services outside the backup |
| Retention | Expiry and availability of the underlying backup data |

Review warnings and partial failures even when some resources succeeded.
Do not assume that Kubernetes manifests contain the data in persistent volumes.

## Verify a recovery drill

An isolated restore is a separate, approved change. Record the intended target,
namespace mappings and conflict-handling behavior before starting it.
After the drill, inspect its existing result:

```sh
velero restore describe sample-restore
velero restore logs sample-restore
```

Replace sample-restore with the actual restore name. Verify application reads,
writes and dependencies using synthetic checks; measure recovery time from
recorded timestamps. Do not publish invented RPO/RTO numbers from status alone.

See the [README](README.md) for installation and versioned documentation.

## Development note

This recovery checklist was added with AI assistance. Upstream code, licenses
and contributor attribution remain unchanged.
