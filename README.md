# Introduction

This is a HTTP based ingestor skeleton project, it uses Spring Boot REST. The ingestor also uses Spring Kafka, all properties are configurable using the present Spring Kafka properties. The goal of this project is to provide an implementation of a HTTP ingestor that allows for a custom implementation.

# Specification

A generated OpenAPI specification document can be found [here](bigg-ingestor-openapi.json). The API has the following endpoints:

## /ingest

This endpoint takes in raw data and pushes it to kafka. It also takes a **topic** and a **key** as request parameters.

## /ingest/harmonize

This endpoint defines an interface with fields that are required for the **harmonization process**. It also takes a **topic** and a **key** as request parameters.


# Configuration

It is possible to configure the ingestor using Spring properties by passing **environment variables** into the docker container. Spring implements different formats to represent configuration properties ["relaxed bindings"](https://docs.spring.io/spring-boot/docs/2.0.x/reference/html/boot-features-external-config.html#boot-features-external-config-relaxed-binding)). Injecting these properties as environment variables happens using the **uppercase format**.

[Find a list of all default Spring properties here](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html)

[Find a list of all available Kafka properties here](https://gist.github.com/geunho/77f3f9a112ea327457353aa407328771)

## Custom Properties

|property|default value|description|
|-|-|-|
|ingestor.kafka.send.timeout|10|How long the ingestor will wait **in seconds** to get and ACK from kafka, before returning an error.|

# Custom Implementation

This ingestor uses an interface called 'CustomService' that allows the processing of data to be extended. In order to do this, a bean should be created that implements the 'CustomService' interface. The functionality in this bean is executed **after** data is received by the ingestor, and **before** the data is sent to Kafka.

The interface requires the following methods to be implemented:

```
public interface CustomService {
    String modifyData(String data);
    HarmonizerInput modifyData(HarmonizerInput data);
}
```
