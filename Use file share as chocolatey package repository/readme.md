# Chocolatey: Use file share as chocolatey package repository
### Documentation and download
Documentation link:

* [Documentation](https://docs.chocolatey.org/en-us/features/host-packages#local-folder-unc-share-cifs)

<b>Objectives:</b>

* Create 'Packages' network share
    * add 'Everyone' or 'Authenticated Users' with READ permissions
* Add packages to network share
    * [Firefox](https://community.chocolatey.org/packages/Firefox)
    * [Zoom](https://community.chocolatey.org/packages/zoom)
    * [GoogleChrome](https://community.chocolatey.org/packages/GoogleChrome)
    * [chocolatey-core.extension](https://community.chocolatey.org/packages/chocolatey-core.extension)
    * [chocolatey-compatibility.extension](https://community.chocolatey.org/packages/chocolatey-compatibility.extension)
    * [All other packages](https://community.chocolatey.org/packages)

Add network share to repository list:
```powershell
choco source add -n=local -s'\\srv02\packages'
```

List available packages in network share:
```powershell
choco search -s local
```

Install package from network share:
```powershell
choco install GoogleChrome -s local -y
```