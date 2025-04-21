

### Temporarily adjust permissions

Temporarily overwrite the codespace token with GitHub CLI token.
This will clear once the terminal session ends or the codespace restarts.

```bash
unset GITHUB_TOKEN
GITHUB_TOKEN=$(gh auth token)
```


### Kill a background process

This is useful if you had the debugger running and then closed the window.
For example, with a web service active.

1. Get the list of running services on our port. Make note of the PID numbers.

```bash
lsof -i :8000
```

1. Kill the processes. Uses the process IDs (PID) from the previous step.
```bash
kill -9 41497 46078
```
