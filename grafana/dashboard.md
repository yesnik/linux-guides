# Grafana Dashboard

See [docs](https://grafana.com/docs/grafana/latest/dashboards/)

See [Dashboards Library](https://grafana.com/grafana/dashboards/)

## Add Data source

- Menu *Connections* - *Data Sources*
- Select item "Prometheus"
- Prometheus server URL: http://localhost:9090 

## Import Dashboard from Prometheus site

Let's import Redis dasboard:

- Visit site: https://grafana.com/grafana/dashboards/?dataSource=prometheus
- In filters select Data Source - "Prometheus", search query - "redis"
- There we found Dashboard: https://grafana.com/grafana/dashboards/14091-redis-dashboard-for-prometheus-redis-exporter-1-x/
- Copy dashboard Id - `14091`
- Visit our Grafana: http://localhost:3000/
- Menu *Dashboards* - press button *New* - *New dashboard*
- Press button *Import dashboard*
- Insert Dashboard Id (`14091`) the field "Find and import dashboards for common applications at grafana.com/dashboards", press button "Load"
