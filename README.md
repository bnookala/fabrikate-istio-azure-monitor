# Fabrikate Istio Azure Monitor

A [Fabrikate](github.com/microsoft/fabrikate) component to install a mixer component for Azure Monitor services.

This component presumes you have istio installed and available on your cluster.

## Configuration

The default configuration is as follows:

```yaml
istioNamespace: "istio-system"

image:
  repository: mcr.microsoft.com/applicationinsights/istiomixeradapter
  tag: v0.2
  pullPolicy: Always

instrumentation:
  # instrumentation key for the Application Insights resource to send telemetry to
  applicationInsightsKey: "00000000-0000-0000-0000-000000000000"
  # comma-separated list of Kubernetes namespaces to monitor. Leave empty if all namespaces must be monitored
  watchingNamespaces: "default"
  # Log level: Info is the default, Trace for detailed tracing, Error for errors only
  logLevel: "Info"
  # See https://docs.microsoft.com/en-us/azure/azure-monitor/app/sampling#types-of-sampling
  adaptiveSamplingLimit: 1000
```

To override the default configuration, you add a new configuration environment in the `./config` directory, following the convention in `prod.yaml` ie:

```yaml
config:
  instrumentation:
    logLevel: Trace
    applicationInsightsKey: "00000000-0000-0000-0000-000000000000"
    watchingNamespaces: "default"
```

Replace the Application Insights Key and the Live Metric Stream Key with the issued keys from Azure Monitor.

## Installation

Download the appropriate [Fabrikate](github.com/microsoft/fabrikate/releases) release for your OS, then invoke the following commands:

```
$ fab install
$ fab generate prod
$ kubectl apply -f --recursive ./generated/prod/
```

## License

MIT - See [LICENSE](./LICENSE) file.
