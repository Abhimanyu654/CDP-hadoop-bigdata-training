# DataNode Down / DataNode Unhealthy

## 1. Production Issue

A DataNode is reported as **Dead**, **Down**, or **Unhealthy**.

Example:

```text
Live DataNodes: 31
Dead DataNodes: 1
```

or Cloudera Manager reports a DataNode health alert.

---

## 2. How to Identify the Issue

### Step 1: Check HDFS DataNodes

```bash
hdfs dfsadmin -report
```

Look for:

```text
Live datanodes
Dead datanodes
Decommissioned datanodes
```

---

### Step 2: Identify the affected DataNode

Note:

```text
Hostname
IP address
Last contact
Configured capacity
DFS used
DFS remaining
```

---

### Step 3: Check whether the server is reachable

```bash
ping <datanode-hostname>
```

Check SSH:

```bash
ssh <datanode-hostname>
```

If SSH is unavailable, investigate the server/infrastructure layer.

---

### Step 4: Check server health

On the DataNode:

```bash
hostname
uptime
df -h
df -i
free -h
```

Check CPU:

```bash
top
```

---

### Step 5: Check Cloudera Agent

```bash
sudo systemctl status cloudera-scm-agent
```

Check recent agent messages:

```bash
sudo journalctl -u cloudera-scm-agent --since "30 minutes ago"
```

---

### Step 6: Check DataNode process

```bash
jps
```

Look for:

```text
DataNode
```

If the DataNode process is missing, investigate why it stopped.

---

## 3. Common Root Causes

### Infrastructure

* Server rebooted.
* Server crashed.
* Hardware failure.
* Disk failure.

### Operating system

* Filesystem full.
* Inode exhaustion.
* High memory pressure.
* OS problem.

### Hadoop

* DataNode process stopped.
* Java/JVM problem.
* DataNode configuration issue.

### Network

* DataNode cannot communicate with NameNode.
* DNS issue.
* Firewall/network issue.

### Security

* Kerberos authentication failure.
* Incorrect keytab.
* Expired credentials.

---

## 4. How to Fix

### Case 1 — DataNode process stopped

Check:

```bash
jps
```

Review DataNode logs and Cloudera Manager health information.

If the service is managed by Cloudera Manager, use Cloudera Manager's **Start** action for the DataNode role according to your operational procedure.

---

### Case 2 — Filesystem is full

Check:

```bash
df -h
```

Check inode usage:

```bash
df -i
```

Identify the cause before removing anything.

---

### Case 3 — Disk failure

Check disks:

```bash
lsblk
```

Check kernel messages:

```bash
dmesg | grep -Ei "error|fail|I/O|disk"
```

Follow your infrastructure/hardware replacement procedure.

---

### Case 4 — Network issue

Check hostname resolution:

```bash
hostname -f
getent hosts <namenode-hostname>
```

Test connectivity to the required Hadoop service port:

```bash
nc -vz <namenode-hostname> <port>
```

Use the actual configured port for your environment.

---

### Case 5 — Kerberos problem

Check:

```bash
klist
```

Test the configured keytab:

```bash
kinit -kt /path/to/keytab <principal>
```

Then:

```bash
klist
```

---

## 5. Check DataNode Logs

Search for errors:

```bash
grep -i "error" <datanode-log-file>
```

Disk problems:

```bash
grep -Ei "disk|space|volume|failed" <datanode-log-file>
```

Kerberos problems:

```bash
grep -Ei "kerberos|authentication|GSS|SASL" <datanode-log-file>
```

---

## 6. Verification

Run:

```bash
hdfs dfsadmin -report
```

The DataNode should return to the **Live DataNodes** list.

Also check:

```bash
hdfs fsck /
```

Look for replication problems.

Verify the DataNode health in Cloudera Manager.

---

## 7. Production Prevention

* Monitor DataNode heartbeat.
* Monitor disk health.
* Monitor disk and inode utilization.
* Monitor JVM health.
* Maintain sufficient disk headroom.
* Monitor network connectivity.
* Maintain correct Kerberos configuration.

---

## 8. Troubleshooting Flow

```text
DataNode Down Alert
        ↓
hdfs dfsadmin -report
        ↓
Identify DataNode
        ↓
Can server be reached?
        ↓
Check df / df -i / memory
        ↓
Check Cloudera Agent
        ↓
Check DataNode process
        ↓
Check logs
        ↓
Network / Disk / OS / Hadoop / Kerberos
        ↓
Fix root cause
        ↓
Verify DataNode returns to service
```

---

## 9. Interview Question

**Q: A DataNode suddenly becomes dead. What will you check?**

I would first confirm the DataNode status with `hdfs dfsadmin -report`, then check server availability, disk and inode usage, Cloudera Agent, DataNode process, network connectivity, logs and Kerberos. I would identify the root cause before restarting anything and then verify the DataNode and HDFS replication health after recovery.

