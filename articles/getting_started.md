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

Machine Head is a minimalistic Clojure MQTT client. It is designed with
ease of use and efficiency in mind.


## Installing MQTT Broker

### RabbitMQ

The RabbitMQ site has a good [installation
guide](http://www.rabbitmq.com/install.html) that addresses many
operating systems.  On Mac OS X, the fastest way to install RabbitMQ
is with [Homebrew](http://mxcl.github.com/homebrew/):

    brew install rabbitmq

Next, enable MQTT plugin:

    rabbitmq-plugins enable rabbitmq_mqtt

then run the broker:

    rabbitmq-server

On Debian and Ubuntu, you can either [download the RabbitMQ .deb
package](http://www.rabbitmq.com/server.html) and install it with
[dpkg](http://www.debian.org/doc/FAQ/ch-pkgtools.en.html) or make use
of the [RabbitMQ apt
repository](http://www.rabbitmq.com/debian.html#apt).

For RPM-based distributions like RedHat or CentOS, the RabbitMQ team
provides an [RPM package](http://www.rabbitmq.com/install.html#rpm).

<div class="alert alert-error"> <strong>Note:</strong> The RabbitMQ
package that ships with some of the recent Ubuntu versions (for
example, 11.10 and 12.04) is outdated and *may not ship with MQTT
plugin* (you will need at least RabbitMQ v3.0 for use with this
guide).  </div>



### Mosquitto

On Mac OS X, the fastest way to install Mosquitto is with
[Homebrew](http://mxcl.github.com/homebrew/):

    brew install mosquitto

then run the broker:

    # alter configuration file path depending on your
    # Homebrew root location
    mosquitto /usr/local/etc/mosquitto/mosquitto.conf


## Adding Langohr Dependency To Your Project

Langohr artifacts are [released to Clojars](https://clojars.org/com.novemberain/langohr).

### With Leiningen

Add Eclipse Paho repository to `project.clj`:

``` clojure
:repositories {"eclipse-paho" {:url "https://repo.eclipse.org/content/groups/paho/"
                               :snapshots false
                               :releases {:checksum :fail}}}
```


And then the dependency:

``` clojure
[clojurewerkz/machine_head "1.0.0-beta1"]
```

### With Maven

Add Clojars and Eclipse Paho repository definitions to your `pom.xml`:

``` xml
<repository>
  <id>clojars.org</id>
  <url>http://clojars.org/repo</url>
</repository>
<repository>
  <id>eclipse-paho</id>
  <url>https://repo.eclipse.org/content/groups/paho/</url>
</repository>
```

And then the dependency:

``` xml
<dependency>
  <groupId>clojurewerkz</groupId>
  <artifactId>machine_head</artifactId>
  <version>1.0.0-beta1</version>
</dependency>
```

### Verifying Your Installation

You can verify your installation in the REPL:

    $ lein repl
    user=> (require '[clojurewerkz.machine-head.client :as mh])
    ;= nil
    user=> (mh/connect)
