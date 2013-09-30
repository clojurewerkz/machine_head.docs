---
title: "Getting Started with MQTT and Clojure"
layout: article
---


## About this guide

This guide is a quick tutorial that helps you to get started with the MQTT protocol in general and the Machine Head in particular.
It should take about 15 minutes to read and study the provided code examples. This guide covers:

 * Installing an MQTT broker
 * Adding Machine Head dependency with [Leiningen](http://leiningen.org) or [Maven](http://maven.apache.org/)
 * Running a "Hello, world" messaging example that is a simple demonstration of 1:1 communication.
 * Creating a "Twitter-like" publish/subscribe example with one publisher and four subscribers that demonstrates 1:n communication.
 * Creating a topic routing example with two publishers and eight subscribers showcasing n:m communication when subscribers only receive messages that they are interested in.

This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/3.0/">Creative Commons Attribution 3.0 Unported License</a>
(including images and stylesheets). The source is available [on Github](https://github.com/clojurewerkz/machine_head.docs).


## What version of Machine Head does this guide cover?

This guide covers Machine Head 1.0.0-beta1.


## Supported Clojure Versions

Machine Head requires Clojure 1.4+. The latest stable release is recommended.


## Supported MQTT Brokers

Machine Head is tested against [RabbitMQ 3.x MQTT plugin](http://www.rabbitmq.com/mqtt.html) and [Mosquitto](http://mosquitto.org/).


## Overview

Machine Head is a Clojure client for [MQTT](http://mqtt.org/) v3.1 brokers. MQTT is
an efficient messaging protocol designed primarily for low-power devices such as
telemetry sensors. 

Clients communicate with MQTT brokers such as RabbitMQ and Mosquitto. Clients that
publish messages are called *producers* or *publishers*, those that consume
messages are *consumers* or *subscribers*. Message distribution happens in
communication points known as *topics*, which filter published messages and
deliver those that match to consumers.

