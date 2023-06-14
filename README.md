# Dashboard for monitoring a Mastodon instance

![Screenshot of the dashboard](https://portfolio.lukenelson.uk/MastodonDashboard.png)

This project was inspired by [this blog post](https://ipng.ch/s/articles/2022/11/27/mastodon-3.html) by [IPngNetworks](https://ublog.tech/@IPngNetworks) [Mastodon Setup with Docker Compose](https://github.com/raeffs/mastodon-docker-compose) by [raeffs](https://github.com/raeffs). In his blog, Pim discusses how he uses Prometheus and Grafana to monitor his Mastodon instance. Everything used is FOSS and available as a container, so I was keen to get it all working in a docker compose stack.

## Setup

1. Clone the repository

```bash
git clone https://github.com/luc122c/mastodon-metrics.git
```

2. Create a `.env` file and set up your credentials

```sh
ADMIN_USER='admin'
ADMIN_PASSWORD='pleasechangeme'
ALLOW_SIGN_UP=false
```

3. Create a volume for Grafana with the command `docker volume create grafana-data`
4. Run `docker compose up` and wait for it all to load
5. Visit `localhost:3000` and login. You should see the 'Mastodon Stats' dashboard available.
6. Configure Mastodon to log [StatsD](https://docs.joinmastodon.org/admin/config/#statsd) events to your server with the following environment variable. If Mastodon is running on a separate machine, replace `localhost` with the IP address of that machine.

```sh
STATSD_ADDR='localhost:9125'
```

7. Restart Mastodon so that the changes take effect. Data should start to flow into the dashboard.

## Configuration

I've commented out some ports that don't need to be exposed for it to work. You can open `9102` on `exporter` to see the raw data and `9090` on `prometheus` to see the the Prometheus output.

One other thing I've changed from IPngNetworks' config is the UID of the datasource. The existing value wasn't working so it is now `sausage`.

## References and Thanks

Below are the links to the software, configuration files and articles that make this work.

### References

- [Mastodon - Part 3 - statsd and Prometheus](https://ipng.ch/s/articles/2022/11/27/mastodon-3.html) - The brains of the operation
- [Mastodon Setup with Docker Compose](https://github.com/raeffs/mastodon-docker-compose) - Thank you for getting me into Mastodon and for inspiring me to make this stack
- [Mastodon StatsD documentation](https://docs.joinmastodon.org/admin/config/#statsd)

### Software

- [StatsD to Prometheus metrics exporter](https://github.com/prometheus/statsd_exporter)
- [Prometheus](https://github.com/prometheus/prometheus) - Monitoring and Database
- [Grafana](https://github.com/grafana/grafana) - data visualization platform

### Configuration files

- [Mastodon Stats Grafana dashboard](https://grafana.com/grafana/dashboards/17492-mastodon-stats/) - Makes the pretty visuals
- [StatsD Exporter mapping configuration file](https://ipng.ch/assets/mastodon/statsd-mapping.yaml) - Speaks Mastodon and Prometheus at the same time
- [Metrics with Prometheus StatsD Exporter and Grafana](https://dev.to/kirklewis/metrics-with-prometheus-statsd-exporter-and-grafana-5145) - Thank you for helping me configure the automatic provisioning of the dashboard and datasource.
- [Grafana Data Source on Startup](https://community.grafana.com/t/data-source-on-startup/8618/2) - Thank you for helping me configure the automatic provisioning of the dashboard and datasource.
