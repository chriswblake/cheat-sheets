### Rename master to main

git branch -m master main
git fetch origin
git branch -u origin/main main
git remote set-head origin -a


### Merge 2 repositories

```bash
# Go to repo that will receive merge
cd path/to/repo1

# Add remote to repo to be pulled/merged
git remote add repo2 /path/to/repo2
git fetch repo2 --tags

# merge repo2 into repo1. Repeat for each branch
git merge --allow-unrelated-histories repo2/master
git merge --allow-unrelated-histories repo2/main
git merge --allow-unrelated-histories repo2/develop

# Remove reference to the merged repo. Delete it once confirmed.
git remote remove project-a
```


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

```
python git-filter-repo --path-glob '*.secrets' --invert-paths --force
python git-filter-repo
```
> Reference: This is a python tool. If not installed in $PATH, it has to be called directuly using python.
https://github.com/newren/git-filter-repo

### Remove local copies of removed remote branches (origin/*)
```
git fetch --prune
```

### Create blank branch
```
git checkout --orphan <branch name>
```

### Copy all branches from one remote to anothe (origin to target)
```
git push target 'refs/remotes/origin/*:refs/heads/*'
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

### Git LFS - Setup and track

1. Install on machine (if needed)
brew install git-lfs
1. Install for user (if needed)
git lfs install
1. Track a file type with LFS.
git lfs track "*.psd"
1. Ensure the .gitattributes file is tracked (for future clones).
git add .gitattributes

Reference: https://git-lfs.com/

### Git LFS  - Migrate existing repo

git lfs migrate import --include="*.zip"
git lfs migrate import --include="*.zip" --include-ref=refs/heads/master --include-ref=refs/heads/my-feature

git push --force


Reference: https://github.com/git-lfs/git-lfs/wiki/Tutorial#migrating-existing-repository-data-to-lfs

References: https://github.com/git-lfs/git-lfs/blob/main/docs/man/git-lfs-migrate.adoc?utm_source=gitlfs_site&utm_medium=doc_man_migrate_link&utm_campaign=gitlfs


### Run github actions locally (with ACT)

Trigger types
```bash

act pull_request -W .github/workflows/workflow-name.yml --secret-file .secrets.act

act tags -W .github/workflows/workflow-name.yml --secret-file .secrets.act 

act repository_dispatch -W .github/workflows/workflow-name.yml  --secret-file .secrets.act --eventpath webhook_payload.json -p=false 

```

Examples
```
act repository_dispatch -W .github/workflows/send-notification.yml --actor chriswblake --secret-file .secrets.act --eventpath webhook_payload.json -p=false 
act pull_request -W .github/workflows/build-and-publish.yml --secret-file .secrets.act 
```

Pick user
```
--actor chriswblake
```
Add Payload
```
--eventpath payload.json
```
Set Environment Variables
```
--env-file .env.local
```
Set Secrets
```
--secret-file .secrets.act
```
Disables force pull of images
```
-p=false
```