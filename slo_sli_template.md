# API Service

| Category     | SLI                                                                                           | SLO                                                                                                         |
|--------------|-----------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| Availability | Percentage of successful requests over total requests (Success rate)                           | 99%                                                                                                         |
| Latency      | Percentage of requests that complete within 100ms (e.g., P90 latency)                          | 90% of requests below 100ms                                                                                 |
| Error Budget | Percentage of failed requests or error rate over a given time period (e.g., failure rate <= 20%)| Error budget is defined at 20%. This means that 20% of the requests can fail and still be within the budget |
| Throughput   | Requests per second (RPS) measured over a specified time interval                              | 5 RPS indicates the application is functioning                                                              |



# Creating the SLO/SLI Dashboard for Prometheus
Availability (Percentage of successful requests):
Prometheus Query: 
    sum (rate(apiserver_request_total{job="apiserver",code!~"5.."}[2d])) / sum (rate(apiserver_request_total{job="apiserver"}[2d]))

Throughput:
Prometheus Query:
    sum(rate(apiserver_request_total{job="apiserver",code=~"2.."}[5m]))

Latency:
Prometheus Query:   
    histogram_quantile(0.95,sum(rate(apiserver_request_duration_seconds_bucket{job="apiserver"}[5m])) by (le, verb))

Error Budget:
Prometheus Query:
    1 - ((1 - (sum(increase(apiserver_request_total{job="apiserver", code="200"}[7d])) by (verb)) / sum(increase(apiserver_request_total{job="apiserver"}[7d])) by (verb)) / (1 - .90))

# Dashboard Panels
Each of these queries will be visualized in the respective panels of the dashboard to track the SLO metrics:
1. Availability Panel: Displays the percentage of successful requests.
2. Throughput Panel: Displays the rate of successful requests per second.
3. Latency Panel: Displays the 90th percentile of request completion time in milliseconds. 
4. Error Budget Panel: Displays the remaining error budget as a percentage.

