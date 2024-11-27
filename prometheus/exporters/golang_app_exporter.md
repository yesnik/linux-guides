# Golang App Exporter

## Install Go

Available Go releases: https://go.dev/dl/

```bash
cd ~
wget https://go.dev/dl/go1.23.3.linux-amd64.tar.gz
tar -xzvf go1.23.3.linux-amd64.tar.gz
rm -rf go1.23.3.linux-amd64.tar.gz
```
Edit `.bashrc` for our user:

```bash
echo 'export GOROOT=$HOME/go' >> .bashrc
echo 'export PATH=$PATH:$GOROOT/bin' >> .bashrc
```
Check Go version:

```bash
go version
```

## Create simple Golang app

```bash
mkdir -p ~/app/src
```

File `~/app/src/main.go`

```go
package main

import (
    "fmt"
    "log"
    "net/http"
    "time"
    "math/rand"

    // Prometheus Cli for Prometheus:
    "github.com/prometheus/client_golang/prometheus"
    "github.com/prometheus/client_golang/prometheus/promhttp"
)

// Define our metrics
var (
    // API Duration
    apiDurations = prometheus.NewSummary(
        prometheus.SummaryOpts{
            Name: "app_api_durations_seconds",
            Help: "API latency distributions.",
            Objectives: map[float64]float64{0.5: 0.05, 0.9: 0.01, 0.99: 0.001},
        }
    )
    
    // Processed requests amount
    apiProcessed = prometheus.NewCounter(prometheus.CounterOpts{
        Name: "app_api_processed_ops_total",
        Help: "The total number of processed requests",
    })
)

func handler(w http.ResponseWriter, r *http.Request) {
    // Increment requests counter
    apiProcessed.Inc()

    // Start time of the request
    start := time.Now()

    time.Sleep(time.Duration(rand.Intn(2)) * time.Second)

    fmt.Fprintf(w, "%s\n", r.URL.Path)

    // Execution time
    duration := time.Since(start)
    // Save execution time to metric
    apiDurations.Observe(duration.Seconds())
}

func main() {
    // Metrics Init
    prometheus.MustRegister(apiDurations)
    prometheus.MustRegister(apiProcessed)

    rand.Seed(time.Now().UnixNano())
    http.HandleFunc("/", handler)

    // Handler for URI '/metrics'
    http.Handle("/metrics", promhttp.Handler())

    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

Let's build our app:

```bash
export GOPATH=$HOME/app
go mod init
go get
go build -o app main.go
```

Test our app:

```bash
curl localhost:8080/hello
```

Test if we can get metrics:

```bash
curl localhost:8080/metrics
```

## Add exporter to Prometheus

Edit `prometheus.yml`:

```
scrape_configs:
  - job_name: 'app'
    static_configs:
    - targets: ['localhost:8080']
```
