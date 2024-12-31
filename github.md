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