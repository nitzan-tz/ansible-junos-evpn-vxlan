# 'monitoring-analytics' role
Generate configuration for Mirroring or Analyzer on QFX5100 switches that can be send to remote IP 
encapsulated in GRE (ERSPAN) 
To decode it in Wireshark go to edit->preferences->protocols->ERSPAN and set Force to decode Fake ERSPAN frame 

The configuration per device is in host_vars/{inventory_hostname}/monitoring.mirror-analyzer.yaml



################Configuration Example ###################################
		
analyzer:
    destination_ip: 192.168.5.107 # The ip that capture the GRE traffic it should be accessible from inet.0 (and not the management interface)
    interfaces:
        -   name: ge-0/0/4 		  # List of interfaces to send all traffic from them to the analyzer
        -   name: xe-0/0/7
        
mirror:
    destination_ip: 192.168.5.107 # The ip that capture the GRE traffic it should be accessible from inet.0 (and not the management interface)

#########################################################################

analyzer send all of the traffic from the interface to the analyzer server
port-mirrot can limit the traffic based on firewall filter on ###ingress only### so you must capture on two switches to see the traffic
https://www.juniper.net/techpubs/en_US/junos15.1/topics/concept/port-mirroring-qfx-series-understanding.html 
You have to define the filter manually for example

filter MIRROR {
    term MIRROR {
        from {
            destination-port 80;
            ip-source-address {
                10.10.1.1/32;
            }
            ip-protocol tcp;
        }
        then {
            accept;
            port-mirror-instance PORT_MIRROR;
        }
    }
    term ALL {      ######## Dont forget this term otherwise it will drop all traffic
        then accept;
    }	
}

And apply it is input filter like this

set interfaces ge-0/0/4 unit 0 family ethernet-switching filter input MIRROR 
	