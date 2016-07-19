### Patch for KPL

Patch for https://github.com/awslabs/amazon-kinesis-producer so it can work with China regions.

### Why?

For KinesisProducerConfiguration:
a. If you set "region" to cn-north-1, the endpoint is incorrect without ".cn" trailing the DNS name.
b. If you set customEndpoint, kinesis works but cloudwatch metrics will be sent to kinesis service endpoint.

ERROR Example(a):


ERROR Example(b):

`[2016-07-19 12:45:28.685895] [0x00007fff78f27300] [info] [metrics_manager.h:148] Uploading metrics to kinesis.cn-north-1.amazonaws.com.cn:443
[2016-07-19 12:46:29.223858] [0x00007fff78f27300] [error] [metrics_manager.cc:190] Metrics upload failed: `

### How?

0. git clone https://github.com/awslabs/amazon-kinesis-producer.git

1. Replace /aws/metrics/metrics_manager.h with the one provided in this patch.

2. Follow the README.md in https://github.com/awslabs/amazon-kinesis-producer to compile and install.

3. This fix works with China regions (currently Beijing) only if you set both customEndpoint and region properties correctly in KinesisProducerConfiguration.

e.g:
	`KinesisProducerConfiguration config = new KinesisProducerConfiguration()
        					.setCustomEndpoint("kinesis.cn-north-1.amazonaws.com.cn")
        					.setRegion("cn-north-1")
						...`

