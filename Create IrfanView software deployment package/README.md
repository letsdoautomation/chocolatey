# Chocolatey: Create IrfanView software deployment package
### Documentation and download links

<b>Download:</b>

* [IrfanView](https://www.irfanview.com/)

<b>Documentation:</b>

* [Install-ChocolateyPackage](https://docs.chocolatey.org/en-us/create/functions/install-chocolateypackage)

<b>Objective: </b>

* Package and install IrfanView

### Creating package

<b>Softwre installation snippet:</b>

```powershell
$ErrorActionPreference = 'Stop'
$exeFileName = ''
$tools = Split-Path -Parent $MyInvocation.MyCommand.Definition
$exe = Join-Path $tools -ChildPath $exeFileName

$package = @{
  PackageName    = ""
  FileType       = ""
  File           = $exe
  SilentArgs     = ""
  ValidExitCodes = @(0)
}
Install-ChocolateyInstallPackage @package

Remove-Item $exe -ea SilentlyContinue -force
```

<b>Silent install switch:</b>

```powershell
/silent /allusers=1 /desktop=1
```

<b>Package install command:</b>
```powershell
choco install irfanview -s D:\Downloads -y
```

## Related videos

<b>Windows tools:</b>

[Download and install NuGet Package Explorer](https://youtu.be/94u9jDCpifM)

<b>Chocolatey:</b>

[Chocolatey: Creating empty choco package](https://youtu.be/grueS3wnRNw) <br />
[Chocolatey: Installing and basic commands](https://youtu.be/vEH7t5eqJq4)