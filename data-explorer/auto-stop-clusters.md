---
title: Automatic stop of inactive clusters in Azure Data Explorer
description: Learn when your cluster will be stopped using the Automatic stop feature, and how to enable/disable the Automatic stop.
author: orspod
ms.author: orspodek
ms.reviewer: orhasban
ms.service: data-explorer
ms.topic: how-to
ms.date: 11/03/2021
---
# Automatic stop of inactive Azure Data Explorer clusters

Azure Data Explorer clusters that have been *inactive* for a specified time interval are automatically stopped. Inactivity is defined by having no data ingestion or queries, and cluster average CPU below a certain threshold (1% of capacity).

Automatic stop (Auto-Stop) happens on the following schedule:

First, you'll receive several days of recommendations to stop the cluster through Azure Advisor. For more information about these recommendations, see [Cost recommendations](azure-advisor.md#cost-recommendations).
* If the inactive cluster has a small amount or no data, it's stopped after five days.
* If the inactive cluster has data ingested, it's stopped after 10 days.

Cluster behavior isn't automatically resumed. To restart the cluster, do so manually.

> [!NOTE]
> Leader clusters aren't automatically stopped. For more information, see [follower databases](follower.md).

## Manage Automatic stop behavior on your cluster

Azure Data Explorer clusters are created by default with the cluster property of `enableAutoStop = true`. This property can be set or altered either on cluster creation or post creation.

Set this property using one of the following methods, our using the [Azure portal](#azure-portal):
* [ARM Templates](/azure/templates/microsoft.kusto/clusters?tabs=json#trustedexternaltenant-object)
* [Azure CLI](/cli/azure/kusto/cluster?view=azure-cli-latest#az_kusto_cluster_update-optional-parameters)
* [PowerShell](/powershell/module/az.kusto/new-azkustocluster)
* [Azure Resource Explorer](https://resources.azure.com/). 

For more information, see [Azure Data Explorer cluster request body](/rest/api/azurerekusto/clusters/createorupdate#request-body). 

## Azure portal

### Set Auto-Stop settings while creating a new cluster

1. Follow the steps in [Create an Azure Data Explorer cluster and database](create-cluster-database-portal.md).
1. In the **Configurations** tab, select **Auto-Stop cluster** > **On**.

:::image type="content" source="media/auto-stop-clusters/auto-stop-cluster-creation.png" alt-text="Screenshot of auto-stop configuration.":::

### Modify settings on an existing cluster

To enable/disable Auto-Stop cluster after cluster was created:

1. Sign into the [Azure portal](https://ms.portal.azure.com/).
1. Browse to your Azure Data Explorer cluster.
1. In **Settings**, select **Configurations**.
1. In the **Configurations** pane, select **On**/**Off** to enable/disable **Auto-Stop cluster**.
1. Select **Save**.

:::image type="content" source="media/auto-stop-clusters/auto-stop-cluster-update.png" alt-text="Screenshot of auto-stop configuration in Azure portal.":::

## Verify Auto-Stop using the Activity log

When a cluster is automatically stopped, an Activity log is sent. To verify when and how the cluster was stopped, use the following steps:

1. Sign into the [Azure portal](https://ms.portal.azure.com/).
1. Browse to Azure Data Explorer cluster.
1. On the left pane, select **Activity log**.
1. Choose a timespan.
1. Under **Operation name**, look for **Stop Clusters**.

:::image type="content" source="media/auto-stop-clusters/auto-stop-cluster-activity-log.png" alt-text="Screenshot of activity log.":::

## Examples

### REST example

Update the cluster using the following operation:

```http
PATCH https://management.azure.com/subscriptions/12345678-1234-1234-1234-123456789098/resourceGroups/kustorgtest/providers/Microsoft.Kusto/clusters/kustoclustertest?api-version=2021-08-27
```

#### Request body to disable Auto-Stop

```json
{
    "properties": { 
        "enableAutoStop": false 
    }
}
```

#### Request body to enable Auto-Stop

```json
{
  "properties": {
    "enableAutoStop": true
  }
}
```