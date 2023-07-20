# Grafana - Influx - Telegraf

## Starting

Start the setup using:
```
docker compose up -d
```

## Playing around

### Logging in

You can access Grafana on http://localhost:3000.
Log in with the default credentials: admin/admin.

### Adding influxdb as data source

Add an influxDB data source in Grafana with the following settings:
- URL: http://influx-grafana:8086
- Auth: Basic Auth with credentials admin/admin (these are the influxdb credentials, which are separate from the
  aforementioned grafana credentials although they look the same).
- Database: tickdemo

Leave the other settings as they are. Then hit "Save & Test" and you should get confirmation that the data source
is working.

### Sending simple metrics

In this simple setup, Telegraf is configured to receive metrics over statsd (a simple line protocol) and add
them to influx. You can send a simple counter metric called "tickdemo.counter"" in this way with the following 
command that you execute from the terminal (on your host):
```
echo "tickdemo.counter,tag1=val1,tag2=val2:1|c" | nc -w 1 -u localhost 8125
```

Feel free to tune the command and play around with different tag values and such...
Telegraf has a wide range of plugins. It can receive metrics from a variety of sources. Check out 
[the plugin directory](https://docs.influxdata.com/telegraf/v1.26/plugins/) for an overview.

### Visualing metrics in a Grafana dashboard

In Grafana, create a new dashboard from an empty panel. Make sure to use the following data:
- Data Source: tickdemo (should already be selected)
- From: `tickdemo.counter` should appear in the dropdown

This should already visualize the metric you sent earlier. From here, start playing with the other options
(e.g. grouping metrics per 10 minutes, visualizing tag values, ...)



