# Chocolatey: Create Brave Browser settings package

<b>Brave Browser settings pakcage:</b>

* Disable Brave Rewards
* Disable Brave VPN
* Disable Brave Wallet
* Disable Tor

<b>chocolateyinstall.ps1 snippet:</b>

```powershell
$brave_settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\BraveSoftware\Brave"
    Value = 1 # 0 - Enable, 1 - Disable
    Name  = "BraveRewardsDisabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\BraveSoftware\Brave"
    Value = 1 # 0 - Enable, 1 - Disable
    Name  = "BraveVPNDisabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\BraveSoftware\Brave"
    Value = 1 # 0 - Enable, 1 - Disable
    Name  = "BraveWalletDisabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\BraveSoftware\Brave"
    Value = 1 # 0 - Enable, 1 - Disable
    Name  = "TorDisabled"
} | group Path

foreach($setting in $brave_settings){
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
$brave_settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\BraveSoftware\Brave"
    Name  = "BraveRewardsDisabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\BraveSoftware\Brave"
    Name  = "BraveVPNDisabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\BraveSoftware\Brave"
    Name  = "BraveWalletDisabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\BraveSoftware\Brave"
    Name  = "TorDisabled"
} | group Path

foreach($setting in $brave_settings){
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
choco install brave-settings -s c:\choco -y
```

<b>Choco uninstall command:</b>

```powershell
choco uninstall brave-settings -y
```

## Related videos

<b>Windows tools:</b>

* [Download and install NuGet Package Explorer](https://youtu.be/94u9jDCpifM)

<b>Chocolatey:</b>

* [Chocolatey: Creating empty choco package](https://youtu.be/grueS3wnRNw) <br />
* [Chocolatey: Installing and basic commands](https://youtu.be/vEH7t5eqJq4) <br />

<b>More Brave Browser settings:</b>

* [Brave Browser settings](https://www.youtube.com/playlist?list=PLVncjTDMNQ4RR2YCyeUAg9u0UX_qXWtkA)