# Chocolatey: Create Brave software deployment package
### Documentation and download links

<b>Download:</b>

* [Brave](https://github.com/brave/brave-browser)

<b>Documentation:</b>

* [Install-ChocolateyPackage](https://docs.chocolatey.org/en-us/create/functions/install-chocolateypackage)

<b>Objective: </b>

* Package and install Brave

### Creating package

<b>Setup active setup:</b>

```powershell
$ErrorActionPreference = 'Stop'
$exeFileName = ''
$tools = Split-Path -Parent $MyInvocation.MyCommand.Definition
[System.IO.FileInfo]$exe = Join-Path $tools -ChildPath $exeFileName

ni "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\runBrave" | New-ItemProperty -Name "StubPath" -Value ('REG ADD "HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce" /v runBrave /t REG_SZ /d "{0}"' -f $exe)
```

<b>Package install command:</b>
```powershell
choco install brave -s D:\Downloads -y
```

## Related videos

<b>Windows tools:</b>

[Download and install NuGet Package Explorer](https://youtu.be/94u9jDCpifM)

<b>Chocolatey:</b>

[Chocolatey: Creating empty choco package](https://youtu.be/grueS3wnRNw) <br />
[Chocolatey: Installing and basic commands](https://youtu.be/vEH7t5eqJq4)

<b>Windows Registry</b>

[Windows Registry: Run and RunOnce](https://youtu.be/zgFzCq5uEPw) <br />
[Windows Registry: Active Setup](https://youtu.be/HrVJ7wdvfmo)