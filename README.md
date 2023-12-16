# Home network

My home network config aiming ad-blocking and DNS over HTTPs by default. The hosts of this configuration were ubuntu only, you can try in your favorite distro by changing parts from APT to your package manager.

## Requirements

These requirements are for managing Ansible tool on control node (probably your main PC on network), all managed nodes requirements are automated by ansible tasks and roles.

### Ansible

Install ansible on control node.

### Linux

At the moment, ansible only works on linux based system (but you can try on docker).

### SSH

Ansible work mode is based on ssh, so you need to connect the control node to managed nodes by SSH.

## Components

For this setup all components choosen are open-source, there are other alternatives but these components have a great support by community and are widely used

### Ubuntu

The distro choosen for the host was Ubuntu 22.04 LTS with BTRFS filesystem.
There's nothing special here I personally use Arch Linux (BTW) on my desktop but it's easy for me to get working on remote systems with systemd APT-based distros.

### PiHole

Pihole is a open-source ad-blocking solution that intercepts network traffic and block advertisements based on lists that you can provide.
It was created to run on Raspiberry Pi but we can install in ubuntu easily, in this context I choose to run in docker.

### PiHole Exporter

This isn't necessary for this setup but I wanted it to center all relevant metrics in one place, Grafana. So this exporter will be used for Prometheus metrics ingestion

### Cloudflared

Clouflared is a daemon built by cloudflare that provide us DNS over HTTPs functionality, usually our Internet Service Provider does not provide a secure way to transfer data on internet sending in plain text.
This will be used as PiHole DNS configuration to apply DoH in our DNS queries, these queries are proxied to Cloudflare 1.1.1.1 and 1.0.0.1

### Prometheus

PiHole and Cloudflared are the only requirements to get this running, but as every production system needs observability I wanted to have necessary metrics for short-period storage and the capacity of alerts.
Prometheus is maybe the most used tool for infrastructure monitoring working as Time Series Database, it uses a pull model to get metrics, meaning that our services will have a http /metrics endpoint to be queried by prometheus

### Grafana

Prometheus is used for collecting and storing metrics, but Grafana is the greatest tool to create dashboards for these prometheus metrics.
Grafana is a Dashboard system created by Grafana Labs, company that has great tools like Loki (Logs), Grafana (View), Mimir (Metrics/Prometheus), Tempo (Tracing)
