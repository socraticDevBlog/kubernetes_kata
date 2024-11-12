# jobs_and_cron_jobs

## jobs

> A Kubernetes Job is a resource that runs a one-time task or batch process to completion, creating Pods to perform the task until it succeeds or reaches a specified failure limit.

Create a Kubernetes `Job` named `kata-log-cleaner-jo` in `kata-ns` namespace
that simulates a log-cleaning process, where the Job runs a container that
repeatedly prints "Cleaning logs" every 5 seconds and then finishes with "logs
cleaned."

The job should run for about 20-30 seconds

The job should run once and complete successfully.

### validation

run the job and then

watch the logs stream over a period of 20-30 seconds

```bash
kubectl logs -n kata-ns -f $(kubectl get pods -n kata-ns -l job-name=kata-log-cleaner-job -o jsonpath="{.items[0].metadata.name}")

```

## cron jobs

Create a CronJob with the following configuration:

    Set the schedule to * * * * * (every minute).
    Use the busybox image.
    Set the command as specified.
    Set the restartPolicy to OnFailure.

You can reuse the same container command that outputs fictitious logs:

```yaml
command:
- sh
- -c
- echo "Cleaning logs"; for i in {1..6}; do echo "Cleaning logs"; sleep 25; done; echo "logs cleaned"
```

### validation

read the logs from the latest cron job

```bash
kubectl logs -n kata-ns -f $(kubectl get pods -n kata-ns -o jsonpath="{.items[0].metadata.name}")

```

list out the jobs

```bash
kubectl get jobs -n kata-ns
```

## more realistic exercice (unicorn level)

in this kata, no actual logs are deleted. The `Job` and `CronJob` commands simply print messages to simulate log cleaning and rotation, so nothing is actually removed. 

If you want to make it more realistic and actually delete files:

1. **Mount a Persistent Volume**: You could use a `PersistentVolumeClaim` and an `emptyDir` or another storage class that supports ephemeral data.
   
2. **Create Temporary Files**: Modify the commands to create files before deleting them. For example, you could have each job generate some log files, then delete them as part of the "cleaning" or "rotation" process.

   ```yaml
   command: ["sh", "-c", "touch /logs/log1 && touch /logs/log2 && echo 'Deleting logs...' && rm -rf /logs/*"]
   ```

3. **Access Logs Across Jobs**: Mount a shared volume (like `emptyDir`) so that both `Job` and `CronJob` instances can read and delete files within the shared directory.

These steps would add some realism to the exercise, though it's a bit more complex. Let me know if you’d like help setting this up!