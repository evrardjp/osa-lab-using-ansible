# BOOTSTRAP HOSTS USAGE

1. Create an inventory with your node names.
2. Create your network for encapsulation on your cloud.
3. Create your instances using your favorite method (create-instances doesn't work yet). Don't forget to connect your host to the previously created network
4. Define your group_vars and host_vars.

    4.1 Make sure group_vars have at least:

            bootstrap_host_aio_config: False
            encapsulation_interface: <your encapsulation interface> (for example eth2)

    4.2 Make sure each node has a node_id (using host_vars)

    4.3 If you want your nodes to use the internet, override it's state_change scripts in the host_vars. Example:

            bridge_vlan_state_change_scripts: |
                pre-up ip link add br-vlan-veth type veth peer name eth12 || true
                pre-up ip link set br-vlan-veth up
                pre-up ip link set eth12 up
                post-down ip link del br-vlan-veth || true
                {{ bridge_iptables_rules }}