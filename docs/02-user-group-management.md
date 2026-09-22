# Module 2: User & Group Management

## Objective
Set up realistic role-based access: separate groups for different
responsibilities, and sudo policy scoped to what each role actually needs —
rather than every user having identical, ungoverned access.

## Prerequisites
- Module 1 complete (base system updated, SSH access working)

## Steps

### 1. Create groups for different roles

```bash
sudo groupadd sysadmins
sudo groupadd devteam
```

### 2. Create users and assign to groups

```bash
sudo adduser maureenchege
sudo usermod -aG sysadmins maureenchege

sudo adduser jdoe
sudo usermod -aG devteam jdoe
```

### 3. Grant scoped sudo access via /etc/sudoers.d

```bash
sudo visudo -f /etc/sudoers.d/sysadmins
```
```
%sysadmins ALL=(ALL) ALL
```

This grants full sudo access to anyone in the `sysadmins` group — no per-user sudoers entry
needed. `devteam` intentionally has no matching rule, so its members get no elevated access
by default.

### 4. Review permissions

```bash
groups maureenchege
groups jdoe
sudo -l -U maureenchege
sudo -l -U jdoe
```

## Verification

Confirmed the group-based permission model actually works end to end by comparing two accounts:

| User | Group | `sudo -l -U <user>` result |
|------|-------|------------------------------|
| `maureenchege` | `sysadmins` | `(ALL) ALL` — full sudo access |
| `jdoe` | `devteam` | "not allowed to run sudo on ubuntu-server" — no sudo access |

This is the core proof of the module: permissions are granted to the **group**, and any user's
access changes the moment their group membership changes — no per-user sudoers entry required.

## Issues & Troubleshooting

- **Typo'd `getent group dev team`** (with a space) instead of `devteam` — Linux read it as two
  separate arguments and returned nothing useful. Fixed by removing the space.
- **`sudo -aG sysadmins <user>` failed with "invalid option provided"** — forgot the actual
  `usermod` command; `-aG` is a flag *for* `usermod`, not something `sudo` understands directly.
  Correct form: `sudo usermod -aG sysadmins <username>`.
- **`jdoe` initially showed `(ALL) ALL` unexpectedly** — turned out `jdoe` had been added to
  `sysadmins` during an earlier test before `maureenchege` was created, so this wasn't a bug, just
  a leftover from testing. Used `sudo gpasswd -d jdoe sysadmins` to remove them from `sysadmins`
  and `sudo usermod -aG devteam jdoe` to move them to `devteam` instead, to get a clean before/after
  comparison.
- Repeated lowercase-L vs. capital-i mix-up from Module 1 (`sudo -l -U` vs `-I`) — same font
  rendering issue in the terminal, resolved by typing carefully.

## Key Takeaways
Group membership — not individual configuration — is what actually determines a user's access in
a well-run Linux environment. Granting a role's permissions once, at the group level, and adding
users into (or out of) that group is exactly how real teams manage access at scale: change one
group's `sudoers.d` rule, and it applies to everyone in that group immediately, without touching
each person's account individually. Removing `jdoe` from `sysadmins` and confirming their sudo
access disappeared instantly was a strong, concrete illustration of this in a real, hands-on demo.
