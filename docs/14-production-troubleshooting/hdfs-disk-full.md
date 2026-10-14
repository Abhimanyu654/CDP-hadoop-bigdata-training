# HDFS Disk Full / DataNode Disk Usage High

## 1. Production Issue

One or more DataNodes are running out of disk space.

Typical situation:

```text
DataNode disk utilization: 90%+
HDFS remaining capacity: very low
Spark jobs failing
HDFS write operations failing
DataNode health alerts
```

---

## 2. How to Identify the Issue

### Step 1: Check overall HDFS capacity

```bash
hdfs dfs -df -h
```

Check:

* Total capacity
* Used capacity
* Remaining capacity

---

### Step 2: Check DataNode utilization

```bash
hdfs dfsadmin -report
```

Look for:

```text
Configured Capacity
DFS Used
DFS Remaining
Non DFS Used
```

Pay particular attention to DataNodes with significantly higher utilization than others.

---

### Step 3: Check the operating-system disk

SSH to the affected DataNode:

```bash
ssh <datanode-host>
```

Then:

```bash
df -h
```

This is important because high disk utilization can come from both:

* HDFS data
* Non-HDFS data

---

### Step 4: Check inode utilization

```bash
df -i
```

A filesystem can have free GB available but still run out of inodes because of a very large number of files.

---

### Step 5: Find large HDFS directories

For example:

```bash
hdfs dfs -du -h /
```

Check specific directories:

```bash
hdfs dfs -du -h /user
```

```bash
hdfs dfs -du -h /tmp
```

```bash
hdfs dfs -du -h /user/spark
```

Identify which directories are consuming the most space.

---

## 3. Common Root Causes

### Cause 1 — Large datasets

A large amount of new data has been loaded into HDFS.

### Cause 2 — Old data

Old datasets are no longer required but were never removed.

### Cause 3 — Spark/application temporary data

Temporary or staging data is consuming significant space.

### Cause 4 — High replication factor

For example:

```text
1 TB logical data
Replication factor = 3

Approximate physical HDFS storage = 3 TB
```

Check replication:

```bash
hdfs fsck /path/to/file -files -blocks -locations
```

### Cause 5 — Non-HDFS files

Check the DataNode filesystem:

```bash
df -h
```

If OS-level usage is much higher than HDFS usage, investigate non-HDFS directories.

---

## 4. How to Fix

### Fix 1 — Remove obsolete data

First confirm:

* Data owner
* Retention policy
* Whether the data is still required
* Whether another copy exists

Then remove confirmed obsolete data:

```bash
hdfs dfs -rm -r /path/to/obsolete/data
```

**Never delete production data just because it is large.**

---

### Fix 2 — Clean unnecessary temporary data

Identify temporary data first:

```bash
hdfs dfs -du -h /tmp
```

Then follow your organization's approved cleanup process.

---

### Fix 3 — Reduce replication where appropriate

If business requirements permit a lower replication factor:

```bash
hdfs dfs -setrep -w 2 /path/to/data
```

Verify:

```bash
hdfs fsck /path/to/data -files -blocks -locations
```

**Do not reduce replication blindly.**

Replication provides data availability and fault tolerance.

---

### Fix 4 — Run HDFS Balancer

If the problem is uneven DataNode utilization:

```bash
hdfs dfsadmin -report
```

Then, according to your cluster's approved procedure:

```bash
hdfs balancer -threshold 10
```

The threshold should be selected according to the cluster's operational requirements.

---

### Fix 5 — Add DataNode capacity

If the cluster is genuinely running out of capacity and cleanup is insufficient, the long-term solution may be:

```text
Add disks
       OR
Add DataNodes
       OR
Increase storage capacity
```

---

## 5. Verification

After the fix:

```bash
hdfs dfs -df -h
```

Then:

```bash
hdfs dfsadmin -report
```

Check:

* DataNode utilization
* Remaining capacity
* HDFS health
* Under-replicated blocks

For a targeted check:

```bash
hdfs fsck /path/to/data
```

---

## 6. Production Prevention

* Monitor DataNode disk utilization.
* Maintain disk-usage alert thresholds.
* Implement data-retention policies.
* Clean temporary data regularly.
* Review replication policies.
* Perform capacity planning.
* Monitor non-HDFS disk usage.
* Monitor inode utilization.

---

## 7. Production Troubleshooting Flow

```text
Disk usage alert
       ↓
hdfs dfs -df -h
       ↓
hdfs dfsadmin -report
       ↓
Identify affected DataNode
       ↓
df -h / df -i
       ↓
Check HDFS directories
       ↓
Identify root cause
       ↓
Cleanup / replication / balancing / capacity expansion
       ↓
Verify HDFS health
```

---

## 8. Important Commands

```bash
hdfs dfs -df -h
hdfs dfsadmin -report
hdfs dfs -du -h /
hdfs fsck /path
hdfs dfs -setrep -w 2 /path
hdfs balancer -threshold 10
df -h
df -i
```

---

## 9. Interview Question

**Q: A DataNode is at 90% disk utilization. How will you troubleshoot it?**

First check HDFS capacity using `hdfs dfs -df -h` and `hdfs dfsadmin -report`. Then identify the affected DataNode and check OS-level disk usage with `df -h`. Next identify large HDFS directories, temporary data, replication overhead and non-HDFS usage. After identifying the root cause, clean only approved obsolete data, adjust replication if appropriate, balance the cluster, or add capacity. Finally verify the cluster health.
