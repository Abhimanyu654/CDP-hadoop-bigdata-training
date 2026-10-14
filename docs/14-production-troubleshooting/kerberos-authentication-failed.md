# Kerberos Authentication Failed

## 1. Production Issue

A Hadoop command, service, application or user receives a Kerberos authentication error.

Common examples:

```text
kinit: Password incorrect
Client not found in Kerberos database
GSSException
SASL authentication failed
Authentication failed
No valid credentials provided
```

---

## 2. How to Identify

### Step 1 — Check current ticket

```bash
klist
```

If there is no valid ticket, obtain one:

```bash
kinit <principal>
```

Then:

```bash
klist
```

---

## 3. Test Keytab Authentication

For a service account:

```bash
kinit -kt /path/to/service.keytab <principal>
```

Then:

```bash
klist
```

If `kinit -kt` fails, investigate the principal/keytab/KDC configuration.

---

## 4. Check Keytab Contents

```bash
klist -kte /path/to/service.keytab
```

Check:

* Principal
* Realm
* Key version number
* Encryption types

Make sure the expected principal exists.

---

## 5. Check Hostname

Kerberos principals are often hostname-sensitive.

Run:

```bash
hostname
```

```bash
hostname -f
```

Check DNS:

```bash
getent hosts $(hostname -f)
```

---

## 6. Check Time Synchronization

Kerberos authentication depends on synchronized system clocks.

Check:

```bash
date
```

```bash
timedatectl
```

If chrony is used:

```bash
chronyc tracking
```

---

## 7. Common Root Causes

### Expired ticket

The user's Kerberos ticket has expired.

### Wrong password

User authentication fails during `kinit`.

### Incorrect principal

The principal does not match the expected account.

### Incorrect keytab

The keytab is missing, outdated or contains the wrong principal.

### DNS/hostname mismatch

The hostname doesn't match the service principal configuration.

### Clock synchronization

The server's time differs too much from the KDC.

### KDC unavailable

The Kerberos KDC cannot be reached.

---

## 8. How to Fix

### Fix 1 — Renew user ticket

```bash
kdestroy
```

Then:

```bash
kinit <principal>
```

Verify:

```bash
klist
```

---

### Fix 2 — Test service keytab

```bash
kinit -kt /path/to/service.keytab <principal>
```

Then:

```bash
klist
```

If this fails, verify the principal and keytab with the Kerberos/Hadoop administration team.

---

### Fix 3 — Correct time synchronization

Check:

```bash
timedatectl status
```

```bash
chronyc tracking
```

Correct time synchronization according to the organization's standard.

---

### Fix 4 — Check hostname/DNS

```bash
hostname -f
```

```bash
getent hosts <hostname>
```

Correct DNS/hostname configuration if it doesn't match the deployed Kerberos service principal configuration.

---

### Fix 5 — Check KDC connectivity

Check network connectivity to the configured KDC according to your environment.

Do not assume a port or modify firewall rules without confirming the actual Kerberos configuration.

---

## 9. Verify Hadoop Access

After obtaining a valid ticket:

```bash
klist
```

Then test:

```bash
hdfs dfs -ls /
```

If successful, the user's Hadoop authentication is working.

For a service, test the actual affected service/application as well.

---

## 10. Production Prevention

* Monitor KDC availability.
* Maintain DNS consistency.
* Maintain time synchronization.
* Protect keytabs.
* Document service principals.
* Follow keytab rotation procedures.
* Monitor authentication failures.
* Maintain correct service configurations.

---

## 11. Troubleshooting Flow

```text
Kerberos Error
      ↓
klist
      ↓
Valid ticket?
      ↓
NO → kinit
      ↓
YES → Check principal
      ↓
Keytab problem?
      ↓
klist -kte
      ↓
Check hostname/DNS
      ↓
Check time synchronization
      ↓
Check KDC connectivity
      ↓
Test Hadoop operation
```

---

## 12. Interview Question

**Q: A Hadoop user suddenly gets a Kerberos authentication failure. What will you check?**

I would first run `klist` to check the current ticket. If necessary, I would obtain a new ticket using `kinit`. For service authentication, I would test the keytab using `kinit -kt` and inspect it with `klist -kte`. I would then verify the principal, hostname/DNS, time synchronization and KDC connectivity before testing the affected Hadoop operation again.
