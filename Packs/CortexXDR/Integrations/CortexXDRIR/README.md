## Overview
---

Cortex XDR is the world's first detection and response app that natively integrates network, endpoint and cloud data to stop sophisticated attacks.
This integration was integrated and tested with version xx of Cortex XDR - IR
## Playbooks
---
#### Cortex XDR Incident Handling
The playbook syncs and updates new XDR alerts that construct the incident.
It enriches indicators using Threat Intelligence integrations and Palo Alto Networks
AutoFocus. The incident's severity is then updated based on the indicators reputation
and an analyst is assigned for manual investigation. If chosen, automated remediation
with Palo Alto Networks FireWall is initiated. After a manual review by the
SOC analyst, the XDR incident is closed automatically.

  
## Use Cases
---
- Fetch incidents from XDR
- Enrich incident with alerts and incident from XDR
- Update incident in XDR
- Search for endpoints
- Isolate/unisolate endpoints
- Insert parsed alerts into XDR
- Insert CEF alerts into XDR
- Query for agent audit reports
- Query for audit management logs
- Create distribution
- Get distribution download URL
- Get distribution versions

## Automation
---
To sync incidents between Demisto and Cortex XDR, you should use the **XDRSyncScript** script, which you can find in the automation page.

## Configuration
---
You need to collect several pieces of information in order to configure the integration on Cortex XSOAR.

#### Generate an API Key and API Key ID
1. In your Cortex XDR platform, go to **Settings**.
2. Click the **+New Key** button in the top right corner
3. Generate a key of type **Advanced**.
4. Copy and paste the key.
5. From the ID column, copy the Key ID.

#### URL
1. In your Cortex XDR platform, go to **Settings**.
2. Click the **Copy URL** button in the top right corner.

#### Configure integration parameters
1. Navigate to __Settings__ > __Integrations__ > __Servers & Services__.
2. Search for Cortex XDR - IR.
3. Click __Add instance__ to create and configure a new integration instance.
    * __Name__: a textual name for the integration instance.
    * __Fetch incidents__
    * __Incident type__
    * __Server URL (copy URL from XDR - click ? to see more info.)__
    * __API Key ID__
    * __API Key__
    * __Maximum number of incidents per fetch__
    * __First fetch timestamp (&lt;number&gt; &lt;time unit&gt;, e.g., 12 hours, 7 days)__
    * __HTTP Timeout__ (default is 120 seconds)
    * __Fetch incident alerts and artifacts__
    * __First fetch timestamp (&lt;number&gt; &lt;time unit&gt;, e.g., 12 hours, 7 days)__
    * __Incidend Mirroring Direction__
    * __Sync Incident Owners__
    * __Trust any certificate (not secure)__
    * __Use system proxy settings__
4. Click __Test__ to validate the URLs, token, and connection.
## Fetched Incidents Data
---
```
incident_id:31
creation_time:1564594008755
modification_time:1566339537617
detection_time:null
status:new
severity:low
description:6 'Microsoft Windows RPC Fragment Evasion Attempt' alerts detected by PAN NGFW on 6 hosts
assigned_user_mail:null
assigned_user_pretty_name:null
alert_count:6
low_severity_alert_count:0
med_severity_alert_count:6
high_severity_alert_count:0
user_count:1
host_count:6
notes:null
resolve_comment:null
manual_severity:low
manual_description:null
xdr_url:https://1111.paloaltonetworks.com/incident-view/31
```

* Note: By checking the `Fetch incident alerts and artifacts` integration configuration parameter - fetched incidents will include additional data.

## XDR Incident Mirroring
**Note this feature is available from Cortex XSOAR version 6.0.0**

You can enable incident mirroring between Cortex XSOAR incidents and Cortex XDR incidents.
To setup the mirroring follow these instructions:
1. Navigate to __Settings__ > __Integrations__ > __Servers & Services__.
2. Search for Cortex XDR - IR and select your integration instance.
3. Enable `Fetches incidents`.
4. In the `Incident Mirroring Direction` integration parameter, select in which direction should incidents be mirrored:
  * Incoming - Any changes in XDR incidents will be reflected in XSOAR incidents.
  * Outgoing - Any changes in XSOAR incidents will be reflected in XDR incidents.
  * Both - Changes in XSOAR and XDR incidents will be reflected in both directions.
  * None - Choose this to turn off incident mirroring.
5. Optional: Check the `Sync Incident Owners` integration parameter to sync the incident owners in both XDR and XSOAR.
  * Note: This feature will only work if the same users are registered both in Cortex XSOAR and Cortex XDR.
6. Newly fetched incidents will be mirrored in the chosen direction.
  * Note: this will not effect existing incidents.

### XDR Mirroring Notes, limitations and Troubleshooting

* While you can mirror changes in incident fields both in and out in each incident, you can only mirror in a single direction at a time. For example:
  If we have an incident with two fields (A and B) in XDR and XSOAR while *Incoming And Outgoing* mirroring is selected: 
   * I can mirror field A from XDR to XSOAR and field B from XSOAR to XDR.
   * I cannot mirror changes from field A in both directions.
   
  Initially all fields are mirrored in from XDR to XSOAR. Once they are changed in XSOAR, they can only be mirrored out.
* **Do not use the `XDRSyncScript` automation nor any playbook that uses this automation** 
  (e.g `Cortex XDR Incident Sync` or `Cortex XDR incident handling v2`), as it impairs the mirroring functionality.

* When migrating an existing instance to the mirroring feature, or in case the mirroring does not work as expected, make sure that:
   * The default playbook of the `Cortex XDR Incident` incident type is not `Cortex XDR Incident Sync`, change it to a 
     different playbook that does not use `XDRSyncScript`.
   * The XDR integration instance incoming mapper is set to `Cortex XDR - Incoming Mapper` and the outgoing mapper is set to `Cortex XDR - Outgoing Mapper`.

## Commands
---
You can execute these commands from the Demisto CLI, as part of an automation, or in a playbook.
After you successfully execute a command, a DBot message appears in the War Room with the command details.
1. xdr-get-incidents
2. xdr-get-incident-extra-data
3. xdr-update-incident
4. xdr-insert-parsed-alert
5. xdr-insert-cef-alerts
6. xdr-isolate-endpoint
7. xdr-unisolate-endpoint
8. xdr-get-endpoints
9. xdr-get-distribution-versions
10. xdr-create-distribution
11. xdr-get-distribution-url
12. xdr-get-create-distribution-status
13. xdr-get-audit-management-logs
14. xdr-get-audit-agent-reports
15. xdr-get-policy
16. xdr-get-endpoint-violations
17. xdr-retrieve-files
18. xdr-retrieve-file-details
19. xdr-get-scripts
20. xdr-get-script-metadata
21. xdr-get-script-code
22. xdr-run-script
23. xdr-get-script-execution-status
24. xdr-insert-simple-indicators
25. xdr-action-status-get
26. xdr-delete-endpoints

### 1. xdr-get-incidents
---
Returns a list of incidents, which you can filter by a list of incident IDs (max. 100), the time the incident was last modified, and the time the incident was created.
      If you pass multiple filtering arguments, they will be concatenated using the AND condition. The OR condition is not supported.
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-get-incidents`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| lte_creation_time | Time format 2019-12-31T23:59:00. | Optional | 
| gte_creation_time | Returned incidents that were created on or after the specified date/time, in the format 2019-12-31T23:59:00. | Optional | 
| lte_modification_time | Filters returned incidents that were created on or before the specified date/time, in the format 2019-12-31T23:59:00. | Optional | 
| gte_modification_time | Filters returned incidents that were modified on or after the specified date/time, in the format 2019-12-31T23:59:00. | Optional | 
| incident_id_list | An array or CSV string of incident IDs. | Optional | 
| since_creation_time | Filters returned incidents that were created on or after the specified date/time range, for example, 1 month, 2 days, 1 hour, and so on. | Optional | 
| since_modification_time | Filters returned incidents that were modified on or after the specified date/time range, for example, 1 month, 2 days, 1 hour, and so on. | Optional | 
| sort_by_modification_time | Sorts returned incidents by the date/time that the incident was last modified ("asc" - ascending, "desc" - descending). | Optional | 
| sort_by_creation_time | Sorts returned incidents by the date/time that the incident was created ("asc" - ascending, "desc" - descending). | Optional | 
| page | Page number (for pagination). The default is 0 (the first page). | Optional | 
| limit | Maximum number of incidents to return per page. The default and maximum is 100. | Optional | 


##### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.Incident.incident_id | String | Unique ID assigned to each returned incident. | 
| PaloAltoNetworksXDR.Incident.manual_severity | String | Incident severity assigned by the user. This does not affect the calculated severity. Can be "low","medium","high" | 
| PaloAltoNetworksXDR.Incident.manual_description | String | Incident description provided by the user. | 
| PaloAltoNetworksXDR.Incident.assigned_user_mail | String | Email address of the assigned user. | 
| PaloAltoNetworksXDR.Incident.high_severity_alert_count | String | Number of alerts with the severity HIGH. | 
| PaloAltoNetworksXDR.Incident.host_count | number | Number of hosts involved in the incident. | 
| PaloAltoNetworksXDR.Incident.xdr_url | String | A link to the incident view on XDR. | 
| PaloAltoNetworksXDR.Incident.assigned_user_pretty_name | String | Full name of the user assigned to the incident. | 
| PaloAltoNetworksXDR.Incident.alert_count | number | Total number of alerts in the incident. | 
| PaloAltoNetworksXDR.Incident.med_severity_alert_count | number | Number of alerts with the severity MEDIUM. | 
| PaloAltoNetworksXDR.Incident.user_count | number | Number of users involved in the incident. | 
| PaloAltoNetworksXDR.Incident.severity | String | Calculated severity of the incident. Can be "low", "medium", or "high". | 
| PaloAltoNetworksXDR.Incident.low_severity_alert_count | String | Number of alerts with the severity LOW. | 
| PaloAltoNetworksXDR.Incident.status | String | Current status of the incident. Can be "new", "under_investigation", "resolved_threat_handled", "resolved_known_issue", "resolved_duplicate", "resolved_false_positive", or "resolved_other". | 
| PaloAltoNetworksXDR.Incident.description | String | Dynamic calculated description of the incident. | 
| PaloAltoNetworksXDR.Incident.resolve_comment | String | Comments entered by the user when the incident was resolved. | 
| PaloAltoNetworksXDR.Incident.notes | String | Comments entered by the user regarding the incident. | 
| PaloAltoNetworksXDR.Incident.creation_time | date | Date and time the incident was created on XDR. | 
| PaloAltoNetworksXDR.Incident.detection_time | date | Date and time that the first alert occurred in the incident. | 
| PaloAltoNetworksXDR.Incident.modification_time | date | Date and time that the incident was last modified. | 


##### Command Example
```!xdr-get-incidents gte_creation_time=2010-10-10T00:00:00 limit=3 sort_by_creation_time=desc```

##### Context Example
```
{
    "PaloAltoNetworksXDR.Incident": [
        {
            "host_count": 1, 
            "incident_id": "4", 
            "manual_severity": "medium", 
            "description": "5 'This alert from content  TestXDRPlaybook' alerts detected by Checkpoint - SandBlast  ", 
            "severity": "medium", 
            "modification_time": 1579290004178, 
            "assigned_user_pretty_name": null, 
            "notes": null, 
            "creation_time": 1577276587937, 
            "alert_count": 5, 
            "med_severity_alert_count": 1, 
            "detection_time": null, 
            "assigned_user_mail": null, 
            "resolve_comment": "This issue was solved in Incident number 192304", 
            "status": "new", 
            "user_count": 1, 
            "xdr_url": "https://some.xdr.url.com/incident-view/4", 
            "starred": false, 
            "low_severity_alert_count": 0, 
            "high_severity_alert_count": 4, 
            "manual_description": null
        }, 
        {
            "host_count": 1, 
            "incident_id": "3", 
            "manual_severity": "medium", 
            "description": "'test 1' generated by Virus Total - Firewall", 
            "severity": "medium", 
            "modification_time": 1579237974014, 
            "assigned_user_pretty_name": "woo@demisto.com", 
            "notes": null, 
            "creation_time": 1576100096594, 
            "alert_count": 1, 
            "med_severity_alert_count": 0, 
            "detection_time": null, 
            "assigned_user_mail": "woo@demisto.com", 
            "resolve_comment": null, 
            "status": "new", 
            "user_count": 1, 
            "xdr_url": "https://some.xdr.url.com/incident-view/3", 
            "starred": false, 
            "low_severity_alert_count": 0, 
            "high_severity_alert_count": 1, 
            "manual_description": null
        }, 
        {
            "host_count": 1, 
            "incident_id": "2", 
            "manual_severity": "high", 
            "description": "'Alert Name Example 333' along with 1 other alert generated by Virus Total - VPN & Firewall-3 and Checkpoint - SandBlast", 
            "severity": "high", 
            "modification_time": 1579288790259, 
            "assigned_user_pretty_name": null, 
            "notes": null, 
            "creation_time": 1576062816474, 
            "alert_count": 2, 
            "med_severity_alert_count": 0, 
            "detection_time": null, 
            "assigned_user_mail": null, 
            "resolve_comment": null, 
            "status": "under_investigation", 
            "user_count": 1, 
            "xdr_url": "https://some.xdr.url.com/incident-view/2", 
            "starred": false, 
            "low_severity_alert_count": 0, 
            "high_severity_alert_count": 2, 
            "manual_description": null
        }
    ]
}
```

##### Human Readable Output
### Incidents
|alert_count|assigned_user_mail|assigned_user_pretty_name|creation_time|description|detection_time|high_severity_alert_count|host_count|incident_id|low_severity_alert_count|manual_description|manual_severity|med_severity_alert_count|modification_time|notes|resolve_comment|severity|starred|status|user_count|xdr_url|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 5 |  |  | 1577276587937 | 5 'This alert from content  TestXDRPlaybook' alerts detected by Checkpoint - SandBlast   |  | 4 | 1 | 4 | 0 |  | medium | 1 | 1579290004178 |  | This issue was solved in Incident number 192304 | medium | false | new | 1 | `https://some.xdr.url.com/incident-view/4` |
| 1 | woo@demisto.com | woo@demisto.com | 1576100096594 | 'test 1' generated by Virus Total - Firewall |  | 1 | 1 | 3 | 0 |  | medium | 0 | 1579237974014 |  |  | medium | false | new | 1 | `https://some.xdr.url.com/incident-view/3` |
| 2 |  |  | 1576062816474 | 'Alert Name Example 333' along with 1 other alert generated by Virus Total - VPN & Firewall-3 and Checkpoint - SandBlast |  | 2 | 1 | 2 | 0 |  | high | 0 | 1579288790259 |  |  | high | false | under_investigation | 1 | `https://some.xdr.url.com/incident-view/2` |


