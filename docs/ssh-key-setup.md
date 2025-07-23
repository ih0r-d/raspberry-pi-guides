# 🔐 SSH Key Authentication for Raspberry Pi (Ubuntu Server)

This guide helps you configure **SSH key-based login** to your Raspberry Pi — so you never have to type your password again.

---

## ✅ Assumptions

- Raspberry Pi is running Ubuntu Server
- You can access it via SSH with login/password
- You have a Mac or Linux machine as your local device

---

## 1️⃣ Generate SSH Key (on your Mac)

```bash
ssh-keygen -t ed25519 -C "raspberry-pi"
```

Just press `Enter` through all prompts to use the default path: `~/.ssh/id_ed25519`

---

## 2️⃣ Copy Your Public Key to Raspberry Pi

Replace `<user>` and `<ip>` accordingly:

```bash
ssh-copy-id <user>@<raspberry_pi_ip>
```

Example:

```bash
ssh-copy-id ihord@192.168.0.150
```

You will be asked for your password **one last time**.

---

### ❗ ssh-copy-id not found?

Use this alternative:

```bash
cat ~/.ssh/id_ed25519.pub | ssh <user>@<raspberry_pi_ip> "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

---

## 3️⃣ Test Login (No Password Prompt)

```bash
ssh <user>@<raspberry_pi_ip>
```

✅ You should log in **without being asked for a password**

---

## 4️⃣ (Optional but Recommended) Disable Password Login

Once the key login works — disable password authentication:

```bash
sudo nano /etc/ssh/sshd_config
```

Set or edit the following lines:

```
PasswordAuthentication no
PermitRootLogin no
```

Then restart SSH:

```bash
sudo systemctl restart ssh
```

⚠️ Make sure SSH key login works before doing this — or you could lock yourself out.

---

## ✅ Done

- You can now log in securely without a password
- Safer, faster, and more convenient!
