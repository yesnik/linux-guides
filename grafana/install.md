# Grafana Install

See [docs](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/)

```bash
sudo apt-get install -y apt-transport-https software-properties-common wget
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
# Updates the list of available packages
sudo apt-get update
# Installs the latest OSS release:
sudo apt-get install grafana

sudo systemctl daemon-reload
sudo systemctl start grafana-server

# Check status
sudo systemctl status grafana-server
```
Open page in browser: http://site.org:3000/

Use credentials: `admin / admin`
