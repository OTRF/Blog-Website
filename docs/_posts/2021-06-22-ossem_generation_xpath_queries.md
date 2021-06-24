---
layout: post
current: post
cover:  assets/images/blog/ossem_creating_xpath_queries/2021-06-22_00_blog_image.png
navigation: True
title: 'OSSEM Detection Modeling: Leveraging Data Relationships to Generate Windows Event XPath Queries'
date: 2021-06-22 12:00:00
tags: [ossem]
class: post-template
subclass: 'post'
author: jose
---

In this blogpost, we will introduce the [OSSEM Detection Modeling](https://github.com/OTRF/OSSEM-DM) project and how we can leverage its content to define **XPath queries** in order to define a more effective data collection strategy when working with [Azure Sentinel To-Go](https://github.com/OTRF/Azure-Sentinel2Go).

# Prerequisites

In order to get a better understanding of the OSSEM project and its Data Dictionaries component, we recommend you to read the following blogpost: [OSSEM Data Dictionaries: Correlating Security Telemetry](https://blog.openthreatresearch.com/ossem_security_telemetry_correlation)

# Introducing OSSEM - Detection Modeling

### What is OSSEM?

Open Source Security Events Metadata (OSSEM) is a community-led project that focuses primarily on the documentation and standardization of security event logs from diverse data sources and operating systems. Security events are documented in a `data dictionary (DD)` format, and they can be used as a reference while mapping data sources to data analytics used to validate the detection of adversarial techniques. In addition, the project provides a `common data model (CDM)` that can be used for data engineers during data normalization procedures to allow security analysts to query and analyze data across diverse data sources. Finally, the project also provides documentation about the structure and relationships identified in specific data sources to facilitate the development of data analytics and adversary behavior representation from a data perspective. This relationships are stored under the `detection model (DM)` section of the project.

![](assets/images/blog/ossem_correlating_events/2021-06-21_01_ossem_components.png)

### What are Detection Relationships?

Relationships are structures that can help us to describe and represent actions and behaviors (group of related actions) that can be performed within a network environment. The term `detection` helps us to categorize some of these relationships as potentially performed by adversaries. [OSSEM detection relationships](https://github.com/OTRF/OSSEM-DM/tree/main/relationships) are stored in an easy to consume format,`yaml`. Here is an example of a detection relationship that describes the action of a [user requesting access to a file](https://github.com/OTRF/OSSEM-DM/blob/main/relationships/user_requested_access_to_file.yml):

```yaml
name: User requested access to File
contributors:
- Jose Rodriguez @Cyb3rPandaH
- Roberto Rodriguez @Cyb3rWard0g
attack:
  data_source: File
  data_component: file access
behavior:
  source: user
  relationship: requested access to
  target: file
security_events:
- event_id: 4656
  name: A handle to an object was requested.
  platform: Windows
  audit_category: Object Access
  audit_sub_category: File System
  log_channel: Security
  log_provider: Microsoft-Windows-Security-Auditing
  filter_in:
    ObjectType: File
references:
  - https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4656
notes:
  - 'Event 4656: This event generates only if object’s SACL has required ACE to handle specific access right use.'
```

### What metadata is provided by the OSSEM - Detection Modeling project?

- **ATT&CK mapping**: The data sources piece of the [MITRE-ATT&CK framework](https://attack.mitre.org/) has been updated recently, and it now includes more metadata such as platform, collection layer, data components and data relationships. You can find more information about it [here](https://github.com/mitre-attack/attack-datasources). If an OSSEM detection relationship is part of ATT&CK data sources metadata, we are providing the name of the data source and the data component.

- **Behavior**: The activity described by a detection relationship may involve a network concept (`source`) that performs the action, another network concept (`target`) that receives the action or adds more context, and a basic description of the activity itself (`relationship`).

- **Security Events**: Every relationship within the OSSEM-DM project is created based on security telemetry. Therefore, in this section of the yaml file we are sharing event logs that can be collected in you network environment and are related to detection relationships. Most of the metadata provided for every event log is already documented by the telemetry provider, and we are adding a `filter_in` section in each event log where it is applicable. The reason for the filter_in feature is that the context provided by some event logs can be different based on the values of some data fields. If we use Security Auditing event log [4656 (A handle to an object was requested)](https://docs.microsoft.com/en-us/windows/security/threat-protection/auditing/event-4656) as an example, we can tell that the values of `ObjectType` field can make reference to different objects such as a `file`, a `windows registry key`, and even a `process`. This can be helpful when trying to define an effective data collection strategy.

### How can we define Detection Relationships?

There are three basic steps that you can follow to define a detection relationship.

**a) Identifying sources of data**

As it was mentioned before, every OSSEM detection relationship is created based on security telemetry. Therefore, we need to pick an event log or table that has been collected in your network environment. After that, we need to get a better understanding of the activity that triggered the generation of the event and the data fields provided by it. We can expedite this process by documenting security telemetry through data dictionaries. In fact, The OSSEM project has a data dictionaries component that you can use as a reference. You can learn and find more information about it [here](https://github.com/OTRF/OSSEM-DD). For the purpose of this section, let's continue using event 4656 as an example. You will find the data dictionary for this event [here](https://github.com/OTRF/OSSEM-DD/blob/main/windows/etw-providers/Microsoft-Windows-Security-Auditing/events/event-4656_v1.yml). In the image below you can see an extract of the data dictionary:

```yaml
title: 'Event ID 4656: A handle to an object was requested'
description: This event indicates that specific access was requested for an object.
  The object could be a file system, kernel, or registry object, or a file system
  object on removable storage or a device.
platform: windows
log_source: Microsoft-Windows-Security-Auditing
event_code: '4656'
event_version: '1'
event_fields:
- standard_name: user_logon_id
  standard_type: TBD
  name: SubjectLogonId
  type: HexInt64
  description: hexadecimal value that can help you correlate this event with recent
    events that might contain the same Logon ID
  sample_value: '0x4367b'
- standard_name: object_type
  standard_type: TBD
  name: ObjectType
  type: UnicodeString
  description: The type of an object that was accessed during the operation.
  sample_value: File
- standard_name: object_name
  standard_type: TBD
  name: ObjectName
  type: UnicodeString
  description: name and other identifying information for the object for which access
    was requested. For example, for a file, the path would be included.
  sample_value: C:\Documents\HBI Data.txt
references:
- text: MS Source
  link: https://github.com/MicrosoftDocs/windows-itpro-docs/blob/master/windows/security/threat-protection/auditing/event-4656.md
tags:
- Object Access
- Audit File System
- Audit Kernel Object
- Audit Registry
- Audit Removable Storage
```

**b) Identifying data entities**

After understanding the security context provided by an event log or table, we can identify network concepts that are involved in the activity that triggered the generation of telemetry. At Open Threat Research (OTR), we called these network concepts `data entities`. Before we continue, it is important to understand that there is not a right way to define data entities because it depends on the level of detail of your analysis and the relaity of every organization. Continuing with our example, by reviewing event 4656 data dictionary we can identify data entities such as an `user` and an object that can represent a `file`, a `windows registry key`, or a `process`.

**c) Identifying relationships among data entities**

Based on the documentation of security event 4656, we were able to understand that this event was generated because there was a `request of access to an object`. We also identified data entities that were involved on this action such as a user, a file, a windows registry key, and a process. Using these concepts as a reference, we can start describing interactions among data entities through detection relationships. The image below shows some of the relationships that we have identified so far:

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_01_relationships_among_entities.png)

# Operationalizing Detection Relationships

There are different ways we can leverage OSSEM detection relationships in order to expedite security operations activities within our organization such as `representation of adversary behavior` and `identification of relevant security telemetry`. For the purpose of this blogpost, I will share a use case related to filtering Microsoft Security Auditing telemetry through XPath queries based on detection relationships.

### How can we filter Microsoft Security Auditing Telemetry?

**a) Collecting telemetry through Event Viewer**

When it comes to Security Auditing telemetry collection, one of the main applications used by the InfoSec community is `EventViewer`. In the image below, you can see `Security` logs under the `Windows Logs` group. We have colected different event logs such as `4688`, `4656`, and `5156`.

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_02_event_viewer_application.png)

