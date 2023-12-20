# Chocolatey: Deploy files and shortcuts with choco
### Documentation and download links

* [install-chocolateyshortcut](https://docs.chocolatey.org/en-us/create/functions/install-chocolateyshortcut)

<b>Objective: </b>
  
* Create package that deploys files
* Create shortcuts

# Creating package

<b>Copy files from package to computer:</b>

```powershell
$ErrorActionPreference = 'Stop'
$tools = Split-Path -Parent $MyInvocation.MyCommand.Definition
$files = Join-Path $tools -ChildPath 'files'

[System.IO.DirectoryInfo]$destination = "$($env:ProgramData)\letsdoautomation\files"

if(!$destination.Exists){
    $destination.Create()
}

gci $files | % {
    cp $_.FullName "$($destination.FullName)\$($_.Name)" -Force
}
```

<b>Create file shortcuts:</b>

```powershell
$destination.GetFiles() | % {
    $shortcut = @{
        ShortcutFilePath = "C:\Users\Public\Desktop\$($_.BaseName).lnk"
        TargetPath       = $_.FullName 
        IconLocation     = "shell32.dll,24"
    }
    Install-ChocolateyShortcut @shortcut
}
```

<b>Install package:</b>

```powershell
choco install files -s D:\Downloads -y
```

## Related videos

<b>Windows tools:</b>

[Download and install NuGet Package Explorer](https://youtu.be/94u9jDCpifM)

<b>Chocolatey:</b>

[Chocolatey: Creating empty choco package](https://youtu.be/grueS3wnRNw) <br />
[Chocolatey: Installing and basic commands](https://youtu.be/vEH7t5eqJq4)
