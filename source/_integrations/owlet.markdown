---
title: Owlet
description: Instructions on how to set up the Owlet integration in Home Assistant.
ha_release: 2025.4.0
ha_category:
  - Sensor
ha_iot_class: Cloud Polling
ha_config_flow: true
ha_domain: owlet
ha_platforms:
  - sensor
ha_codeowners:
  - '@ryanbdclark'
ha_integration_type: integration
---

The Owlet integration allows you to integration a v2 or v3 [Owlet Smart Sock](https://owletbabycare.co.uk/) into Home Assistant.
The integration uses the [pyowletapi](https://pypi.org/project/pyowletapi/) library.

All that is required to add this integration is an Owlet account with a Smart Sock v2/v3 connected to this account.

The Owlet sock is a great device that provides peace of my mind to new parents, unfortunately without a 3rd party integration like this one you have to keep the app 
open to view your child's stats. This integration enables you to integrate these devices, the stats could then be display on a central dashboard or low power display without
having to have your phone open and app open all the time.

## Supported devices

The integration will add any v2/v3 socks that are present on the account used. 

## Prerequisites

1. Install the Owlet app from the relevent app store
2. Create an account
3. Follow the instructions on the app to add the sock to your account

{% include integrations/config_flow.md %}

{% configuration_basic %}
Region:
    description: "The region your Owlet account is in, the current options are Europe and World, if you are getting invalid credentials errors when setting up it may be that your region isn't currently supported."
Email:
    description: "The email address used to login to your Owlet account"
Password:
    description: "The password used to login to your Owlet account"
{% endconfiguration_basic %}


## Supported funtionality

### Owlet sock v2, v3, dream sock

#### Sensors

- **Battery Percentage** - The sock's remaining battery percentage. Enabled by default.
- **Battery Remaining** - The sock's remaining battery in minutes. Enabled by default.
- **O2 Saturation 10 Minute Average** - The sock wearer's average Oxygen Saturation over the last 10 minutes as a percentage. Enabled by default.
- **Movement** - The sock's current movement value. Disabled by default
- **Movement bucket** - The sock's current movement bucket. Disabled by default
- **Heart Rate** - The sock wearer's current heart rate in bpm. Enabled by default.
- **Oxygen Saturation** - The sock wearer's current Oxygen Saturation as a percentage. Enabled by default.
- **Skin Temperature** - The sock wearer's current skin temperature in celsius. Enabled by default.
- **Signal strength** - The sock's current signal strength to the base. Enabled by default.
- **Sleep state** - The sock wearer's current sleep state, options are: "Uknown", "Awake", "Light sleep", "Deep sleep". Enabled by default.

## Data updates

This integration fetches data from the Owlet servers every 5 seconds. This is the same update interval that you see when using the Owlet app.

## Known limitations

The integration does not provide access to the Owlet cameras this integration currently only exposes Owlet socks.
The only regions currently support are "Europe" and "World"

## Supported devices

The following devices are known to be supported by the integration:
- Owlet sock v2
- Owlet sock v3
- Owlet dream sock

## Unsupported devices

The following devices are not supported by the integration:
- Owlet cam
- Owlet cam 2

## Remove integration

This integration follows standard integration removal, no extra steps are required.

{% include integrations/remove_device_service.md %}

## Automation

To send a notification when the heart rate sensor is low the below automation could be used, the device to notify would need to be changed. This does not replace the notitication that would be recieved from the Owlet app.

{% raw %}

```yaml
- alias: "Low Heart Rate Alert"
  triggers:
    - trigger: numeric_state
      entity_id: sensor.owlet_baby_care_sock_heart_rate
      below: 70
  actions:
    action: notify.mobile_app_iphone
    metadata: {}
    data:
        title: Low heart rate
        message: >-
            {{ trigger.to_state.attributes.friendly_name }} is low, {{trigger.to_state.state}} bpm
```

{% endraw %}

## Troubleshooting

### Can’t set up the device

#### Symptom: “Entered credentials are incorrect”

When trying to set up the integration, the form shows the message “Entered credentials are incorrect”.

##### Description

This means that either your email and password are incorrect or you are attempting to login to the wrong region. The Owlet API only returns a generic error about incorrect login credentials so it's hard to say if it's the email/password combination or the region that is incorrect.

##### Resolution

Make sure that the email and password entered is correct. 
Make sure that the region selected is correct for your account


#### Symptom: “No devices found”

When trying to set up the integration, the form shows the message “No devices found”.

##### Description

This means that the authentication to the Owlet was succesful however no valid devices were found on your account.

##### Resolution

Only v2/v3/dream socks are supported, ensure your device is one of these.
Make sure the account logged in has the device assigned to it.