**b) Filtering relevant telemetry**

Even though it is really nice to collect every event log available from our network environment, we need to prioritize the collection of security telemetry to support our analysis. We have two options when trying to filter our current logs, `Filter` and `XML`. Let's filter event `4656` as an example by using the Filter option. When you use the this option, EventViewer will automatically create a `XPath query` in xml format for you.

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_03_event_viewer_filters.png)

In the image below, we can see only events 4656 after applying our filter.

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_04_event_4656_filtered.png)

**c) Filtering relevant telemetry (XPath Queries)**

Even though the `Filter` option allows you to reduce the number of event logs to review, it has limitations when accessing other parameters to perform a more complex queries. If we want to define a filter considering different data fields and multiple event logs, using `XPath queries` is a better option.

### Transforming Detection Relationships into XPath Queries using PowerShell

As it was described previously, if we want to filter Microsoft Security Auditing telemetry using a more complex logic and multiple event logs, we should use XPath queries. Let's say we want to filter telemetry documented in the OSSEM detection relationships project. We will need to translate every relationship content, currently in yaml format, in `XPath` form. We will use PowerShell to complete thi process.

**a) Downloading detection relationships mapped to ATT&CK**

Within the OSSEM - Detection Modeling project, the Open Threat Research (OTR) community is documenting different use cases in which we leverage the concept of detection relationships. One of these use cases is [MITRE-ATT&CK](https://github.com/OTRF/OSSEM-DM/tree/main/use-cases/mitre_attack). Within this folder, you can find a `JSON` file that contains all the metadata for every detection relationship that is mapped to the ATT&CK framework. This file is updated programatically after orginial yaml files are created and updated.

- Open PowerShell with Administrator rights.

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_05_running_powershell.png)

