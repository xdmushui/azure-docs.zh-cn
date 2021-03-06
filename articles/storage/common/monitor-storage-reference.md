---
title: Azure 存储监视数据参考 | Microsoft Docs
description: 来自 Azure 存储的监视数据的日志和指标参考。
author: normesta
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 05/01/2020
ms.author: normesta
ms.subservice: logs
ms.custom: monitoring
ms.openlocfilehash: 28a127b4debeacd2562867008bc594897558d50d
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2020
ms.locfileid: "87446843"
---
# <a name="azure-storage-monitoring-data-reference"></a>Azure 存储监视数据参考

请参阅[监视 Azure 存储](monitor-storage.md)，详细了解如何收集和分析 Azure 存储的监视数据。

## <a name="metrics"></a>指标

以下各表列出了为 Azure 存储收集的平台指标。 

### <a name="capacity-metrics"></a>容量度量值

容量指标每隔一小时发送到 Azure Monitor。 值每日刷新。 时间粒度定义呈现指标值的时间间隔。 所有容量指标的受支持时间粒度为一小时 (PT1H)。

Azure 存储在 Azure Monitor 中提供以下容量指标。

#### <a name="account-level"></a>帐户级别

下表显示[帐户级别指标](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccounts)。

| 指标 | 说明 |
| ------------------- | ----------------- |
| UsedCapacity | 存储帐户使用的存储量。 对于标准存储帐户，该指标是 Blob、表、文件和队列使用的容量总和。 对于高级存储帐户和 Blob 存储帐户，它与 BlobCapacity 相同。 <br/><br/> 单位：字节 <br/> 聚合类型：平均值 <br/> 值示例：1024 |

#### <a name="blob-storage"></a>Blob 存储

下表显示 [Blob 存储指标](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccountsblobservices)。

