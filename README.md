### Patch for KPL

Patch for https://github.com/awslabs/amazon-kinesis-producer, so it can work with China regions.

### Why?

For KinesisProducerConfiguration:
* a. If you set "region" to cn-north-1, the endpoint is incorrect without ".cn" trailing the DNS name.
* b. If you set customEndpoint, kinesis works but cloudwatch metrics will be sent to kinesis service endpoint.

ERROR Example(a):
```
[error] [http_client.cc:148] Failed to open connection to monitoring.cn-north-1.amazonaws.com:443 : Host not found (authoritative)
[error] [http_client.cc:148] Failed to open connection to kinesis.cn-north-1.amazonaws.com:443 : Host not found (authoritative)
```

ERROR Example(b):
```
[error] [metrics_manager.cc:190] Metrics upload failed:
```

### How?

0. Clone https://github.com/awslabs/amazon-kinesis-producer.git

1. Replace /aws/metrics/metrics_manager.h with the one provided in this patch.

2. Follow the README.md in https://github.com/awslabs/amazon-kinesis-producer to compile and install. After the final step, by ignoring testing errors you might need to package binaries under java/amazon-kinesis-producer/target/classes into a jar file manully, for example by using jar -cvf. 

3. In your application, set BOTH customEndpoint and region properties correctly in KinesisProducerConfiguration.

e.g:
```	
KinesisProducerConfiguration config = new KinesisProducerConfiguration()
.setCustomEndpoint("kinesis.cn-north-1.amazonaws.com.cn")
.setRegion("cn-north-1")
...
```

