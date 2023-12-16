# Chocolatey: Creating empty choco package
### Documentation and download links

* [choco new](https://docs.chocolatey.org/en-us/create/commands/new)
* [choco pack](https://docs.chocolatey.org/en-us/create/commands/pack) 

<b>Objective: </b>
  
* Create template package that will be used in future videos

# Creating package

<b>Generate choco package files:</b>
```powershell
choco new template
```

<b>template.nuspec file contents:</b>
```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2015/06/nuspec.xsd">
  <metadata>
    <id>template</id>
    <version>1.0.0</version>
    <title>template (Install)</title>
    <authors>letsdoautomation</authors>
    <description>template</description>
  </metadata>
  <files>
    <file src="tools\**" target="tools" />
  </files>
</package>
```

<b>Genere package:</b>
```powershell
choco pack template\template.nuspec
```

<b>Install package:</b>
```powershell
choco install template -s D:\downloads -y
```

## Related videos

<b>Windows tools:</b>

[Download and install NuGet Package Explorer](https://youtu.be/94u9jDCpifM)

<b>Chocolatey:</b>

[Chocolatey: Installing and basic commands](https://youtu.be/vEH7t5eqJq4)