| 指标 | 说明 |
| ------------------- | ----------------- |
| BlobCapacity | 存储帐户中使用的 Blob 存储总计。 <br/><br/> 单位：字节 <br/> 聚合类型：平均值 <br/> 值示例：1024 <br/> 尺寸：**BlobType** 和 **BlobTier**（[定义](#metrics-dimensions)） |
| BlobCount    | 在存储帐户中存储的 Blob 对象数。 <br/><br/> 单位：计数 <br/> 聚合类型：平均值 <br/> 值示例：1024 <br/> 尺寸：**BlobType** 和 **BlobTier**（[定义](#metrics-dimensions)） |
| BlobProvisionedSize | 存储帐户中预配的存储量。 此指标仅适用于高级存储帐户。 <br/><br/> 单位：字节 <br/> 聚合类型：平均值 |
| ContainerCount    | 存储帐户中的容器数。 <br/><br/> 单元：计数 <br/> 聚合类型：平均值 <br/> 值示例：1024 |
| IndexCapacity     | ADLS Gen2 分层索引所使用的存储量 <br/><br/> 单元：字节 <br/> 聚合类型：平均值 <br/> 值示例：1024 |

#### <a name="table-storage"></a>表存储

下表显示[表存储指标](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccountstableservices)。

| 指标 | 描述 |
| ------------------- | ----------------- |
| TableCapacity | 存储帐户使用的表存储量。 <br/><br/> 单元：字节 <br/> 聚合类型：平均值 <br/> 值示例：1024 |
| TableCount   | 存储帐户中的表数目。 <br/><br/> 单位：计数 <br/> 聚合类型：平均值 <br/> 值示例：1024 |
| TableEntityCount | 存储帐户中的表实体数目。 <br/><br/> 单元：计数 <br/> 聚合类型：平均值 <br/> 值示例：1024 |

#### <a name="queue-storage"></a>队列存储

下表显示[队列存储指标](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccountsfileservices)。

| 指标 | 说明 |
| ------------------- | ----------------- |
| QueueCapacity | 存储帐户使用的队列存储量。 <br/><br/> 单元：字节 <br/> 聚合类型：平均值 <br/> 值示例：1024 |
| QueueCount   | 存储帐户中的队列数目。 <br/><br/> 单元：计数 <br/> 聚合类型：平均值 <br/> 值示例：1024 |
| QueueMessageCount | 存储帐户的队列服务中的队列消息的大致数目。 <br/><br/>单元：计数 <br/> 聚合类型：平均值 <br/> 值示例：1024 |

#### <a name="file-storage"></a>文件存储

下表显示[文件存储指标](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported#microsoftstoragestorageaccountsqueueservices)。

| 指标 | 描述 |
| ------------------- | ----------------- |
| FileCapacity | 存储帐户使用的文件存储量。 <br/><br/> 单元：字节 <br/> 聚合类型：平均值 <br/> 值示例：1024 |
| FileCount   | 存储帐户中的文件数目。 <br/><br/> 单位：计数 <br/> 聚合类型：平均值 <br/> 值示例：1024 |
| FileShareCount | 存储帐户中的文件共享数目。 <br/><br/> 单元：计数 <br/> 聚合类型：平均值 <br/> 值示例：1024 |
| FileShareProvisionedIOPS | 文件共享上预配的 IOPS 数。 此指标仅适用于高级文件存储。 <br/><br/> 单位：字节 <br/> 聚合类型：平均值 |

### <a name="transaction-metrics"></a>事务指标

从 Azure 存储到 Azure Monitor 的每个存储帐户请求都会发出事务指标。 如果存储帐户中没有任何活动，则在此期间不会有关于事务指标的数据。 所有事务指标在帐户和服务级别（Blob 存储、表存储、Azure 文件和队列存储）提供。 时间粒度定义呈现指标值的时间间隔。 所有事务指标的受支持时间粒度为 PT1H 和 PT1M。

Azure 存储在 Azure Monitor 中提供以下事务指标。

| 指标 | 说明 |
| ------------------- | ----------------- |
| 事务 | 向存储服务或指定的 API 操作发出的请求数。 此数字包括成功和失败的请求数，以及引发错误的请求数。 <br/><br/> 单位：计数 <br/> 聚合类型：总计 <br/> 适用的维度：ResponseType、GeoType、ApiName 和 Authentication（[定义](#metrics-dimensions)）<br/> 值示例：1024 |
| 流入量 | 流入数据量。 此数字包括从外部客户端到 Azure 存储流入的数据量，以及流入 Azure 中的数据量。 <br/><br/> 单元：字节 <br/> 聚合类型：总计 <br/> 适用的维度：GeoType、ApiName 和 Authentication（[定义](#metrics-dimensions)） <br/> 值示例：1024 |
| 流出量 | 流出数据量。 此数字包括从外部客户端到 Azure 存储流出的数据量，以及流出 Azure 中的数据量。 因此，此数字不反映计费的流出量。 <br/><br/> 单元：字节 <br/> 聚合类型：总计 <br/> 适用的维度：GeoType、ApiName 和 Authentication（[定义](#metrics-dimensions)） <br/> 值示例：1024 |
| SuccessServerLatency | Azure 存储处理成功请求所用的平均时间。 此值不包括 SuccessE2ELatency 中指定的网络延迟。 <br/><br/> 单位：毫秒 <br/> 聚合类型：平均值 <br/> 适用的维度：GeoType、ApiName 和 Authentication（[定义](#metrics-dimensions)） <br/> 值示例：1024 |
| SuccessE2ELatency | 向存储服务或指定的 API 操作发出的成功请求的平均端到端延迟。 此值包括在 Azure 存储中读取请求、发送响应和接收响应确认所需的处理时间。 <br/><br/> 单位：毫秒 <br/> 聚合类型：平均值 <br/> 适用的维度：GeoType、ApiName 和 Authentication（[定义](#metrics-dimensions)） <br/> 值示例：1024 |
| 可用性 | 存储服务或指定的 API 操作的可用性百分比。 可用性通过由“计费请求总数”值除以适用的请求数（包括引发意外错误的请求）计算得出。 所有意外错误都会导致存储服务或指定的 API 操作的可用性下降。 <br/><br/> 单位：百分比 <br/> 聚合类型：平均值 <br/> 适用的维度：GeoType、ApiName 和 Authentication（[定义](#metrics-dimensions)） <br/> 值示例：99.99 |

<a id="metrics-dimensions"></a>

## <a name="metrics-dimensions"></a>指标维度

Azure 存储支持对 Azure Monitor 中的指标使用以下维度。

| 维度名称 | 说明 |
| ------------------- | ----------------- |
| **BlobType** | 仅限 Blob 指标的 Blob 类型。 支持的值为 **BlockBlob**、**PageBlob** 和 **Azure Data Lake Storage**。 追加 blob 包含在**BlockBlob**中。 |
| **BlobTier** | Azure 存储提供不同的访问层，使你能够以最具成本效益的方式存储 Blob 对象数据。 请在 [Azure 存储 Blob 层](../blobs/storage-blob-storage-tiers.md)中查看详细信息。 支持的值包括： <br/> <li>**Hot**：热层</li> <li>**Cool**：冷层</li> <li>**存档**：存档层</li> <li>**Premium**：块 Blob 的高级层</li> <li>**P4/P6/P10/P15/P20/P30/P40/P50/P60**：高级页 Blob 的层类型</li> <li>**标准**：标准页 Blob 的层类型</li> <li>**Untiered**：常规用途 v1 存储帐户的层类型</li> |
| **GeoType** | 来自主要或辅助群集的事务。 可用值包括 **Primary** 和 **Secondary**。 从辅助租户读取对象时，该维度会应用到读取访问异地冗余存储 (RA-GRS)。 |
| **ResponseType** | 事务响应类型。 可用的值包括： <br/><br/> <li>**ServerOtherError**：除描述的错误以外的其他所有服务器端错误 </li> <li>**ServerBusyError**：已经过身份验证的请求返回了 HTTP 503 状态代码。 </li> <li>**ServerTimeoutError**：已经过身份验证的超时请求返回了 HTTP 500 状态代码。 由于服务器错误而发生超时。 </li> <li>**AuthorizationError**：由于未经授权访问数据或者授权失败，经过身份验证的请求失败。 </li> <li>**NetworkError**：由于网络错误，经过身份验证的请求失败。 往往发生于客户端在超时失效之前提前关闭了连接时。 </li><li>**ClientAccountBandwidthThrottlingError**：因为超出了[存储帐户可伸缩性限制](scalability-targets-standard-account.md)，在带宽方面对请求进行了限制。</li><li>**ClientAccountRequestThrottlingError**：因为超出了[存储帐户可伸缩性限制](scalability-targets-standard-account.md)，在请求速率方面对请求进行了限制。<li>**ClientThrottlingError**：其他客户端限制错误。 不包括 ClientAccountBandwidthThrottlingError 和 ClientAccountRequestThrottlingError。</li> <li>**ClientTimeoutError**：已经过身份验证的超时请求返回了 HTTP 500 状态代码。 如果将客户端的网络超时或请求超时设置为比存储服务预期值更小的值，则预期会发生此超时。 否则，会报告为 ServerTimeoutError。 </li> <li>**ClientOtherError**：除描述的错误以外的其他所有客户端错误。 </li> <li>**成功**：请求成功</li> <li> **SuccessWithThrottling**：请求成功，具体表现在：头一次或头几次尝试时，SMB 客户端会被限制，但重试后会成功。</li> |
| **ApiName** | 操作的名称。 例如： <br/> <li>**CreateContainer**</li> <li>**DeleteBlob**</li> <li>**GetBlob**</li> 有关所有操作名称，请参阅[文档](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)。 |
| **身份验证** | 事务中所用的身份验证类型。 可用的值包括： <br/> <li>**AccountKey**：事务通过存储帐户密钥进行身份验证。</li> <li>**SAS**：事务通过共享访问签名进行身份验证。</li> <li>**OAuth**：事务通过 OAuth 访问令牌进行身份验证。</li> <li>**Anonymous**：事务以匿名方式请求。 不包括预检请求。</li> <li>**AnonymousPreflight**：事务为预检请求。</li> |

对于支持维度的指标，需要指定维度值才能查看相应的指标值。 例如，如果查看成功响应的 **Transactions** 值，需要使用 **Success** 筛选 **ResponseType** 维度。 或者，如果查看块 Blob 的 **BlobCount** 值，需要使用 **BlockBlob** 筛选 **BlobType** 维度。

## <a name="resource-logs-preview"></a>资源日志（预览版）

> [!NOTE]
> Azure Monitor 中的 Azure 存储日志目前为公共预览版，可在所有公有云区域中用于预览测试。 若要注册预览版，请参阅[此页](https://forms.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxW65f1VQyNCuBHMIMBV8qlUM0E0MFdPRFpOVTRYVklDSE1WUTcyTVAwOC4u)。  此预览版为常规用途 v1 和常规用途 v2 存储帐户中的 Blob（包括 Azure Data Lake Storage Gen2）、文件、队列、表和高级存储帐户启用日志。 不支持经典存储帐户。

下表列出了在 Azure Monitor 日志或 Azure 存储中收集 Azure 存储资源日志时这些资源日志的属性。 属性描述了操作、服务以及用来执行该操作的授权类型。

### <a name="fields-that-describe-the-operation"></a>描述操作的字段

```json
{
    "time": "2019-02-28T19:10:21.2123117Z",
    "resourceId": "/subscriptions/12345678-2222-3333-4444-555555555555/resourceGroups/mytestrp/providers/Microsoft.Storage/storageAccounts/testaccount1/blobServices/default",
    "category": "StorageWrite",
    "operationName": "PutBlob",
    "operationVersion": "2017-04-17",
    "schemaVersion": "1.0",
    "statusCode": 201,
    "statusText": "Success",
    "durationMs": 5,
    "callerIpAddress": "192.168.0.1:11111",
    "correlationId": "ad881411-201e-004e-1c99-cfd67d000000",
    "location": "uswestcentral",
    "uri": "http://mystorageaccount.blob.core.windows.net/cont1/blobname?timeout=10"
}
```

| 属性 | 描述 |
|:--- |:---|
|**time** | 存储收到请求时的协调世界时 (UTC) 时间。 例如：`2018/11/08 21:09:36.6900118`。|
|**resourceId** | 存储帐户的资源 ID。 例如： `/subscriptions/208841be-a4v3-4234-9450-08b90c09f4/resourceGroups/`<br>`myresourcegroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount/storageAccounts/blobServices/default`|
|**category** | 所请求的操作的类别。 例如：`StorageRead`、`StorageWrite` 或 `StorageDelete`。|
|**operationName** | 执行的 REST 操作的类型。 <br> 有关操作的完整列表，请参阅[“存储分析记录的操作和状态消息”主题](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)。 |
|**operationVersion** | 发出请求时指定的存储服务版本。 这等效于 **x-ms-version** 标头的值。 例如：`2017-04-17`。|
|**schemaVersion** | 日志的架构版本。 例如：`1.0`。|
|**statusCode** | 请求的 HTTP 状态代码。 如果请求被中断，此值可能会设置为 `Unknown`。 <br> 例如：`206` |
|**statusText** | 所请求的操作的状态。  有关状态消息的完整列表，请参阅[“存储分析记录的操作和状态消息”主题](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)。 在版本 2017-04-17 及更高版本中，不使用状态消息 `ClientOtherError`。 相反，此字段包含错误代码。 例如：`SASSuccess`  |
|**durationMs** | 执行所请求操作的总时间（以毫秒为单位）。 这包括读取传入请求和向请求者发送响应的时间。 例如：`12`。|
|**callerIpAddress** | 请求者的 IP 地址，包括端口号。 例如：`192.100.0.102:4362`。 |
|**correlationId** | 用来跨资源将日志关联起来的 ID。 例如：`b99ba45e-a01e-0042-4ea6-772bbb000000`。 |
|**位置** | 存储帐户的位置。 例如：`North Europe`。 |
|protocol|操作中使用的协议。 例如：`HTTP`、`HTTPS`、`SMB` 或 `NFS`|
| **uri** | 所请求的统一资源标识符。 例如：`http://myaccountname.blob.core.windows.net/cont1/blobname?timeout=10`。 |

### <a name="fields-that-describe-how-the-operation-was-authenticated"></a>描述如何对操作进行身份验证的字段

```json
{
    "identity": {
        "authorization": [
            {
                "action": "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
                "principals": [
                    {
                        "id": "fde5ba15-4355-4223-b811-cccccccccccc",
                        "type": "User"
                    }
                ],
                "roleAssignmentId": "ecf75cb8-491c-4a25-ad6e-aaaaaaaaaaaa",
                "roleDefinitionId": "b7e6dc6d-f1e8-4753-8033-ffffffffffff"
            }
        ],
        "requester": {
            "appId": "691458b9-1327-4635-9f55-bbbbbbbbbbbb",
            "audience": "https://storage.azure.com/",
            "objectId": "fde5ba15-4355-4223-b811-cccccccccccc",
            "tenantId": "72f988bf-86f1-41af-91ab-dddddddddddd",
            "tokenIssuer": "https://sts.windows.net/72f988bf-86f1-41af-91ab-eeeeeeeeeeee/"
           },
        "type": "OAuth"
    },
}

```

| Property | 说明 |
|:--- |:---|
|**identity / type** | 用来发出请求的身份验证的类型。 例如：`OAuth`、`SAS Key`、`Account Key` 或 `Anonymous` |
|**identity / tokenHash**|此字段为保留字段，仅供内部使用。 |
|**authorization / action** | 分配给请求的操作。 例如： `Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read` |
|**authorization / roleAssignmentId** | 角色分配 ID。 例如：`4e2521b7-13be-4363-aeda-111111111111`。|
|**authorization / roleDefinitionId** | 角色定义 ID。 例如：`ba92f5b4-2d11-453d-a403-111111111111"`。|
|**principals / id** | 安全主体的 ID。 例如：`a4711f3a-254f-4cfb-8a2d-111111111111`。|
|**principals / type** | 安全主体的类型。 例如：`ServicePrincipal`。 |
|**requester / appID** | 用作请求者的 Open Authorization (OAuth) 应用程序 ID。 <br> 例如：`d3f7d5fe-e64a-4e4e-871d-333333333333`。|
|**requester / audience** | 请求的 OAuth 受众。 例如：`https://storage.azure.com`。 |
|**requester / objectId** | 请求者的 OAuth 对象 ID。 对于 Kerberos 身份验证，此项表示已经过 Kerberos 身份验证的用户的对象标识符。 例如：`0e0bf547-55e5-465c-91b7-2873712b249c`。 |
|**requester / tenantId** | 标识的 OAuth 租户 ID。 例如：`72f988bf-86f1-41af-91ab-222222222222`。|
|**requester / tokenIssuer** | OAuth 令牌颁发者。 例如：`https://sts.windows.net/72f988bf-86f1-41af-91ab-222222222222/`。|
|**requester / upn** | 请求者的用户主体名称 (UPN)。 例如：`someone@contoso.com`。 |
|**requester / userName** | 此字段为保留字段，仅供内部使用。|

### <a name="fields-that-describe-the-service"></a>描述服务的字段

```json
{
    "properties": {
        "accountName": "testaccount1",
        "requestUrl": "https://testaccount1.blob.core.windows.net:443/upload?restype=container&comp=list&prefix=&delimiter=%2F&marker=&maxresults=30&include=metadata&_=1551405598426",
        "userAgentHeader": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.140 Safari/537.36 Edge/17.17134",
        "referrerHeader": "blob:https://ms.portal.azure.com/6f50025f-3b88-488d-b29e-3c592a31ddc9",
        "clientRequestId": "",
        "etag": "",
        "serverLatencyMs": 63,
        "serviceType": "blob",
        "operationCount": 0,
        "requestHeaderSize": 2658,
        "requestBodySize": 0,
        "responseHeaderSize": 295,
        "responseBodySize": 2018,
        "contentLengthHeader": 0,
        "requestMd5": "",
        "serverMd5": "",
        "lastModifiedTime": "",
        "conditionsUsed": "",
        "smbTreeConnectID" : "0x3",
        "smbPersistentHandleID" : "0x6003f",
        "smbVolatileHandleID" : "0xFFFFFFFF00000065",
        "smbMessageID" : "0x3b165",
        "smbCreditsConsumed" : "0x3",
        "smbCommandDetail" : "0x2000 bytes at offset 0xf2000",
        "smbFileId" : " 0x9223442405598953",
        "smbSessionID" : "0x8530280128000049",
        "smbCommandMajor" : "0x6",
        "smbCommandMinor" : "DirectoryCloseAndDelete"
    }

}
```

| 属性 | 说明 |
|:--- |:---|
|**accountName** | 存储帐户的名称。 例如：`mystorageaccount`。  |
|**requestUrl** | 所请求的 URL。 例如：`http://mystorageaccount.blob.core.windows.net/cont1/blobname?timeout=10`。|
|**userAgentHeader** | **User-Agent** 标头值，带引号。 例如：`WA-Storage/6.2.0 (.NET CLR 4.0.30319.42000; Win32NT 6.2.9200.0)`。|
|**referrerHeader** | **Referrer** 标头值。 例如：`http://contoso.com/about.html`。|
|**clientRequestId** | 请求的 **x-ms-client-request-id** 标头值。 例如：`360b66a6-ad4f-4c4a-84a4-0ad7cb44f7a6`。 |
|**etag** | 返回的对象的 ETag 标识符，带引号。 例如：`0x8D101F7E4B662C4`。  |
|**serverLatencyMs** | 执行所请求操作的总时间（以毫秒为单位）。 此值不包括网络延迟（读取传入请求和向请求者发送响应的时间）。 例如：`22`。 |
|**serviceType** | 与此请求关联的服务。 例如：`blob`、`table`、`files` 或 `queue`。 |
|**operationCount** | 请求中涉及的每个已记录操作的编号。 此计数从索引 `0` 开始。 某些请求（例如复制 blob 的请求）需要多个操作。 大多数请求仅执行一个操作。 例如：`1`。 |
|**requestHeaderSize** | 请求标头的大小（以字节为单位）。 例如：`578`。 <br>如果请求失败，此值可能为空。 |
|**requestBodySize** | 存储服务读取的请求数据包的大小（以字节为单位）。 <br> 例如：`0`。 <br>如果请求失败，此值可能为空。  |
|**responseHeaderSize** | 响应标头的大小（以字节为单位）。 例如：`216`。 <br>如果请求失败，此值可能为空。  |
|**responseBodySize** | 存储服务写入的响应数据包的大小（以字节为单位）。 如果请求失败，此值可能为空。 例如：`216`。  |
|**requestMd5** | 请求中的 **Content-MD5** 标头或 **x-ms-content-md5** 标头的值。 此字段中指定的 MD5 哈希值表示请求中的内容。 例如：`788815fd0198be0d275ad329cafd1830`。 <br>此字段可以为空。  |
|**serverMd5** | 存储服务计算的 MD5 哈希的值。 例如：`3228b3cf1069a5489b298446321f8521`。 <br>此字段可以为空。  |
|**lastModifiedTime** | 返回的对象的上次修改时间 (LMT)。  例如：`Tuesday, 09-Aug-11 21:13:26 GMT`。 <br>对于可以返回多个对象的操作，此字段为空。 |
|**conditionsUsed** | 表示条件的键/值对的分号分隔列表。 条件可以是以下任意一种： <li> If-Modified-Since <li> If-Unmodified-Since <li> If-Match <li> If-None-Match  <br> 例如：`If-Modified-Since=Friday, 05-Aug-11 19:11:54 GMT`。 |
|**contentLengthHeader** | 发送到存储服务的请求的 Content-Length 标头值。 如果请求成功，则此值等于 requestBodySize。 如果请求失败，则此值可能不等于 requestBodySize，也可能为空。 |
|**tlsVersion** | 请求在连接时使用的 TLS 版本。 例如：`TLS 1.2`。 |
|**smbTreeConnectID** | 在树连接时建立的服务器消息块 (SMB) **treeConnectId**。 例如：`0x3` |
|**smbPersistentHandleID** | SMB2 CREATE 请求在经历网络重新连接后会保留的持久性句柄 ID。  在 [MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/f1d9b40d-e335-45fc-9d0b-199a31ede4c3) 2.2.14.1 中称为 **SMB2_FILEID.Persistent**。 例如：`0x6003f` |
|**smbVolatileHandleID** | SMB2 CREATE 请求在网络重新连接时将回收的易失句柄 ID。  在 [MS-SMB2](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/f1d9b40d-e335-45fc-9d0b-199a31ede4c3) 2.2.14.1 中称为 **SMB2_FILEID.Volatile**。 例如：`0xFFFFFFFF00000065` |
|**smbMessageID** | 连接相关 **MessageId**。 例如：`0x3b165` |
|**smbCreditsConsumed** | 请求消耗的流入量或流出量（以 64k 为单位）。 例如：`0x3` |
|**smbCommandDetail** | 有关此特定请求而不是常规类型请求的详细信息。 例如：`0x2000 bytes at offset 0xf2000` |
|**smbFileId** | 与文件或目录关联的 **FileId**。  大致类似于 NTFS FileId。 例如：`0x9223442405598953` |
|**smbSessionID** | 在建立会话时建立的 SMB2 **SessionId**。 例如：`0x8530280128000049` |
|**smbCommandMajor  uint32** | **SMB2_HEADER.Command** 中的值。 目前，这是一个 0 到 18（含）之间的数字。 例如：`0x6` |
|**smbCommandMinor** | **SmbCommandMajor** 的子类（如果适用）。 例如： `DirectoryCloseAndDelete` |

## <a name="see-also"></a>另请参阅

- 有关如何监视 Azure 存储的说明，请参阅[监视 Azure 存储](monitor-storage.md)。
- 有关监视 Azure 资源的详细信息，请参阅[使用 Azure Monitor 监视 Azure 资源](../../azure-monitor/insights/monitor-azure-resource.md)。