### 2. xdr-get-incident-extra-data
---
Returns additional data for the specified incident, for example, related alerts, file artifacts, network artifacts, and so on.
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-get-incident-extra-data`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| incident_id | The ID of the incident for which to get additional data. | Required | 
| alerts_limit | Maximum number of alerts to return. Default is 1,000. | Optional | 


##### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.Incident.incident_id | String | Unique ID assigned to each returned incident. | 
| PaloAltoNetworksXDR.Incident.creation_time | Date | Date and time the incident was created on XDR. | 
| PaloAltoNetworksXDR.Incident.modification_time | Date | Date and time that the incident was last modified. | 
| PaloAltoNetworksXDR.Incident.detection_time | Date | Date and time that the first alert occurred in the incident. | 
| PaloAltoNetworksXDR.Incident.status | String | Current status of the incident:
"new","under_investigation","resolved_threat_handled","resolved_known_issue","resolved_duplicate","resolved_false_positive","resolved_other" | 
| PaloAltoNetworksXDR.Incident.severity | String | Calculated severity of the incident "low","medium","high" | 
| PaloAltoNetworksXDR.Incident.description | String | Dynamic calculated description of the incident. | 
| PaloAltoNetworksXDR.Incident.assigned_user_mail | String | Email address of the assigned user. | 
| PaloAltoNetworksXDR.Incident.assigned_user_pretty_name | String | Full name of the user assigned to the incident. | 
| PaloAltoNetworksXDR.Incident.alert_count | Number | Total number of alerts in the incident. | 
| PaloAltoNetworksXDR.Incident.low_severity_alert_count | Number | Number of alerts with the severity LOW. | 
| PaloAltoNetworksXDR.Incident.med_severity_alert_count | Number | Number of alerts with the severity MEDIUM. | 
| PaloAltoNetworksXDR.Incident.high_severity_alert_count | Number | Number of alerts with the severity HIGH. | 
| PaloAltoNetworksXDR.Incident.user_count | Number | Number of users involved in the incident. | 
| PaloAltoNetworksXDR.Incident.host_count | Number | Number of hosts involved in the incident | 
| PaloAltoNetworksXDR.Incident.notes | Unknown | Comments entered by the user regarding the incident. | 
| PaloAltoNetworksXDR.Incident.resolve_comment | String | Comments entered by the user when the incident was resolved. | 
| PaloAltoNetworksXDR.Incident.manual_severity | String | Incident severity assigned by the user. This does not affect the calculated severity low medium high | 
| PaloAltoNetworksXDR.Incident.manual_description | String | Incident description provided by the user. | 
| PaloAltoNetworksXDR.Incident.xdr_url | String | A link to the incident view on XDR. | 
| PaloAltoNetworksXDR.Incident.starred | Boolean | Incident starred | 
| PaloAltoNetworksXDR.Incident.alerts.alert_id | String | Unique ID for each alert. | 
| PaloAltoNetworksXDR.Incident.alerts.detection_timestamp | Date | Date and time that the alert occurred. | 
| PaloAltoNetworksXDR.Incident.alerts.source | String | Source of the alert. The product/vendor this alert came from. | 
| PaloAltoNetworksXDR.Incident.alerts.severity | String | Severity of the alert.,"low","medium","high""" | 
| PaloAltoNetworksXDR.Incident.alerts.name | String | Calculated name of the alert. | 
| PaloAltoNetworksXDR.Incident.alerts.category | String | Category of the alert, for example, Spyware Detected via Anti-Spyware profile. | 
| PaloAltoNetworksXDR.Incident.alerts.description | String | Textual description of the alert. | 
| PaloAltoNetworksXDR.Incident.alerts.host_ip | String | Host IP involved in the alert. | 
| PaloAltoNetworksXDR.Incident.alerts.host_name | String | Host name involved in the alert. | 
| PaloAltoNetworksXDR.Incident.alerts.user_name | String | User name involved with the alert. | 
| PaloAltoNetworksXDR.Incident.alerts.event_type | String | Event type "Process Execution","Network Event","File Event","Registry Event","Injection Event","Load Image Event","Windows Event Log" | 
| PaloAltoNetworksXDR.Incident.alerts.action | String | The action that triggered the alert. "REPORTED", "BLOCKED", "POST_DETECTED", "SCANNED", "DOWNLOAD", "PROMPT_ALLOW", "PROMPT_BLOCK", "DETECTED", "BLOCKED_1", "BLOCKED_2", "BLOCKED_3", "BLOCKED_5", "BLOCKED_6", "BLOCKED_7", "BLOCKED_8", "BLOCKED_9", "BLOCKED_10", "BLOCKED_11", "BLOCKED_13", "BLOCKED_14", "BLOCKED_15", "BLOCKED_16", "BLOCKED_17", "BLOCKED_24", "BLOCKED_25", "DETECTED_0", "DETECTED_4", "DETECTED_18", "DETECTED_19", "DETECTED_20", "DETECTED_21", "DETECTED_22", "DETECTED_23" | 
| PaloAltoNetworksXDR.Incident.alerts.action_pretty | String | The action that triggered the alert "Detected (Reported)" "Prevented (Blocked)" "Detected (Post Detected)" "Detected (Scanned)" "Detected (Download)" "Detected (Prompt Allow)" "Prevented (Prompt Block)" "Detected" "Prevented (Denied The Session)" "Prevented (Dropped The Session)" "Prevented (Dropped The Session And Sent a TCP Reset)" "Prevented (Blocked The URL)" "Prevented (Blocked The IP)" "Prevented (Dropped The Packet)" "Prevented (Dropped All Packets)" "Prevented (Terminated The Session And Sent a TCP Reset To Both Sides Of The Connection)" "Prevented (Terminated The Session And Sent a TCP Reset To The Client)" "Prevented (Terminated The Session And Sent a TCP Reset To The Server)" "Prevented (Continue)" "Prevented (Block-Override)" "Prevented (Override-Lockout)" "Prevented (Override)" "Prevented (Random-Drop)" "Prevented (Silently Dropped The Session With An ICMP Unreachable Message To The Host Or Application)" "Prevented (Block)" "Detected (Allowed The Session)" "Detected (Raised An Alert)" "Detected (Syncookie Sent)" "Detected (Forward)" "Detected (Wildfire Upload Success)" "Detected (Wildfire Upload Failure)" "Detected (Wildfire Upload Skip)" "Detected (Sinkhole)" | 
| PaloAltoNetworksXDR.Incident.alerts.actor_process_image_name | String | Image name | 
| PaloAltoNetworksXDR.Incident.alerts.actor_process_command_line | String | Command line | 
| PaloAltoNetworksXDR.Incident.alerts.actor_process_signature_status | String | Signature status "Signed" "Invalid Signature" "Unsigned" "Revoked" "Signature Fail" "N/A" "Weak Hash" | 
| PaloAltoNetworksXDR.Incident.alerts.actor_process_signature_vendor | String | Singature vendor name | 
| PaloAltoNetworksXDR.Incident.alerts.causality_actor_process_image_name | String | Image name | 
| PaloAltoNetworksXDR.Incident.alerts.causality_actor_process_command_line | String | Command line | 
| PaloAltoNetworksXDR.Incident.alerts.causality_actor_process_signature_status | String | Signature status "Signed" "Invalid Signature" "Unsigned" "Revoked" "Signature Fail" "N/A" "Weak Hash" | 
| PaloAltoNetworksXDR.Incident.alerts.causality_actor_process_signature_vendor | String | Signature vendor | 
| PaloAltoNetworksXDR.Incident.alerts.causality_actor_causality_id | Unknown | Causality id | 
| PaloAltoNetworksXDR.Incident.alerts.action_process_image_name | String | Image name | 
| PaloAltoNetworksXDR.Incident.alerts.action_process_image_command_line | String | Command line | 
| PaloAltoNetworksXDR.Incident.alerts.action_process_image_sha256 | String | Image SHA256 | 
| PaloAltoNetworksXDR.Incident.alerts.action_process_signature_status | String | Signature status "Signed" "Invalid Signature" "Unsigned" "Revoked" "Signature Fail" "N/A" "Weak Hash" | 
| PaloAltoNetworksXDR.Incident.alerts.action_process_signature_vendor | String | Signature vendor name | 
| PaloAltoNetworksXDR.Incident.alerts.action_file_path | String | File path | 
| PaloAltoNetworksXDR.Incident.alerts.action_file_md5 | String | File MD5 | 
| PaloAltoNetworksXDR.Incident.alerts.action_file_sha256 | String | File SHA256 | 
| PaloAltoNetworksXDR.Incident.alerts.action_registry_data | String | Registry data | 
| PaloAltoNetworksXDR.Incident.alerts.action_registry_full_key | String | Registry full key | 
| PaloAltoNetworksXDR.Incident.alerts.action_local_ip | String | Local IP | 
| PaloAltoNetworksXDR.Incident.alerts.action_local_port | Number | Local port | 
| PaloAltoNetworksXDR.Incident.alerts.action_remote_ip | String | Remote IP | 
| PaloAltoNetworksXDR.Incident.alerts.action_remote_port | Number | Remote port | 
| PaloAltoNetworksXDR.Incident.alerts.action_external_hostname | String | External hostname | 
| PaloAltoNetworksXDR.Incident.alerts.fw_app_id | Unknown | Firewall app id | 
| PaloAltoNetworksXDR.Incident.alerts.is_whitelisted | String | Is whitelisted "Yes" "No" | 
| PaloAltoNetworksXDR.Incident.alerts.starred | Boolean | Alert starred | 
| PaloAltoNetworksXDR.Incident.network_artifacts.type | String | Network artifact type "IP" | 
| PaloAltoNetworksXDR.Incident.network_artifacts.network_remote_port | number | The remote port related to the artifact. | 
| PaloAltoNetworksXDR.Incident.network_artifacts.alert_count | number | Number of alerts related to the artifact. | 
| PaloAltoNetworksXDR.Incident.network_artifacts.network_remote_ip | String | The remote IP related to the artifact. | 
| PaloAltoNetworksXDR.Incident.network_artifacts.is_manual | boolean | Whether the artifact was created by the user (manually). | 
| PaloAltoNetworksXDR.Incident.network_artifacts.network_domain | String | The domain related to the artifact. | 
| PaloAltoNetworksXDR.Incident.network_artifacts.type | String | The artifact type. "META", "GID", "CID", "HASH", "IP", "DOMAIN", "REGISTRY", "HOSTNAME" | 
| PaloAltoNetworksXDR.Incident.network_artifacts.network_country | String | The country related to the artifact | 
| PaloAltoNetworksXDR.Incident.file_artifacts.file_signature_status | String | Digital signature status of the file. "SIGNATURE_UNAVAILABLE" "SIGNATURE_SIGNED" "SIGNATURE_INVALID" "SIGNATURE_UNSIGNED" "SIGNATURE_WEAK_HASH" | 
| PaloAltoNetworksXDR.Incident.file_artifacts.is_process | boolean | Whether the file artifact is related to a process execution. | 
| PaloAltoNetworksXDR.Incident.file_artifacts.file_name | String | Name of the file. | 
| PaloAltoNetworksXDR.Incident.file_artifacts.file_wildfire_verdict | String | The file verdict, calculated by Wildfire. "BENIGN" "MALWARE" "GRAYWARE" "PHISING" "UNKNOWN" | 
| PaloAltoNetworksXDR.Incident.file_artifacts.alert_count | number | Number of alerts related to the artifact. | 
| PaloAltoNetworksXDR.Incident.file_artifacts.is_malicious | boolean | Whether the artifact is malicious, decided by the Wildfire verdic | 
| PaloAltoNetworksXDR.Incident.file_artifacts.is_manual | boolean | Whether the artifact was created by the user (manually). | 
| PaloAltoNetworksXDR.Incident.file_artifacts.type | String | The artifact type "META" "GID" "CID" "HASH" "IP" "DOMAIN" "REGISTRY" "HOSTNAME" | 
| PaloAltoNetworksXDR.Incident.file_artifacts.file_sha256 | String | SHA-256 hash of the file | 
| PaloAltoNetworksXDR.Incident.file_artifacts.file_signature_vendor_name | String | File signature vendor name | 
| Account.Username | String | The username in the relevant system. | 
| Endpoint.Hostname | String | The hostname that is mapped to this endpoint. | 


##### Command Example
```!xdr-get-incident-extra-data incident_id=4 alerts_limit=10```

##### Context Example
```
{
    "Account": {
        "Username": [
            null
        ]
    },
    "Endpoint": {
        "Hostname": [
            null
        ]
    },
    "PaloAltoNetworksXDR.Incident": {
        "host_count": 1, 
        "manual_severity": "medium", 
        "xdr_url": "https://some.xdr.url.com/incident-view/4", 
        "assigned_user_pretty_name": null, 
        "alert_count": 5, 
        "med_severity_alert_count": 1, 
        "detection_time": null, 
        "user_count": 1, 
        "severity": "medium", 
        "alerts": [
            {
                "action_process_signature_status": "N/A", 
                "action_pretty": [
                    "VALUE_NA", 
                    "N/A"
                ], 
                "event_type": "Network Event", 
                "alert_id": "6", 
                "action_file_sha256": null, 
                "action_external_hostname": null, 
                "causality_actor_process_command_line": null, 
                "description": "Test - alert generated by Test XDR Playbook", 
                "category": null, 
                "severity": "medium", 
                "source": "Cisco - Sandblast", 
                "action_remote_port": 8000, 
                "causality_actor_process_signature_status": "N/A", 
                "fw_app_id": null, 
                "is_whitelisted": "No", 
                "action_local_ip": "196.168.0.1", 
                "action_registry_data": null, 
                "action_process_image_sha256": null, 
                "user_name": null, 
                "action_remote_ip": "2.2.2.2", 
                "action_process_signature_vendor": "N/A", 
                "actor_process_signature_status": "N/A", 
                "name": "Test - alert generated by Test XDR Playbook", 
                "causality_actor_causality_id": null, 
                "host_ip": null, 
                "action_process_image_name": null, 
                "detection_timestamp": 1577276586921, 
                "action_file_md5": null, 
                "causality_actor_process_image_name": null, 
                "action_file_path": null, 
                "action_process_image_command_line": null, 
                "action_local_port": 7000, 
                "actor_process_image_name": null, 
                "action_registry_full_key": null, 
                "actor_process_signature_vendor": "N/A", 
                "actor_process_command_line": null, 
                "host_name": null, 
                "action": [
                    "VALUE_NA", 
                    "N/A"
                ], 
                "starred": false, 
                "causality_actor_process_signature_vendor": "N/A"
            }, 
            {
                "action_process_signature_status": "N/A", 
                "action_pretty": [
                    "VALUE_NA", 
                    "N/A"
                ], 
                "event_type": "Network Event", 
                "alert_id": "7", 
                "action_file_sha256": null, 
                "action_external_hostname": null, 
                "causality_actor_process_command_line": null, 
                "description": "This alert from content  TestXDRPlaybook description", 
                "category": null, 
                "severity": "high", 
                "source": "Checkpoint - SandBlast", 
                "action_remote_port": 6000, 
                "causality_actor_process_signature_status": "N/A", 
                "fw_app_id": null, 
                "is_whitelisted": "No", 
                "action_local_ip": "196.168.0.111", 
                "action_registry_data": null, 
                "action_process_image_sha256": null, 
                "user_name": null, 
                "action_remote_ip": "2.2.2.2", 
                "action_process_signature_vendor": "N/A", 
                "actor_process_signature_status": "N/A", 
                "name": "This alert from content  TestXDRPlaybook", 
                "causality_actor_causality_id": null, 
                "host_ip": null, 
                "action_process_image_name": null, 
                "detection_timestamp": 1577776701589, 
                "action_file_md5": null, 
                "causality_actor_process_image_name": null, 
                "action_file_path": null, 
                "action_process_image_command_line": null, 
                "action_local_port": 2000, 
                "actor_process_image_name": null, 
                "action_registry_full_key": null, 
                "actor_process_signature_vendor": "N/A", 
                "actor_process_command_line": null, 
                "host_name": null, 
                "action": [
                    "VALUE_NA", 
                    "N/A"
                ], 
                "starred": false, 
                "causality_actor_process_signature_vendor": "N/A"
            }, 
            {
                "action_process_signature_status": "N/A", 
                "action_pretty": [
                    "VALUE_NA", 
                    "N/A"
                ], 
                "event_type": "Network Event", 
                "alert_id": "8", 
                "action_file_sha256": null, 
                "action_external_hostname": null, 
                "causality_actor_process_command_line": null, 
                "description": "This alert from content  TestXDRPlaybook description", 
                "category": null, 
                "severity": "high", 
                "source": "Checkpoint - SandBlast", 
                "action_remote_port": 6000, 
                "causality_actor_process_signature_status": "N/A", 
                "fw_app_id": null, 
                "is_whitelisted": "No", 
                "action_local_ip": "196.168.0.111", 
                "action_registry_data": null, 
                "action_process_image_sha256": null, 
                "user_name": null, 
                "action_remote_ip": "2.2.2.2", 
                "action_process_signature_vendor": "N/A", 
                "actor_process_signature_status": "N/A", 
                "name": "This alert from content  TestXDRPlaybook", 
                "causality_actor_causality_id": null, 
                "host_ip": null, 
                "action_process_image_name": null, 
                "detection_timestamp": 1577958479843, 
                "action_file_md5": null, 
                "causality_actor_process_image_name": null, 
                "action_file_path": null, 
                "action_process_image_command_line": null, 
                "action_local_port": 2000, 
                "actor_process_image_name": null, 
                "action_registry_full_key": null, 
                "actor_process_signature_vendor": "N/A", 
                "actor_process_command_line": null, 
                "host_name": null, 
                "action": [
                    "VALUE_NA", 
                    "N/A"
                ], 
                "starred": false, 
                "causality_actor_process_signature_vendor": "N/A"
            }, 
            {
                "action_process_signature_status": "N/A", 
                "action_pretty": [
                    "VALUE_NA", 
                    "N/A"
                ], 
                "event_type": "Network Event", 
                "alert_id": "9", 
                "action_file_sha256": null, 
                "action_external_hostname": null, 
                "causality_actor_process_command_line": null, 
                "description": "This alert from content  TestXDRPlaybook description", 
                "category": null, 
                "severity": "high", 
                "source": "Checkpoint - SandBlast", 
                "action_remote_port": 6000, 
                "causality_actor_process_signature_status": "N/A", 
                "fw_app_id": null, 
                "is_whitelisted": "No", 
                "action_local_ip": "196.168.0.111", 
                "action_registry_data": null, 
                "action_process_image_sha256": null, 
                "user_name": null, 
                "action_remote_ip": "2.2.2.2", 
                "action_process_signature_vendor": "N/A", 
                "actor_process_signature_status": "N/A", 
                "name": "This alert from content  TestXDRPlaybook", 
                "causality_actor_causality_id": null, 
                "host_ip": null, 
                "action_process_image_name": null, 
                "detection_timestamp": 1578123895414, 
                "action_file_md5": null, 
                "causality_actor_process_image_name": null, 
                "action_file_path": null, 
                "action_process_image_command_line": null, 
                "action_local_port": 2000, 
                "actor_process_image_name": null, 
                "action_registry_full_key": null, 
                "actor_process_signature_vendor": "N/A", 
                "actor_process_command_line": null, 
                "host_name": null, 
                "action": [
                    "VALUE_NA", 
                    "N/A"
                ], 
                "starred": false, 
                "causality_actor_process_signature_vendor": "N/A"
            }, 
            {
                "action_process_signature_status": "N/A", 
                "action_pretty": [
                    "VALUE_NA", 
                    "N/A"
                ], 
                "event_type": "Network Event", 
                "alert_id": "10", 
                "action_file_sha256": null, 
                "action_external_hostname": null, 
                "causality_actor_process_command_line": null, 
                "description": "This alert from content  TestXDRPlaybook description", 
                "category": null, 
                "severity": "high", 
                "source": "Checkpoint - SandBlast", 
                "action_remote_port": 6000, 
                "causality_actor_process_signature_status": "N/A", 
                "fw_app_id": null, 
                "is_whitelisted": "No", 
                "action_local_ip": "196.168.0.111", 
                "action_registry_data": null, 
                "action_process_image_sha256": null, 
                "user_name": null, 
                "action_remote_ip": "2.2.2.2", 
                "action_process_signature_vendor": "N/A", 
                "actor_process_signature_status": "N/A", 
                "name": "This alert from content  TestXDRPlaybook", 
                "causality_actor_causality_id": null, 
                "host_ip": null, 
                "action_process_image_name": null, 
                "detection_timestamp": 1578927443615, 
                "action_file_md5": null, 
                "causality_actor_process_image_name": null, 
                "action_file_path": null, 
                "action_process_image_command_line": null, 
                "action_local_port": 2000, 
                "actor_process_image_name": null, 
                "action_registry_full_key": null, 
                "actor_process_signature_vendor": "N/A", 
                "actor_process_command_line": null, 
                "host_name": null, 
                "action": [
                    "VALUE_NA", 
                    "N/A"
                ], 
                "starred": false, 
                "causality_actor_process_signature_vendor": "N/A"
            }
        ], 
        "low_severity_alert_count": 0, 
        "status": "new", 
        "description": "5 'This alert from content  TestXDRPlaybook' alerts detected by Checkpoint - SandBlast  ", 
        "resolve_comment": "This issue was solved in Incident number 192304", 
        "creation_time": 1577276587937, 
        "modification_time": 1579290004178, 
        "network_artifacts": [
            {
                "network_remote_port": 8000, 
                "alert_count": 5, 
                "network_remote_ip": "2.2.2.2", 
                "is_manual": false, 
                "network_domain": null, 
                "type": "IP", 
                "network_country": null
            }
        ], 
        "file_artifacts": [], 
        "manual_description": null, 
        "incident_id": "4", 
        "notes": null, 
        "assigned_user_mail": null, 
        "starred": false, 
        "high_severity_alert_count": 4
    }
}
```

##### Human Readable Output
### Incident 4
|alert_count|assigned_user_mail|assigned_user_pretty_name|creation_time|description|detection_time|high_severity_alert_count|host_count|incident_id|low_severity_alert_count|manual_description|manual_severity|med_severity_alert_count|modification_time|notes|resolve_comment|severity|starred|status|user_count|xdr_url|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 5 |  |  | 1577276587937 | 5 'This alert from content  TestXDRPlaybook' alerts detected by Checkpoint - SandBlast   |  | 4 | 1 | 4 | 0 |  | medium | 1 | 1579290004178 |  | This issue was solved in Incident number 192304 | medium | false | new | 1 | `https://some.xdr.url.com/incident-view/4` |

