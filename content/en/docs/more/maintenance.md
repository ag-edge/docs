---
title: Maintenance
weight: 80
aliases:
  - /maintenance
---
This document explains how maintenance and updates are performed on Darcy Cloud.

During a maintenance window, you can learn about service availability
at [status.darcy.ai](https://status.darcy.ai). You can find out about
upcoming maintenance events in the [changelog](/docs/more/changelog).
And at any time, you can reach out to [Darcy Support](mailto:support@darcy.ai).

## Affected Systems

A maintenance event can effect one or both of Darcy Cloud or the attached edge nodes.

- **Darcy Cloud maintenance**: Darcy Cloud [Portal](https://cloud.darcy.ai)
  and [API](https://api.darcy.ai/v1/docs).
- **Edge Node maintenance**: updates to the agent software and/or
  configuration installed on your edge devices.

### Darcy Cloud

Typical Darcy Cloud maintenance is invisible to the end user. That is to say,
there is no interruption to service, and you may not have noticed that the update
has occurred. We refer to this as a [Zero-Impact Maintenance](#zero-impact-maintenance) event.
However, on occasion
we may need to temporarily interrupt service to perform significant infrastructure
or backend upgrades. We call this a [Service-Impact Maintenance](#service-impact-maintenance) event.

### Edge Nodes

When you [add an edge node](/docs/cloud/adding-nodes/add-node/) to your Darcy Cloud
project, three software components are installed on the node:

- _Edgeworx Agent_ interacts with Darcy Cloud to monitor the node's status.
- _ioFog Agent_ communicates with the Darcy Cloud ioFog backend to
  manage microservices on your node.
- _Deviceplane Agent_ implements the [keyless SSH](/docs/cloud/node-remote-access/)
  access mechanism. It is also used by Darcy Cloud to perform updates to the agents themselves.

Node maintenance can affect one or more of these agents.

#### Node update process

This is a broad outline of the steps that can occur during node maintenance. Note
that only some of these steps may occur for any particular maintenance event.

- Darcy Cloud initiates the update process by opening a connection with Deviceplane Agent,
  and then executing an update script on the node via Deviceplane.
  - Note that a login session may appear in your node's log files. This is the update
    process at work.
- Edgeworx Agent:
  - Edgeworx Agent's software components may be updated.
  - Edgeworx Agent's configuration files may be modified.
  - The Edgeworx Agent service may be restarted.
- ioFog Agent:
  - ioFog Agent's software components may be updated.
  - ioFog Agent's configuration files may be modified.
  - The ioFog Agent service may be restarted.
    {{<info>}}
    Note that this restart of ioFog Agent **will not affect** the microservices running on
    your node. They will continue to run without interruption.
    {{</info>}}
- Deviceplane Agent:
  - Deviceplane Agent's software components may be updated.
  - Deviceplane Agent's configuration files may be modified.
  - The Deviceplane Agent service may be restarted.
- What does **not** happen:
  - Your applications and microservices running on your nodes will **not** be stopped or restarted.
  - Your node (edge device) will **not** be restarted.
  - Other than the changes to the agents listed above, no other changes are made to your node.

#### Offline nodes

Sometimes a node will be offline during the maintenance period. What happens?
When the node comes back online, the node's Deviceplane Agent reestablishes its connection
to Darcy Cloud. When Darcy Cloud observes that the node is back online, it will
then initiate the deferred update process. The _Deferred Maintenance Window_ is thirty
days.

After that window closes - depending on the nature of that particular node update
process - it may be necessary to re-register the node. From Darcy Cloud's perspective,
this effectively creates a new node. You would also need to re-deploy any microservices.
Please contact [Darcy Support](mailto:support@darcy.ai) if you need assistance with a node
outside the deferred maintenance window.

## Impact

### Zero-Impact Maintenance

Zero-Impact Maintenance, as you might expect, has zero impact on service availability.
Most often, this type of maintenance is to improve backend services or infrastructure.
If there are user-facing changes, you will be able to find information
on those changes in the [changelog](/docs/more/changelog).

### Service-Impact Maintenance

During _Service-Impact Maintenance_, there is interruption to service.

- Darcy Cloud Portal will be unavailable. A visit to [cloud.darcy.ai](https://cloud.darcy.ai) in
  your browser will return an "Undergoing Maintenance" page.
- Darcy Cloud API routes will return a `503/Service Unavailable` HTTP status code.
- `edgectl` commands will return an error.

{{<info>}}
During Service-Impact Maintenance, applications and microservices
running on your nodes **will not be affected**. They will continue to operate as normal.
But you will not be able to push any changes to those applications via Darcy Cloud
during the maintenance period.
{{</info>}}

Any user-facing changes from the update will be noted in the [changelog](/docs/more/changelog).

## Schedule

We understand the importance of keeping our customers in the loop about upcoming
maintenance. We have two classes of scheduled maintenance.

### Regularly Scheduled Maintenance

For _Regularly Scheduled Maintenance_, you will receive an initial email notification at your account's registered email
address at least five business days before the maintenance window. If you have concerns
about the maintenance event impacting your operations, please contact [Darcy Support](mailto:support@darcy.ai)
as early as possible.

{{<info>}}
Again, please note that during any type of maintenance, applications and microservices
running on your nodes **will not be affected**. They will continue to operate as normal.
{{</info>}}

### Expedited Schedule Maintenance

For an _Expedited Schedule Maintenance_ event, we aim to email you at least two days in advance. We realize
that this is not a lot of time to prepare, so we make every effort to avoid expedited maintenance.

### Steps

- You will receive an initial notification email for either Regularly Scheduled or Expedited Schedule Maintenance.
- You will be emailed a reminder the day before the maintenance window.
- You will be emailed again when the maintenance window opens.
- And, finally, you will be emailed one more time when the maintenance event concludes.
- Note that at any time, you can learn more about service availability at [status.darcy.ai](https://status.darcy.ai).
- After the maintenance event concludes, you can find out about any changes or new
  features in the [changelog](/docs/more/changelog).
- At any time, you can check on the availability of new `edgectl` versions by executing `edgectl version`.

## Feedback

If you have any feedback about our process, feel free to start a discussion over
at [discuss.darcy.ai](https://discuss.darcy.ai). If you have a concern about an upcoming
maintenance event impacting your operations, please reach out as early as possible
to [Darcy Support](mailto:support@darcy.ai).