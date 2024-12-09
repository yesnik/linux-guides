# Grafana Install

See [docs](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/)

## Rpm Package

See [actual instruction](https://grafana.com/grafana/download?edition=oss&platform=linux)

```bash
sudo apt-get install -y adduser libfontconfig1 musl
wget https://dl.grafana.com/oss/release/grafana_11.4.0_amd64.deb
sudo dpkg -i grafana_11.4.0_amd64.deb

# Start grafana-server automatically using systemd
sudo /bin/systemctl daemon-reload
sudo /bin/systemctl enable grafana-server

# Start grafana-server
sudo /bin/systemctl start grafana-server
```

## Ubuntu Repo

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