### Alerts
|action|action_external_hostname|action_file_md5|action_file_path|action_file_sha256|action_local_ip|action_local_port|action_pretty|action_process_image_command_line|action_process_image_name|action_process_image_sha256|action_process_signature_status|action_process_signature_vendor|action_registry_data|action_registry_full_key|action_remote_ip|action_remote_port|actor_process_command_line|actor_process_image_name|actor_process_signature_status|actor_process_signature_vendor|alert_id|category|causality_actor_causality_id|causality_actor_process_command_line|causality_actor_process_image_name|causality_actor_process_signature_status|causality_actor_process_signature_vendor|description|detection_timestamp|event_type|fw_app_id|host_ip|host_name|is_whitelisted|name|severity|source|starred|user_name|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| VALUE_NA,<br/>N/A |  |  |  |  | 196.168.0.1 | 7000 | VALUE_NA,<br/>N/A |  |  |  | N/A | N/A |  |  | 2.2.2.2 | 8000 |  |  | N/A | N/A | 6 |  |  |  |  | N/A | N/A | Test - alert generated by Test XDR Playbook | 1577276586921 | Network Event |  |  |  | No | Test - alert generated by Test XDR Playbook | medium | Cisco - Sandblast | false |  |
| VALUE_NA,<br/>N/A |  |  |  |  | 196.168.0.111 | 2000 | VALUE_NA,<br/>N/A |  |  |  | N/A | N/A |  |  | 2.2.2.2 | 6000 |  |  | N/A | N/A | 7 |  |  |  |  | N/A | N/A | This alert from content  TestXDRPlaybook description | 1577776701589 | Network Event |  |  |  | No | This alert from content  TestXDRPlaybook | high | Checkpoint - SandBlast | false |  |
| VALUE_NA,<br/>N/A |  |  |  |  | 196.168.0.111 | 2000 | VALUE_NA,<br/>N/A |  |  |  | N/A | N/A |  |  | 2.2.2.2 | 6000 |  |  | N/A | N/A | 8 |  |  |  |  | N/A | N/A | This alert from content  TestXDRPlaybook description | 1577958479843 | Network Event |  |  |  | No | This alert from content  TestXDRPlaybook | high | Checkpoint - SandBlast | false |  |
| VALUE_NA,<br/>N/A |  |  |  |  | 196.168.0.111 | 2000 | VALUE_NA,<br/>N/A |  |  |  | N/A | N/A |  |  | 2.2.2.2 | 6000 |  |  | N/A | N/A | 9 |  |  |  |  | N/A | N/A | This alert from content  TestXDRPlaybook description | 1578123895414 | Network Event |  |  |  | No | This alert from content  TestXDRPlaybook | high | Checkpoint - SandBlast | false |  |
| VALUE_NA,<br/>N/A |  |  |  |  | 196.168.0.111 | 2000 | VALUE_NA,<br/>N/A |  |  |  | N/A | N/A |  |  | 2.2.2.2 | 6000 |  |  | N/A | N/A | 10 |  |  |  |  | N/A | N/A | This alert from content  TestXDRPlaybook description | 1578927443615 | Network Event |  |  |  | No | This alert from content  TestXDRPlaybook | high | Checkpoint - SandBlast | false |  |

