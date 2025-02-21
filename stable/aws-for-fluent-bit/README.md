# AWS for fluent bit

A helm chart for [AWS-for-fluent-bit](https://github.com/aws/aws-for-fluent-bit)

## Installing the Chart

Add the EKS repository to Helm:

```bash
helm repo add eks https://aws.github.io/eks-charts
```

Install or upgrading the AWS for fluent bit chart with default configuration:

```bash
helm upgrade --install aws-for-fluent-bit --namespace kube-system eks/aws-for-fluent-bit
```

## Uninstalling the Chart

To uninstall/delete the `aws-for-fluent-bit` release:

```bash
helm delete aws-for-fluent-bit --namespace kube-system
```

## Configuration

| Parameter | Description | Default | Required |
| - | - | - | -
| `global.namespaceOverride` | Override the deployment namespace | Not set (`Release.Namespace`) |
| `image.repository` | Image to deploy | `amazon/aws-for-fluent-bit` | ✔
| `image.tag` | Image tag to deploy | `2.21.5`
| `image.pullPolicy` | Pull policy for the image | `IfNotPresent` | ✔
| `imagePullSecrets` | Docker registry pull secret | `[]` |
| `serviceAccount.create` | Whether a new service account should be created | `true` |
| `serviceAccount.name` | Name of the service account | `aws-for-fluent-bit` |
| `serviceAccount.create` | Whether a new service account should be created | `true` |
| `service.extraService` | Append to existing service with this value | `""` |
| `service.parsersFiles` | List of available parser files | `/fluent-bit/parsers/parsers.conf` |
| `service.extraParsers` | Adding more parsers with this value | `""` |
| `input.*` | Values for Kubernetes input | |
| `extraInputs` | Append to existing input with this value | `""` |
| `additionalInputs` | Adding more inputs with this value | `""` |
| `filter.*` | Values for kubernetes filter | |
| `filter.extraFilters` | Append to existing filter with value |
| `additionalFilters` | Adding more filters with value |
| `cloudWatch.enabled` | Whether this plugin should be enabled or not [details](https://github.com/aws/amazon-cloudwatch-logs-for-fluent-bit) | `true` | ✔
| `cloudWatch.match` | The log filter | `*` | ✔
| `cloudWatch.region` | The AWS region for CloudWatch.  | `us-east-1` | ✔
| `cloudWatch.logGroupName` | The name of the CloudWatch Log Group that you want log records sent to. | `"/aws/eks/fluentbit-cloudwatch/logs"` | ✔
| `cloudWatch.logStreamName` | The name of the CloudWatch Log Stream that you want log records sent to. |  |
| `cloudWatch.logStreamPrefix` | Prefix for the Log Stream name. The tag is appended to the prefix to construct the full log stream name. Not compatible with the log_stream_name option. | `"fluentbit-"` |
| `cloudWatch.logKey` | By default, the whole log record will be sent to CloudWatch. If you specify a key name with this option, then only the value of that key will be sent to CloudWatch. For example, if you are using the Fluentd Docker log driver, you can specify logKey log and only the log message will be sent to CloudWatch. |  |
| `cloudWatch.logRetentionDays` | If set to a number greater than zero, and newly create log group's retention policy is set to this many days. | |
| `cloudWatch.logFormat` | An optional parameter that can be used to tell CloudWatch the format of the data. A value of json/emf enables CloudWatch to extract custom metrics embedded in a JSON payload. See the Embedded Metric Format. |  |
| `cloudWatch.roleArn` | ARN of an IAM role to assume (for cross account access). |  |
| `cloudWatch.autoCreateGroup` | Automatically create the log group. Valid values are "true" or "false" (case insensitive). | true |
| `cloudWatch.endpoint` | Specify a custom endpoint for the CloudWatch Logs API. |  |
| `cloudWatch.credentialsEndpoint` | Specify a custom HTTP endpoint to pull credentials from. [more info](https://github.com/aws/amazon-cloudwatch-logs-for-fluent-bit) |  |
| `cloudWatch.extraOutputs` | Append extra outputs with value | `""` |
| `firehose.enabled` | Whether this plugin should be enabled or not, [details](https://github.com/aws/amazon-kinesis-firehose-for-fluent-bit) | `true` | ✔
| `firehose.match` | The log filter | `"*"` | ✔
| `firehose.region` | The region which your Firehose delivery stream(s) is/are in. | `"us-east-1"` | ✔
| `firehose.deliveryStream` | The name of the delivery stream that you want log records sent to. | `"my-stream"` | ✔
| `firehose.dataKeys` | By default, the whole log record will be sent to Kinesis. If you specify a key name(s) with this option, then only those keys and values will be sent to Kinesis. For example, if you are using the Fluentd Docker log driver, you can specify data_keys log and only the log message will be sent to Kinesis. If you specify multiple keys, they should be comma delimited. | |
| `firehose.roleArn` | ARN of an IAM role to assume (for cross account access). | |
| `firehose.endpoint` | Specify a custom endpoint for the Kinesis Firehose API. | |
| `firehose.timeKey` | Add the timestamp to the record under this key. By default the timestamp from Fluent Bit will not be added to records sent to Kinesis. | |
| `firehose.timeKeyFormat` | strftime compliant format string for the timestamp; for example, `%Y-%m-%dT%H:%M:%S%z`. This option is used with `time_key`. | |
| `firehose.extraOutputs` | Append extra outputs with value | `""` |
| `kinesis.enabled` | Whether this plugin should be enabled or not, [details](https://github.com/aws/amazon-kinesis-streams-for-fluent-bit) | `true` | ✔
| `kinesis.match` | The log filter | `"*"` | ✔
| `kinesis.region` | The region which your Kinesis Data Stream is in. | `"us-east-1"` | ✔
| `kinesis.stream` | The name of the Kinesis Data Stream that you want log records sent to. | `"my-kinesis-stream-name"` | ✔
| `kinesis.partitionKey` | A partition key is used to group data by shard within a stream. A Kinesis Data Stream uses the partition key that is associated with each data record to determine which shard a given data record belongs to. For example, if your logs come from Docker containers, you can use container_id as the partition key, and the logs will be grouped and stored on different shards depending upon the id of the container they were generated from. As the data within a shard are coarsely ordered, you will get all your logs from one container in one shard roughly in order. If you don't set a partition key or put an invalid one, a random key will be generated, and the logs will be directed to random shards. If the partition key is invalid, the plugin will print an warning message. | `"container_id"` |
| `kinesis.appendNewline` | If you set append_newline as true, a newline will be addded after each log record. | |
| `kinesis.replaceDots` | Replace dot characters in key names with the value of this option. | |
| `kinesis.dataKeys` | By default, the whole log record will be sent to Kinesis. If you specify key name(s) with this option, then only those keys and values will be sent to Kinesis. For example, if you are using the Fluentd Docker log driver, you can specify data_keys log and only the log message will be sent to Kinesis. If you specify multiple keys, they should be comma delimited. | |
| `kinesis.roleArn` | ARN of an IAM role to assume (for cross account access). | |
| `kinesis.endpoint` | Specify a custom endpoint for the Kinesis Streams API. | |
| `kinesis.stsEndpoint` | Specify a custom endpoint for the STS API; used to assume your custom role provided with `kinesis.roleArn`. | |
| `kinesis.timeKey` | Add the timestamp to the record under this key. By default the timestamp from Fluent Bit will not be added to records sent to Kinesis. | |
| `kinesis.timeKeyFormat` |  strftime compliant format string for the timestamp; for example, `%Y-%m-%dT%H:%M:%S%z`. This option is used with `time_key`. | |
| `kinesis.aggregation` | Setting aggregation to `true` will enable KPL aggregation of records sent to Kinesis. This feature isn't compatible with the `partitionKey` feature.  See more about KPL aggregation [here](https://github.com/aws/amazon-kinesis-streams-for-fluent-bit#kpl-aggregation). | |
| `kinesis.compression` | Setting `compression` to `zlib` will enable zlib compression of each record. By default this feature is disabled and records are not compressed. | |
| `kinesis.extraOutputs` | Append extra outputs with value | `""` |
| `elasticsearch.enabled` | Whether this plugin should be enabled or not, [details](https://docs.fluentbit.io/manual/pipeline/outputs/elasticsearch) | `true` | ✔
| `elasticsearch.match` | The log filter | `"*"` | ✔
| `elasticsearch.awsRegion` | The region which your Firehose delivery stream(s) is/are in. | `"us-east-1"` | ✔
| `elasticsearch.host` | The url of the Elastic Search endpoint you want log records sent to. | | ✔
| `elasticsearch.awsAuth` | Enable AWS Sigv4 Authentication for Amazon ElasticSearch Service | On |
| `elasticsearch.tls` | Enable or disable TLS support | On |
| `elasticsearch.port` | TCP Port of the target service. | 443 |
| `elasticsearch.retryLimit` | Integer value to set the maximum number of retries allowed. N must be >= 1  | 6 |
| `elasticsearch.replaceDots` | Enable or disable Replace_Dots  | On |
| `elasticsearch.extraOutputs` | Append extra outputs with value | `""` |
| `s3.enabled` | Whether this plugin should be enabled or not, [details](https://docs.fluentbit.io/manual/pipeline/outputs/s3) | `false` | ✔
| `s3.match` | The log filter | `"*"` | ✔
| `s3.region` | The AWS region of your S3 bucket | `"us-east-1"` | ✔
| `s3.bucket` | Name of the S3 Bucket | `"my-bucket-name"` | ✔
| `s3.json_date_key` | Specify the name of the time key in the output record. To disable the time key just set the value to `false`. | |
| `s3.json_date_format` | Specify the format of the date. Supported formats are double, epoch, iso8601 (eg: 2018-05-30T09:39:52.000681Z) and java_sql_timestamp (eg: 2018-05-30 09:39:52.000681) | |
| `s3.total_file_size` | Specifies the size of files in S3. Maximum size is 50G, minimim is 1M | `"100M"` |
| `s3.upload_chunk_size` | The size of each 'part' for multipart uploads. Max: 50M | |
| `s3.upload_timeout` | Whenever this amount of time has elapsed, Fluent Bit will complete an upload and create a new file in S3. | |
| `s3.store_dir` | Directory to locally buffer data before sending. When multipart uploads are used, data will only be buffered until the upload_chunk_size is reached. | `"/tmp/fluent-bit/s3"` |
| `s3.s3_key_format` | Format string for keys in S3 | `"/fluent-bit-logs/$TAG/%Y/%m/%d/%H/%M/%S"` |
| `s3.s3_key_format_tag_delimiters` | A series of characters which will be used to split the tag into 'parts' for use with the s3_key_format option. | |
| `s3.static_file_path` | Disables behavior where UUID string is automatically appended to end of S3 key name when $UUID is not provided in s3_key_format. $UUID, time formatters, $TAG, and other dynamic key formatters all work as expected while this feature is set to true. | `false` |
| `s3.use_put_object` | Use the S3 PutObject API, instead of the multipart upload API. When this option is on, key extension is only available when $UUID is specified in `s3_key_format` | `false` |
| `s3.role_arn` | ARN of an IAM role to assume (ex. for cross account access). | |
| `s3.endpoint` | Custom endpoint for the S3 API. An endpoint can contain scheme and port. | |
| `s3.sts_endpoint` | Custom endpoint for the STS API. | |
| `s3.canned_acl` | Predefined Canned ACL policy for S3 objects. | |
| `s3.compression` | Compression type for S3 objects. `gzip` or `arrow`. Default: None | |
| `s3.content_type` | A standard MIME type for the S3 object; this will be set as the Content-Type HTTP header. | |
| `s3.send_content_md5` | Send the Content-MD5 header with PutObject and UploadPart requests, as is required when Object Lock is enabled. | `false` |
| `s3.auto_retry_requests` | Immediately retry failed requests to AWS services once. This option does not affect the normal Fluent Bit retry mechanism with backoff. Instead, it enables an immediate retry with no delay for networking errors, which may help improve throughput when there are transient/random networking issues. | `true` |
| `s3.log_key` | By default, the whole log record will be sent to S3. If you specify a key name with this option, then only the value of that key will be sent to S3. For example, if you are using Docker, you can specify log_key log and only the log message will be sent to S3. | |
| `s3.preserve_data_ordering` | Normally, when an upload request fails, there is a high chance for the last received chunk to be swapped with a later chunk, resulting in data shuffling. This feature prevents this shuffling by using a queue logic for uploads. | |
| `s3.storage_class` | Specify the storage class for S3 objects. If this option is not specified, objects will be stored with the default 'STANDARD' storage class. | |
| `additionalOutputs` | add outputs with value | `""` |
| `priorityClassName` | Name of Priority Class to assign pods | |
| `updateStrategy` | Optional update strategy | `type: RollingUpdate` |
| `affinity` | Map of node/pod affinities | `{}` |
| `env` | Optional List of pod environment variables for the pods | `[]` |
| `tolerations` | Optional deployment tolerations | `[]` |
| `nodeSelector` | Node labels for pod assignment | `{}` |
| `annotations` | Optional pod annotations | `{}` |
| `volumes` | Volumes for the pods, provide as a list of volume objects (see values.yaml) |  volumes for /var/log and /var/lib/docker/containers are present, along with a fluentbit config volume |
| `volumeMounts` | Volume mounts for the pods, provided as a list of volumeMount objects (see values.yaml) | volumes for /var/log and /var/lib/docker/containers are mounted, along with a fluentbit config volume |
| `dnsPolicy` | Optional dnsPolicy | `ClusterFirst` |
| `hostNetwork` | If true, use hostNetwork | `false` |
