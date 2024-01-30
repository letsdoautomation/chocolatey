# Chocolatey: Create Microsoft Edge settings package

<b>Microsoft Edge settings pakcage:</b>

* Install uBlock extension
* Configure search engines
    * google.com primary
    * duckduckgo.com optional
* Hide Browser Essentials from user interface
* Remove "Import Favorites" action from bookmark bar
* Disable "Split Screen"
* Disable "First Run Experience"
* Disable "Edge Shopping Assistant"
* Disable "Personalization Reporting"
* Disable "Hubs Sidebar"
* Disable "Collections"
* Configure Microsoft Edge start page

<b>chocolateyinstall.ps1 snippet:</b>

```powershell
$edge_search_engines =
@{
    is_default = $true
    keyword = "google"
    name = "google.com"
    search_url = "https://www.google.com/search?q={searchTerms}"
},
@{
    keyword = "duck"
    name = "duckduckgo.com"
    search_url = "https://duckduckgo.com/?q={searchTerms}"
} | ConvertTo-Json -Compress

$edge_settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 0
    Name  = "PinBrowserEssentialsToolbarButton"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 0 # 1 - Enable, 0 - Disable
    Name  = "ImportFavorites"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = $edge_search_engines
    Name  = "ManagedSearchEngines"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 0
    Name  = "SplitScreenEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 1
    Name  = "HideFirstRunExperience"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 0
    Name  = "EdgeShoppingAssistantEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 0
    Name  = "PersonalizationReportingEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 0
    Name  = "HubsSidebarEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 0
    Name  = "EdgeCollectionsEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 1 # Remove default sites
    Name  = "NewTabPageHideDefaultTopSites"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 3 # Remove background
    Name  = "NewTabPageAllowedBackgroundTypes"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 0 # Disable APP launcher
    Name  = "NewTabPageAppLauncherEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Value = 0 # Remove news and other stuff
    Name  = "NewTabPageContentEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge\ExtensionInstallForcelist"
    Value = "odfafepnkmbhccpbejgmiehpchacaeak" # uBlock Origin
    Name  = ++$edge_extension_count
} | group Path

foreach($setting in $edge_settings){
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
$edge_settings = 
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "PinBrowserEssentialsToolbarButton"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "ImportFavorites"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "ManagedSearchEngines"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "SplitScreenEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "HideFirstRunExperience"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "EdgeShoppingAssistantEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "PersonalizationReportingEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "HubsSidebarEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "EdgeCollectionsEnabled"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "NewTabPageHideDefaultTopSites"
},
[PSCustomObject]@{ 
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "NewTabPageAllowedBackgroundTypes"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "NewTabPageAppLauncherEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge"
    Name  = "NewTabPageContentEnabled"
},
[PSCustomObject]@{
    Path  = "SOFTWARE\Policies\Microsoft\Edge\ExtensionInstallForcelist"
    Name  = ++$edge_extension_count
} | group Path

foreach($setting in $edge_settings){
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
choco install edge-settings -s c:\choco -y
```

<b>Choco uninstall command:</b>

```powershell
choco uninstall edge-settings -y
```

## Related videos

<b>Windows tools:</b>

* [Download and install NuGet Package Explorer](https://youtu.be/94u9jDCpifM)

<b>Chocolatey:</b>

* [Chocolatey: Creating empty choco package](https://youtu.be/grueS3wnRNw) <br />
* [Chocolatey: Installing and basic commands](https://youtu.be/vEH7t5eqJq4) <br />

<b>More Microsoft Edge settings:</b>

* [Microsoft Edge settings](https://www.youtube.com/playlist?list=PLVncjTDMNQ4QwvLOskFdmFz_rZUKdgTW6)