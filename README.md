coreos_zk_kafka_notes
=====================

# ex-zk1.service

- [ex-zk1.service](https://github.com/solarce/coreos_zk_kafka_notes/blob/master/ex-zk1.service)

This is an example unit file for running an [Exhibitor](https://github.com/Netflix/exhibitor/wiki/Running-Exhibitor) managed [Zookeeper](https://zookeeper.apache.org/) container.

To use it you'll need to create an S3 bucket, IAM account and give the IAM account the ability to write to the S3 bucket. Then fill in the following environment variables in the unit file:

- `-e S3_BUCKET=<BUCKET_NAME>` - The bucket name
- `-e S3_PREFIX=<KEY_NAME>` - Exhibitor will use to make a unique name the config file for a cluster it is managing.
- `-e AWS_ACCESS_KEY_ID=<ACCESS_KEY>` - From the IAM account
- `-e AWS_SECRET_ACCESS_KEY=<SECRET_KEY>` - From the IAM account
- `-e HOSTNAME=<DNS_NAME_OF_COREOS_HOST>` - This will be the FQDN of the CoreOS host this unit is gonna run on. This is a limitation of my experimenting with CoreOS so far and should be able to be address with more digging into complex networking with CoreOS.

# kafka1.service

- [ex-zk1.service](https://github.com/solarce/coreos_zk_kafka_notes/blob/master/kafka1.service)

This is an example unit file for running a [Kafka](https://kafka.apache.org/) container.

To use it you'll need to have setup one or more Zookeeper servers for it to use. Then fill in the following environment variables in the unit file.

- `-e KAFKA_ZOOKEEPER_CONNECT=<LIST_OF_ZOOKEEPER_HOSTS>` - If you're copying the setup I did using CloudFormation and using my Zookeeper unit file, then you'll give it a list of your CoreOS IPs, e.g. `KAFKA_ZOOKEEPER_CONNECT=10.1.1.10:2181,10.1.1.12:2181,10.2.1.15:2181`

# coreos_cfn_template.json

- [coreos_cfn_template.json](https://github.com/solarce/coreos_zk_kafka_notes/blob/master/coreos_cfn_template.json)

This is a CloudFormation template file you can use to launch a three node CoreOS cluster. It'll use an auto-scaling group to launch them, it assumes you have

1. A discovery token from `https://discovery.etcd.io/new`
2. An existing VPC
3. Subnets in AZ1 and AZ2 of the region your VPC is in
4. You only want to deploy to two AZs
5. You have an SSH public key uploaded to EC2
6. You want to use an __m3.large__ for the instances
