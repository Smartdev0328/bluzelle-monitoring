# bluzelle-monitoring

# 1. Install Node Exporter
Please install Node Exporter on the node host.
```
wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
tar xvfz node_exporter-1.5.0.linux-amd64.tar.gz
```
Run
```
cd node_exporter-1.5.0.linux-amd64
./node_exporter
```
# 1. Install Prometheus
## 1) System Configuration 
```
sudo useradd --no-create-home --shell /bin/false prometheus
sudo mkdir /etc/prometheus /var/lib/prometheus
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
```

## 2) install binary

```
curl -LO https://github.com/prometheus/prometheus/releases/download/v2.37.0/prometheus-2.37.0.linux-amd64.tar.gz
tar -xvf prometheus-2.37.0.linux-amd64.tar.gz
sudo cp -p ./prometheus-2.37.0.linux-amd64/prometheus /usr/local/bin
sudo cp -p ./prometheus-2.37.0.linux-amd64/promtool /usr/local/bin
```

## 3) system configuration for user prometheus
```
sudo chown prometheus:prometheus /usr/local/bin/prom*
sudo cp -r ./prometheus-2.37.0.linux-amd64/consoles /etc/prometheus
sudo cp -r ./prometheus-2.37.0.linux-amd64/console_libraries /etc/prometheus
sudo cp -p ./prometheus-2.37.0.linux-amd64/prometheus.yml /etc/prometheus
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
```

## 4) configure settings for prometheus

```
git clone https://github.com/Smartdev0328/bluzelle-monitoring.git
```
Change the yml files in prometheus/sd/mainet and prometheus/sd/testnet. Replace the ip_here with the hosting server.

```
Replace ip_here -------> <hosting server address>
```
 
Then move the config files to /etc/prometheus
```
cd prometheus
cp rules -r /etc/prometheus
cp sd -r /etc/prometheus
cp prometheus.yml
```

## 5) start the prometheus server
```
sudo -u prometheus /usr/local/bin/prometheus --web.listen-address=:19090 --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus --web.console.templates=/etc/prometheus/consoles --web.console.libraries=/etc/prometheus/console_libraries
```


# 2. Install Grafana Server
## 1) Download and Install

```
wget https://dl.grafana.com/enterprise/release/grafana-enterprise-9.4.7.linux-amd64.tar.gz
tar -zxvf grafana-enterprise-9.4.7.linux-amd64.tar.gz
```

## 2) configure settings using provisioning
 Move to github root repo and copy dashboard settings into grafana_server directory.
 ```
 rm -rf <grafana_dir>/conf/provisioning/dashboards/dashboards.yml
 cp -r grafana/provisioning/dashboards/dashboards.yml <grafana_dir>/conf/provisioning/dashboards 
 ```

 Move dashboard data into /etc directory.
 ```
 cp -r grafana /etc
 ```
## 3) start the Grafana server
```
./bin/grafana-server
```
## 4) login <host_ip>:3000 with your browser.
username is "admin"
password is "admin"

Navigate to Configuration/Data sources
Add new Prometheus data source with the URL ```http://localhost:19090```.
Set this dashboard as a default.
## 5) Navigate into dashboards
You can see the dashboards!!


