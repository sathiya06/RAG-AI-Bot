# Managing capacity limits for Amazon OpenSearch Serverless<a name="serverless-scaling"></a>

With Amazon OpenSearch Serverless, you don't have to manage capacity yourself\. OpenSearch Serverless automatically scales compute capacity for your account based on the current workload\. Serverless compute capacity is measured in *OpenSearch Compute Units* \(OCUs\)\. Each OCU is a combination of 6 GiB of memory and corresponding virtual CPU \(vCPU\), as well as data transfer to Amazon S3\. For more information about the decoupled architecture in OpenSearch Serverless, see [How it works](serverless-overview.md#serverless-process)\.

When you create your first collection, OpenSearch Serverless instantiates a total of four OCUs \(two for indexing and two for search\)\. These OCUs always exist, even when there's no indexing or search activity\. All subsequent collections can share these OCUs \(except for collections with unique AWS KMS keys, which instantiate their own set of four OCUs\)\. If needed, OpenSearch Serverless automatically scales out and adds additional OCUs as your indexing and search usage grows\. When traffic on your collection endpoint decreases, capacity scales back down to the minimum number of OCUs required for your data size\. At most, it will scale down to 2 OCUs for indexing and 2 OCUs for search\.

**Note**  
Although capacity can scale up to accomodate data, it can't currently scale up *search* replicas\. OpenSearch Serverless functions with two search replicas running in active\-active mode\. If you need more replicas for your workload, contact [AWS Support](https://console.aws.amazon.com/support/home)\.

For *search* and *vector search* collections, all data is stored in the hot cache to ensure fast query response times\. For *time series* collections, we use a combination of hot and warm caches, keeping the most recent data in the hot cache to optimize query response times for more frequently accessed data\. For more information, see [Choosing a collection type](serverless-overview.md#serverless-usecase)\.

To manage capacity for your collections and to control costs, you can specify the overall maximum indexing and search capacity for the current account and Region, and OpenSearch Serverless scales out your collection resources automatically based on these specifications\.

Because indexing and search capacity scale separately, you specify account\-level limits for each:
+ **Maximum indexing capacity** – OpenSearch Serverless can increase indexing capacity up to this number of OCUs\. 
+ **Maximum search capacity** – OpenSearch Serverless can increase search capacity up to this number of OCUs\.

**Note**  
At this time, capacity settings only apply at the account level\. You can't configure per\-collection capacity limits\.

Your goal should be to ensure that the maximum capacity is high enough to handle spikes in workload\. Based on your settings, OpenSearch Serverless automatically scales out the number of OCUs for your collections to process the indexing and search workload\.

**Topics**
+ [Configuring capacity settings](#serverless-scaling-configure)
+ [Maximum capacity limits](#serverless-scaling-limits)
+ [Monitoring capacity usage](#serverless-scaling-monitoring)

## Configuring capacity settings<a name="serverless-scaling-configure"></a>

To configure capacity settings in the OpenSearch Serverless console, expand **Serverless** in the left navigation pane and select **Dashboard**\. Specify the maximum indexing and search capacity under **Capacity management**:

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/opensearch-service/latest/developerguide/images/ServerlessCapacity.png)

To configure capacity using the AWS CLI, send an [UpdateAccountSettings](https://docs.aws.amazon.com/opensearch-service/latest/ServerlessAPIReference/API_UpdateAccountSettings.html) request:

```
aws opensearchserverless update-account-settings \
    --capacity-limits '{ "maxIndexingCapacityInOCU": 8,"maxSearchCapacityInOCU": 9 }'
```

## Maximum capacity limits<a name="serverless-scaling-limits"></a>

For all three types of collections, the default maximum capacity is 10 OCUs for indexing and 10 OCUs for search\. The minimum allowed capacity for an account is 2 OCUs for indexing and 2 OCUs for search\. For *search* and *time series* collections, the maximum allowed capacity is 100 OCUs for indexing and 100 OCUs for search\. For *vector search* collections, the maximum allowed capacity is 20 OCUs for indexing and 20 OCUs for search\. You can configure the OCU count to be any number from 2 to the maximum allowed capacity according to collection type, in multiples of 2\. 

Each OCU includes enough hot ephemeral storage for 120 GiB of index data\. OpenSearch Serverless supports up to 1 TiB of index data per *search* and *vector search* collections, 6 TiB of index data per *time series* collection, and 6 TiB of index data per account\. You can still ingest more data, which can be stored in S3\.

For a list of all quotas, see [OpenSearch Serverless quotas](limits.md#limits-serverless)\.

## Monitoring capacity usage<a name="serverless-scaling-monitoring"></a>

You can monitor the `SearchOCU` and `IndexingOCU` account\-level CloudWatch metrics to understand how your collections are scaling\. We recommend that you configure alarms to notify you if your account is approaching a threshold for metrics related to capacity, so you can adjust your capacity settings accordingly\.

You can also use these metrics to determine if your maximum capacity settings are appropriate, or if you need to adjust them\. Analyze these metrics to focus your efforts for optimizing the efficiency of your collections\. For more information about the metrics that OpenSearch Serverless sends to CloudWatch, see [Monitoring Amazon OpenSearch Serverless](serverless-monitoring.md)\.