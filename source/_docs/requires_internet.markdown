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

For every integration, the internet requirement is derived automatically from its [IoT class](/docs/iot_class/), unless it declares its own value instead:

- Cloud polling and Cloud push integrations are classified as Required, since they depend on the manufacturer's cloud by definition.
- Local polling, Local push, Calculated, and Assumed state integrations are classified as None. These devices, like infrared and RF remotes, or Bluetooth-based devices, don't depend on anything outside your local network.
- Configurable integrations, like the individual MQTT entity platforms, are classified as Conditional. Whether they need an internet connection depends entirely on how you set them up, such as whether your MQTT broker is local or cloud-hosted, so Conditional is the honest default rather than a guess.

Any integration can override its automatic classification by declaring its own value instead, for a Local polling, Local push, or Assumed state integration that still depends on the manufacturer's cloud, for authentication or otherwise, or for a Configurable integration whose own internet requirement is better known than the default.

### Choosing between Setup and Conditional

Setup and Conditional aren't two equally valid options to pick between. Setup makes a specific promise: after the one-time setup step, the integration never needs an internet connection again. Conditional makes no such promise.

If an integration needs an internet connection during setup and for even one thing afterward, however small or occasional, it doesn't qualify for Setup. Use Conditional instead. For example, an integration might require signing in to a cloud account to authenticate during setup, but also rely on that same cloud account for an update entity or an optional feature later on. Since something is still needed after setup, that integration is Conditional, not Setup.

Setup also assumes the internet connection is actually required, not just convenient. If an integration can be fully set up without an internet connection, using a manual alternative to a cloud-based step, it doesn't qualify for Setup either, even if the cloud-based path is the recommended or more common one. It's Conditional instead: whether an internet connection is needed at all depends on how you choose to set it up, which is exactly the kind of "depends on how you use it" situation Conditional is for.