- Set the current directory (PowerShell session only) by running the following code.

```PowerShell
[Environment]::CurrentDirectory=(Get-Location -PSProvider FileSystem).ProviderPath
```

- Set the url to the raw json file within the GitHub repository.

```PowerShell
$uri = "https://raw.githubusercontent.com/OTRF/OSSEM-DM/main/use-cases/mitre_attack/techniques_to_events_mapping.json"
```

- Initialize a Web Client.

```PowerShell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
$wc = new-object System.Net.WebClient
```

- Download the JSON file.

```PowerShell
$wc.DownloadFile($uri, "techniques_to_events_mapping.json")
```

- Read the JSON file as an Array to validate it was downloaded correctly.

```PowerShell
$mappings = Get-Content .\techniques_to_events_mapping.json | ConvertFrom-Json
$mappings[0] 
```

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_06_downloading_json_file.png)

**b) Creating XML query files**

- Extract the metadata within the JSON file to a dictionary

```PowerShell
$allMappings = @{}
foreach ($item in $mappings) {
    if ($item.log_channel -eq 'Security'){
        if (!($allMappings.contains($item.data_source))){
            $allMappings.$($item.data_source) = @{}
        }
        if (!($allMappings[$item.data_source].contains($item.data_component))){
            $allMappings[$item.data_source][$item.data_component] = @()
        }
        if (!($allMappings[$item.data_source][$item.data_component] | Where-Object {$_.EventID -eq "$($item.event_id)"})) {
            $eventObject = @{
                EventID = "$($item.event_id)"
                EventName = "$($item.event_name)"
            }
            if ($item.filter_in.ToString() -ne 'NaN'){
                $eventObject += @{Filters = $item.filter_in}
            }
            $allMappings[$item.data_source][$item.data_component] += $eventObject
        }
    }
}
```

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_07_metadata_dictionary.png)

- Create XML files for each ATT&CK data source. Indeed, the name of each XML file will be the ATTCK data source name.

```PowerShell
foreach ($ds in $allMappings.Keys){
    $fileName = -join (($ds -replace " ","-").ToLower(), '.xml')
    $StringWriter = New-Object System.IO.StringWriter
    $XmlWriter = New-Object System.XMl.XmlTextWriter $StringWriter
    $xmlWriter.Formatting = "indented"
    $xmlWriter.Indentation = 2
    $xmlWriter.IndentChar = ' '
    $xmlWriter.WriteStartDocument()
    $xmlWriter.WriteStartElement("QueryList")
    $xmlWriter.WriteComment("ATT&CK Data Source - $ds")

    $Counter = 0
    foreach ($dc in $allMappings[$ds].Keys) {
        # Create query element
        $xmlWriter.WriteStartElement("Query")
            $xmlWriter.WriteAttributeString("Id", "$Counter")
            $xmlWriter.WriteAttributeString("Path", "Security")
            $xmlWriter.WriteComment("ATT&CK Data Component - $dc")
            # Create query strings
            $query = ""
            $leftover = @()
            foreach ($event in $allMappings[$ds][$dc]){
                $xmlWriter.WriteComment("$($Event.EventID) - $($Event.EventName)")
                if ($Event.Filters){
                    $leftover += $Event
                }
                else {
                    $query = -join ($query, " EventID=$($Event.EventID) ")
                    if (!($allMappings[$ds][$dc][-1]['EventID'] -eq $($Event.EventID))){
                        $query = -join ($query, "or")
                    }
                }
            }
            if ($allMappings[$ds][$dc].Count -ne $leftover.Count){
                $query = $query.Trim()
                $query = -join ("*[System[(", $query, ")]]")
                if ($leftover.Count -ne 0){
                    $query = -join ($query, ' or ')
                }
            }
            # Process leftover
            if ($leftover){
                foreach ($l in $leftover){
                    $query = -join ($query, "(*[System[EventID=$($l.EventID)]] and (")
                    foreach ($f in $l.Filters) {
                        $key = $f | get-member -MemberType NoteProperty | select -expandproperty Name
                        $query = -join ($query, "(*[EventData[Data[@Name='$($key)']='$($f.$key)'")
                        if (!($l.Filters[-1] -eq $($f))){
                            $query = -join ($query, "]] or ")
                        }
                        else {
                            $query = -join ($query, "]])))")
                        }
                    }
                    if (!($leftover[-1] -eq $($l))){
                        $query = -join ($query, " or ")
                    }
                }
            }
            # Create Select (query) Element
            $xmlWriter.WriteStartElement("Select")
                $xmlWriter.WriteAttributeString("Path", "Security")
                $xmlWriter.WriteString("$query")
            $xmlWriter.WriteEndElement() | out-null
        $xmlWriter.WriteEndElement() | out-null
        $counter += 1
    }
    # Write Close Tag for QueryList Element
    $xmlWriter.WriteEndDocument() | out-null
    # Finish The Document
    $xmlWriter.Flush() | out-null
    $StringWriter.Flush() | out-null
    #Create File
    $StringWriter.ToString() | out-file $fileName
    $xmlWriter.Close()
}
```

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_08_creating_xml_files.png)

