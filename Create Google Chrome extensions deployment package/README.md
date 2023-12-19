# Chocolatey: Package Google Chrome extensions
### Documentation and download
<b>Documentation links:</b>

* [Alternative extension installation methods](https://developer.chrome.com/docs/extensions/how-to/distribute/install-extensions)

<b>Objectives:</b>

* Package Microsoft 365 chrome extension
* Package UBlock Origin chrome extension

<b>Extension links:</b>

* [microsoft-365](https://chromewebstore.google.com/detail/microsoft-365/ndjpnladcallmjemlbaebfadecfhkepb?hl=en-US)
* [ublock-origin](https://chromewebstore.google.com/detail/ublock-origin/cjpalhdlnbpafiamejdnhcphjbkeiagm?hl=en-US)


<b>"Install" extension:</b>

```powershell
$extension = ''
$key_path = "Software\Policies\Google\Chrome\ExtensionInstallForcelist"

$registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($key_path, $true)

if($null -eq $registry){
    $registry = [Microsoft.Win32.Registry]::LocalMachine.CreateSubKey($key_path, $true)
    $registry.SetValue("1", $extension)
}
else{
    $values = $registry.GetValueNames().ForEach({$registry.GetValue($_)})
    if($extension -notin $values){
        $maximum = $registry.GetValueNames().Where({$_ -match "\d"}) | measure -maximum | select -expand maximum
        $maximum += 1
        $registry.SetValue($maximum, $extension)
    }
}

$registry.Dispose()
```

## Related videos

<b>Windows tools:</b>

[Download and install NuGet Package Explorer](https://youtu.be/94u9jDCpifM)

<b>Chocolatey:</b>

[Chocolatey](https://youtu.be/grueS3wnRNw)
[Chocolatey: Installing and basic commands](https://youtu.be/vEH7t5eqJq4)