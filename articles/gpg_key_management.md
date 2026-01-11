# Managing GPG Keys for GitHub

*January 6, 2026*

If you're signing Git commits with GPG (and you should be), you'll eventually need to renew keys, add email addresses, or just figure out which key you're actually using. Here's everything you need to know.

## Finding Your Key ID

Before doing anything else, you need to know which key you're working with:

```bash
# List all your keys with IDs
gpg --list-secret-keys --keyid-format=long
```

The output looks like this:

```
sec   rsa4096/ABCD1234EFGH5678 2023-01-15 [SC] [expires: 2025-01-15]
      1234567890ABCDEF1234567890ABCDEF12345678
uid                 [ultimate] Your Name <your.email@example.com>
ssb   rsa4096/IJKL9012MNOP3456 2023-01-15 [E] [expires: 2025-01-15]
```

Your key ID is **ABCD1234EFGH5678** (the part after `rsa4096/`). The long hex string below is the full fingerprint, which also works anywhere a key ID is needed.

### Quick checks

```bash
# Which key is git using?
git config --global user.signingkey

# Shorter key listing
gpg --list-keys --keyid-format=short
```

If you have multiple keys, look for the one associated with your GitHub email address.

## Renewing an Expiring Key

When your GPG key is approaching expiration, you have two options: extend the existing key or create a new one. **Extending is almost always better** because it preserves your key's identity and doesn't break verification of past commits.

### Extending your existing key (recommended)

```bash
# Edit your key
gpg --edit-key YOUR_KEY_ID

# At the gpg> prompt:
gpg> expire
# Set new expiration (e.g., "2y" for 2 years, "0" for no expiration)

# Also extend subkeys:
gpg> key 1
gpg> expire
# Repeat for key 2, key 3, etc. if you have multiple subkeys

gpg> save
```

After extending, export the updated public key:

```bash
gpg --armor --export YOUR_KEY_ID
```

Copy the output and update GitHub:
- Go to Settings → SSH and GPG keys
- Find your existing GPG key and click **Edit**
- Paste the updated key (replacing the old one)

**Important**: Don't add it as a new key - update the existing one.

### Creating a new key

If your key is compromised or you want a fresh start:

```bash
# Generate new key (follow the prompts)
gpg --full-generate-key

# Export it
gpg --armor --export NEW_KEY_ID
```

Add to GitHub: Settings → SSH and GPG keys → New GPG key

Then update your git configuration:

```bash
git config --global user.signingkey NEW_KEY_ID
```

## Adding Multiple Email Addresses to a Key

If you commit from multiple email addresses (work and personal, different organizations, etc.), you can add multiple User IDs (UIDs) to a single GPG key.

```bash
# Edit your key
gpg --edit-key YOUR_KEY_ID

# Add a new UID
gpg> adduid
# Prompts:
# - Real name: (should match your existing name)
# - Email address: (your additional email)
# - Comment: (usually leave blank)

# Set trust level for the new UID
gpg> uid 2
# (selects the second UID - adjust number as needed)
gpg> trust
# Select 5 (ultimate trust) for your own email

gpg> save
```

After adding the UID, export and update your key on GitHub:

```bash
gpg --armor --export YOUR_KEY_ID
```

Update (don't add) your existing GPG key on GitHub with this output.

### Per-repository email configuration

GitHub will verify commits signed with your key for **any** email address in the key's UIDs. However, you still need to configure git to commit with the correct email:

```bash
# Global configuration
git config --global user.email your.main@email.com
git config --global user.signingkey YOUR_KEY_ID

# Or per-repository for different emails
cd /path/to/repo
git config user.email your.work@email.com
```

## Best Practices

### Backup your keys

Before making any changes:

```bash
# Export private key
gpg --export-secret-keys YOUR_KEY_ID > ~/private-key-backup.gpg

# Store this securely! Consider encrypting it further or using a password manager
```

### Set reasonable expiration dates

Keys should expire. This forces periodic review and limits damage if a key is compromised. Set expirations to 1-2 years:

- Long enough to avoid constant renewal
- Short enough to prompt security hygiene reviews

You can always extend before expiration.

### Test your configuration

After any changes, verify everything works:

```bash
# Make a test commit
echo "test" > test.txt
git add test.txt
git commit -S -m "Test signed commit"

# Verify the signature
git log --show-signature -1
```

Push to GitHub and check that the commit shows as "Verified" with a green badge.

## Troubleshooting

### "gpg: signing failed: No secret key"

Your git signing key doesn't match any key in your keyring:

```bash
# Check what git is configured to use
git config user.signingkey

# Check what keys you have
gpg --list-secret-keys
```

Set the correct key:

```bash
git config --global user.signingkey YOUR_CORRECT_KEY_ID
```

### GitHub shows "Unverified" commits

Common causes:

1. **Email mismatch**: The email in your commit doesn't match any UID in your GPG key
2. **Key not uploaded**: GitHub doesn't have your public key
3. **Key expired**: Your key or subkeys have expired
4. **Wrong key on GitHub**: You updated your local key but didn't update GitHub

Solution: Export your current key and update it on GitHub.

### Multiple keys causing confusion

List all keys and their associated emails:

```bash
gpg --list-keys
```

Delete unused keys:

```bash
gpg --delete-secret-key KEY_ID
gpg --delete-key KEY_ID
```

---

*This article was drafted with assistance from Claude.*
