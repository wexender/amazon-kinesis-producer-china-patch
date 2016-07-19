### Patch for KPL

Patch for https://github.com/awslabs/amazon-kinesis-producer so it can work with China regions.

### Why?

This minor patch is intended to fix the "Even you setup customEndpoint and/or region correctly in KinesisProducerConfiguration your KPL CloudWatch Metrics still cannot be sent to the correct service endpoint becuase I dont't care about China regions." bug.

ERROR Example:

[2016-07-19 12:45:28.685895] [0x00007fff78f27300] [info] [metrics_manager.h:148] Uploading metrics to kinesis.cn-north-1.amazonaws.com.cn:443
[2016-07-19 12:46:29.223858] [0x00007fff78f27300] [error] [metrics_manager.cc:190] Metrics upload failed: 

### How?

1. Replace /aws/metrics/metrics_manager.h with the one provided in this patch.

2. Compile and Install follows the README.md from https://github.com/awslabs/amazon-kinesis-producer

3. This fix works with China regions (currently Beijing) only if you set customEndpoint property in KinesisProducerConfiguration. Setting region property along is not enough.

e.g:
	KinesisProducerConfiguration config = new KinesisProducerConfiguration()
        					.setCustomEndpoint("kinesis.cn-north-1.amazonaws.com.cn")
        					.setRegion("cn-north-1")
						...

