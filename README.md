# IM Instance Schedules

This is a schedules configuration for a pipeline that periodically executes predefined actions on configured IM instances.

## Configuration

The schedule is defined in `schedules.yaml`.

The format is as follows:

```yaml
<group-name>:
  <instance-name>:
    <action>: <time - HH:MM>
```

For example:

```yaml
play:
  dev-2-41:
    reset: '01:20'
emis:
  dev:
    save: '00:30'
  demo:
    reset: '01:30'
```

## Actions

### `reset`

This action will:
1.  Reset the `dhis2-db` instance.
2.  Reset the `dhis2-core` instance.
3.  Wait for the instance to be available.
4.  Trigger analytics table generation.

For the analytics generation to work, a credential named `dhis2-analytics-admin` must exist in Jenkins and the corresponding user must be present in the target DHIS2 instance. If not, the analytics step will fail but the pipeline will not fail.

The hostname for an instance is constructed from the group name (e.g., `https://<group-name>.im.dhis2.org`). For groups with non-standard hostnames, you can add an entry to the `HOSTNAME_EXCEPTIONS` map at the top of the `Jenkinsfile`.

### `reset-database`

This action will reset the `dhis2-db` instance without resetting the core. It will attempt to run analytics as in the `reset` action.

### `save`

This action will save a snapshot of the database using the `save.sh` script from the `im-manager` repository.
