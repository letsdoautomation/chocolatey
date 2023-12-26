# Chocolatey: Create Ryver software deployment package
### Documentation and download links

<b>Download:</b>

* [Ryver](https://ryver.com/downloads/)

<b>Documentation:</b>

* [Install-ChocolateyPackage](https://docs.chocolatey.org/en-us/create/functions/install-chocolateypackage)

<b>Objective: </b>

* Package and install Ryver

### Creating package

<b>Setup active setup:</b>

```powershell
$ErrorActionPreference = 'Stop'
$exeFileName = ''
$silentSwitch = ''
$tools = Split-Path -Parent $MyInvocation.MyCommand.Definition
[System.IO.FileInfo]$exe = Join-Path $tools -ChildPath $exeFileName

ni "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\runRyver" | New-ItemProperty -Name "StubPath" -Value ('REG ADD "HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce" /v runRyver /t REG_SZ /d "{0} {1}"' -f $exe, $silentSwitch)
```

<b>Silent install switch:</b>

```powershell
-s
```

<b>Package install command:</b>
```powershell
choco install ryver -s D:\Downloads -y
```

## Related videos

<b>Windows tools:</b>

[Download and install NuGet Package Explorer](https://youtu.be/94u9jDCpifM)

<b>Chocolatey:</b>

[Chocolatey: Creating empty choco package](https://youtu.be/grueS3wnRNw) <br />
[Chocolatey: Installing and basic commands](https://youtu.be/vEH7t5eqJq4)
