## Fork from @averissimo

> You probably don't wanna be here! see [nicolargo/docker-influxdb-grafana](https://github.com/nicolargo/docker-influxdb-grafana)

Personal notes:

Had to build two images to work in raspberry pi:

* Grafana
* Pi-Hole Influx agent *(clone [jawn/pi-hole-influx](https://github.com/janw/pi-hole-influx/) to parent directory)*

Grafana was optional, as I didn't want it to fail in case there was no internet connection, thus I had to build my own grafana image with plugins already installed.

For that:

* `git clone https://github.com/janw/pi-hole-influx/ pi-hole-agent`
* `git clone https://github.com/grafana/grafana/ grafana-with-built-plugins`
* `cd grafana-with-built-plugins/packaging/docker/custom`
* Run command bellow *(network=host argument is needed as otherwise the container doesn't have internet.. at least in my pi)*

```
$ docker build --platform arm64 -t grafana:latest-with-plugins \
  --build-arg "GRAFANA_VERSION=master" \
  --build-arg GF_INSTALL_PLUGINS=grafana-clock-panel,briangann-gauge-panel,natel-plotly-panel,grafana-simple-json-datasource \
  --network=host .

$ cd ../../../..
$ docker-compose build
$ docker-compose pull
```

---

# Docker-compose files for a simple uptodate
# InfluxDB
# + Grafana stack
# + Telegraf

Get the stack (only once):

```
git clone https://github.com/nicolargo/docker-influxdb-grafana.git
cd docker-influxdb-grafana
docker pull grafana/grafana
docker pull influxdb
docker pull telegraf
```

Run your stack:

```
sudo mkdir -p /srv/docker/grafana/data
docker-compose up -d
sudo chown -R 472:472 /srv/docker/grafana/data

```

Show me the logs:

```
docker-compose logs
```

Stop it:

```
docker-compose stop
docker-compose rm
```

Update it:

```
git pull
docker pull grafana/grafana
docker pull influxdb
docker pull telegraf
```

If you want to run Telegraf, edit the telegraf.conf to yours needs and:

```
docker exec telegraf telegraf
```
