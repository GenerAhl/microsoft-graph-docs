---
title: "softwareUpdateReference resource type"
description: "Represents specific update content."
author: "Alice-at-Microsoft"
localization_priority: Normal
ms.prod: "w10"
doc_type: resourcePageType
---

# softwareUpdateReference resource type

Namespace: microsoft.graph.windowsUpdates

[!INCLUDE [beta-disclaimer](../../includes/beta-disclaimer.md)]

Represents specific update content.

In a deployment, the same softwareUpdateReference could result in devices receiving different update revisions, but the content is considered contextually equivalent for all devices in the deployment.

All software update references exist as one of the following derived types: [featureUpdateReference](../resources/windowsupdates-featureupdatereference.md),  [qualityUpdateReference](../resources/windowsupdates-qualityupdatereference.md), [expeditedQualityUpdateReference](../resources/windowsupdates-expeditedqualityupdatereference.md).

Inherits from [deployableContent](../resources/windowsupdates-deployablecontent.md).

## Properties
None.

## Relationships
None.

## JSON representation
The following is a JSON representation of the resource.
<!-- {
  "blockType": "resource",
  "@odata.type": "microsoft.graph.windowsUpdates.softwareUpdateReference"
}
-->
``` json
{
  "@odata.type": "#microsoft.graph.windowsUpdates.softwareUpdateReference"
}
```

