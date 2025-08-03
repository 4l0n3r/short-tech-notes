Observability :
    Logging
    Metrics
    Tracing

Logging:
    record - application message with timestamp


                                                                    FluentBit
Input                                       outputs
Kubernetes                                  ES
Systemlogs          Fluent Bit              S3
Prometheus                                  Splunk

Architechture :
Input   -   taking logs from source
Parser  -   convert logs to specific format
Filter  -   remove some logs
Buffer  -   Aggregate based on calculation
Routing -   Will send logs to destination based on routes
Outputs


                                                                        Prometheus
Why:
    handling 100 of servers is hard.
    If there is an error in chain of components, we can see the error at UI but don't know where is the exact error. for the exact error we need to debug and go backwards to know the defective service.
    Insted of this we can monitor every service and log status of service whenever there is a failure.
    Can also identify problem before occures.
        70% Usage

target: what does it monitor ?
    Nginx Server / Python Server / Database

metrics: which units are monitered ?
    CPU status / Memory Usage / requests count

Architecture :
    Premotheus Server:
        Retrieval
            to pull the metrics from services
        Storage
            to store the metrics data
        Http Server
            accepts promQL queries and returns the response

    Prometheus Web UI / Grafana : will send the request( PromQL ) to Http Server then visualize the response on Grafana Dashboard.

    AlertManager
        How does prometheus trigger the alert ?
        master -> push -> Alert Manager -> trigger -> email , slack

Metrics types:
    Counter: how many requests ? ( increase )
    Gauge: what is the cpu usage ? ( increase and decrease )
    Histogram: How long and how big ?

How it pulls the data:
    try to retrieve the data from /metrics endpoint
    it will accept only the above 3 formats.

Some services will directly expose the /metrics endpoint so prometheus can directly get the data from there. But all services won't expose the /metrics endpoint. There is a component to satisfy the above 2 conditions.
Exporter tasks: ( mediator btw target and prometheus ) It will get installed as a sidecar container for the services.
    1. will fetch the metrics from target
    2. update the format
    3. expose at /metrics

Exporters will be different based on the service. Its like an application agent in AppDynamics.
There are docker images for Exporters. We should install python_exporter along with python_application inside the pod.

How can we monitor our own application ?
    We can use client libraries to expose /metrics endpoint.

Push System: 
    Like Cloud Watch / New Relic
    Applications / Servers will push the metrics to a cetralized platform.
    Handling this high traffic would be a bottleneck

PushGateWay:
    Short lived job :
        push metrics when exit.

Configuration File:
    How often: prometheus scrape the data
    Rule : for rising alert and aggregating metrics
    What: resources to monitor

Where does prometheus store the data ? local + remote

DisAdvantages:
    Difficult to scale : either increase the prometheus server capacity / decrease the metrics count.


