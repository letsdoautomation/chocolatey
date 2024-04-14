# Chocolatey: Create Microsoft Teams MSIX software deployment package

<b>Download:</b>

[Deploy the new Microsoft Teams client](https://learn.microsoft.com/en-us/microsoftteams/new-teams-vdi-requirements-deploy#deploy-the-new-microsoft-teams-client)

<b>Softwre installation snippet:</b>

```powershell
$ErrorActionPreference = 'Stop'
$exeFileName = ''
$tools = Split-Path -Parent $MyInvocation.MyCommand.Definition
$exe = Join-Path $tools -ChildPath $exeFileName

$package = @{
    PackagePath = $exe
    Online = $true
    SkipLicense = $true
}

Add-AppProvisionedPackage @package

Remove-Item $exe -ea SilentlyContinue -force
```

<b>Package install command:</b>

```powershell
choco install msteams -s D:\Downloads -y
```

## Related videos

<b>Windows tools:</b>

[Download and install NuGet Package Explorer](https://youtu.be/94u9jDCpifM)

<b>Chocolatey:</b>

[Chocolatey: Creating empty choco package](https://youtu.be/grueS3wnRNw) <br />
[Chocolatey: Installing and basic commands](https://youtu.be/vEH7t5eqJq4)