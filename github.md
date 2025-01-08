## Organization
### Audit Logs
```
# By User
actor:chriswblake

# By Repo
repo:my-org/our-repo
```

Reference: https://docs.github.com/en/organizations/keeping-your-organization-secure/managing-security-settings-for-your-organization/reviewing-the-audit-log-for-your-organization


## Actions

### Show full context
```yml
steps:
- name: Show context
    run: echo "${{ toJSON(github) }}"
```

### Show base branch
```yml
- name: Show base branch
    run: echo "${{ github.event.base_ref }}"
```

### Auto restart self-hosted runners

1. Follow the usual process for adding a self-hosted runner to an organization.

   1. Navigate to the organization.
   1. Select **Settings** tab.
   1. In left navigation, select **Actions** and **Runners**
   1. Select green **New runner** button.
   1. Follow prompts.

1. Open the CRON file.
   ```bash
   sudo vim /etc/crontab
   ```
1. Add an entry at the end to restart the runner after reboot.
   - replace `ubuntu` with the username the runner was installed on.
   - adjust the path if the default was not used.
   ```cron
   @reboot ubuntu ~/actions-runner/run.sh
   ```
1. Confirm the file contents
   ```bash
   cat /etc/crontab
   ```
1. Reboot the machine
   ```
   sudo reboot
   ```
1. Confirm the runner is idle on GitHub.
