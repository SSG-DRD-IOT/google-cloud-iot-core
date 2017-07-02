# Experiements with Google Cloud IoT Core

[TOC]

These excercises demonstrate cloud based messaging in an Industrial factory setting.

We will begin with basic, practical excercises and then move toward specific examples that illustrate the themes of [Industrial 4.0](https://en.wikipedia.org/wiki/Industry_4.0).

Let's begin with a quote from American engineer Charles Kettering (1876 to 1958)

> If you’re doing something the same way you have been doing
> it for ten years, the chances are you are doing it wrong.
> [name=Charles Kettering, Engineer] [color=lightgreen]


## Origin and Themes of Industry 4.0

The term **Industrie 4.0** originates from a project in the high-tech strategy of the German government, which promotes the computerization of manufacturing. The members of the Industrie 4.0 committees are recoginzied as the driving forces of this movement.

Major themes of this movement have been organized into **horizontal segments** (themes that effect all segments of the market) and **vertical segments** (themes that are particular to a one or more individual segments).

The vertical segments have been organized into workgroups that address the changings aspects of increased industrialization.

The horizontal segments have been organized into a series of **Design Principles** and laid out in a paper titled [Design Principles for Industrie 4.0 Scenarios](http://ieeexplore.ieee.org/document/7427673/?arnumber=7427673&newsearch=true&queryText=industrie%204.0%20design%20principles).

### Vertical Segments

As connected objects begin to be become more common in industrial applications, the effects can be classified into vertical and horitzonal categories.

**Industrie 4.0 Workgroups**
Co-Chair Henning Kagermann and Siegfried Dais
| WG# | Description    | Chairperson |
| :----------: | -------------- | ----------- |
| 1 | The Smart Factory       | Manfred Wittenstein|
| 2 | The Real Environment    | Siegfried Russwurm|
| 3 | The Economic Environment| Stephan Fische|
| 4 | Human Beings and Work   | Wolfgang Wahlster|
| 5 | The Technology Factor   | Heinz Derenbach|

### Horizontal Segments

The design priciples can be thought of as a guiding philosophy. We will see how Messaging Services.

These horizontal design principles are sourced from the Wikipedia articule on [Industry 4.0](https://en.wikipedia.org/wiki/Industry_4.0#Design_principles).

- **Interoperability**: The ability of machines, devices, sensors, and people to connect and communicate with each other via the Internet of Things (IoT)
- **Information transparency**: The ability of information systems to create a virtual copy of the physical world by enriching digital plant models with sensor data. This requires the aggregation of raw sensor data to higher-value context information.
- **Technical assistance**: First, the ability of assistance systems to support humans by aggregating and visualizing information comprehensibly for making informed decisions and solving urgent problems on short notice. Second, the ability of cyber physical systems to physically support humans by conducting a range of tasks that are unpleasant, too exhausting, or unsafe for their human co-workers.
- **Decentralized decisions**: The ability of cyber physical systems to make decisions on their own and to perform their tasks as autonomously as possible. Only in the case of exceptions, interferences, or conflicting goals, are tasks delegated to a higher level.


## MQTT Overview

### Diagram of Simple MQTT Interaction
MQTT Pub/Sub
![](https://i.imgur.com/kClZdzu.png)

<!-- ```sequence
Subscriber->Broker: sub(topic)
Publisher->Broker: pub(topic, data)
Broker->Subscriber: pub(topic, data)
``` -->

## Google Cloud IoT Core: Bash Shell Client

### Setup Environment Variables
For the convenience of being able to copy these commands directly from this document to your Bash Shell, we will setup a couple of environment variables.

:::info
Insert infomation that is particular to your project in the command line below, replacing the text in **<>**
:::

``` bash
export PROJECT_ID = <project_id>;
export REGION_ID = us-central1;
export REGISTRY_ID = factory-registry

```

### Create a IoT Device Registry

``` bash
gcloud beta iot registries create $REGISTRY_ID \
    --project=$PROJECT_ID \
    --region=$REGION_ID \
    --pubsub-topic=projects/$PROJECT_ID/topics/device-events
```

### Create a RSA Key
``` bash
openssl req -x509 -newkey rsa:2048 -keyout rsa_private.pem -nodes -out \
    rsa_cert.pem -subj "/CN=unused"
```

### Create devices. To create an RS256 authenticated device using the RSA certificate key, run the following commands:
``` bash
gcloud beta iot devices create my-rs256-device \
  --project=lightswitch-172303 \
  --region=us-central1 \
  --registry=my-registry \
  --public-key path=rsa_cert.pem,type=rs256
```

### To create an ES256 authenticated device using the Elliptic Curve public key, run the following commands:
``` bash
gcloud beta iot devices create my-es256-device \
  --project=lightswitch-172303 \
  --region=us-central1 \
  --registry=my-registry \
  --public-key path=ec_public.pem,type=es256
```

# Device authentication
projects/lightswitch-172303/locations/us-central1/registries/my-registry/devices/{device-id}
## Registering an IoT Device from the Bash Shell
