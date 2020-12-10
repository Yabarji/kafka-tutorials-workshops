# Kafka Tutorial Workshops

Code supporting meetup workshops based on [Kafka Tutorials](https://kafka-tutorials.confluent.io/).


## Introduction

This workshop is based on the [scheduling operations](https://kafka-tutorials.confluent.io/kafka-streams-schedule-operations/kstreams.html) Kafka Tutorial.  

### Workshop Steps

####  Provision a new ccloud-stack on Confluent Cloud

This part assumes you have already set-up an account on [Confluent CLoud](https://confluent.cloud/) and you've installed the [Confluent Cloud CLI](https://docs.confluent.io/ccloud-cli/current/install.html). We're going to use the `ccloud-stack` utility to get everything set-up to work along with the workshop.  

A copy of the [ccloud_library.sh](https://github.com/confluentinc/examples/blob/latest/utils/ccloud_library.sh) is included in this repo and let's run this command now:

```
source ./ccloud_library.sh
```

Then let's create the stack of Confluent Cloud resources:

```
CLUSTER_CLOUD=aws
CLUSTER_REGION=us-west-2
ccloud::create_ccloud_stack
```

NOTE: Make sure you destroy all resources when the workshop concludes.

The `create` command generates a local config file, `java-service-account-NNNNN.config` when it completes. The `NNNNN` represents the service account id.  Take a quick look at the file:

```
cat stack-configs/java-service-account-*.config
```

You should see something like this:

```
# ENVIRONMENT ID: <ENVIRONMENT ID>
# SERVICE ACCOUNT ID: <SERVICE ACCOUNT ID>
# KAFKA CLUSTER ID: <KAFKA CLUSTER ID>
# SCHEMA REGISTRY CLUSTER ID: <SCHEMA REGISTRY CLUSTER ID>
# ------------------------------
ssl.endpoint.identification.algorithm=https
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
bootstrap.servers=<BROKER ENDPOINT>
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="<API KEY>" password="<API SECRET>";
basic.auth.credentials.source=USER_INFO
schema.registry.basic.auth.user.info=<SR API KEY>:<SR API SECRET>
schema.registry.url=https://<SR ENDPOINT>
```

#### Working with nested JSON
Kafka-tutorial link: https://kafka-tutorials.confluent.io/working-with-nested-json/ksql.html

Because we setup our Kafka cluster and ksqlDB application in CCloud, we can skip to 3 in the project.



####  Clean Up

If you haven't done so already, now is a good time to shut down all the resources we've created and started.  Because your Confluent Cloud cluster is using real cloud resources and is billable, delete the connector and clean up your Confluent Cloud environment when you complete this tutorial. You can use Confluent Cloud CLI or Confluent UI, but for this tutorial you can use the ccloud_library.sh library again. Pass in the `SERVICE_ACCOUNT_ID` that was generated when the `ccloud-stack was` created.

First clean up the `ccloud-stack`:

```
ccloud::destroy_ccloud_stack $SERVICE_ACCOUNT_ID
```

Then shut down the connect docker container:

```
docker-compose down
```
