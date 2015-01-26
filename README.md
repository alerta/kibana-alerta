Alerta-to-Kibana
================

Forward Alerta alerts via Logstash to Elasticsearch for visualisation in Kibana3.

Installation
------------

Install the following packages:

1. Logstash
2. Elasticsearch
3. Kibana

Configuration
-------------

Enable the `logstash` plug-in which is included in `alerta` by default:

```
PLUGINS = ['reject','logstash']
LOGSTASH_HOST = 'localhost'
LOGSTASH_PORT = 1514
```

Configure `logstash` to parse json-encoded alerts and forward them to elasticsearch:

```
input {
    tcp {
        port  => 1514
        codec => json_lines
    }
}
output {
    # stdout {}
    elasticsearch {
        protocol => "http"
        host     => "localhost"
    }
}
```

Either configure a Kibana dashboard manually or load the example dashboard from this repo.

    Menu -> Load -> Advanced -> Choose File

Testing
-------

Run `logstash` in debug mode:

    $ stop logstash
    $ /opt/logstash/bin/logstash agent -f /etc/logstash/conf.d/alerta.conf -vvv

To view alerts as they would be sent to elasticsearch uncomment the `stdout{}` line in the `logstash.conf` file above.

List elasticsearch indices:

    http://localhost:9200/_cat/indices?v


Vagrant
-------

Alternatively, make use of the [vagrant-try-alerta](https://github.com/alerta/vagrant-try-alerta) repo...

    $ git clone https://github.com/alerta/vagrant-try-alerta.git
    $ cd vagrant-try-alerta
    $ vagrant up alerta-kibana
    $ vagrant ssh alerta-kibana

License
-------

Copyright (c) 2014 Nick Satterly. Available under the MIT License.

