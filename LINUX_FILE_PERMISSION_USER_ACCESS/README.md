
# Step 1: Create a dedicated monitoring user

```bash
sudo adduser monitoruser
```

Set a password when prompted.

Verify:

```bash
id monitoruser
```

Example:

```text
uid=1001(monitoruser) gid=1001(monitoruser) groups=1001(monitoruser)
```

---

# Step 2: Create the monitoring directory

```bash
sudo mkdir -p /opt/container-monitor/logs
```

Verify:

```bash
ls -ld /opt/container-monitor
```

---

# Step 3: Assign ownership

Give ownership to the monitoring user:

```bash
sudo chown -R monitoruser:monitoruser /opt/container-monitor
```

Verify:

```bash
ls -ld /opt/container-monitor
```

Expected:

```text
drwxr-xr-x 3 monitoruser monitoruser 4096 Jul 11 18:30 /opt/container-monitor
```

---

# Step 4: Give full access to the monitoring user

```bash
sudo chmod -R 700 /opt/container-monitor
```

This means:

* **7** → Owner: read, write, execute
* **0** → Group: no access
* **0** → Others: no access

Verify:

```bash
ls -ld /opt/container-monitor
```

Expected:

```text
drwx------ 3 monitoruser monitoruser 4096 Jul 11 18:30 /opt/container-monitor
```

---

# Step 5: Move the monitoring script (if needed)

Your structure should look like this:

```text
/opt/container-monitor/
├── monitor.sh
└── logs/
    └── container_usage.log
```

If your script is still in `logs/`:

```bash
sudo mv /opt/container-monitor/logs/monitor.sh /opt/container-monitor/
```

Update ownership again:

```bash
sudo chown -R monitoruser:monitoruser /opt/container-monitor
```

---

# Step 6: Verify access control

Switch to the monitoring user:

```bash
su - monitoruser
```

List the directory:

```bash
ls -l /opt/container-monitor
```

You should be able to access it.

Exit back to root:

```bash
exit
```

Now test with another user.

Create a test user:

```bash
sudo adduser testuser
```

Switch to it:

```bash
su - testuser
```

Try to access the monitoring directory:

```bash
cd /opt/container-monitor
```

Expected:

```text
bash: cd: /opt/container-monitor: Permission denied
```

or

```bash
ls /opt/container-monitor
```

Expected:

```text
ls: cannot open directory '/opt/container-monitor': Permission denied
```

Exit:

```bash
exit
```

---

# Step 7: Verify permissions

Run:

```bash
ls -ld /opt/container-monitor
```

Example output:

```text
drwx------ 3 monitoruser monitoruser 4096 Jul 11 18:30 /opt/container-monitor
```

Check ownership:

```bash
stat /opt/container-monitor
```

---

# Final Structure

```text
/opt/container-monitor/
├── monitor.sh
└── logs/
    └── container_usage.log
```

Permissions:

```text
Owner : monitoruser
Group : monitoruser
Permissions : drwx------
```

---

## Assignment Checklist

| Requirement                     | Command                                                              |
| ------------------------------- | -------------------------------------------------------------------- |
| Create dedicated user           | `sudo adduser monitoruser`                                           |
| Create `/opt/container-monitor` | `sudo mkdir -p /opt/container-monitor/logs`                          |
| Assign ownership                | `sudo chown -R monitoruser:monitoruser /opt/container-monitor`       |
| Full access for monitoring user | `sudo chmod -R 700 /opt/container-monitor`                           |
| Restrict access for others      | `chmod 700`                                                          |
| Verify access control           | `su - monitoruser`, `su - testuser`, `ls -ld /opt/container-monitor` |

