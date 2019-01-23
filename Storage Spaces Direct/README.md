# Storage Spaces Direct Performance monitoring

## Counters

S2D_counters.json contains suggested performance counters.

The counters reference file can be read by PowerShell and converted to any desired format, such as YAML:

```powershell
$Counters = Get-Content -Path '~\Git\WindowsPerformance\Storage Spaces Direct\Performance counters\S2D_counters.json' | ConvertFrom-Json

Install-Module -Name powershell-yaml -Scope CurrentUser
$Counters | Select-Object -ExpandProperty Counter | ConvertTo-Yaml
```

If you want to contribute and know which counter set to find suggested counters from, you can use this snippet to retrieve all counters from the specified counter set:

```powershell
$Counters = Get-Counter -ListSet *
$CounterSet = 'Cluster CSV File System'
$Counters | where CounterSetName -eq $CounterSet | select -ExpandProperty Paths | select @{n='Counter';e={$_}},@{n='Suggested threshold';e={}},@{n='Comment';e={}} | Out-GridView -PassThru | ConvertTo-Json | clip
```

A grid window will popup allowing you to select counters. The selection will be converted to JSON and placed on the clipboard, ready to paste into S2D_counters.json.

## Links

- [Storing and Visualizing Time Series Data with InfluxDB and Universal Dashboard](https://poshtools.com/2018/12/06/storing-and-visualizing-time-series-data-with-influxdb-and-universal-dashboard/)
- [Hyper-V Metrics](https://grafana.com/dashboards/2618)
- [Windows Metric Dashboards with InfluxDB and Grafana](https://hodgkins.io/windows-metric-dashboards-with-influxdb-and-grafana)
- [WSLab - S2D and Grafana](https://github.com/Microsoft/WSLab/tree/master/Scenarios/S2D%20and%20Grafana)