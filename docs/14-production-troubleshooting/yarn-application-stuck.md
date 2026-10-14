# YARN Application Stuck

## 1. Production Issue

A YARN application is stuck in:

```text
ACCEPTED
```

or remains in:

```text
RUNNING
```

for an unexpectedly long time.

---

## 2. How to Identify

List running/submitted applications:

```bash
yarn application -list
```

Check the specific application:

```bash
yarn application -status <application_id>
```

Example:

```bash
yarn application -status application_123456789_0001
```

---

## 3. If Application Is Stuck in ACCEPTED

An application in `ACCEPTED` generally means YARN has accepted the application but has not yet launched the ApplicationMaster.

Check NodeManagers:

```bash
yarn node -list
```

Check queue:

```bash
yarn queue -status <queue_name>
```

Review the queue and cluster resource utilization in Cloudera Manager.

---

## 4. Common Causes of ACCEPTED

* Insufficient memory.
* Insufficient CPU.
* Queue capacity exhausted.
* Queue/user limits reached.
* Too many applications already running.
* NodeManagers unavailable.
* Application requests very large resources.
* Queue configuration restrictions.

---

## 5. If Application Is RUNNING but Not Progressing

Get application logs:

```bash
yarn logs -applicationId <application_id>
```

Save them:

```bash
yarn logs -applicationId <application_id> > application.log
```

Search for errors:

```bash
grep -i "error" application.log
```

Search for failures:

```bash
grep -Ei "exception|failed|killed|timeout" application.log
```

---

## 6. Check YARN Resources

Check NodeManagers:

```bash
yarn node -list
```

Review in Cloudera Manager:

```text
YARN
 ├── Memory utilization
 ├── CPU utilization
 ├── NodeManagers
 ├── Queues
 └── Applications
```

Check whether the requested application resources are unusually high.

---

## 7. Common Root Causes

### Resource starvation

Application requests more memory/CPU than currently available.

### Queue saturation

The queue has reached its configured capacity.

### Application problem

The application itself is stuck or failing.

### Data skew

One or more tasks take significantly longer than others.

### External dependency

The application is waiting for another system.

---

## 8. How to Fix

### Case 1 — Queue is full

Review the queue configuration and running applications.

If appropriate, stop obsolete/stuck applications after confirming ownership.

---

### Case 2 — Application has excessive resource requirements

Review the application's memory and CPU configuration.

For Spark-on-YARN, inspect:

```text
spark.executor.memory
spark.executor.cores
spark.executor.instances
spark.driver.memory
```

Do not reduce production resources blindly; determine the application's actual requirements first.

---

### Case 3 — Application is genuinely stuck

After confirming with the application owner:

```bash
yarn application -kill <application_id>
```

Then investigate why the application became stuck before allowing it to run again.

---

## 9. Verification

Check:

```bash
yarn application -list
```

Verify the affected application is no longer stuck.

Check:

```bash
yarn node -list
```

Verify NodeManagers are healthy.

Confirm that other applications are receiving resources normally.

---

## 10. Prevention

* Monitor queue utilization.
* Monitor long-running applications.
* Set appropriate queue capacity.
* Educate developers on resource sizing.
* Monitor failed applications.
* Review Spark executor/driver sizing.
* Monitor data skew and application performance.

---

## 11. Troubleshooting Flow

```text
Application Stuck
       ↓
yarn application -status
       ↓
ACCEPTED or RUNNING?
       ↓
ACCEPTED → Check queue/resources/NodeManagers
       ↓
RUNNING → Check logs/tasks/resources
       ↓
Identify root cause
       ↓
Fix configuration/application/queue
       ↓
Kill only if approved
       ↓
Verify application and YARN health
```

---

## 12. Interview Question

**Q: A YARN application has been in ACCEPTED state for 30 minutes. What will you check?**

I would check the application's requested resources, YARN queue capacity, available cluster resources, NodeManager health, queue/user limits and other running applications. I would determine whether the application is waiting for resources before changing or killing anything.
