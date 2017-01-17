overlay:
    tenants:
      VRF_1:							# VRF name
        id: 30							# VRF ID
        bridge_domains:
        - vlan_id: 3000					#Vlan ID
          vni_id: 13000
          description: "TTT"			#Description
          vip_ip: 10.152.200.1			#Virtual-gateway-address
          network: 10.152.200.0/22		#Subnet of the network       
        static_route:					#Static routes per VRF
        - route: 0.0.0.0/0
          next_hop: 10.152.204.29
        import_tenants:					#vrf-import of other VRF
        - name: VRF_2					# VRF Name of the other VRF (for policy term name)
          id: 31						# VRF id  of the other VRF (For the community )
        - name: VRF_3
          id: 32          
        input_filter:					# Input filter name (you have to add the filter to role/common/templates/main.conf.j2 )
		output_filter:					# Output filter name (you have to add the filter to role/common/templates/main.conf.j2 )



#***************Old way ****************************

# 'generate-tenant-vni' role


Generate variable files to scale easily the number of tenant and VNI per tenant.
This template will generate a YAML file for each device under host_vars directory named 'generated_tenant_vni.yaml'

Template can be found in [roles/generate-tenant-vni/templates/main.conf.j2 ](templates/main.conf.j2)

## Variables needed by the template

```yaml
generate_tenant:
  nbr_tenant:             # Number of tenant that will be created
  nbr_vni_tenant:         # Number of VNI to create per tenant
  tenant_start_id:        # Where do we start to create tenant ID
  vlan_start:             # Where do we start to create vlan ID
  vni_prefix:             # VNI will be generated with <vni_prefix><vlan_id>
  vip_id: 1               # Id of the virtual gateway in the subnet

  network_pool:           # Network pool to use to generate one subnet for each VNI
  network_size:           # Size of the network to generate for each VNI

  loopback_pool:          # Network pool to use to generate loopback for each tenant per device
  loopback_size: 24       # Size of the network to use to generate these loopback
```

### Example

**For Group 'all'**
```yaml
# group_vars/all/tenant_vni.yaml
generate_tenant:
  nbr_tenant: 10
  nbr_vni_tenant: 10
  tenant_start_id: 10
  vlan_start: 100
  vni_prefix: 10
  vip_id: 1

  network_pool: 100.0.0.0/16
  network_size: 24

  loopback_pool: 101.0.0.0/16
  loopback_size: 24

```
