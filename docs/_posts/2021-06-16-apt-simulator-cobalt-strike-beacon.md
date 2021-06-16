---
layout: post
current: post
cover:  assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_00_APTSimulator_blogpost_image.png
navigation: True
title: Simulating Cobalt Strike Beacon Activity
date: 2021-06-16 12:00:00
tags: [cobalt-strike,mordor]
class: post-template
subclass: 'post'
author: jose
---

In this quick blogpost, I will share the steps that I completed to simulate Cobalt Strike beacon activity using [APTSimulator](https://github.com/NextronSystems/APTSimulator) in a Windows 10 virtual machine.

## Pre-requisites
In order to complete the steps of this blogpost, you need to clone the APTSimulator project in your computer.

### a) Using Git Clone
Run the following commands to clone the repository in your preferred directory.
```
git clone https://github.com/NextronSystems/APTSimulator.git
```
### b) Downloading the GitHub Repository in Zip Format
Use the `Download Zip` option from the GitHub website to download the repository files in your preferred directory.

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_01_zip_download.png)

If the Windows Defender antivirus application is on, it might block the download process.

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_02_threat_found.png)

Here are some examples of files categorized as threats by Windows Defender

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_03_current_threats_found.png)

After turning the Windows Defender antivirus application off, you should be able to download the APTSimulator zip folder.

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_04_zip_file_downloaded.png)

After downloading the zip folder, you will need to extract the APTSimulator files.

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_05_extracting_zip_content.png)

Select a preferred destination for the APTSimulator files. For the purpose of this blogpost, I will use the `Documents` folder.

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_06_selecting_zip_destination.png)

You should be able to see the APTSimulator files now.

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_07_zip_files_extracted.png)

## Simulating Cobalt Strike Beaconing
### 1) Extracting Tools and Files
Open the `Command Prompt (CMD)` with Administrator rights.

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_08_cmd_administrator.png)

Execute the following commands to change your current directory to the `APTSimulator-master` folder and run the **build_pack.bat** file.

```
cd Documents/APTSimulator-master

build_pack.bat
```

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_09_build_pack.png)

### 2) Executing APTSimulator
In the same Command Prompt (CMD) window with Administrator rights, use the following commands to execute the **APTSimulator.bat** file.

```
APTSimulator.bat
```

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_10_aptsimulator_command.png)

After executing the `.bat` file, you will get a warning message that you need to answer with **Y** (Yes).

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_11_aptsimulator_warning.png)

After answering `Yes` to the warning message, you will see all the options provided by `APTSimulator`. For the purpose of this blogpost, I will use the **CobaltStrike Beacon Simulation** option that is represented by the letter **C**.

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_12_aptsimulator_options.png)

### 3) Simulating Cobalt Strike Beaconing
Running option C will allow you to start the simulation process.

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_13_cobalt_strike_simulation.png)

After the creation of named pipes and services, you will see HTTP beaconing activity (http://10.0.2.15/pixel.gif).

![](assets/images/blog/cobalt_strike_beacon_simulation/2021-06-14_14_cobalt_strike_beaconing.png)

## Do you want to analyze sample data for this behavior?
I have created a pre-recorded Mordor dataset with Sysmon, Security, and System events that were triggered when simulating `Cobalt Strike Beacon Activity` using `APTSimulator`.

* [Dataset metadata](https://mordordatasets.com/notebooks/small/windows/05_defense_evasion/SDWIN-210611210814.html)
* [Dataset](https://raw.githubusercontent.com/OTRF/mordor/master/datasets/small/windows/other/aptsimulator_cobaltstrike.zip)

## References
* https://github.com/NextronSystems/APTSimulator
* https://twitter.com/cyb3rops/status/1403253268051107840
* https://twitter.com/OTR_Community/status/1403551913459728387