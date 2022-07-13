List all packages
```
Get-AppxPackage -AllUsers | Select Name, PackageFullName
```


Remove a package
```
get-appxpackage *xbox* | remove-appxpackage
```