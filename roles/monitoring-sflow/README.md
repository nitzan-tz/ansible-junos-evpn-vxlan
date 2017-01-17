# 'monitoring-sflow' role
Generate configuration for sflow on QFX5100 switches that can be send to sflow collector

The configuration per device is in host_vars/{inventory_hostname}/monitoring.sflow.yaml

sflow:
    collector: 192.168.5.107     # the sflow collector IP (The source ip is the lo0 ip )
    collector_port: 2025         # the collector port
    interfaces:                  # List of interfaces to gather queue and traffic stats
        -   name: et-0/0/48
        -   name: xe-0/0/4