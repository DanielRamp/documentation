---
title: "Alert"
weight: 95
---

{{< toc >}}

The alert system integrates with various third-party services.
Tuning alerts also helps personalize TrueNAS to any highly-sensitive issues.

## Alert Services

Alert Services are the various methods built into to TrueNAS that can notify you of a system alert.

{{< hint warning >}}
Some of alert services are third-party integrations that can charge for message or data usage.
{{< /hint >}}

### Adding a Service

To add a new alert service, go to **System > Alert Services** and click *ADD*.

![SystemAlertServicesAdd](/images/CORE/12.0/SystemAlertServicesAdd.png "New Alert Service")

{{< include file="static/includes/Reference/SystemAlertServicesAddEditFields.md.part" markdown="true" >}}

Choosing a *Type* adds options specific to that alert service:

{{< tabs "Authentication Options" >}}
{{< tab "AWS" >}}

| Name | Description |
|------|-------------|
| AWS Region | Enter the [AWS account region](https://docs.aws.amazon.com/sns/latest/dg/sms_supported-countries.html). |
| ARN | Topic [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/sns/latest/dg/CreateTopic.html) for publishing. Example: `arn:aws:sns:us-west-2:111122223333:MyTopic`. |
| Key ID | Access Key ID for the linked AWS account. |
| Secret Key | Secret Access Key for the linked AWS account. |

{{< /tab >}}
{{< tab "Email" >}}

| Name | Description |
|------|-------------|
| Email Address | Enter a valid email address to receive alerts from this system. |

{{< /tab >}}
{{< tab "InfluxDB" >}}

| Name | Description |
|------|-------------|
| Host | Enter the [InfluxDB](https://docs.influxdata.com/influxdb/) hostname. |
| Username | Username for this service. |
| Password | Enter password. |
| Database | Name of the InfluxDB database. |
| Series | InfluxDB time series name for collected points. |

{{< /tab >}}
{{< tab "Mattermost" >}}

| Name | Description |
|------|-------------|
| Webhook URL | Enter or paste the [incoming webhook](https://docs.mattermost.com/developer/webhooks-incoming.html) URL associated with this service. |
| Username | Mattermost username. |
| Channel | Name of the [channel](https://docs.mattermost.com/messaging/managing-channels.html) to receive notifications. This overrides the default channel in the incoming webhook settings. |
| Icon Url | Icon file to use as the profile picture for new messages. Example: https://mattermost.org/wp-content/uploads/2016/04/icon.png. Requires configuring Mattermost to [override profile picture icons](https://docs.mattermost.com/configure/configuration-settings.html#enable-integrations-to-override-profile-picture-icons). |

{{< /tab >}}
{{< tab "OpsGenie" >}}

| Name | Description |
|------|-------------|
| API Key | Enter or paste the [API key](https://docs.opsgenie.com/v1.0/docs/api-integration). Find the API key by signing into the OpsGenie web interface and going to Integrations/Configured Integrations. Click the desired integration, Settings, and read the API Key field. |
| API URL | Leave empty for default OpsGenie API. |

{{< /tab >}}
{{< tab "Pager Duty" >}}

| Name | Description |
|------|-------------|
| Service Key | Enter or paste the "integration/service" key for this system to access the [PagerDuty API](https://v2.developer.pagerduty.com/v2/docs/events-api). |
| Client Name | PagerDuty client name. |

{{< /tab >}}
{{< tab "Slack" >}}

| Name | Description |
|------|-------------|
| Webhook URL | Paste the [incoming webhook](https://api.slack.com/incoming-webhooks) URL associated with this service. |

{{< /tab >}}
{{< tab "SNMP Trap" >}}

| Name | Description |
|------|-------------|
| Hostname | Hostname or IP address of the system to receive SNMP trap notifications. |
| Port | UDP port number on the system receiving SNMP trap notifications. The default is 162. |
| SNMPv3 Security Model | Enable the SNMPv3 security model. |
| SNMP Community | Network community string. The community string acts like a user ID or password. A user with the correct community string has access to network information. The default is public. For more information, see this helpful [SNMP Community Strings tutorial](https://www.dnsstuff.com/snmp-community-string). |

{{< /tab >}}
{{< tab "Victor Ops" >}}

| Name | Description |
|------|-------------|
| API Key | Enter or paste the [VictorOps API key](https://help.victorops.com/knowledge-base/api/). |
| Routing Key | Enter or paste the [VictorOps routing key](https://portal.victorops.com/public/api-docs.html). |

{{< /tab >}}
{{< /tabs >}}

Test the service configuration by clicking **SEND TEST ALERT**.

## Alert Settings

To modify the default system alerts, go to **System > Alert Settings**.

![System Alert Settings](/images/CORE/12.0/SystemAlertSettings.png "Alert Settings")

The alerts are grouped into sections based on type.
For example, alerts that are related to pools appear in the **Storage** alert section.

{{< include file="static/includes/Reference/SystemAlertSettingsFields.md.part" markdown="true" >}}

Changing any of these options affects every configured alert service.

### Alert Warning Levels

Each warning level has a different icon and color to express its importance. To make the system email you when alerts with a specific warning level trigger, set up an email [Alert Service](https://www.truenas.com/docs/core/system/alert/#alert-services) with that warning level. 

| Level | Icon | Alert Notification? |
|-------|------|-------------|
| 1 INFO | ![COREAlertLevelInfoNoticeAlertEmergency](/images/CORE/12.0/COREAlertLevelInfoNoticeAlertEmergency.png "Alert Levels") | No |
| 2 NOTICE | ![COREAlertLevelInfoNoticeAlertEmergency](/images/CORE/12.0/COREAlertLevelInfoNoticeAlertEmergency.png "Alert Levels") | Yes |
| 3 WARNING | ![COREAlertLevelWarning](/images/CORE/12.0/COREAlertLevelWarning.png "Alert Levels") | Yes |
| 4 ERROR | ![COREAlertLevelErrorCritical](/images/CORE/12.0/COREAlertLevelErrorCritical.png "Alert Levels") | Yes |
| 5 CRITICAL | ![COREAlertLevelErrorCritical](/images/CORE/12.0/COREAlertLevelErrorCritical.png "Alert Levels") | Yes |
| 6 ALERT | ![COREAlertLevelInfoNoticeAlertEmergency](/images/CORE/12.0/COREAlertLevelInfoNoticeAlertEmergency.png "Alert Levels") | Yes |
| 7 EMERGENCY | ![COREAlertLevelInfoNoticeAlertEmergency](/images/CORE/12.0/COREAlertLevelInfoNoticeAlertEmergency.png "Alert Levels") | Yes |
