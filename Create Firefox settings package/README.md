# Chocolatey: Create Firefox settings package

<b>Firefox settings package:</b>

* Install uBlock extension
* Customize Firefox home page
* Remove default bookmarks
* Disable Pocket
* Disable first run page
* Disable Firefox accounts
* Disable profile import
* Disable default browser check

<b>chocolateyinstall.ps1 snippet:</b>

```powershell
$firefox_settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 1
    Name  = "Locked"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 1
    Name  = "Search"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 0
    Name  = "Highlights"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 0
    Name  = "Pocket"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 0
    Name  = "Snippets"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 0
    Name  = "SponsoredPocket"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 0
    Name  = "SponsoredTopSites"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Value = 0
    Name  = "TopSites"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Value = 1
    Name  = "NoDefaultBookmarks"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Value = 1
    Name  = "DisablePocket"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Value = "" 
    Name  = "OverrideFirstRunPage"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Value = 1
    Name  = "DisableFirefoxAccounts"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Value = 1
    Name  = "DisableProfileImport"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Extensions\Install"
    Value = "https://addons.mozilla.org/firefox/downloads/file/4216633/ublock_origin-1.55.0.xpi"
    Name  = ++$firefox_extension_count
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Value = 1
    Name  = "DontCheckDefaultBrowser"
} | group Path

foreach($setting in $firefox_settings){
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
$firefox_settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Name  = "Locked"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Name  = "Search"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Name  = "Highlights"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Name  = "Pocket"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Name  = "Snippets"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Name  = "SponsoredPocket"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Name  = "SponsoredTopSites"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\FirefoxHome"
    Name  = "TopSites"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Name  = "NoDefaultBookmarks"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Name  = "DisablePocket"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Name  = "OverrideFirstRunPage"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Name  = "DisableFirefoxAccounts"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Name  = "DisableProfileImport"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox\Extensions\Install"
    Name  = ++$firefox_extension_count
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Mozilla\Firefox"
    Name  = "DontCheckDefaultBrowser"
} | group Path

foreach($setting in $firefox_settings){
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
choco install firefox-settings -s c:\choco -y
```

<b>Choco uninstall command:</b>

```powershell
choco uninstall firefox-settings -y
```

## Related videos

<b>Windows tools:</b>

* [Download and install NuGet Package Explorer](https://youtu.be/94u9jDCpifM)

<b>Chocolatey:</b>

* [Chocolatey: Creating empty choco package](https://youtu.be/grueS3wnRNw) <br />
* [Chocolatey: Installing and basic commands](https://youtu.be/vEH7t5eqJq4) <br />

<b>More Firefox settings:</b>

* [Firefox settings](https://www.youtube.com/playlist?list=PLVncjTDMNQ4SCsMyYad3CO0erlh-mGwiM)
