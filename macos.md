### Clear iCloud local cache of files
```
brctl evict *
```

### Keep computer awake for 60sec
```
caffeinate -t 60
```

###  Scan IPs and MAC addresses
```
sudo nmap -sn 192.168.1.0/24
```

### Speed test
```
networkQuality
```

### Copy/paste clipboard
```
echo "Copy to clipboard" | pbcopy
pbpaste
```

### Time
```
#UTC
date -u
# Unix Timestamp
date +%s
```