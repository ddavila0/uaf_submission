# UAF Monitoring

The UAF monitoring can be see [here](https://graph.t2.ucsd.edu:3000/dashboard/db/ucsd-glideinwms-pool?orgId=1&from=now-24h&to=now&var-Sites=All)

## The setup

As in most of the monitoring setups there are 3 main components in the system: a **producer** wich *produces* or fetches the metrics data, the **database** to store the metrics and the **consumer** which makes use of the stored data to produce nice plots.

In this setup the producer is the condor_glanglia daemon that lives in the Collector. This daemon with the help of a custom python script called [metrics-influxdb-condor-gangliad](monitoring/metrics-influxdb-condor-gangliad) sends condor data to the Influx data base. The details about this setup can bee seen below on "The producer"

The consumer is this [dashboard](https://graph.t2.ucsd.edu:3000/dashboard/db/ucsd-glideinwms-pool?orgId=1&from=now-24h&to=now&var-Sites=All) in grapaha that queries the data base to produce plots. The details about how the Site plots are automatically created can be seen below under "The consumer"

### The producer
HTCondor can be easily integrated into Ganglia to monitor condor metrics. Look at the [ganglia chapter](http://research.cs.wisc.edu/htcondor/manual/latest/Monitoring.html) on the condor manual for more information.

In the common way, condor uses the **condor_gangliad** daemon commonly to send condor metrics to *gmond* (a ganglia daemon in charge of collecting metrics) using the libganglia. gmond in turn will send these metrics, together with other ganglia specific metrics, to the central monitor node.


Since we are not interested in sending data to Ganglia but to our monitoring facility, we need a way to intercept the metrics coming from the **condor_gangliad**. Here is when the condor knob **GANGLIA_GMETRIC** comes in practice. See [3.5.34 condor_gangliad Configuration File Macros](http://research.cs.wisc.edu/htcondor/manual/latest/ConfigurationMacros.html#x33-2300003.5.34) on the condor manual for more details.

The **GANGLIA_GMETRIC** knob tells condor to use *gmetric* (a ganglia binary used to send metrics to *gmond*) instead of the libganglia to communicate with *gmond* and also it defines the location of the *gmetric* binary. We use this knob to point to a custom python scripit: [metrics-influxdb-condor-gangliad](monitoring/metrics-influxdb-condor-gangliad) that receives the metrics from **condor_gangliad** and send them to the Influx database.


#### The metrics-influxdb-condor-gangliad script

The script does 3 main things:

1. Parse a configuration file (See example [here](monitor/config_file_example)

[1](metrics-influxdb-condor-gangliad)
[py](metrics-influxdb-condor-gangliad.py)
[txt](metrics-influxdb-condor-gangliad.txt)
[md](metrics-influxdb-condor-gangliad.md)

### The consumer


