# 'monitoring-analytics' role
Generate configuration for service analytics on QFX5100 switches that can be send to open-nti

The configuration per device is in host_vars/{inventory_hostname}/monitoring.analytics.yaml

analytics:
    collector: 192.168.5.107 # the analytics collector IP e.g. open-nti it should be accessible from inet.0
    collector_port: 50020    # the collector port
    interfaces:				 # List of interfaces to gather queue and traffic stats
        -   name: et-0/0/48
        -   name: ge-0/0/4