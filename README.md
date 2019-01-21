# AMQP output plugin for Fluentd

Fluentd output plugin to publish events to an amqp broker.

This project is a fork from the original [fluent-plugin-amqp](https://github.com/restorando/fluent-plugin-amqp). It adds a new _time_ field in the fluentd payload that is natively compatible with Elasticsearch date field and the possibility to set a custom routing key the AMQP Exchange.

Events are published one by one using the Fluentd tag as the routing key by default, in JSON format like:

```javascript
{ "key": "fluentd-tag", "timestamp": 1547769614, "payload": "event-payload", "time": "2019-01-21T01:18:39+00:00" }
```

## Installation

Add this line to your application's Gemfile:

    gem 'fluent-plugin-amqp2'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install fluent-plugin-amqp2

## Configuration

```
<match pattern>
  type amqp

  # Set broker host and port
  host localhost
  port 5672

  # Set user and password for authentication
  user guest
  password guest

  # Configure amqp entities vhost, exchange id and type
  vhost /
  exchange my_exchange
  exchange_type topic
  exchange_durable true # optionally set exchange durability - default is true.
  routing_key my_routing_key # optional - default is fluentd tag
  passive false # If true, will not try to create the exchange - default is false.
  payload_only false # optional - default is false. if true, only the payload will be sent. if false, data format is { "key" => tag, "timestamp" => time, "payload" => record }.
  content_type application/octet-stream # optional - default is application/octet-stream. some amqp consumers will expect application/json.
  priority 0 # the priority for the message - requires bunny >= 1.1.6 and rabbitmq >= 3.5
</match>
```

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
