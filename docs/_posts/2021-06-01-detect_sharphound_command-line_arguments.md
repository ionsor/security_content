---
title: "Detect SharpHound Command-Line Arguments"
excerpt: "Domain Account, Local Groups, Domain Trust Discovery, Local Account, Account Discovery, Domain Groups, Permission Groups Discovery"
categories:
  - Endpoint
last_modified_at: 2021-06-01
toc: true
toc_label: ""
tags:
  - Domain Account
  - Discovery
  - Local Groups
  - Discovery
  - Domain Trust Discovery
  - Discovery
  - Local Account
  - Discovery
  - Account Discovery
  - Discovery
  - Domain Groups
  - Discovery
  - Permission Groups Discovery
  - Discovery
  - Splunk Enterprise
  - Splunk Enterprise Security
  - Splunk Cloud
  - Endpoint
---



[Try in Splunk Security Cloud](https://www.splunk.com/en_us/cyber-security.html){: .btn .btn--success}

#### Description

The following analytic identifies common command-line arguments used by SharpHound `-collectionMethod` and `invoke-bloodhound`. Being the script is FOSS, function names may be modified, but these changes are dependent upon the operator. In most instances the defaults are used. This analytic works to identify the common command-line attributes used. It does not cover the entirety of every argument in order to avoid false positives.

- **Type**: TTP
- **Product**: Splunk Enterprise, Splunk Enterprise Security, Splunk Cloud
- **Datamodel**: [Endpoint](https://docs.splunk.com/Documentation/CIM/latest/User/Endpoint)
- **Last Updated**: 2021-06-01
- **Author**: Michael Haag, Splunk
- **ID**: a0bdd2f6-c2ff-11eb-b918-acde48001122


#### [ATT&CK](https://attack.mitre.org/)

| ID          | Technique   | Tactic         |
| ----------- | ----------- |--------------- |
| [T1087.002](https://attack.mitre.org/techniques/T1087/002/) | Domain Account | Discovery |

| [T1069.001](https://attack.mitre.org/techniques/T1069/001/) | Local Groups | Discovery |

| [T1482](https://attack.mitre.org/techniques/T1482/) | Domain Trust Discovery | Discovery |

| [T1087.001](https://attack.mitre.org/techniques/T1087/001/) | Local Account | Discovery |

| [T1087](https://attack.mitre.org/techniques/T1087/) | Account Discovery | Discovery |

| [T1069.002](https://attack.mitre.org/techniques/T1069/002/) | Domain Groups | Discovery |

| [T1069](https://attack.mitre.org/techniques/T1069/) | Permission Groups Discovery | Discovery |

#### Search

```

| tstats `security_content_summariesonly` count min(_time) as firstTime max(_time) as lastTime from datamodel=Endpoint.Processes where Processes.process IN ("*-collectionMethod*","*invoke-bloodhound*") by Processes.dest Processes.user Processes.parent_process Processes.process_name Processes.process Processes.process_id Processes.parent_process_id 
| `drop_dm_object_name(Processes)` 
| `security_content_ctime(firstTime)` 
| `security_content_ctime(lastTime)` 
| `detect_sharphound_command_line_arguments_filter`
```

#### Associated Analytic Story
* [Discovery Techniques](/stories/discovery_techniques)
* [Ransomware](/stories/ransomware)


#### How To Implement
To successfully implement this search you need to be ingesting information on process that include the name of the process responsible for the changes from your endpoints into the `Endpoint` datamodel in the `Processes` node.

#### Required field
* _time
* Processes.dest
* Processes.user
* Processes.parent_process
* Processes.process_name
* Processes.process
* Processes.process_id
* Processes.parent_process_id


#### Kill Chain Phase
* Reconnaissance


#### Known False Positives
False positives should be limited as the arguments used are specific to SharpHound. Filter as needed or add more command-line arguments as needed.


#### RBA

| Risk Score  | Impact      | Confidence   | Message      |
| ----------- | ----------- |--------------|--------------|
| 24.0 | 30 | 80 | Possible SharpHound command-Line arguments identified on $dest$ |




#### Reference

* [https://attack.mitre.org/software/S0521/](https://attack.mitre.org/software/S0521/)
* [https://thedfirreport.com/?s=bloodhound](https://thedfirreport.com/?s=bloodhound)
* [https://github.com/BloodHoundAD/BloodHound/tree/master/Collectors](https://github.com/BloodHoundAD/BloodHound/tree/master/Collectors)
* [https://github.com/BloodHoundAD/SharpHound3](https://github.com/BloodHoundAD/SharpHound3)
* [https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1059.001/T1059.001.md#atomic-test-2---run-bloodhound-from-local-disk](https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1059.001/T1059.001.md#atomic-test-2---run-bloodhound-from-local-disk)



#### Test Dataset
Replay any dataset to Splunk Enterprise by using our [`replay.py`](https://github.com/splunk/attack_data#using-replaypy) tool or the [UI](https://github.com/splunk/attack_data#using-ui).
Alternatively you can replay a dataset into a [Splunk Attack Range](https://github.com/splunk/attack_range#replay-dumps-into-attack-range-splunk-server)

* [https://media.githubusercontent.com/media/splunk/attack_data/master/datasets/attack_techniques/T1059.001/sharphound/windows-sysmon.log](https://media.githubusercontent.com/media/splunk/attack_data/master/datasets/attack_techniques/T1059.001/sharphound/windows-sysmon.log)



[*source*](https://github.com/splunk/security_content/tree/develop/detections/endpoint/detect_sharphound_command-line_arguments.yml) \| *version*: **1**