### Network Artifacts
|alert_count|is_manual|network_country|network_domain|network_remote_ip|network_remote_port|type|
|---|---|---|---|---|---|---|
| 5 | false |  |  | 2.2.2.2 | 8000 | IP |

### File Artifacts
**No entries.**


### 3. xdr-update-incident
---
Updates one or more fields of a specified incident. Missing fields will be ignored. To remove the assignment for an incident, pass a null value in assignee email argument.
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-update-incident`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| incident_id | XDR incident ID. You can get the incident ID from the output of the 'xdr-get-incidents' command or the 'xdr-get-incident-extra-details' command. | Required | 
| manual_severity | Severity to assign to the incident (LOW, MEDIUM, or HIGH). | Optional | 
| assigned_user_mail | Email address of the user to assigned to the incident. | Optional | 
| assigned_user_pretty_name | Full name of the user assigned to the incident. | Optional | 
| status | Status of the incident (NEW, UNDER_INVESTIGATION, RESOLVED_THREAT_HANDLED, RESOLVED_KNOWN_ISSUE, RESOLVED_DUPLICATE, RESOLVED_FALSE_POSITIVE, RESOLVED_OTHER). | Optional | 
| resolve_comment | Comment explaining why the incident was resolved. This should be set when the incident is resolved. | Optional | 
| unassign_user | If true, will remove all assigned users from the incident. | Optional | 


##### Context Output

There is no context output for this command.

##### Command Example
```!xdr-update-incident incident_id="4" status="RESOLVED_KNOWN_ISSUE" resolve_comment="This issue was solved in Incident number 192304"```

##### Human Readable Output
Incident 4 has been updated

### 4. xdr-insert-parsed-alert
---
Upload alert from external alert sources in Cortex XDR format. Cortex XDR displays alerts that are parsed
successfully in related incidents and views. You can send 600 alerts per minute. Each request can contain a
maximum of 60 alerts.
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-insert-parsed-alert`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| product | String value that defines the product. | Required | 
| vendor | String value that defines the product. | Required | 
| local_ip | String value for the source IP address | Optional | 
| local_port | Integer value for the source port. | Required | 
| remote_ip | String value of the destination IP<br/>address. | Required | 
| remote_port | Integer value for the destination<br/>port. | Required | 
| event_timestampt | Integer value representing the epoch of the time the alert occurred in milliseconds or String value of date format 2019-10-23T10:00:00. If not set then the event time will be defined as now. | Optional | 
| severity | String value of alert severity:<br/>Informational, Low, Medium, High, or Unknown | Optional | 
| alert_name | String defining the alert name | Required | 
| alert_description | String defining the alert description | Optional | 


##### Context Output

There is no context output for this command.

##### Command Example
```!xdr-insert-parsed-alert product="SandBlast" vendor="Checkpoint" local_ip="196.168.0.1" local_port="600" remote_ip="5.5.5.5" remote_port="500" event_timestampt="2020-01-01T00:00:00" severity="High" alert_name="some alert" alert_description="this is test alert"```

##### Human Readable Output
Alert inserted successfully

### 5. xdr-insert-cef-alerts
---
Upload alerts in CEF format from external alert sources. After you map CEF alert fields to Cortex XDR fields, Cortex XDR displays the alerts in related incidents and views. You can send 600 requests per minute. Each request can contain a maximum of 60 alerts.
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-insert-cef-alerts`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| cef_alerts | List of alerts in CEF format. | Required | 


##### Context Output

There is no context output for this command.

##### Command Example
```!xdr-insert-cef-alerts cef_alerts="CEF:0|Check Point|VPN-1 & FireWall-1|Check Point|Log|microsoft-ds|Unknown|act=AcceptdeviceDirection=0 rt=1569477512000 spt=56957 dpt=445 cs2Label=Rule Name cs2=ADPrimery layer_name=FW_Device_blackened Securitylayer_uuid=07693fc7-1a5c-4f31-8afe-77ae96c71b8c match_id=1806 parent_rule=0rule_action=Accept rule_uid=8e45f36b-d106-4d81-a1f0-9d1ed9a6be5c ifname=bond2logid=0 loguid={0x5d8c5388,0x61,0x29321fac,0xc0000022} origin=1.1.1.1originsicname=CN=DWdeviceBlackend,O=Blackend sequencenum=363 version=5dst=1.1.1.1 inzone=External outzone=Internal product=VPN-1 & FireWall-1 proto=6service_id=microsoft-ds src=1.1.1.1"```

##### Human Readable Output
Alerts inserted successfully

### 6. xdr-isolate-endpoint
---
Isolates the specified endpoint.
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-isolate-endpoint`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| endpoint_id | The endpoint ID (string) to isolate. You can retrieve the string from the xdr-get-endpoints | Required | 


##### Context Output

There is no context output for this command.

##### Command Example
```!xdr-isolate-endpoint endpoint_id="f8a2f58846b542579c12090652e79f3d"```

##### Human Readable Output
Endpoint f8a2f58846b542579c12090652e79f3d has isolated successfully

### 7. xdr-unisolate-endpoint
---
Reverses the isolation of an endpoint.
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-unisolate-endpoint`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| endpoint_id | The endpoint ID (string) for which to reverse the isolation. You can retrieve it from the xdr-get-endpoints | Required | 


##### Context Output

There is no context output for this command.

##### Command Example
```!xdr-unisolate-endpoint endpoint_id="f8a2f58846b542579c12090652e79f3d"```

##### Human Readable Output
Endpoint f8a2f58846b542579c12090652e79f3d already unisolated

### 8. xdr-get-endpoints
---
Gets a list of endpoints, according to the passed filters. Filtering by multiple fields will be concatenated using AND condition (OR is not supported). Maximum result set size is 100. Offset is the zero-based number of endpoint from the start of the result set (start by counting from 0).
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-get-endpoints`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| endpoint_id_list | A comma-separated list of endpoint IDs. | Optional | 
| dist_name | A comma-separated list of distribution package names or installation package names. <br/>Example: dist_name1,dist_name2 | Optional | 
| ip_list | A comma-separated list of IP addresses.<br/>Example: 8.8.8.8,1.1.1.1 | Optional | 
| group_name | The group name to which the agent belongs.<br/>Example: group_name1,group_name2 | Optional | 
| platform | The endpoint platform. Can be "windows", "linux", "macos", or "android".  | Optional | 
| alias_name | A comma-separated list of alias names.<br/>Examples: alias_name1,alias_name2 | Optional | 
| isolate | "Specifies whether the endpoint was isolated or unisolated. Can be "isolated" or "unisolated". | Optional | 
| hostname | Hostname<br/>Example: hostname1,hostname2 | Optional | 
| first_seen_gte | All the agents that were first seen after {first_seen_gte}.<br/>Supported values:<br/>1579039377301 (time in milliseconds)<br/>"3 days" (relative date)<br/>"2019-10-21T23:45:00" (date) | Optional | 
| first_seen_lte | All the agents that were first seen before {first_seen_lte}.<br/>Supported values:<br/>1579039377301 (time in milliseconds)<br/>"3 days" (relative date)<br/>"2019-10-21T23:45:00" (date) | Optional | 
| last_seen_gte | All the agents that were last seen before {last_seen_gte}.<br/>Supported values:<br/>1579039377301 (time in milliseconds)<br/>"3 days" (relative date)<br/>"2019-10-21T23:45:00" (date) | Optional | 
| last_seen_lte | All the agents that were last seen before {last_seen_lte}.<br/>Supported values:<br/>1579039377301 (time in milliseconds)<br/>"3 days" (relative date)<br/>"2019-10-21T23:45:00" (date) | Optional | 
| page | Page number (for pagination). The default is 0 (the first page). | Optional | 
| limit | Maximum number of endpoints to return per page. The default and maximum is 30. | Optional | 
| sort_by | Specifies whether to sort endpoints by the first time or last time they were seen. Can be "first_seen" or "last_seen". | Optional | 
| sort_order | The order by which to sort results. Can be "asc" (ascending) or "desc" ( descending). Default set to asc. | Optional | 


##### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.Endpoint.endpoint_id | String | The endpoint ID. | 
| PaloAltoNetworksXDR.Endpoint.endpoint_name | String | The endpoint name. | 
| PaloAltoNetworksXDR.Endpoint.endpoint_type | String | The endpoint type. | 
| PaloAltoNetworksXDR.Endpoint.endpoint_status | String | The status of the endpoint' | 
| PaloAltoNetworksXDR.Endpoint.os_type | String | The endpoint OS type. | 
| PaloAltoNetworksXDR.Endpoint.ip | Unknown | A list of IP addresses. | 
| PaloAltoNetworksXDR.Endpoint.users | Unknown | A list of users. | 
| PaloAltoNetworksXDR.Endpoint.domain | String | The endpoint domain. | 
| PaloAltoNetworksXDR.Endpoint.alias | String | The endpoint's aliases. | 
| PaloAltoNetworksXDR.Endpoint.first_seen | Unknown | First seen date/time in Epoch (milliseconds). | 
| PaloAltoNetworksXDR.Endpoint.last_seen | Date | Last seen date/time in Epoch (milliseconds). | 
| PaloAltoNetworksXDR.Endpoint.content_version | String | Content version. | 
| PaloAltoNetworksXDR.Endpoint.installation_package | String | Installation package. | 
| PaloAltoNetworksXDR.Endpoint.active_directory | String | Active directory. | 
| PaloAltoNetworksXDR.Endpoint.install_date | Date | Install date in Epoch (milliseconds). | 
| PaloAltoNetworksXDR.Endpoint.endpoint_version | String | Endpoint version. | 
| PaloAltoNetworksXDR.Endpoint.is_isolated | String | Whether the endpoint is isolated. | 
| PaloAltoNetworksXDR.Endpoint.group_name | String | The name of the group to which the endpoint belongs. | 
| Endpoint.Hostname | String | The hostname that is mapped to this endpoint. | 
| Endpoint.ID | String | The unique ID within the tool retrieving the endpoint. | 
| Endpoint.IPAddress | String | The IP address of the endpoint. | 
| Endpoint.Domain | String | The domain of the endpoint. | 
| Endpoint.OS | String | Endpoint OS. | 

