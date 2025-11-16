+++
title = 'Hello World'
date = 2024-02-09T17:46:41+05:30
# categories = ["Hugo", "Basic"]
# tags = ["Hugo", "howto"]

+++


- Personally I think the whole Grafana OSS stack is great for most use cases - Prometheus/Cortex/Mimir (metrics), OpenTelemetry/Tempo (traces), Fluent/Loki (logs). You'll have to do more work to build useful dashboards and "intelligence" based on all the data, but it's a very technically capable system that scales very well, and the cost is so much lower than DataDog/NewRelic/etc if you can invest a little headcount into running it yourself.