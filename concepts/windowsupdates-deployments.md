---
title: "Deployments in the Windows Update for Business deployment service"
description: "Deployments are the foundation of the Windows Update for Business deployment service. Through a deployment you can target a set of devices to receive specific content from Windows Update, such as a software update."
author: "aarononeal"
localization_priority: Normal
ms.prod: "w10"
doc_type: conceptualPageType
---

# Deployments in the Windows Update for Business deployment service

Deployments are the foundation of the Windows Update for Business deployment service. Through a deployment you can target a set of devices to receive specific content from Windows Update, such as a [software update](windowsupdates-software-updates.md).

Deployments have the following key aspects:

1. Content: The update available to deploy from the catalog.
2. Audience: The devices to receive content.
3. Settings: The settings governing how and when content should be delivered to devices.
4. State: The current state of the deployment within its lifecycle.

## Create a deployment with content and an audience

Because content and audience are key to the definition of a deployment, both are required to be assigned at the time of creation. Content and audience assignments cannot be changed later, however, device membership within an audience can.

To learn more about creating a deployment, see [Deploy an update using the Windows Update for Business deployment service](windowsupdates-deploy-update.md) and [Deploy and expedited update using the Windows Update for Business deployment service](windowsupdates-deploy-expedited-update.md).

## Configure settings

### Rollout

Rollout settings govern how the content is deployed over time to devices in the deployment audience. Rollout settings can be configured for deployments of feature updates.

To learn more, see [Schedule a deployment using the Windows Update for Business deployment service](windowsupdates-schedule-deployment.md).

### Monitoring

You can use monitoring settings to configure alerts and automated actions to take based on update signals from devices. Monitoring settings can be configured for deployments of feature updates.

To learn more, see [Manage monitoring rules for a deployment using the Windows Update for Business deployment service](windowsupdates-manage-monitoring-rules.md).

### User experience

For deployments of expedited quality updates, user experience settings temporarily override existing policies on the device for update experience.

To learn more, see [Deploy and expedited update using the Windows Update for Business deployment service](windowsupdates-deploy-expedited-update.md).

## Get or set lifecycle state

### States

Deployments move through the following lifecycle states:

| State     | Description                                                                                       |
|-----------|---------------------------------------------------------------------------------------------------|
| Scheduled | The deployment is waiting for offer conditions to be met to start offering the update to devices. |
| Offering  | The deployment is offering the update to devices.                                                 |
| Paused    | The deployment is paused and prevented from offering the update to devices until it is unpaused.  |


### Transitions

| Transition                     | Condition                                |
|--------------------------------|------------------------------------------|
| scheduled → offering           | Scheduling condition is met               |
| offering → scheduled           | Scheduling condition is not met           |
| scheduled or offering → paused | There is a request or automatic action to pause. |
| paused → scheduled or offering | There is no longer a request or automatic action to pause. |

### Resource model

The [deployment](../api-reference/beta/resources/windowsupdates-deployment.md) resource has a `state` property of type [deploymentState](../api-reference/beta/resources/windowsupdates-deploymentstate.md) which provides information about the current lifecycle state.

The service will determine the effective `value` of the deployment state as a net result of several inputs and asynchronous processes, but you can request a particular value by setting `requestedValue` as one of these inputs. Other inputs to the effective deployment state value include rollout settings and monitoring settings.

<!-- | Property       | Type                                                                                                              | Description                                                                                               |
|:---------------|:------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------|
| value          | deploymentStateValue                                                                                              | Specifies the state of the deployment. Read-only. Possible values are: `scheduled`, `offering`, `paused`. |
| reasons        | [deploymentStateReason](/graph/api/resources/windowsupdates-deploymentstatereason) collection | Specifies the reasons the deployment has its state value. Read-only.                                      |
| requestedValue | requestedDeploymentStateValue                                                                                     | Specifies the requested state of the deployment. Possible values are: `none`, `paused`.                   | -->

## Multiple deployments

A device can be assigned to multiple deployments at one time. These deployments can be for content of different update categories, as well as for content of the same update category.

When a device is assigned to two deployments for content of different update categories (for example, a feature update and an expedited quality update), the deployment service will offer content in a sequence according to Microsoft’s recommendation.

When a device is assigned to two deployments for content of the same update category (for example, feature update versions 20H1 and 20H2, or quality updates from March 2021 and April 2021), the deployment service will offer the content that is highest ranked by Microsoft. For feature updates and quality updates, an update that was released more recently is higher ranked. This behavior does not apply if one of the deployments is still scheduled for the device and is not ready to offer content. In that case, the other deployment’s content will be delivered to the device.
