---
title: "Internet requirement"
description: "How Home Assistant classifies whether an integration needs an internet connection to work."
---

Home Assistant classifies every integration by whether it actually needs an internet connection to work, separately from its [IoT class](/docs/iot_class/). The IoT class describes *how* Home Assistant talks to a device or service, such as locally or through the cloud. The internet requirement describes *whether* that communication depends on an internet connection at all, which isn't always the same thing. An integration can talk to a device entirely over your local network and still depend on the manufacturer's cloud for things like authentication.

{% note %}
An integration classified as local in its [IoT class](/docs/iot_class/) can still require an internet connection. The two classifications answer different questions, so check both if you want to know whether an integration will keep working during an internet outage.
{% endnote %}

## Classifiers

- {% icon "mdi:cloud-off-outline" %} None: The integration works fully without an internet connection, including setup.
- {% icon "mdi:cloud-key-outline" %} Setup: An internet connection is only needed once, to set up or pair the integration. After that, it keeps working without one.
- {% icon "mdi:cloud-question-outline" %} Conditional: An internet connection may be needed during normal use, either all the time or only for specific features, depending on the integration and how you use it.
- {% icon "mdi:cloud" %} Required: The integration always needs an internet connection to work, no matter how it's set up.

## How this is determined

For most integrations, the internet requirement follows directly from their [IoT class](/docs/iot_class/):

- Cloud polling and Cloud push integrations are always classified as Required, since they depend on the manufacturer's cloud by definition.
- Local polling, Local push, Calculated, and Assumed state integrations are always classified as None. These devices, like infrared and RF remotes, or Bluetooth-based devices, don't depend on anything outside your local network.

One integration class doesn't fit that pattern and needs to declare its internet requirement manually instead:

- Configurable integrations, like MQTT, depend entirely on how you set them up, so they can't get a fixed internet requirement any more than they can get a fixed IoT class.

Any Local polling, Local push, or Assumed state integration that still depends on the manufacturer's cloud, for authentication or otherwise, overrides the automatic None classification with Setup or Conditional instead.
