
# Task 5: Firewall Configuration using UFW

## Step 1: Install UFW

```bash
sudo apt update
sudo apt install ufw -y
```

Verify:

```bash
sudo ufw status
```

Expected:

```text
Status: inactive
```

---

# Step 2: Find Your Public IP Address

On your **local computer** (not the EC2 instance), run:

```bash
curl ifconfig.me
```

Example:

```text
49.207.15.100
```

We'll use this IP in the SSH rule.

---

# Step 3: Allow SSH Only from Your IP

Replace `<YOUR_PUBLIC_IP>` with your IP:

```bash
sudo ufw allow from <YOUR_PUBLIC_IP> to any port 22 proto tcp
```

Example:

```bash
sudo ufw allow from 49.207.15.100 to any port 22 proto tcp
```

Check:

```bash
sudo ufw status numbered
```

Expected:

```text
22/tcp ALLOW IN 49.207.15.100
```

---

# Step 4: Allow HTTP (Port 80)

```bash
sudo ufw allow 80/tcp
```

---

# Step 5: Allow Docker Application (Port 8000)

```bash
sudo ufw allow 8000/tcp
```

---

# Step 6: Enable UFW

```bash
sudo ufw enable
```

Type:

```text
y
```

Verify:

```bash
sudo ufw status verbose
```

Expected:

```text
Status: active

22/tcp     ALLOW IN    49.207.15.100
80/tcp     ALLOW IN    Anywhere
8000/tcp   ALLOW IN    Anywhere
```

---

# Step 7: Test the Rules

From your local machine:

SSH:

```bash
ssh -i key.pem ubuntu@<EC2-PUBLIC-IP>
```

Open your browser:

```text
http://<EC2-PUBLIC-IP>
```

and

```text
http://<EC2-PUBLIC-IP>:8000
```

Both should work.

---

# Step 8: Verify Firewall Rules

```bash
sudo ufw status numbered
```

Example:

```text
Status: active

[1] 22/tcp    ALLOW IN    49.207.15.100
[2] 80/tcp    ALLOW IN    Anywhere
[3] 8000/tcp  ALLOW IN    Anywhere
```

---

# Assignment Checklist

| Requirement                | Command                                                         |
| -------------------------- | --------------------------------------------------------------- |
| Install UFW                | `sudo apt install ufw -y`                                       |
| Allow SSH from specific IP | `sudo ufw allow from <YOUR_PUBLIC_IP> to any port 22 proto tcp` |
| Allow HTTP                 | `sudo ufw allow 80/tcp`                                         |
| Allow Port 8000            | `sudo ufw allow 8000/tcp`                                       |
| Enable Firewall            | `sudo ufw enable`                                               |
| Verify Rules               | `sudo ufw status numbered`                                      |

---

