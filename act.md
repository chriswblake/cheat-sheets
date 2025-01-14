## My Typical Act Commands

```bash
act pull_request -W .github/workflows/my-workflow.yml
```

```bash
act push -W .github/workflows/my-workflow.yml
```

```bash
act tags -W .github/workflows/my-workflow.yml
```

Use a different event file

```bash
act pull_request -W .github/workflows/my-workflow.yml -e .actrc.event-1.json
```

## My Typical Act Config

Put these files in the project's root folder, or wherever `act` will be ran from.

### .actrc

```bash
# Load event payload from file
-e .actrc.event.json

# Load workflow inputs from file
--input-file .actrc.inputs

# Load secrets and environment variables from file
--var-file .actrc.vars
--secret-file .actrc.secrets

# Cache used Actions
--action-offline-mode

# Run as AMD64 if on ARM64
--container-architecture linux/amd64
```

### .actrc.vars

```env
VAR1=(...)
VAR2=(...)
```

### .actrc.secrets

```env
GITHUB_TOKEN=(...)
```

### .actrc.inputs

```json
{
  "inputs": {
    "var-1": "Hello",
    "var-2": "World"
  }
}
```

## Event Scenarios

Typical content for `.actrc.event.json`.

### Push branch

```json
{
  "ref": "refs/heads/my-feature"
}
```

### Push tag

```json
{
  "base_ref": "refs/heads/main",
  "ref": "refs/tags/v1.0.0"
}
```

### Pull request

```json
{
  "base_ref": "refs/heads/main",
  "ref": "refs/pull/123/merge"
}
```

## Reference

### Predownload runner images

ref: https://nektosact.com/usage/runners.html#runners

Generally the medimum size is good.
However, there are some advanced workflows that require the large image.

```bash
# Large
docker pull act -P ubuntu-latest=-self-hosted
# Medium
docker pull catthehacker/ubuntu:act-latest
```

```bash
act -P ubuntu-latest=catthehacker/ubuntu:full-latest
```

### Config file location

ref: https://nektosact.com/usage/index.html#configuration-file

When act runs it will load command arguments from the following locations

- `~/.config/act/actrc`
- `~/.actrc`
- `<current-folder>/.actrc`

Any of the CLI flags can be added to the file.
It just appends them when being ran.

### Run as AMD64 on ARM64

```bash
act push --container-architecture linux/amd64
```

### Skip running in container. Run directly on host.

```bash
act -P ubuntu-latest=-self-hosted
act -P windows-latest=-self-hosted
act -P macos-latest=-self-hosted
```

### Use cached actions (to speed things up)

ref: https://nektosact.com/usage/index.html#action-offline-mode

```bash
act --action-offline-mode
```

### Change user

```
act --actor chriswblake
```

### Supress debug output

```
act | grep -v '::'
```
