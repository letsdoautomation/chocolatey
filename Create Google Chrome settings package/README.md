# Chocolatey: Create Google Chrome settings package

<b>Google Chrome settings package:</b>

* Install uBlock extension
* Disable "Turn on an ad privacy feature" message
* Disable Privacy Sandbox Ad measurement
* Disable Privacy Sandbox Ad topics
* Disable Privacy Sandbox Site-suggested ads
* Disable Web Site NotificationsSetting


<b>chocolateyinstall.ps1 snippet:</b>

```powershell
$chrome_settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0
    Name  = "PrivacySandboxPromptEnabled" # notification
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0
    Name  = "PrivacySandboxAdMeasurementEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0
    Name  = "PrivacySandboxAdTopicsEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 0
    Name  = "PrivacySandboxSiteEnabledAdsEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Value = 2
    Name  = "DefaultNotificationsSetting"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome\ExtensionInstallForcelist"
    Value = "cjpalhdlnbpafiamejdnhcphjbkeiagm"
    Name  = ++$chrome_extension_count
}  | group Path

foreach($setting in $chrome_settings){
    $registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        $registry = [Microsoft.Win32.Registry]::LocalMachine.CreateSubKey($setting.Name, $true)
    }
    $setting.Group | %{
        $registry.SetValue($_.name, $_.value)
    }
    $registry.Dispose()
}
```

<b>chocolateyuninstall.ps1 snippet:</b>

```powershell
$chrome_settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Name  = "PrivacySandboxPromptEnabled" # notification
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Name  = "PrivacySandboxAdMeasurementEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Name  = "PrivacySandboxAdTopicsEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Name  = "PrivacySandboxSiteEnabledAdsEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome"
    Name  = "DefaultNotificationsSetting"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Google\Chrome\ExtensionInstallForcelist"
    Name  = ++$chrome_extension_count
}  | group Path

foreach($setting in $chrome_settings){
    $registry = [Microsoft.Win32.Registry]::LocalMachine.OpenSubKey($setting.Name, $true)
    if ($null -eq $registry) {
        continue
    }

    foreach($item in $setting.Group.Where({$null -ne $registry.GetValue($_.name)})){
        $registry.DeleteValue($item.name, $true)
    }
    $registry.Dispose()
}
```

<b>Choco install command with c:\choco as package repository:</b>

```powershell
choco install chrome-settings -s c:\choco -y
```

<b>Choco uninstall command:</b>

```powershell
choco uninstall chrome-settings -y
```

## Related videos

<b>Windows tools:</b>

* [Download and install NuGet Package Explorer](https://youtu.be/94u9jDCpifM)

<b>Chocolatey:</b>

* [Chocolatey: Creating empty choco package](https://youtu.be/grueS3wnRNw) <br />
* [Chocolatey: Installing and basic commands](https://youtu.be/vEH7t5eqJq4) <br />

<b>More Google Chrome settings:</b>

* [Google Chrome settings](https://www.youtube.com/playlist?list=PLVncjTDMNQ4QNF4Npbo_eUzOUT_p6Or2k)
