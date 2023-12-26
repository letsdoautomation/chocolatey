# Chocolatey: Create Telegram software deployment package
### Documentation and download links

<b>Download:</b>

* [Telegram](https://desktop.telegram.org/)

<b>Documentation:</b>

* [Install-ChocolateyPackage](https://docs.chocolatey.org/en-us/create/functions/install-chocolateypackage)

<b>Objective: </b>

* Package and install Telegram

### Creating package

<b>Setup active setup:</b>

```powershell
$ErrorActionPreference = 'Stop'
$exeFileName = ''
$silentSwitch = ''
$tools = Split-Path -Parent $MyInvocation.MyCommand.Definition
[System.IO.FileInfo]$exe = Join-Path $tools -ChildPath $exeFileName

ni "HKLM:\SOFTWARE\Microsoft\Active Setup\Installed Components\runTelegram" | New-ItemProperty -Name "StubPath" -Value ('REG ADD "HKCU\Software\Microsoft\Windows\CurrentVersion\RunOnce" /v runTelegram /t REG_SZ /d "{0} {1}"' -f $exe, $silentSwitch)
```

<b>Silent install switch:</b>

```powershell
/VERYSILENT /NORESTART
```

<b>Package install command:</b>
```powershell
choco install telegram -s D:\Downloads -y
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
