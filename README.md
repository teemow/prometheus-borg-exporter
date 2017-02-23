# Borg exporter

Export borg information to prometheus.

## Dependencies

 * Prometheus (obviously)
 * Node Exporter with textfile collector
 * Borg (https://github.com/borgbackup/borg)

## Install

Copy `borg_exporter` to `/usr/local/bin`.

Copy `borg.env` to `/etc/borg` and replace your repokey and repository in it.

Copy the systemd unit to `/etc/systemd/system` and run 

```
systemctl enable prometheus-borg-exporter.timer
systemctl start prometheus-borg-exporter.timer
```

Alternative: Use `ExecStartPost` in your borg backupt timer itself to write our the metrics.

## Configure your node exporter

Make sure your node exporter uses `textfile` in `--collectors.enabled` and add the following parameter: `--collector.textfile.directory=/var/lib/node_exporter/textfile_collector`

## Example queries

```
backup_total_size_dedup{job='node'}
backup_last_size_dedup{job='node'}
backup_chunks_total{job='node'}
```

### Grafana dashboard

See [here](https://grafana.net/dashboards/1573) for a sample grafana dashboard.
