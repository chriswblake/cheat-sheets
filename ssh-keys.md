## Create SSH key locally and install in remote machine

1. On client, navigate to the ssh folder.
   ```
   cd ~/.ssh
   ```
1. Generate a key pair, public and private. Make sure to specify a unique name to allow ids for connecting to multiple devices.
   ```
   ssh-keygen
   ```
1. Add the key to the ssh agent, so it is used when attempting to connect.
   ```
   ssh-add ~/.ssh/key-file-name
   ```
1. Copy to the target server.
   ```
   ssh-copy-id username@remote-host
   ```
1. Verify by trying to connect to the remote host.
   ```
   ssh user@remote-host -i ~/.ssh/key-file-name
   ```

## Reference

```
# List keys known by ssh-agent
ssh-add -l
```