##### Command Example
```!xdr-get-endpoints isolate="unisolated" first_seen_gte="3 month" page="0" limit="30" sort_order="asc"```

##### Context Example
```
{
    "Endpoint": [
        {
            "Domain": "WORKGROUP",
            "Hostname": "aaaaa.compute.internal",
            "ID": "ea303670c76e4ad09600c8b346f7c804",
            "IPAddress": [
                "172.31.11.11"
            ],
            "OS": "AGENT_OS_WINDOWS"
        },
        {
            "Domain": "WORKGROUP",
            "Hostname": "EC2AMAZ-P7PPOI4",
            "ID": "f8a2f58846b542579c12090652e79f3d",
            "IPAddress": [
                "2.2.2.2"
            ],
            "OS": "AGENT_OS_WINDOWS"
        }
    ],
    "PaloAltoNetworksXDR.Endpoint": [
        {
            "domain": "", 
            "users": [
                "ec2-user"
            ], 
            "endpoint_name": "aaaaa.compute.internal", 
            "ip": [
                "172.31.11.11"
            ], 
            "install_date": 1575795969644, 
            "endpoint_version": "7.0.0.1915", 
            "group_name": null, 
            "installation_package": "linux", 
            "alias": "", 
            "active_directory": null, 
            "endpoint_status": "CONNECTED", 
            "os_type": "AGENT_OS_LINUX", 
            "endpoint_id": "ea303670c76e4ad09600c8b346f7c804", 
            "content_version": "111-17757", 
            "first_seen": 1575795969644, 
            "endpoint_type": "AGENT_TYPE_SERVER", 
            "is_isolated": "AGENT_UNISOLATED", 
            "last_seen": 1579290023629
        }, 
        {
            "domain": "WORKGROUP", 
            "users": [
                "Administrator"
            ], 
            "endpoint_name": "EC2AMAZ-P7PPOI4", 
            "ip": [
                "2.2.2.2"
            ], 
            "install_date": 1575796381739, 
            "endpoint_version": "7.0.0.27797", 
            "group_name": null, 
            "installation_package": "Windows Server 2016", 
            "alias": "", 
            "active_directory": null, 
            "endpoint_status": "CONNECTED", 
            "os_type": "AGENT_OS_WINDOWS", 
            "endpoint_id": "f8a2f58846b542579c12090652e79f3d", 
            "content_version": "111-17757", 
            "first_seen": 1575796381739, 
            "endpoint_type": "AGENT_TYPE_SERVER", 
            "is_isolated": "AGENT_UNISOLATED", 
            "last_seen": 1579289957412
        }
    ]
}
```

##### Human Readable Output
### Endpoints
|active_directory|alias|content_version|domain|endpoint_id|endpoint_name|endpoint_status|endpoint_type|endpoint_version|first_seen|group_name|install_date|installation_package|ip|is_isolated|last_seen|os_type|users|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|  |  | 111-17757 |  | ea303670c76e4ad09600c8b346f7c804 | aaaaa.compute.internal | CONNECTED | AGENT_TYPE_SERVER | 7.0.0.1915 | 1575795969644 |  | 1575795969644 | linux | 172.31.11.11 | AGENT_UNISOLATED | 1579290023629 | AGENT_OS_LINUX | ec2-user |
|  |  | 111-17757 | WORKGROUP | f8a2f58846b542579c12090652e79f3d | EC2AMAZ-P7PPOI4 | CONNECTED | AGENT_TYPE_SERVER | 7.0.0.27797 | 1575796381739 |  | 1575796381739 | Windows Server 2016 | 2.2.2.2 | AGENT_UNISOLATED | 1579289957412 | AGENT_OS_WINDOWS | Administrator |


### 9. xdr-get-distribution-versions
---
Gets a list of all the agent versions to use for creating a distribution list.
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-get-distribution-versions`
##### Input

There are no input arguments for this command.

##### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.DistributionVersions.windows | Unknown | A list of Windows agent versions. | 
| PaloAltoNetworksXDR.DistributionVersions.linux | Unknown | A list of Linux agent versions. | 
| PaloAltoNetworksXDR.DistributionVersions.macos | Unknown | A list of Mac agent versions. | 


##### Command Example
```!xdr-get-distribution-versions```

##### Context Example
```
{
    "PaloAltoNetworksXDR.DistributionVersions": {
        "windows": [
            "5.0.8.29673", 
            "5.0.9.30963", 
            "6.1.4.28751", 
            "7.0.0.28644"
        ], 
        "macos": [
            "6.1.4.1681", 
            "7.0.0.1914"
        ], 
        "linux": [
            "6.1.4.1680", 
            "7.0.0.1916"
        ]
    }
}
```

##### Human Readable Output
### windows
|versions|
|---|
| 5.0.8.29673 |
| 5.0.9.30963 |
| 6.1.4.28751 |
| 7.0.0.28644 |


### linux
|versions|
|---|
| 6.1.4.1680 |
| 7.0.0.1916 |


### macos
|versions|
|---|
| 6.1.4.1681 |
| 7.0.0.1914 |


### 10. xdr-create-distribution
---
Creates an installation package. This is an asynchronous call that returns the distribution ID. This does not mean that the creation succeeded. To confirm that the package has been created, check the status of the distribution by running the Get Distribution Status API.
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-create-distribution`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| name | A string representing the name of the installation package. | Required | 
| platform | String, valid values are:<br/>• windows <br/>• linux<br/>• macos <br/>• android | Required | 
| package_type | A string representing the type of package to create.<br/>standalone - An installation for a new agent<br/>upgrade - An upgrade of an agent from ESM | Required | 
| agent_version | agent_version returned from xdr-get-distribution-versions. Not required for Android platfom | Required | 
| description | Information about the package. | Optional | 


##### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.Distribution.id | String | The installation package ID. | 
| PaloAltoNetworksXDR.Distribution.name | String | The name of the installation package. | 
| PaloAltoNetworksXDR.Distribution.platform | String | The installation OS. | 
| PaloAltoNetworksXDR.Distribution.agent_version | String | Agent version. | 
| PaloAltoNetworksXDR.Distribution.description | String | Information about the package. | 


##### Command Example
```!xdr-create-distribution agent_version=6.1.4.1680 name="dist_1" package_type=standalone platform=linux description="some description"```

##### Context Example
```
{
    "PaloAltoNetworksXDR.Distribution": {
        "description": "some description", 
        "package_type": "standalone", 
        "platform": "linux", 
        "agent_version": "6.1.4.1680", 
        "id": "43aede7f846846fa92b50149663fbb25", 
        "name": "dist_1"
    }
}
```

##### Human Readable Output
Distribution 43aede7f846846fa92b50149663fbb25 created successfully

### 11. xdr-get-distribution-url
---
Gets the distribution URL for downloading the installation package.
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-get-distribution-url`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| distribution_id | The ID of the installation package.<br/>Copy the distribution_id from the "id" field on Endpoints > Agent Installation page. | Required | 
| package_type | The installation package type. Valid<br/>values are:<br/>• upgrade<br/>• sh - For Linux<br/>• rpm - For Linux<br/>• deb - For Linux<br/>• pkg - For Mac<br/>• x86 - For Windows<br/>• x64 - For Windows | Required | 


##### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.Distribution.id | String | Distribution ID. | 
| PaloAltoNetworksXDR.Distribution.url | String | URL for downloading the installation package. | 


##### Command Example
```!xdr-get-distribution-url distribution_id=2c74c11b63074653aa01d575a82bf52a package_type=sh```

##### Human Readable Output


### 12. xdr-get-create-distribution-status
---
Gets the status of the installation package.
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-get-create-distribution-status`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| distribution_ids | A comma-separated list of distribution IDs to get the status of. | Required | 


##### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.Distribution.id | String | Distribution ID. | 
| PaloAltoNetworksXDR.Distribution.status | String | The status of installation package. | 


##### Command Example
```!xdr-get-create-distribution-status distribution_ids=2c74c11b63074653aa01d575a82bf52a```

##### Context Example
```
{
    "PaloAltoNetworksXDR.Distribution": [
        {
            "status": "Completed", 
            "id": "2c74c11b63074653aa01d575a82bf52a"
        }
    ]
}
```

##### Human Readable Output
### Distribution Status
|id|status|
|---|---|
| 2c74c11b63074653aa01d575a82bf52a | Completed |


