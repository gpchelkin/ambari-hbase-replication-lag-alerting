# Ambari HBase Replication Lag Alerting

Ambari Alerts triggered from the HBase replication status: this alert is triggered if HBase replication is lagging or failing

Works in both Kerberised and non-Kerberised environments.

The alert parses the HBase shell `status 'replication'` command output and alerts when certain values reach thresholds.

## Alert definition

The alert takes two parameters for configuration:

1. Maximum length of the HBase replication log queue in WALs.
2. Maximum value of the HBase replication lag in ms.

## Alert installation

> Taken from [https://github.com/monolive/ambari-custom-alerts](https://github.com/monolive/ambari-custom-alerts)

Push the new alert via Ambari REST API.

```sh
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X POST -d @alert_hbase_replication.json http://ambari.cloudapp.net:8080/api/v1/clusters/hdptest/alert_definitions
```
You will also need to copy the python script in `/var/lib/ambari-server/resources/host_scripts` and restart the ambari-server:

```sh
cp alert_hbase_replication.py /var/lib/ambari-server/resources/host_scripts/
ambari-server restart
```
After restart the script will be pushed in `/var/lib/ambari-agent/cache/host_scripts` on the HBase Master hosts.

You can find the ID of your alerts by running
```sh
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X GET http://ambari.cloudapp.net:8080/api/v1/clusters/hdptest/alert_definitions
```

If we assume, that your alert is id 103. You can force the alert to run by
```sh
curl -u admin:admin -i -H 'X-Requested-By: ambari' -X PUT  http://ambari.cloudapp.net:8080/api/v1/clusters/hdptest/alert_definitions/103?run_now=true
```

## [License](LICENSE)

Copyright (c) 2016 Alex Bush.
Licensed under the [Apache License](LICENSE).
