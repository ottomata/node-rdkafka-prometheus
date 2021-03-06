# node-rdkafka-prometheus [![Build Status](https://travis-ci.org/Collaborne/node-rdkafka-prometheus.svg?branch=master)](https://travis-ci.org/Collaborne/node-rdkafka-prometheus)

Helper for exposing node-rdkafka statistics through prometheus

## Fork of [Collaborne/node-rdkafka-prometheus](https://github.com/Collaborne/node-rdkafka-prometheus) for use at the Wikimedia Foundation.

## Usage

```js
const RdkafkaPrometheus = require('node-rdkafka-prometheus');
const prometheus = require('prom-client');

const rdkafkaPrometheus= new RdkafkaPrometheus({
  // It is recommended that you pass in your require of prom-client to
  // RdkafkaPrometheus, as prom-client does not work well when it is required
  // more than once in the same process.
  // If you don't provide this, RdkafkaPrometheus will require('prom-client') itself.
  prometheus: prometheus, // optional
  extraLabels: { my_custom_label: 'label_value' },  // optional
  namePrefix: 'metric_name_prefix'  // optional
  // registers: [prometheus.register] // optional, this is the default.
})

// When setting up a consumer or producer:
const stream = rdkafka.KafkaConsumer.createReadStream({'statistics.interval.ms': 1000});
stream.consumer.on('event.stats', msg => {
  const stats = JSON.parse(msg.message);
  // Optional: override the default label value for any declared extraLabels.
  // If you use extraLabels here, the keys must have already been declared
  // in the extraLables RdkafaStats contructor option.
  const extraLabels = { my_custom_label: 'special_label_value' };
  rdkafkaPrometheus.observe(stats, extraLabels);
});
```

## License

This software is licensed under the Apache 2 license, quoted below.

```license
    Copyright 2011-2020 Collaborne B.V. <http://github.com/Collaborne/>

    Licensed under the Apache License, Version 2.0 (the "License"); you may not
    use this file except in compliance with the License. You may obtain a copy of
    the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
    WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the
    License for the specific language governing permissions and limitations under
    the License.
```
