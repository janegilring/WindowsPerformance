# Storage Spaces Direct Performance monitoring

## Grafana, InfluxDB and Telegraf

- [Grafana Hyper-V Metrics Dashboard](https://github.com/janegilring/WindowsPerformance/blob/master/Storage%20Spaces%20Direct/Grafana/Hyper-V%20Metrics.json) - based on [Hyper-V Metrics dashboard by Allan Good](https://grafana.com/dashboards/2618)
- [Telegraf.conf](https://github.com/janegilring/WindowsPerformance/blob/master/Storage%20Spaces%20Direct/Telegraf/telegraf.conf) contains the necessary counters for the dashboard

Sample screenshots:

![Hyper-V Dashboard - example 01](https://github.com/janegilring/WindowsPerformance/raw/master/Storage%20Spaces%20Direct/Images/Hyper-V_dashboard_01.png)

![Hyper-V Dashboard - example 02](https://github.com/janegilring/WindowsPerformance/raw/master/Storage%20Spaces%20Direct/Images/Hyper-V_dashboard_02.png)

![Hyper-V Dashboard - example 03](https://github.com/janegilring/WindowsPerformance/raw/master/Storage%20Spaces%20Direct/Images/Hyper-V_dashboard_03.png)

![Hyper-V Dashboard - example 04](https://github.com/janegilring/WindowsPerformance/raw/master/Storage%20Spaces%20Direct/Images/Hyper-V_dashboard_04.png)

## Counters

[S2D_counters.csv](https://github.com/janegilring/WindowsPerformance/blob/master/Storage%20Spaces%20Direct/Performance%20counters/S2D_counters.csv) contains suggested performance counters for monitoring S2D.

The counters reference file can be read by PowerShell and converted to any desired format, such as YAML:

```powershell
$Counters = Import-Csv -Path '~\Git\WindowsPerformance\Storage Spaces Direct\Performance counters\S2D_counters.csv'

Install-Module -Name powershell-yaml -Scope CurrentUser
$Counters | Select-Object -ExpandProperty Counter | ConvertTo-Yaml
```

If you want to contribute and know which counter set to find suggested counters from, you can use this snippet to retrieve all counters from the specified counter set:

```powershell
$Counters = Get-Counter -ListSet *
$CounterSet = 'Cluster CSV File System'
$Counters | where CounterSetName -eq $CounterSet | select -ExpandProperty Paths | select @{n='Counter';e={$_}},@{n='Suggested threshold';e={}},@{n='Comment';e={}} | Out-GridView -PassThru | ConvertTo-Csv | clip
```

A grid window will popup allowing you to select counters. The selection will be converted to CSV and placed on the clipboard, ready to paste into S2D_counters.csv.

## Links

- [Storing and Visualizing Time Series Data with InfluxDB and Universal Dashboard](https://poshtools.com/2018/12/06/storing-and-visualizing-time-series-data-with-influxdb-and-universal-dashboard/)
- [Hyper-V Metrics](https://grafana.com/dashboards/2618)
- [Windows Metric Dashboards with InfluxDB and Grafana](https://hodgkins.io/windows-metric-dashboards-with-influxdb-and-grafana)
- [WSLab - S2D and Grafana](https://github.com/Microsoft/WSLab/tree/master/Scenarios/S2D%20and%20Grafana)