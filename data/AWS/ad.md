# Anomaly detection in Amazon OpenSearch Service<a name="ad"></a>

Anomaly detection in Amazon OpenSearch Service automatically detects anomalies in your OpenSearch data in near\-real time by using the Random Cut Forest \(RCF\) algorithm\. RCF is an unsupervised machine learning algorithm that models a sketch of your incoming data stream\. The algorithm computes an `anomaly grade` and `confidence score` value for each incoming data point\. Anomaly detection uses these values to differentiate an anomaly from normal variations in your data\. 

You can pair the anomaly detection plugin with the [Configuring alerts in Amazon OpenSearch Service](alerting.md) plugin to notify you as soon as an anomaly is detected\. 

Anomaly detection is available on domains running any OpenSearch version or Elasticsearch 7\.4 or later\. All instance types support anomaly detection except for `t2.micro` and `t2.small`\. Full documentation for anomaly detection, including detailed steps and API descriptions, is available in the [OpenSearch documentation](https://opensearch.org/docs/latest/monitoring-plugins/ad/index/)\.

## <a name="ad-example"></a>

### Prerequisites<a name="ad-prereq"></a>

Anomaly detection has the following prerequisites:
+ Anomaly detection requires OpenSearch or Elasticsearch 7\.4 or later\. 
+ Anomaly detection only supports [fine\-grained access control](fgac.md) on Elasticsearch versions 7\.9 and later and all versions of OpenSearch\. Prior to Elasticsearch 7\.9, only admin users can create, view, and manage detectors\. 
+ If your domain uses fine\-grained access control, non\-admin users must be [mapped](fgac.md#fgac-mapping) to the `anomaly_read_access` role in OpenSearch Dashboards in order to view detectors, or `anomaly_full_access` in order to create and manage detectors\.

### Getting started with anomaly detection<a name="ad-example-es"></a>

To get started, choose **Anomaly Detection** in OpenSearch Dashboards\.

#### Step 1: Create a detector<a name="ad-example-1"></a>

A detector is an individual anomaly detection task\. You can create multiple detectors, and all the detectors can run simultaneously, with each analyzing data from different sources\.

#### Step 2: Add features to your detector<a name="ad-example-2"></a>

A feature is the field in your index that you check for anomalies\. A detector can discover anomalies across one or more features\. You must choose one of the following aggregations for each feature: `average()`, `sum()`, `count()`, `min()`, or `max()`\. 

**Note**  
The `count()` aggregation method is only available in OpenSearch and Elasticsearch 7\.7 or later\. For Elasticsearch 7\.4, use a custom expression like the following:  

```
{
  "aggregation_name": {
     "value_count": {
        "field": "field_name"
     }
  }
}
```

The aggregation method determines what constitutes an anomaly\. For example, if you choose `min()`, the detector focuses on finding anomalies based on the minimum values of your feature\. If you choose `average()`, the detector finds anomalies based on the average values of your feature\. You can add a maximum of five features per detector\.

You can configure the following optional settings \(available in Elasticsearch 7\.7 and later\):
+ **Category field** \- Categorize or slice your data with a dimension like IP address, product ID, country code, and so on\.
+ **Window size** \- Set the number of aggregation intervals from your data stream to consider in a detection window\.

After you set up your features, preview sample anomalies and adjust the feature settings if necessary\.

#### Step 3: Observe the results<a name="ad-example-3"></a>

![\[The following visualizations are available on the anomaly detection dashboard:\]](http://docs.aws.amazon.com/opensearch-service/latest/developerguide/images/ad.png)
+ **Live anomalies** \- displays the live anomaly results for the last 60 intervals\. For example, if the interval is set to 10, it shows the results for the last 600 minutes\. This chart refreshes every 30 seconds\.
+ **Anomaly history** \- plots the anomaly grade with the corresponding measure of confidence\.
+ **Feature breakdown** \- plots the features based on the aggregation method\. You can vary the date\-time range of the detector\.
+ **Anomaly occurrence** \- shows the `Start time`, `End time`, `Data confidence`, and `Anomaly grade` for each anomaly detected\.

  If you set the category field, you see an additional **Heat map** chart that correlates results for anomalous entities\. Choose a filled rectangle to see a more detailed view of the anomaly\.

#### Step 4: Set up alerts<a name="ad-example-4"></a>

To create a monitor to send you notifications when any anomalies are detected, choose **Set up alerts**\. The plugin redirects you to the [https://opensearch.org/docs/monitoring-plugins/alerting/monitors/#create-monitors](https://opensearch.org/docs/monitoring-plugins/alerting/monitors/#create-monitors) page where you can configure an alert\.