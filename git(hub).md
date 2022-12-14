

### Username/Email
```bash
# Global
git config --global user.name "Christopher W. Blake"
git config --global user.email "chriswblake@gmail.com"
git config --global user.email "christopher.blake@bakerhughes.com"

# Per Repo
git config user.name "Christopher W. Blake"
git config user.email "chriswblake@gmail.com"
```

### Global gitignore (macos)
```
git config --global core.excludesfile ~/.gitignore
```

```
# Folder view configuration files
.DS_Store
Desktop.ini

# Thumbnail cache files
._*
Thumbs.db

# Files that might appear on external disks
.Spotlight-V100
.Trashes
```

### Permanent delete file in Git Repository
```
git filter-repo --path-glob '*.mp4' --invert-paths --force
git push origin --force --all
```

### Remove local copies of removed remote branches (origin/*)
```
git fetch --prune
```

### Create blank branch
```
git checkout --orphan <branch name>
```

### Ignore changes to tracked file
```
git update-index --skip-worktree <file>
```

### Logs - Audit Org
```
# By User
actor:chriswblake

# By Repo
repo:my-org/our-repo
```

Reference: https://docs.github.com/en/organizations/keeping-your-organization-secure/managing-security-settings-for-your-organization/reviewing-the-audit-log-for-your-organization