### 13. xdr-get-audit-management-logs
---
Gets management logs. You can filter by multiple fields, which will be concatenated using AND condition (OR is not supported). Maximum result set size is 100. Offset is the zero-based number of management logs from the start of the result set (start by counting from 0).
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-get-audit-management-logs`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| email | User’s email address. | Optional | 
| type | The audit log type. | Optional | 
| sub_type | The audit log subtype. | Optional | 
| result | Result type | Optional | 
| timestamp_gte | Return logs for which the timestamp is after 'log_time_after'.<br/>Supported values:<br/>1579039377301 (time in milliseconds)<br/>"3 days" (relative date)<br/>"2019-10-21T23:45:00" (date) | Optional | 
| timestamp_lte | Return logs for which the timestamp is before the 'log_time_after'.<br/>Supported values:<br/>1579039377301 (time in milliseconds)<br/>"3 days" (relative date)<br/>"2019-10-21T23:45:00" (date) | Optional | 
| page | Page number (for pagination). The default is 0 (the first page). | Optional | 
| limit | Maximum number of audit logs to return per page. The default and maximum is 30. | Optional | 
| sort_by | Specifies the field by which to sort the results. By default the sort is defined as creation-time and DESC. Can be "type", "sub_type", "result", or "timestamp". | Optional | 
| sort_order | The sort order. Can be "asc" (ascending) or "desc" (descending). Default set to "desc".  | Optional | 


##### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_ID | Number | Audit log ID. | 
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_OWNER_NAME | String | Audit owner name. | 
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_OWNER_EMAIL | String | Audit owner email address. | 
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_ASSET_JSON | String | Asset JSON. | 
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_ASSET_NAMES | String | Audit asset names. | 
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_HOSTNAME | String | Host name. | 
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_RESULT | String | Audit result. | 
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_REASON | String | Audit reason. | 
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_DESCRIPTION | String | Description of the audit. | 
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_ENTITY | String | Audit entity (e.g., AUTH, DISTRIBUTIONS). | 
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_ENTITY_SUBTYPE | String | Entity subtype (e.g., Login, Create). | 
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_CASE_ID | Number | Audit case ID. | 
| PaloAltoNetworksXDR.AuditManagementLogs.AUDIT_INSERT_TIME | Date | Log's insert time. | 


##### Command Example
```!xdr-get-audit-management-logs result=SUCCESS type=DISTRIBUTIONS limit=2 timestamp_gte="3 month"```

##### Context Example
```
{
    "PaloAltoNetworksXDR.AuditManagementLogs": [
        {
            "AUDIT_OWNER_EMAIL": "", 
            "AUDIT_SESSION_ID": null, 
            "AUDIT_ID": 217, 
            "AUDIT_REASON": null, 
            "AUDIT_CASE_ID": null, 
            "AUDIT_DESCRIPTION": "Created a Linux Standalone installer installation package 'dist_1' with agent version 6.1.4.1680", 
            "AUDIT_INSERT_TIME": 1579287926547, 
            "AUDIT_ENTITY": "DISTRIBUTIONS", 
            "AUDIT_OWNER_NAME": "Public API - 1", 
            "AUDIT_ASSET_JSON": "{}", 
            "AUDIT_RESULT": "SUCCESS", 
            "AUDIT_ASSET_NAMES": "", 
            "AUDIT_HOSTNAME": null, 
            "AUDIT_ENTITY_SUBTYPE": "Create"
        }, 
        {
            "AUDIT_OWNER_EMAIL": "", 
            "AUDIT_SESSION_ID": null, 
            "AUDIT_ID": 214, 
            "AUDIT_REASON": null, 
            "AUDIT_CASE_ID": null, 
            "AUDIT_DESCRIPTION": "Created a Linux Standalone installer installation package 'ddd' with agent version 6.1.4.1680", 
            "AUDIT_INSERT_TIME": 1579121478199, 
            "AUDIT_ENTITY": "DISTRIBUTIONS", 
            "AUDIT_OWNER_NAME": "Public API - 1", 
            "AUDIT_ASSET_JSON": "{}", 
            "AUDIT_RESULT": "SUCCESS", 
            "AUDIT_ASSET_NAMES": "", 
            "AUDIT_HOSTNAME": null, 
            "AUDIT_ENTITY_SUBTYPE": "Create"
        }
    ]
}
```

##### Human Readable Output
### Audit Management Logs
|AUDIT_ID|AUDIT_RESULT|AUDIT_DESCRIPTION|AUDIT_OWNER_NAME|AUDIT_OWNER_EMAIL|AUDIT_ASSET_JSON|AUDIT_ASSET_NAMES|AUDIT_HOSTNAME|AUDIT_REASON|AUDIT_ENTITY|AUDIT_ENTITY_SUBTYPE|AUDIT_SESSION_ID|AUDIT_CASE_ID|AUDIT_INSERT_TIME|
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| 217 | SUCCESS | Created a Linux Standalone installer installation package 'dist_1' with agent version 6.1.4.1680 | Public API - 1 |  | {} |  |  |  | DISTRIBUTIONS | Create |  |  | 1579287926547 |
| 214 | SUCCESS | Created a Linux Standalone installer installation package 'ddd' with agent version 6.1.4.1680 | Public API - 1 |  | {} |  |  |  | DISTRIBUTIONS | Create |  |  | 1579121478199 |


### 14. xdr-get-audit-agent-reports
---
Gets agent event reports. You can filter by multiple fields, which will be concatenated using AND condition (OR is not supported). Maximum result set size is 100. Offset is the zero-based number of reports from the start of the result set (start by counting from 0).
##### Required Permissions
**FILL IN REQUIRED PERMISSIONS HERE**
##### Base Command

`xdr-get-audit-agent-reports`
##### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| endpoint_ids | A comma-separated list of endpoint IDs. | Optional | 
| endpoint_names | A comma-separated list of endpoint names. | Optional | 
| type | The report type. Can be "Installation", "Policy", "Action", "Agent Service", "Agent Modules", or "Agent Status". | Optional | 
| sub_type | The report subtype. | Optional | 
| result | The result type. Can be "Success" or "Fail". If not passed, returns all event reports. | Optional | 
| timestamp_gte | Return logs that their timestamp is greater than 'log_time_after'.<br/>Supported values:<br/>1579039377301 (time in milliseconds)<br/>"3 days" (relative date)<br/>"2019-10-21T23:45:00" (date) | Optional | 
| timestamp_lte | Return logs for which the timestamp is before the 'timestamp_lte'.<br/><br/>Supported values:<br/>1579039377301 (time in milliseconds)<br/>"3 days" (relative date)<br/>"2019-10-21T23:45:00" (date) | Optional | 
| page | Page number (for pagination). The default is 0 (the first page). | Optional | 
| limit | The maximum number of reports to return. Default and maximum is 30. | Optional | 
| sort_by | The field by which to sort results. Can be "type", "category", "trapsversion", "timestamp", or "domain"). | Optional | 
| sort_order | The sort order. Can be "asc" (ascending) or "desc" (descending). Default is "asc".  | Optional | 


##### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.AuditAgentReports.ENDPOINTID | String | Endpoint ID. | 
| PaloAltoNetworksXDR.AuditAgentReports.ENDPOINTNAME | String | Endpoint name. | 
| PaloAltoNetworksXDR.AuditAgentReports.DOMAIN | String | Agent domain. | 
| PaloAltoNetworksXDR.AuditAgentReports.TRAPSVERSION | String | Traps version. | 
| PaloAltoNetworksXDR.AuditAgentReports.RECEIVEDTIME | Date | Received time in Epoch time. | 
| PaloAltoNetworksXDR.AuditAgentReports.TIMESTAMP | Date | Timestamp in Epoch time. | 
| PaloAltoNetworksXDR.AuditAgentReports.CATEGORY | String | Report category (e.g., Audit). | 
| PaloAltoNetworksXDR.AuditAgentReports.TYPE | String | Report type (e.g., Action, Policy). | 
| PaloAltoNetworksXDR.AuditAgentReports.SUBTYPE | String | Report subtype (e.g., Fully Protected,Policy Update,Cancel Isolation). | 
| PaloAltoNetworksXDR.AuditAgentReports.RESULT | String | Report result. | 
| PaloAltoNetworksXDR.AuditAgentReports.REASON | String | Report reason. | 
| PaloAltoNetworksXDR.AuditAgentReports.DESCRIPTION | String | Agent report description. | 


##### Command Example
```!xdr-get-audit-agent-reports result=Success timestamp_gte="100 days" endpoint_ids=ea303670c76e4ad09600c8b346f7c804 type=Policy limit=2```

##### Context Example
```
{
    "PaloAltoNetworksXDR.AuditAgentReports": [
        {
            "CATEGORY": "Audit", 
            "DOMAIN": "", 
            "DESCRIPTION": "XDR Agent policy updated on aaaaa.compute.internal", 
            "TIMESTAMP": 1579284369143.7048, 
            "RECEIVEDTIME": 1579286565904.3281, 
            "REASON": null, 
            "SUBTYPE": "Policy Update", 
            "ENDPOINTNAME": "aaaaa.compute.internal", 
            "RESULT": "Success", 
            "ENDPOINTID": "ea303670c76e4ad09600c8b346f7c804", 
            "TRAPSVERSION": "7.0.0.1915", 
            "TYPE": "Policy"
        }, 
        {
            "CATEGORY": "Audit", 
            "DOMAIN": "", 
            "DESCRIPTION": "XDR Agent policy updated on aaaaa.compute.internal", 
            "TIMESTAMP": 1579280769141.43, 
            "RECEIVEDTIME": 1579282965742.36, 
            "REASON": null, 
            "SUBTYPE": "Policy Update", 
            "ENDPOINTNAME": "aaaaa.compute.internal", 
            "RESULT": "Success", 
            "ENDPOINTID": "ea303670c76e4ad09600c8b346f7c804", 
            "TRAPSVERSION": "7.0.0.1915", 
            "TYPE": "Policy"
        }
    ]
}
```

##### Human Readable Output
### Audit Agent Reports
|CATEGORY|DESCRIPTION|DOMAIN|ENDPOINTID|ENDPOINTNAME|REASON|RECEIVEDTIME|RESULT|SUBTYPE|TIMESTAMP|TRAPSVERSION|TYPE|
|---|---|---|---|---|---|---|---|---|---|---|---|
| Audit | XDR Agent policy updated on aaaaa.compute.internal |  | ea303670c76e4ad09600c8b346f7c804 | aaaaa.compute.internal |  | 1579286565904.3281 | Success | Policy Update | 1579284369143.7048 | 7.0.0.1915 | Policy |
| Audit | XDR Agent policy updated on aaaaa.compute.internal |  | ea303670c76e4ad09600c8b346f7c804 | aaaaa.compute.internal |  | 1579282965742.36 | Success | Policy Update | 1579280769141.43 | 7.0.0.1915 | Policy |


## Troubleshooting
 - In case you encounter ReadTimeoutError, we recommend increasing the HTTP request timeout by setting it in the **HTTP Timeout** integration parameter.
### 15. xdr-get-policy
***
Get the policy name for a specific endpoint.


#### Base Command

`xdr-get-policy`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| endpoint_id | The endpoint ID. Can be retrieved by running the xdr-get-endpoints command. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.policyName | string | Name of the policy allocated with the endpoint. | 
| PaloAltoNetworksXDR.policyName.policy_name | unknown | Name of the policy allocated with the endpoint. | 
| PaloAltoNetworksXDR.policyName.endpoint_id | unknown | Endpoint id | 


#### Command Example
```!xdr-get-policy endpoint_id="f8a2f58846b542579c12090652e79f3d"```

#### Context Example
```
{
    "PaloAltoNetworksXDR": {
        "policyName": {
            "endpoint_id": "f8a2f58846b542579c12090652e79f3d",
            "policy_name": "Windows Default"
        }
    }
}
```

#### Human Readable Output

The policy name of endpoint f8a2f58846b542579c12090652e79f3d is Windows Default.

### 16. xdr-get-endpoint-violations
***
Gets a list of device control violations filtered by selected fields. You can retrieve up to 100 violations.


#### Base Command

`xdr-get-endpoint-violations`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| endpoint_ids | Comma separated list of endpoint IDs. | Optional | 
| type | Type of violation | Optional | 
| timestamp_gte | Timestamp of the violation - GTE | Optional | 
| timestamp_lte | Timestamp of the violation - LTE | Optional | 
| ip_list | Comma separated list of IP addresses | Optional | 
| vendor | Name of vendor | Optional | 
| vendor_id | Vendor ID | Optional | 
| product | Name of product | Optional | 
| product_id | Product ID | Optional | 
| serial | Serial Number | Optional | 
| hostname | Hostname | Optional | 
| violation_id_list | Comma separated list of violation IDs | Optional | 
| username | Username | Optional | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.EndpointViolations | unknown | Number of total results of this filter without paging, Number of alerts actually returned as result \(integer\),  A list of violations \(list\). | 
| PaloAltoNetworksXDR.EndpointViolations.total_count | unknown | Number of total results of this filter without paging. | 
| PaloAltoNetworksXDR.EndpointViolations.result_count | unknown | Number of alerts actually returned as result \(integer\). | 
| PaloAltoNetworksXDR.EndpointViolations.violations | unknown | A list of violations \(list\). | 
| PaloAltoNetworksXDR.EndpointViolations.violations.os_type | unknown | Type of the operation system | 
| PaloAltoNetworksXDR.EndpointViolations.violations.hostname | unknown | Hostname | 
| PaloAltoNetworksXDR.EndpointViolations.violations.username | unknown | Username | 
| PaloAltoNetworksXDR.EndpointViolations.violations.ip | unknown | IP address  | 
| PaloAltoNetworksXDR.EndpointViolations.violations.timestamp | unknown | Timestamp of the violation | 
| PaloAltoNetworksXDR.EndpointViolations.violations.violation_id | unknown | Violation ID | 
| PaloAltoNetworksXDR.EndpointViolations.violations.type | unknown | Type of violation | 
| PaloAltoNetworksXDR.EndpointViolations.violations.vendor_id | unknown | Vendor ID | 
| PaloAltoNetworksXDR.EndpointViolations.violations.vendor | unknown | Name of vendor | 
| PaloAltoNetworksXDR.EndpointViolations.violations.product_id | unknown | Product ID | 
| PaloAltoNetworksXDR.EndpointViolations.violations.product | unknown | Name of product | 
| PaloAltoNetworksXDR.EndpointViolations.violations.serial | unknown | Serial Number | 
| PaloAltoNetworksXDR.EndpointViolations.violations.endpoint_id | unknown | Endpoint ID | 


#### Command Example
```!xdr-get-endpoint-violations violation_id_list=100,90,80```

#### Context Example
```
{
    "PaloAltoNetworksXDR": {
        "EndpointViolations": {
            "result_count": 3,
            "total_count": 3,
            "violations": [
                {
                    "endpoint_id": "eca20ea25d4e4cfdad3a317997322693",
                    "hostname": "$HOSTNAME",
                    "ip": "$IP",
                    "os_type": "AGENT_OS_WINDOWS",
                    "product": "Cruzer Blade",
                    "product_id": "0x5567",
                    "serial": "4C530010060624106156",
                    "timestamp": 1586813964000,
                    "type": "Disk Drive",
                    "username": "st3.local\assaflevi",
                    "vendor": "SanDisk Corp.",
                    "vendor_id": "0x0781",
                    "violation_id": 100
                },
                {
                    "endpoint_id": "e4b83018ae2c411899d847930e68d398",
                    "hostname": "$HOSTNAME",
                    "ip": "$IP",
                    "os_type": "AGENT_OS_WINDOWS",
                    "product": "Cruzer Blade",
                    "product_id": "0x5567",
                    "serial": "4C530010060624106156",
                    "timestamp": 1586813953000,
                    "type": "Disk Drive",
                    "username": "st3.local\assaflevi",
                    "vendor": "SanDisk Corp.",
                    "vendor_id": "0x0781",
                    "violation_id": 90
                },
                {
                    "endpoint_id": "bef78d0ddbcc4ae488190bab0dcb31cd",
                    "hostname": "$HOSTNAME",
                    "ip": "$IP",
                    "os_type": "AGENT_OS_WINDOWS",
                    "product": "Cruzer Blade",
                    "product_id": "0x5567",
                    "serial": "4C530010060624106156",
                    "timestamp": 1586813942000,
                    "type": "Disk Drive",
                    "username": "st3.local\assaflevi",
                    "vendor": "SanDisk Corp.",
                    "vendor_id": "0x0781",
                    "violation_id": 80
                }
            ]
        }
    }
}
```

#### Human Readable Output

>### Endpoint Violation
>|timestamp|username|ip|type|violation_id|vendor|product|serial|
>|---|---|---|---|---|---|---|---|
>| 1586813964000 | st3.localssaflevi | $IP | Disk Drive | 100 | SanDisk Corp. | Cruzer Blade | 4C530010060624106156 |
>| 1586813953000 | st3.localssaflevi | $IP | Disk Drive | 90 | SanDisk Corp. | Cruzer Blade | 4C530010060624106156 |
>| 1586813942000 | st3.localssaflevi | $IP | Disk Drive | 80 | SanDisk Corp. | Cruzer Blade | 4C530010060624106156 |


### 17. xdr-retrieve-files
***
Retrieve files from selected endpoints. You can retrieve up to 20 files, from no more than 10 endpoints. Please enter endpoint_ids and at least one file path.


#### Base Command

`xdr-retrieve-files`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| endpoint_ids | Comma separated list of endpoint IDs | Required | 
| windows_file_paths | The type of platform and files you want to retrieve - comma separated list of strings.  | Optional | 
| linux_file_paths | The type of platform and files you want to retrieve - comma separated list of strings.  | Optional | 
| mac_file_paths | The type of platform and files you want to retrieve - comma separated list of strings.  | Optional | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.retrievedFiles.actionId | unknown | ID of action to retrieve files from selected endpoints. | 


#### Command Example
```!xdr-retrieve-files endpoint_ids=aeec6a2cc92e46fab3b6f621722e9916 windows_file_paths="C:\Users\demisto\Desktop\demisto.txt"```

#### Context Example
```
{
    "PaloAltoNetworksXDR": {
        "retrievedFiles": {
            "actionId": 2056
        }
    }
}
```

#### Human Readable Output

>### Retrieve files
>|Action Id|
>|---|
>| 2056 |


### 18. xdr-retrieve-file-details
***
View the file retrieved by the Retrieve File request according to the action ID.


#### Base Command

`xdr-retrieve-file-details`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| action_id | String the represents the Action ID of the Retrieve File request. Action ID it is an output of Retrieve File request. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.retrievedFileDetails | unknown | Data — Link to the file retrieved by Retrieve File request. | 
| PaloAltoNetworksXDR.retrievedFileDetails.endpoint_id | string | Endpoint ID. | 
| PaloAltoNetworksXDR.retrievedFileDetails.file_link | string | Link to the file retrieved by Retrieve File request. | 
| PaloAltoNetworksXDR.retrievedFileDetails.action_id | string | Action ID of the Retrieve File request | 


#### Command Example
```!xdr-retrieve-file-details action_id=1763```

#### Context Example
```
{
    "PaloAltoNetworksXDR": {
        "retrievedFileDetails": {
            "action_id": 1763,
            "endpoint_id": "aeec6a2cc92e46fab3b6f621722e9916",
            "file_link": "https://demisto-dev.xdr.us.paloaltonetworks.com/api/webapp/download/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJsY2Fhc19pZCI6IjM3NjY5OTIyMyIsImV4cCI6MTYwMzI2NjgwNSwiZGF0YSI6eyJmaWxlX3BhdGgiOiJjb21wbGV0ZWRfdXBsb2Fkcy9hZWVjNmEyY2M5MmU0NmZhYjNiNmY2MjE3MjJlOTkxNi9zYW06M2FhYzg2ZmZjNDA3MTYxYjFmMWFhM2Y3ODIyNzgyOTFlZmIzN2ZjZWIxMDNjYzI0NmNkMjgwMDQ3YTM2MTk2OCIsImZpbGVfbmFtZSI6InJlcG9ydF9kYXRhX2MwNmMzODUwYmRjODRiYTU4ZDNjNDVjNmI2ZDBjZGUwLnppcCIsImJ1Y2tldF9uYW1lIjoieGRyLXVzLTM3NjY5OTIyMy1hZ2VudC11cGxvYWRzIn19.QZCq-D-8v9pCYLtRiMpaD3UY1tAyiak_xLQGLea1xUA"
        }
    }
}
```

#### Human Readable Output

>### Retrieve file Details
>|action_id|endpoint_id|file_link|
>|---|---|---|
>| 1763 | aeec6a2cc92e46fab3b6f621722e9916 | https://demisto-dev.xdr.us.paloaltonetworks.com/api/webapp/download/eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJsY2Fhc19pZCI6IjM3NjY5OTIyMyIsImV4cCI6MTYwMzI2NjgwNSwiZGF0YSI6eyJmaWxlX3BhdGgiOiJjb21wbGV0ZWRfdXBsb2Fkcy9hZWVjNmEyY2M5MmU0NmZhYjNiNmY2MjE3MjJlOTkxNi9zYW06M2FhYzg2ZmZjNDA3MTYxYjFmMWFhM2Y3ODIyNzgyOTFlZmIzN2ZjZWIxMDNjYzI0NmNkMjgwMDQ3YTM2MTk2OCIsImZpbGVfbmFtZSI6InJlcG9ydF9kYXRhX2MwNmMzODUwYmRjODRiYTU4ZDNjNDVjNmI2ZDBjZGUwLnppcCIsImJ1Y2tldF9uYW1lIjoieGRyLXVzLTM3NjY5OTIyMy1hZ2VudC11cGxvYWRzIn19.QZCq-D-8v9pCYLtRiMpaD3UY1tAyiak_xLQGLea1xUA |


### 19. xdr-get-scripts
***
Get a list of scripts available in the scripts library.


#### Base Command

`xdr-get-scripts`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| script_name | String of the script name, or comma separated list of strings | Optional | 
| description | String of the script description, or comma separated list of strings | Optional | 
| created_by | String of the user name of who created the script or comma separated list of strings | Optional | 
| windows_supported | Whether the script can be executed on Windows operating system. | Optional | 
| linux_supported | Whether the script can be executed on Linux operating system. | Optional | 
| macos_supported | Whether the script can be executed on Mac operating system. | Optional | 
| is_high_risk | Whether the script has a high-risk outcome. | Optional | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.Scripts | unknown | An array of scripts containing the following information: name,  description, modification_date, created_by, windows_supported, linux_supported, macos_supported, is_high_risk. | 
| PaloAltoNetworksXDR.Scripts.script_id | unknown | Script ID | 
| PaloAltoNetworksXDR.Scripts.name | string | Name of the script | 
| PaloAltoNetworksXDR.Scripts.description | string | Description of the script | 
| PaloAltoNetworksXDR.Scripts.modification_date | unknown | Timestamp of when the script was last modified. | 
| PaloAltoNetworksXDR.Scripts.created_by | string | name of the user who created the script. | 
| PaloAltoNetworksXDR.Scripts.windows_supported | unknown | Whether the script can be executed on Windows OS. | 
| PaloAltoNetworksXDR.Scripts.linux_supported | unknown | Whether the script can be executed on Linux OS. | 
| PaloAltoNetworksXDR.Scripts.macos_supported | unknown | Whether the script can be executed on macOS. | 
| PaloAltoNetworksXDR.Scripts.is_high_risk | unknown | Whether the script has a high-risk outcome. | 
| PaloAltoNetworksXDR.Scripts.script_uid | unknown | GUID, global ID of the script, used to identify the script when executing. | 


#### Command Example
```!xdr-get-scripts created_by="Palo Alto Networks" is_high_risk=true```

#### Context Example
```
{
    "PaloAltoNetworksXDR": {
        "Scripts": [
            {
                "created_by": "Palo Alto Networks",
                "description": "Kill processes by name",
                "is_high_risk": true,
                "linux_supported": true,
                "macos_supported": true,
                "modification_date": 1587466892269,
                "name": "process_kill_name",
                "script_id": 8,
                "script_uid": "fd0a544a99a9421222b4f57a11839481",
                "windows_supported": true
            },
            {
                "created_by": "Palo Alto Networks",
                "description": "Kill all processes with a RAM usage higher than specified",
                "is_high_risk": true,
                "linux_supported": true,
                "macos_supported": true,
                "modification_date": 1587466892260,
                "name": "process_kill_mem",
                "script_id": 7,
                "script_uid": "87d4547df6d4882a3c006ec58c3b8bf4",
                "windows_supported": true
            },
            {
                "created_by": "Palo Alto Networks",
                "description": "Delete a file by path",
                "is_high_risk": true,
                "linux_supported": true,
                "macos_supported": true,
                "modification_date": 1587466891981,
                "name": "delete_file",
                "script_id": 1,
                "script_uid": "548023b6e4a01ec51a495ba6e5d2a15d",
                "windows_supported": true
            },
            {
                "created_by": "Palo Alto Networks",
                "description": "Set registry value",
                "is_high_risk": true,
                "linux_supported": false,
                "macos_supported": false,
                "modification_date": 1599385307669,
                "name": "registry_set",
                "script_id": 11,
                "script_uid": "896392a13b2ef0ae75b3f2396125037d",
                "windows_supported": true
            },
            {
                "created_by": "Palo Alto Networks",
                "description": "Get registry value data and its type",
                "is_high_risk": true,
                "linux_supported": false,
                "macos_supported": false,
                "modification_date": 1587466892337,
                "name": "registry_get",
                "script_id": 10,
                "script_uid": "699fa2e98ab1eb5677489dce54125769",
                "windows_supported": true
            },
            {
                "created_by": "Palo Alto Networks",
                "description": "Delete registry value or delete registry key with all its values",
                "is_high_risk": true,
                "linux_supported": false,
                "macos_supported": false,
                "modification_date": 1587466892277,
                "name": "registry_delete",
                "script_id": 9,
                "script_uid": "ad36488a20cdbdd1604ec4bec9da5c41",
                "windows_supported": true
            },
            {
                "created_by": "Palo Alto Networks",
                "description": "Execute list of shell commands",
                "is_high_risk": true,
                "linux_supported": true,
                "macos_supported": true,
                "modification_date": 1587466892157,
                "name": "execute_commands",
                "script_id": 2,
                "script_uid": "a6f7683c8e217d85bd3c398f0d3fb6bf",
                "windows_supported": true
            },
            {
                "created_by": "Palo Alto Networks",
                "description": "Kill all processes with a CPU usage higher than specified",
                "is_high_risk": true,
                "linux_supported": true,
                "macos_supported": true,
                "modification_date": 1587466892247,
                "name": "process_kill_cpu",
                "script_id": 6,
                "script_uid": "3d928a24f61cd3c1116544900c424098",
                "windows_supported": true
            }
        ]
    }
}
```

#### Human Readable Output

>### Scripts
>|name|description|script_uid|modification_date|created_by|windows_supported|linux_supported|macos_supported|is_high_risk|
>|---|---|---|---|---|---|---|---|---|
>| process_kill_name | Kill processes by name | fd0a544a99a9421222b4f57a11839481 | 1587466892269 | Palo Alto Networks | true | true | true | true |
>| process_kill_mem | Kill all processes with a RAM usage higher than specified | 87d4547df6d4882a3c006ec58c3b8bf4 | 1587466892260 | Palo Alto Networks | true | true | true | true |
>| delete_file | Delete a file by path | 548023b6e4a01ec51a495ba6e5d2a15d | 1587466891981 | Palo Alto Networks | true | true | true | true |
>| registry_set | Set registry value | 896392a13b2ef0ae75b3f2396125037d | 1599385307669 | Palo Alto Networks | true | false | false | true |
>| registry_get | Get registry value data and its type | 699fa2e98ab1eb5677489dce54125769 | 1587466892337 | Palo Alto Networks | true | false | false | true |
>| registry_delete | Delete registry value or delete registry key with all its values | ad36488a20cdbdd1604ec4bec9da5c41 | 1587466892277 | Palo Alto Networks | true | false | false | true |
>| execute_commands | Execute list of shell commands | a6f7683c8e217d85bd3c398f0d3fb6bf | 1587466892157 | Palo Alto Networks | true | true | true | true |
>| process_kill_cpu | Kill all processes with a CPU usage higher than specified | 3d928a24f61cd3c1116544900c424098 | 1587466892247 | Palo Alto Networks | true | true | true | true |


### 20. xdr-get-script-metadata
***
Get the full definitions of a specific script in the scripts library.


#### Base Command

`xdr-get-script-metadata`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| script_uid | Unique identifier of the script, returned by the “xdr-get-scripts” command. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.scriptMetadata | unknown | The full definitions of a specific script in the scripts library. | 
| PaloAltoNetworksXDR.scriptMetadata.script_id | unknown | Script ID | 
| PaloAltoNetworksXDR.scriptMetadata.name | unknown | Script name | 
| PaloAltoNetworksXDR.scriptMetadata.description | unknown | Script Description | 
| PaloAltoNetworksXDR.scriptMetadata.modification_date | unknown | Timestamp of when the script was last modified. | 
| PaloAltoNetworksXDR.scriptMetadata.created_by | unknown | Name of the user who created the script. | 
| PaloAltoNetworksXDR.scriptMetadata.is_high_risk | unknown | Whether the script has a high-risk outcome. | 
| PaloAltoNetworksXDR.scriptMetadata.windows_supported | unknown | Whether the script can be executed on Windows OS. | 
| PaloAltoNetworksXDR.scriptMetadata.linux_supported | unknown | Whether the script can be executed on Linux OS. | 
| PaloAltoNetworksXDR.scriptMetadata.macos_supported | unknown | Whether the script can be executed on macOS. | 
| PaloAltoNetworksXDR.scriptMetadata.entry_point | unknown | Name of the entry point selected for the script. Empty string indicate the script defined as just run. | 
| PaloAltoNetworksXDR.scriptMetadata.script_input | unknown | Name and type for the specified entry point. | 
| PaloAltoNetworksXDR.scriptMetadata.script_output_type | unknown | Type of output | 
| PaloAltoNetworksXDR.scriptMetadata.script_output_dictionary_definitions | unknown | In case of the script_output_type is a dictionary an array with friendly_name, name, and type for each output is returned. | 


#### Command Example
```!xdr-get-script-metadata script_uid=43973479d389f2ac7e99b6db88eaee40```

#### Context Example
```
{
    "PaloAltoNetworksXDR": {
        "scriptMetadata": {
            "created_by": "Palo Alto Networks",
            "description": "List all directories under path",
            "entry_point": "run",
            "is_high_risk": false,
            "linux_supported": true,
            "macos_supported": true,
            "modification_date": 1587466892181,
            "name": "list_directories",
            "script_id": 4,
            "script_input": [
                {
                    "name": "path",
                    "type": "string"
                },
                {
                    "name": "number_of_levels",
                    "type": "number"
                }
            ],
            "script_output_dictionary_definitions": null,
            "script_output_type": "string_list",
            "script_uid": "43973479d389f2ac7e99b6db88eaee40",
            "windows_supported": true
        }
    }
}
```

#### Human Readable Output

>### Script Metadata
>|script_id|name|description|modification_date|created_by|is_high_risk|windows_supported|linux_supported|macos_supported|script_uid|entry_point|script_input|script_output_type|
>|---|---|---|---|---|---|---|---|---|---|---|---|---|
>| 4 | list_directories | List all directories under path | 1587466892181 | Palo Alto Networks | false | true | true | true | 43973479d389f2ac7e99b6db88eaee40 | run | {'name': 'path', 'type': 'string'},<br/>{'name': 'number_of_levels', 'type': 'number'} | string_list |


### 21. xdr-get-script-code
***
Get the code of a specific script in the script library.


#### Base Command

`xdr-get-script-code`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| script_uid | Unique identifier of the script, returned by the “xdr-get-scripts” command. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.scriptCode | unknown | The code of a specific script in the script library. | 
| PaloAltoNetworksXDR.scriptCode.code | unknown | The code of a specific script in the script library. | 
| PaloAltoNetworksXDR.scriptCode.script_uid | unknown | Script uid | 


#### Command Example
```!xdr-get-script-code script_uid=548023b6e4a01ec51a495ba6e5d2a15d```

#### Context Example
```
{
    "PaloAltoNetworksXDR": {
        "scriptCode": {
            "code": "import os
import sys
import traceback


def run(file_path):
    path = os.path.expanduser(file_path)
    path = os.path.expandvars(path)
    if os.path.isabs(path):
        try:
            os.remove(path)
        except IOError:
            sys.stderr.write(f\"File not accessible: {path}\")
            return False
        except Exception as e:
            sys.stderr.write(f\"Exception occured: {traceback.format_exc()}\")
            return False
    return True
",
            "script_uid": "548023b6e4a01ec51a495ba6e5d2a15d"
        }
    }
}
```

#### Human Readable Output

>Script code is :
> import os
>import sys
>import traceback
>
>
>def run(file_path):
>    path = os.path.expanduser(file_path)
>    path = os.path.expandvars(path)
>    if os.path.isabs(path):
>        try:
>            os.remove(path)
>        except IOError:
>            sys.stderr.write(f"File not accessible: {path}")
>            return False
>        except Exception as e:
>            sys.stderr.write(f"Exception occured: {traceback.format_exc()}")
>            return False
>    return True


### 22. xdr-run-script
***
Initiate a new endpoint script execution action using a script from the script library.


#### Base Command

`xdr-run-script`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| script_uid | GUID, unique identifier of the script, returned by the “get scripts” API per script. | Required | 
| endpoint_ids | List of endpoint IDs. | Required | 
| parameters | Please enter comma separated parameters at the following way : “ param1_name=param1_value, param2_name=param2_value “. You can find which parameters required for the script, by running "xdr-get-script-metadata" command. | Optional | 
| timeout | Integer, represents the timeout in seconds for this execution. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.runScript.actionId | unknown | Action ID of the action initiated. ID will be used as a reference to track in the action center. | 


#### Command Example
```!xdr-run-script script_uid=43973479d389f2ac7e99b6db88eaee40 endpoint_ids=23a86310665d413a958926fce5b794b3 timeout=30 parameters="path=C:\,number_of_levels=3"```

#### Context Example
```
{
    "PaloAltoNetworksXDR": {
        "runScript": {
            "actionId": 2057
        }
    }
}
```

#### Human Readable Output

>### Run Script Command
>|Action Id|
>|---|
>| 2057 |


### 23. xdr-get-script-execution-status
***
Retrieve the status of a script execution action.


#### Base Command

`xdr-get-script-execution-status`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| action_id | Integer, identifier of the action, can be found in Cortex XDR console ResponseAction CenterAction ID field, or as return value after running - "xdr-run-script" command. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.scriptExecutionStatus | unknown | Action ID of the action initiated. ID will be used as a reference to track in the action center. | 
| PaloAltoNetworksXDR.scriptExecutionStatus.general_status | unknown | general status of the action, considering the status of all the endpoints. | 
| PaloAltoNetworksXDR.scriptExecutionStatus.error_message | unknown | Error message regarding permissions for running APIs or the action doesn’t exist. | 
| PaloAltoNetworksXDR.scriptExecutionStatus.endpoints_timeout | unknown | number of endpoints in “timeout” status. | 
| PaloAltoNetworksXDR.scriptExecutionStatus.endpoints_pending_abort | unknown | Number of endpoints in “pending abort” status. | 
| PaloAltoNetworksXDR.scriptExecutionStatus.endpoints_pending | unknown | Number of endpoints in pending status. | 
| PaloAltoNetworksXDR.scriptExecutionStatus.endpoints_in_progress | unknown | Nnumber of endpoints in “in progress” status. | 
| PaloAltoNetworksXDR.scriptExecutionStatus.endpoints_failed | unknown | Number of endpoints in “failed” status. | 
| PaloAltoNetworksXDR.scriptExecutionStatus.endpoints_expired | unknown | Number of endpoints in “expired” status. | 
| PaloAltoNetworksXDR.scriptExecutionStatus.endpoints_completed_successfully | unknown | Number of endpoints in “completed successfully” status. | 
| PaloAltoNetworksXDR.scriptExecutionStatus.endpoints_canceled | unknown | Number of endpoints in “canceled” status. | 
| PaloAltoNetworksXDR.scriptExecutionStatus.endpoints_aborted | unknown | Number of endpoints in “aborted” status. | 
| PaloAltoNetworksXDR.scriptExecutionStatus.action_id | unknown | Action ID of script execution. | 


#### Command Example
```!xdr-get-script-execution-status action_id=1777```

#### Context Example
```
{
    "PaloAltoNetworksXDR": {
        "scriptExecutionStatus": {
            "action_id": "1777",
            "endpoints_aborted": 0,
            "endpoints_canceled": 0,
            "endpoints_completed_successfully": 1,
            "endpoints_expired": 0,
            "endpoints_failed": 0,
            "endpoints_in_progress": 0,
            "endpoints_pending": 0,
            "endpoints_pending_abort": 0,
            "endpoints_timeout": 0,
            "general_status": "COMPLETED_SUCCESSFULLY"
        }
    }
}
```

#### Human Readable Output

>### Execution Status
>|action_id|endpoints_aborted|endpoints_canceled|endpoints_completed_successfully|endpoints_expired|endpoints_failed|endpoints_in_progress|endpoints_pending|endpoints_pending_abort|endpoints_timeout|general_status|
>|---|---|---|---|---|---|---|---|---|---|---|
>| 1777 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | COMPLETED_SUCCESSFULLY |


### 24. xdr-insert-simple-indicators
***
Upload IOCs as JSON objects that you retrieved from external threat intelligence sources.


#### Base Command

`xdr-insert-simple-indicators`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| indicator | String that identifies the indicator you want to insert into Cortex XDR. | Required | 
| type | Keyword identifying the type of indicator. | Required | 
| severity | Keyword identifying the indicator's severity.  | Required | 
| expiration_date | Integer representing the indicator's expiration timestamp. This is a Unix epoch timestamp value, in milliseconds. If this indicator has no expiration, use -1. If this value is NULL, the indicator receives the indicator's type default expiration date | Optional | 
| comment | Comment string. | Optional | 
| reputation | Keyword representing the indicator's reputation. | Optional | 
| reliability | Character representing the indicator's reliability rating. Valid values are A - F. A is the most reliable, F is the least. | Optional | 
| vendors | JSON array of objects representing the vendors from which this IOC was obtained. Each field in the array is: vendor_name = String representing the name of the vendor who reported this indicator. reputation = Keyword representing the vendor's reputation. Valid values are: GOOD BAD SUSPICIOUS UNKNOWN. reliability = Character representing the vendor's reliability rating. Valid values are A - F. A is the most reliable, F is the least. | Optional | 
| class | String representing the indicator class (for example, 'Malware'). | Optional | 
| vendor_name | String representing the name of the vendor who reported this indicator. You should enter vendor_name, vendor_reputation and vendor_reliability all of this arguments together, or none of them.  | Optional | 
| vendor_reputation | Keyword representing the vendor's reputation. Valid values are: GOOD,BAD,SUSPICIOUS,UNKNOWN. You should enter vendor_name, vendor_reputation and vendor_reliability all of this arguments together, or none of them.  | Optional | 
| vendor_reliability | Character representing the vendor's reliability rating. Valid values are A - F. A is the most reliable, F is the least. You should enter vendor_name, vendor_reputation and vendor_reliability all of this arguments together, or none of them.  | Optional | 


#### Context Output

There is no context output for this command.

#### Command Example
```!xdr-insert-simple-indicators indicator=IndicatorTest severity=LOW type=DOMAIN_NAME class="Test" comment="Test this integration" reliability=C reputation=UNKNOWN expiration_date=12413524```

#### Context Example
```json
{}
```

#### Human Readable Output

>IOCs successfully uploaded

### 25. xdr-action-status-get
***
Retrieve the status of the requested actions according to the action ID.


#### Base Command

`xdr-action-status-get`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| action_id | The Action ID of the selected request. After performing an action, you will receive an action id. | Required | 


#### Context Output

| **Path** | **Type** | **Description** |
| --- | --- | --- |
| PaloAltoNetworksXDR.getActionStatus | unknown | Status of the action ID. You will receive a table of endpoint id and its status. | 
| PaloAltoNetworksXDR.getActionStatus.endpoint_id | unknown | endpoint id. | 
| PaloAltoNetworksXDR.getActionStatus.status | unknown | status of specific endpoint id. | 
| PaloAltoNetworksXDR.getActionStatus.action_id | unknown | Action ID that was at the input. | 


#### Command Example
```!xdr-action-status-get action_id="1819"```

#### Context Example
```
{
    "PaloAltoNetworksXDR": {
        "getActionStatus": {
            "action_id": 1819,
            "endpoint_id": "aeec6a2cc92e46fab3b6f621722e9916",
            "status": "COMPLETED_SUCCESSFULLY"
        }
    }
}
```

#### Human Readable Output

>### Get Action Status
>|endpoint_id|status|
>|---|---|
>| aeec6a2cc92e46fab3b6f621722e9916 | COMPLETED_SUCCESSFULLY |


### 26. xdr-delete-endpoints
***
Delete selected endpoints in the Cortex XDR app. You can delete up to 1000 endpoints.


#### Base Command

`xdr-delete-endpoints`
#### Input

| **Argument Name** | **Description** | **Required** |
| --- | --- | --- |
| endpoint_ids | Comma separated list of endpoint IDs. | Required | 


#### Context Output

There is no context output for this command.

#### Command Example
```!xdr-delete-endpoints endpoint_ids=aeec6a2cc92e46fab3b6f621722e9916```


#### Human Readable Output

>Endpoints aeec6a2cc92e46fab3b6f621722e9916 successfully deleted