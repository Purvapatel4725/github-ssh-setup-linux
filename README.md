# GitHub SSH Authentication on Linux (No More Password Prompts)

This guide explains how to set up **SSH authentication for GitHub on Linux** so Git **never asks for your username or password again** when pushing or pulling code.

It is beginner-friendly, repeatable, and works for all modern Linux distributions.

## ✅ Why Use SSH Instead of HTTPS?

- No password required for every `git push`
- More secure than HTTPS passwords
- Works seamlessly with automation and CI
- GitHub officially recommends SSH keys

## 📋 Prerequisites

- Linux terminal access
- Git installed
- A GitHub account

## 1️⃣ Check If You Already Have an SSH Key

Open a terminal and run:

```bash
ls ~/.ssh
````

If you see files like:

* `id_ed25519` and `id_ed25519.pub`
* or `id_rsa` and `id_rsa.pub`

You already have an SSH key → skip to **Step 3**.

If the directory is empty or doesn’t exist, continue to Step 2.

## 2️⃣ Generate a New SSH Key (Recommended)

### Preferred (modern & secure):

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

### Fallback (older systems):

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

When prompted:

* **File location** → press **Enter**
* **Passphrase** → optional but recommended

This creates:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

## 3️⃣ Start the SSH Agent and Add Your Key

Start the SSH agent:

```bash
eval "$(ssh-agent -s)"
```

Add your SSH key:

```bash
ssh-add ~/.ssh/id_ed25519
```

(If using RSA, replace with `id_rsa`)

This ensures Git can use your key without prompting.

## 4️⃣ Add the SSH Key to GitHub

Copy your **public key**:

```bash
cat ~/.ssh/id_ed25519.pub
```

Then:

1. Go to **GitHub**
2. Open **Settings → SSH and GPG keys**
3. Click **New SSH key**
4. Paste the copied key
5. Save

## 5️⃣ Test the SSH Connection

Run:

```bash
ssh -T git@github.com
```

Expected output:

```text
Hi <username>! You've successfully authenticated.
```

If you see this, SSH is working correctly.

## 6️⃣ Ensure Your Repository Uses SSH (Important)

Check your Git remote:

```bash
git remote -v
```

If you see `https://github.com/...`, switch it to SSH:

```bash
git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
```

Verify again:

```bash
git remote -v
```

You should now see:

```text
git@github.com:USERNAME/REPOSITORY.git
```

## ✅ Result: Password-Free Git

From now on:

```bash
git push
git pull
git commit
```

✔ No password
✔ No username
✔ Secure SSH authentication

## 🔄 Optional: Auto-Load SSH Key on Login

To avoid re-adding your key after reboot, add this to `~/.bashrc` or `~/.zshrc`:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## 🛠 Troubleshooting

**Permission denied (publickey)?**

* Make sure your key is added to GitHub
* Ensure SSH agent is running
* Verify remote URL uses `git@github.com`

**Multiple GitHub accounts?**

* Use separate SSH keys and an SSH config file

## 👤 Author

**Purva Patel**
