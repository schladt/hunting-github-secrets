# Hunting GitHub Secrets Across Time and Space

Welcome to the interactive Git tutorial puzzle! In this repo, we’ve hidden a real secret flag "deep" (okay, not maybe not-so-deep) in the history of a branch this respositiory. All visible files should show only a redacted placeholder `FLAG{****}`.

Your mission: **Locate the one commit that contains the true flag `FLAG{####}` — where #### == 1234**.

---

## 1. Explore branches

Clone the repo and fetch all branches:

```bash
git clone https://github.com/your-username/hunting-github-secrets.git
cd hunting-github-secrets

# pull down all branches locally
git fetch --all

# List all branches (local & remote)
git branch -a
```

## 2. Verify no real flag in current files

Search the latest content for any flag:

```bash
git grep "FLAG{"
```

All matches will show FLAG{****}. No 1234 appears here.

## 3. Search the full commit history

To search every commit in every branch for occurrences of `FLAG{`, use:

```bash
git grep "FLAG{" $(git rev-list --all)
```

This lists every commit and file containing `FLAG{`, including redacted placeholders.

## 3.1 Narrow to real flag

Since placeholders are always FLAG{****}, filter them out:

```bash
git grep "FLAG{" $(git rev-list --all) | grep -v "FLAG{\*\*\*\*}"
```

Alternatively, use a regex for four digits:
```bash
git grep -E "FLAG\{[0-9]{4}\}" $(git rev-list --all)
```
This should return exactly one line

## 4. Inspect the hidden commit

Once you have the commit SHA (e.g., abc123def456), view its contents:

```bash
git show abc123def456
git show abc123def456:docs/secret.md
```
You will see the real flag in context.