- Validate the creation of `XML` files.

```PowerShell
# Printing all the files in my current directory
ls
```

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_09_validating_creation_xml_files.png)

- Read the content of the XML file validate it was created correctly. Let's use the `windows registry` data source as an example (Remember that, in case the name of the data source contains a blank space within the name, replace it with a `-` character. For example: user account --> user-account).

```PowerShell
get-content .\windows-registry.xml
```

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_10_validating_xml_file_structure.png)

### Testing XPath Queries

As a final validation step, let's test our XPath queries using EventViewer and PowerShell.

**a) Testing through EventViewer**

- Copy the output of the `get-content` command in the `XML` filter option and click on `OK`.

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_11_validating_xml_query_event_viewer.png)

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_12_xml_query_event_viewer_result.png)

**b) Testing through PowerShell**

- Execute the XPath querry using the `Get-WinEvent` command.

```PowerShell
[xml]$registry_test = get-content .\windows-registry.xml
Get-WinEvent -FilterXml $registry_test
```

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_13_validating_xml_query_execution.png)

# What else can we do with OSSEM - XPath Queries?

Within the Open Threat Research (OTR) projects portfolio, you can find the [Azure Sentinel To-Go](https://github.com/OTRF/Azure-Sentinel2Go) project. It allows you to expedite the deployment of an Azure Sentinel lab along with other Azure resources and a data ingestion pipeline to consume pre-recorded datasets for research purposes. You can use XPath queries to ingest security event logs to Azure Sentinel in a more effective way thanks to the new connector for Windows Security Events.

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_14_windows_security_events_connector.png)

### Creating a JSON file based on OSSEM XML files**

To make XPath queries available for Azure Sentinel To-Go, you can use the following PowerShell code to aggregate all the `.xml` files, that we obtained previously, in a `.json` file.

```PowerShell
$allFiles = Get-ChildItem -Path *.xml

$AllDataSources = @()
$DataSource = [ordered]@{}
# Name of Data Source
$DataSource['Name'] = "eventLogsDataSource"
# Transfer Period
$DataSource['scheduledTransferPeriod'] = "PT1M"
# Streams
$DataSource['streams'] = @(
    "Microsoft-SecurityEvent"
)
# Process XPath Queries
$DataSource['xPathQueries'] = @()
foreach ($file in $allFiles){
    [xml]$XmlQuery = Get-Content -path $file
    $queries = $xmlQuery.QueryList.Query
    ForEach ($query in $queries){
        $QueryString = "$(-join ($query.Select.Path, '!', $query.Select.'#text'))"
        if ("$QueryString" -notin $DataSource['xPathQueries']){
            $DataSource['xPathQueries'] += $QueryString
        } 
    }
}
$AllDataSources += $DataSource

@{
    windowsEventLogs = $AllDataSources
} | Convertto-Json -Depth 4 | Set-Content "ossem-attack.json" -Encoding UTF8
```

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_15_getting_json_file_azure_sentinel.png)

You can validate the generation of the JSON file by using the `get-content` function.

```PowerShell
get-content .\ossem-attack.json
```

![](assets/images/blog/ossem_creating_xpath_queries/2021-06-22_16_json_file_review.png)



# References
* [Complete PowerShell script to generate XML files with XPath queries based on OSSEM detection relationships](https://github.com/OTRF/OSSEM-DM/blob/main/scripts/ossem_attack_dm_to_xml.ps1)
* [Complete PowerShell script to generate JSON file with aggregated XPath queries based on OSSEM detection relationships](https://github.com/OTRF/OSSEM-DM/blob/main/scripts/ossem_attack_dm_to_azure_json.ps1)
* [OSSEM Detection Modeling GitHub repository](https://github.com/OTRF/OSSEM-DD)
* [OSSEM Project Website](https://ossemproject.com/intro.html)