---
title: LinknLink
description: Instructions on how to integrate LinknLink eMotion Ultra devices with Home Assistant.
ha_category:
  - Binary sensor
  - Sensor
ha_config_flow: true
ha_release: 2026.8
ha_iot_class: Local Polling
ha_codeowners:
  - '@acmen0102'
ha_domain: linknlink
ha_platforms:
  - binary_sensor
  - sensor
ha_integration_type: device
works_with:
  - local
ha_quality_scale: bronze
---

The **LinknLink** {% term integration %} connects eMotion Ultra and Ultra2 presence sensors directly to Home Assistant over the local network. The integration uses the LinknLink DNA protocol for device identification and legacy Ultra communication. Ultra2 sensor states are read from the device's standard ESPHome local API. Communication remains local between Home Assistant and the device.

## Supported devices

- LinknLink eMotion Ultra
- LinknLink eMotion Ultra2

## Prerequisites

Before setting up the integration:

1. Complete Wi-Fi setup using a supported provisioning method for the device.
2. Connect Home Assistant and the device to the same local network.
3. Find the device IP address in your router.
4. Ensure that UDP traffic from Home Assistant to port `80` on the device is allowed.
5. For Ultra2, ensure that TCP traffic from Home Assistant to port `6053` on the device is allowed.

{% include integrations/config_flow.md %}

{% configuration_basic %}
Host:
  description: "The IP address or hostname of the eMotion Ultra device."
{% endconfiguration_basic %}

## Supported functionality

The available entities depend on the sensors and child devices reported by the eMotion Ultra.

### Sensors

The integration can provide the following sensor entities:

- Temperature
- Humidity
- Illuminance
- Target count
- Persons in fenced zones
- Target count for zones 1 through 4
- Wi-Fi signal strength

The Wi-Fi signal strength entity is disabled by default.

### Binary sensors

The integration provides occupancy binary sensors for the complete detection area and zones 1 through 4 when those values are supported by the device.

## Data updates

This integration uses local {% term polling %}. Home Assistant requests updated device and child-device state every 30 seconds. A temporary communication error makes the entities unavailable; polling resumes automatically and the local session is re-established when the device becomes reachable again.

## Actions

This integration does not provide custom actions.

## Known limitations

- The integration does not configure Wi-Fi. Initial provisioning must be completed before adding the device to Home Assistant.
- Automatic network discovery is not provided in the initial release.
- The initial release uses polling and does not enable local UDP position push.
- Target coordinates, target distance, radar tuning, and device MQTT settings are not exposed.
- The device network address can be changed from the integration's **Reconfigure** action. Assigning a stable DHCP lease is still recommended.

## Troubleshooting

### The device cannot be added

1. Confirm that the device is powered on and connected to Wi-Fi.
2. Confirm that the entered IP address belongs to the eMotion Ultra device.
3. Check that Home Assistant can reach the device network without client isolation or a firewall blocking UDP port `80`.
4. For Ultra2, confirm that TCP port `6053` is reachable from Home Assistant.
5. Stop other local software controlling the device temporarily, then retry setup.

### Entities are unavailable

Confirm that the device still uses the configured IP address. Restart the device and verify that the required UDP and TCP traffic between Home Assistant and the device is not blocked.

## Removing the integration

This integration follows standard integration removal. No data needs to be removed from the device.

{% include integrations/remove_device_service.md %}
